---
title: Solução de problemas usando a ferramenta de diagnóstico de malha protegida
ms.custom: na
ms.prod: windows-server
ms.topic: article
ms.assetid: 07691d5b-046c-45ea-8570-a0a85c3f2d22
manager: dongill
author: rpsqrd
ms.technology: security-guarded-fabric
ms.date: 01/14/2020
ms.openlocfilehash: c69fc70282ff61ecce25f6413244d7ba3a5ba3bc
ms.sourcegitcommit: c5709021aa98abd075d7a8f912d4fd2263db8803
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 01/18/2020
ms.locfileid: "76265818"
---
# <a name="troubleshooting-using-the-guarded-fabric-diagnostic-tool"></a>Solução de problemas usando a ferramenta de diagnóstico de malha protegida

>Aplica-se a: Windows Server 2019, Windows Server (canal semestral), Windows Server 2016

Este tópico descreve o uso da ferramenta de diagnóstico de malha protegida para identificar e corrigir falhas comuns na implantação, configuração e operação contínua da infraestrutura de malha protegida. Isso inclui o serviço de guardião de host (HGS), todos os hosts protegidos e serviços de suporte, como DNS e Active Directory. A ferramenta de diagnóstico pode ser usada para executar uma primeira passagem na triagem de uma malha protegida com falha, fornecendo aos administradores um ponto de partida para resolver interrupções e identificar ativos configurados incorretamente. A ferramenta não é uma substituição por um sólido entendimento da operação de uma malha protegida e serve apenas para verificar rapidamente os problemas mais comuns encontrados durante as operações cotidianas.

A documentação completa dos cmdlets usados neste artigo pode ser encontrada na referência do [módulo HgsDiagnostics](https://docs.microsoft.com/powershell/module/hgsdiagnostics/?view=win10-ps).

[!INCLUDE [Guarded fabric diagnostics tool](../../../includes/guarded-fabric-diagnostics-tool.md)]

## <a name="quick-start"></a>Início Rápido

Você pode diagnosticar um host protegido ou um nó HGS chamando o seguinte de uma sessão do Windows PowerShell com privilégios de administrador local:

```PowerShell
Get-HgsTrace -RunDiagnostics -Detailed
```

Isso detectará automaticamente a função do host atual e diagnosticará quaisquer problemas relevantes que possam ser detectados automaticamente.  Todos os resultados gerados durante esse processo são exibidos devido à presença da opção de `-Detailed`.

O restante deste tópico fornecerá uma explicação detalhada sobre o uso avançado de `Get-HgsTrace` para fazer coisas como diagnosticar vários hosts de uma vez e detectar uma configuração complexa de nó cruzado.

## <a name="diagnostics-overview"></a>Visão geral do diagnóstico

O diagnóstico de malha protegida está disponível em qualquer host com ferramentas e recursos relacionados à máquina virtual blindada instalados, incluindo hosts que executam o Server Core.  Atualmente, os diagnósticos estão incluídos nos seguintes recursos/pacotes:

1. Função de serviço guardião de host
2. Suporte do Hyper-V ao Guardião de Host
3. Ferramentas de Blindagem de VM para Gerenciamento de Malha
4. Ferramentas de Administração de Servidor Remoto (RSAT)

Isso significa que as ferramentas de diagnóstico estarão disponíveis em todos os hosts protegidos, nós HGS, determinados servidores de gerenciamento de malha e todas as estações de trabalho do Windows 10 com o [rsat](https://www.microsoft.com/download/details.aspx?id=45520) instalado.  O diagnóstico pode ser invocado de qualquer um dos computadores acima com a intenção de diagnosticar qualquer host protegido ou nó HGS em uma malha protegida; usando destinos de rastreamento remotos, o diagnóstico pode localizar e se conectar a hosts diferentes do computador que executa o diagnóstico.

Cada host direcionado pelo diagnóstico é chamado de "destino de rastreamento".  Os destinos de rastreamento são identificados por seus nomes de host e funções.  As funções descrevem a função que um destino de rastreamento específico executa em uma malha protegida.  Atualmente, os destinos de rastreamento dão suporte às funções `HostGuardianService` e `GuardedHost`.  Observe que é possível que um host ocupe várias funções de uma vez e também tenha suporte do diagnóstico, no entanto, isso não deve ser feito em ambientes de produção.  Os hosts do HGS e do Hyper-V devem ser mantidos separados e distintos em todos os momentos.

Os administradores podem iniciar qualquer tarefa de diagnóstico executando `Get-HgsTrace`.  Esse comando executa duas funções distintas com base nas opções fornecidas em tempo de execução: coleta de rastreamento e diagnóstico.  Essas duas combinadas compõem todo o que é a ferramenta de diagnóstico de malha protegida.  Embora não seja explicitamente necessário, os diagnósticos mais úteis exigem rastreamentos que só podem ser coletados com credenciais de administrador no destino de rastreamento.  Se privilégios insuficientes forem mantidos pelo usuário executando a coleta de rastreamento, os rastreamentos que exigem elevação falharão enquanto todos os outros passarão.  Isso permite o diagnóstico parcial no evento que um operador sob privilégios está executando a triagem. 

### <a name="trace-collection"></a>Coleta de rastreamento

Por padrão, `Get-HgsTrace` só coletará rastreamentos e os salvará em uma pasta temporária.  Os rastreamentos assumem a forma de uma pasta, nomeada após o host de destino, preenchido com arquivos especialmente formatados que descrevem como o host é configurado.  Os rastreamentos também contêm metadados que descrevem como os diagnósticos foram invocados para coletar os rastreamentos.  Esses dados são usados pelo diagnóstico para reidratar informações sobre o host ao executar o diagnóstico manual.

Se necessário, os rastreamentos podem ser revisados manualmente.  Todos os formatos são legíveis ao homem (XML) ou podem ser prontamente inspecionados usando ferramentas padrão (por exemplo, certificados X509 e extensões do Windows crypto Shell).  No entanto, observe que os rastreamentos não são projetados para diagnóstico manual e é sempre mais eficaz processar os rastreamentos com os recursos de diagnóstico do `Get-HgsTrace`.

Os resultados da execução da coleta de rastreamento não fazem nenhuma indicação quanto à integridade de um determinado host.  Eles simplesmente indicam que os rastreamentos foram coletados com êxito.  É necessário usar os recursos de diagnóstico do `Get-HgsTrace` para determinar se os rastreamentos indicam um ambiente com falha.

Usando o parâmetro `-Diagnostic`, você pode restringir a coleta de rastreamento somente aos rastreamentos necessários para operar o diagnóstico especificado.  Isso reduz a quantidade de dados coletados, bem como as permissões necessárias para invocar o diagnóstico.

### <a name="diagnosis"></a>Diagnóstico

Os rastreamentos coletados podem ser diagnosticados, desde que `Get-HgsTrace` o local dos rastreamentos por meio do parâmetro `-Path` e a especificação do comutador `-RunDiagnostics`.  Além disso, `Get-HgsTrace` pode executar a coleta e o diagnóstico em uma única passagem, fornecendo a opção `-RunDiagnostics` e uma lista de destinos de rastreamento.  Se não forem fornecidos destinos de rastreamento, o computador atual será usado como um destino implícito, com sua função inferida inspecionando os módulos instalados do Windows PowerShell.

O diagnóstico fornecerá resultados em um formato hierárquico que mostra quais destinos de rastreamento, conjuntos de diagnóstico e diagnósticos individuais são responsáveis por uma falha específica.  As falhas incluem recomendações de resolução e correção se uma determinação puder ser feita em qual ação deve ser executada em seguida.  Por padrão, os resultados de passagem e irrelevantes ficam ocultos.  Para ver tudo testado pelo diagnóstico, especifique a opção `-Detailed`.  Isso fará com que todos os resultados sejam exibidos independentemente do seu status.

É possível restringir o conjunto de diagnósticos que são executados usando o parâmetro `-Diagnostic`.  Isso permite especificar quais classes de diagnóstico devem ser executadas em relação aos destinos de rastreamento e suprimir todas as outras.  Exemplos de classes de diagnóstico disponíveis incluem rede, práticas recomendadas e hardware de cliente.  Consulte a [documentação do cmdlet](https://technet.microsoft.com/library/mt718831.aspx) para encontrar uma lista atualizada de diagnósticos disponíveis.

> [!WARNING]
> O diagnóstico não é uma substituição para um pipeline forte de monitoramento e resposta a incidentes.  Há um pacote System Center Operations Manager disponível para monitorar malhas protegidas, bem como vários canais de log de eventos que podem ser monitorados para detectar problemas antecipadamente.  O diagnóstico pode ser usado para fazer uma triagem rápida dessas falhas e estabelecer um curso de ação.

## <a name="targeting-diagnostics"></a>Direcionamento de diagnóstico

`Get-HgsTrace` opera com destinos de rastreamento.  Um destino de rastreamento é um objeto que corresponde a um nó HGS ou a um host protegido dentro de uma malha protegida.  Pode ser pensado como uma extensão para um `PSSession` que inclui informações necessárias apenas por diagnósticos, como a função do host na malha.  Os destinos podem ser gerados implicitamente (por exemplo, diagnóstico local ou manual) ou explicitamente com o comando `New-HgsTraceTarget`.

### <a name="local-diagnosis"></a>Diagnóstico local

Por padrão, `Get-HgsTrace` se destinará ao localhost (ou seja, onde o cmdlet está sendo invocado).  Isso é chamado de destino local implícito.  O destino local implícito só é usado quando nenhum destino é fornecido no parâmetro `-Target` e nenhum rastreamento pré-existente é encontrado no `-Path`.

O destino local implícito usa a inferência de função para determinar qual função o host atual desempenha na malha protegida.  Isso se baseia nos módulos do Windows PowerShell instalados que correspondem aproximadamente aos recursos que foram instalados no sistema.  A presença do módulo `HgsServer` fará com que o destino de rastreamento assuma a função `HostGuardianService` e a presença do módulo `HgsClient` fará com que o destino de rastreamento assuma a função `GuardedHost`.  É possível que um determinado host tenha ambos os módulos presentes nesse caso, ele será tratado como um `HostGuardianService` e um `GuardedHost`.

Portanto, a invocação padrão de diagnóstico para coletar rastreamentos localmente:

```PowerShell
Get-HgsTrace
```

... é equivalente ao seguinte:

```PowerShell
New-HgsTraceTarget -Local | Get-HgsTrace
```

> [!TIP]
> `Get-HgsTrace` pode aceitar destinos por meio do pipeline ou diretamente por meio do parâmetro `-Target`.  Não há nenhuma diferença entre as duas operações.

### <a name="remote-diagnosis-using-trace-targets"></a>Diagnóstico remoto usando destinos de rastreamento

É possível diagnosticar remotamente um host gerando destinos de rastreamento com informações de conexão remota.  Tudo o que é necessário é o nome do host e um conjunto de credenciais capazes de se conectar usando a comunicação remota do Windows PowerShell.
```PowerShell
$server = New-HgsTraceTarget -HostName "hgs-01.secure.contoso.com" -Role HostGuardianService -Credential (Enter-Credential)
Get-HgsTrace -RunDiagnostics -Target $server
```
Este exemplo irá gerar um prompt para coletar as credenciais de usuário remoto e, em seguida, o diagnóstico será executado usando o host remoto em `hgs-01.secure.contoso.com` para concluir a coleta de rastreamento.  Os rastreamentos resultantes são baixados para o localhost e, em seguida, diagnosticados.  Os resultados do diagnóstico são apresentados da mesma forma que ao executar o [diagnóstico local](#local-diagnosis).  Da mesma forma, não é necessário especificar uma função, pois ela pode ser inferida com base nos módulos do Windows PowerShell instalados no sistema remoto.

O diagnóstico remoto utiliza a comunicação remota do Windows PowerShell para todos os acessos ao host remoto.  Portanto, é um pré-requisito que o destino de rastreamento tem a comunicação remota do Windows PowerShell habilitada (consulte [habilitar PSRemoting](https://technet.microsoft.com/library/hh849694.aspx)) e que o localhost está configurado corretamente para iniciar conexões com o destino.

> [!NOTE]
> Na maioria dos casos, só é necessário que o localhost seja uma parte da mesma floresta Active Directory e que um nome de host DNS válido seja usado.  Se seu ambiente utiliza um modelo de Federação mais complicado ou se você deseja usar endereços IP diretos para conectividade, talvez seja necessário executar configurações adicionais, como definir os [hosts confiáveis](https://technet.microsoft.com/library/ff700227.aspx)do WinRM.

Você pode verificar se um destino de rastreamento está corretamente instanciado e configurado para aceitar conexões usando o cmdlet `Test-HgsTraceTarget`:
```PowerShell
$server = New-HgsTraceTarget -HostName "hgs-01.secure.contoso.com" -Role HostGuardianService -Credential (Enter-Credential)
$server | Test-HgsTraceTarget
```
Esse comando retornará `$True` se e somente se `Get-HgsTrace` puder estabelecer uma sessão de diagnóstico remoto com o destino de rastreamento.  Após a falha, esse cmdlet retornará informações de status relevantes para a solução de problemas da conexão remota do Windows PowerShell.

#### <a name="implicit-credentials"></a>Credenciais implícitas

Ao executar o diagnóstico remoto de um usuário com privilégios suficientes para se conectar remotamente ao destino de rastreamento, não é necessário fornecer credenciais para `New-HgsTraceTarget`.  O cmdlet `Get-HgsTrace` reutilizará automaticamente as credenciais do usuário que invocou o cmdlet ao abrir uma conexão.

> [!WARNING]
> Algumas restrições se aplicam à reutilização de credenciais, especialmente ao executar o que é conhecido como "segundo salto".  Isso ocorre quando se tenta reutilizar credenciais de dentro de uma sessão remota para outra máquina.  É necessário configurar o [CredSSP](https://technet.microsoft.com/library/hh849872.aspx) para dar suporte a esse cenário, mas isso está fora do escopo do gerenciamento de malha protegida e solução de problemas.

#### <a name="using-windows-powershell-just-enough-administration-jea-and-diagnostics"></a>Usando o Windows PowerShell apenas JEA (administração suficiente) e diagnósticos

O diagnóstico remoto dá suporte ao uso de pontos de extremidade do Windows PowerShell JEA. Por padrão, os destinos de rastreamento remotos se conectarão usando o ponto de extremidade de `microsoft.powershell` padrão.  Se o destino de rastreamento tiver a função `HostGuardianService`, ele também tentará usar o ponto de extremidade `microsoft.windows.hgs`, que é configurado quando o HGS é instalado.

Se você quiser usar um ponto de extremidade personalizado, deverá especificar o nome da configuração da sessão ao construir o destino de rastreamento usando o parâmetro `-PSSessionConfigurationName`, como abaixo:

```PowerShell
New-HgsTraceTarget -HostName "hgs-01.secure.contoso.com" -Role HostGuardianService -Credential (Enter-Credential) -PSSessionConfigurationName "microsoft.windows.hgs"
```

#### <a name="diagnosing-multiple-hosts"></a>Diagnosticando vários hosts

Você pode passar vários destinos de rastreamento para `Get-HgsTrace` de uma vez.  Isso inclui uma combinação de destinos locais e remotos.  Cada destino será rastreado por vez e, em seguida, os rastreamentos de cada destino serão diagnosticados simultaneamente.  A ferramenta de diagnóstico pode usar o maior conhecimento de sua implantação para identificar incorretas configurações complexas entre nós que, de outra forma, não poderiam ser detectáveis.  O uso desse recurso requer apenas fornecer rastreamentos de vários hosts simultaneamente (no caso de diagnóstico manual) ou direcionar vários hosts ao chamar `Get-HgsTrace` (no caso de diagnóstico remoto).

Veja um exemplo de como usar o diagnóstico remoto para fazer uma triagem de uma malha composta por dois nós HGS e dois hosts protegidos, em que um dos hosts protegidos está sendo usado para iniciar o `Get-HgsTrace`.

```PowerShell
$hgs01 = New-HgsTraceTarget -HostName "hgs-01.secure.contoso.com" -Credential (Enter-Credential)
$hgs02 = New-HgsTraceTarget -HostName "hgs-02.secure.contoso.com" -Credential (Enter-Credential)
$gh01 = New-HgsTraceTarget -Local
$gh02 = New-HgsTraceTarget -HostName "guardedhost-02.contoso.com"
Get-HgsTrace -Target $hgs01,$hgs02,$gh01,$gh02 -RunDiagnostics
```

> [!NOTE]
> Você não precisa diagnosticar toda a malha protegida ao diagnosticar vários nós.  Em muitos casos, é suficiente incluir todos os nós que possam estar envolvidos em uma determinada condição de falha.  Isso geralmente é um subconjunto dos hosts protegidos e alguns nós do cluster HGS.

## <a name="manual-diagnosis-using-saved-traces"></a>Diagnóstico manual usando rastreamentos salvos

Às vezes, talvez você queira executar novamente o diagnóstico sem coletar rastreamentos novamente ou pode não ter as credenciais necessárias para diagnosticar remotamente todos os hosts em sua malha simultaneamente.  O diagnóstico manual é um mecanismo pelo qual você ainda pode executar uma triagem de malha inteira usando `Get-HgsTrace`, mas sem usar a coleta de rastreamento remoto.

Antes de executar o diagnóstico manual, você precisará garantir que os administradores de cada host na malha que serão desatualizadas estejam prontos e pretendam executar comandos em seu nome.  A saída do rastreamento de diagnóstico não expõe nenhuma informação que geralmente é exibida como confidencial, no entanto, ela é responsável pelo usuário de determinar se é seguro expor essas informações para outras pessoas.

> [!NOTE]
> Os rastreamentos não são anônimos e revelam a configuração de rede, as configurações de PKI e outras configurações que, às vezes, são consideradas informações privadas.  Portanto, os rastreamentos só devem ser transmitidos a entidades confiáveis dentro de uma organização e nunca postados publicamente.

As etapas para executar um diagnóstico manual são as seguintes:

1. Solicite que cada administrador de host execute `Get-HgsTrace` especificando um `-Path` conhecido e a lista de diagnósticos que você pretende executar nos rastreamentos resultantes.  Por exemplo:

   ```PowerShell
   Get-HgsTrace -Path C:\Traces -Diagnostic Networking,BestPractices
   ```

2. Solicite que cada administrador do host Empacote a pasta de rastreamentos resultante e envie-a para você.  Esse processo pode ser controlado por email, por meio de compartilhamentos de arquivos ou qualquer outro mecanismo baseado nas políticas operacionais e nos procedimentos estabelecidos pela sua organização.

3. Mesclar todos os rastreamentos recebidos em uma única pasta, sem nenhum outro conteúdo ou pasta.

    * Por exemplo, suponha que você tenha seus administradores que enviem rastreamentos coletados de quatro computadores chamados HGS-01, HGS-02, RR1N2608-12 e RR1N2608-13.  Cada administrador lhe enviou uma pasta com o mesmo nome.  Você montaria uma estrutura de diretório que aparece da seguinte maneira:

      ```
      FabricTraces
      |- HGS-01
      |  |- TargetMetadata.xml
      |  |- Metadata.xml
      |  |- [any other trace files for this host]
      |- HGS-02
      |  |- [...]
      |- RR1N2608-12
      |  |- [...]
      |- RR1N2608-13
         |- [..]
      ```

4. Execute o diagnóstico, fornecendo o caminho para a pasta de rastreamento montada no parâmetro `-Path` e especificando a opção `-RunDiagnostics`, bem como o diagnóstico para o qual você pediu aos administradores para coletar rastreamentos.  O diagnóstico pressupõe que ele não pode acessar os hosts encontrados dentro do caminho e, portanto, tentará usar apenas os rastreamentos previamente coletados.  Se algum rastreamento estiver ausente ou danificado, o diagnóstico falhará apenas nos testes afetados e continuará normalmente.  Por exemplo:

   ```PowerShell
   Get-HgsTrace -RunDiagnostics -Diagnostic Networking,BestPractices -Path ".\FabricTraces"
   ```

### <a name="mixing-saved-traces-with-additional-targets"></a>Misturar rastreamentos salvos com destinos adicionais

Em alguns casos, você pode ter um conjunto de rastreamentos previamente coletados que você deseja aumentar com rastreamentos de host adicionais.  É possível misturar rastreamentos previamente coletados com destinos adicionais que serão rastreados e diagnosticados em uma única chamada de diagnóstico.

Seguindo as instruções para coletar e montar uma pasta de rastreamento especificada acima, chame `Get-HgsTrace` com destinos de rastreamento adicionais não encontrados na pasta de rastreamento previamente coletada:

```PowerShell
$hgs03 = New-HgsTraceTarget -HostName "hgs-03.secure.contoso.com" -Credential (Enter-Credential)
Get-HgsTrace -RunDiagnostics -Target $hgs03 -Path .\FabricTraces
``` 

O cmdlet de diagnóstico identificará todos os hosts previamente coletados e um host adicional que ainda precisa ser rastreado e executará o rastreamento necessário.  A soma de todos os rastreamentos previamente coletados e coletados recentemente será diagnosticada.  A pasta de rastreamento resultante conterá os rastreamentos novo e antigo.

## <a name="known-issues"></a>Problemas conhecidos

O módulo de diagnóstico de malha protegida tem limitações conhecidas quando executado no Windows Server 2019 ou no Windows 10, versão 1809 e versões de sistema operacional mais recentes.
O uso dos seguintes recursos pode causar resultados errados:

* Atestado de chave de host
* Configuração de HGS somente para atestado (para cenários de Always Encrypted SQL Server)
* Uso de artefatos de política v1 em um servidor HGS onde o padrão de política de atestado é v2

Uma falha no `Get-HgsTrace` ao usar esses recursos não indica necessariamente que o servidor HGS ou o host protegido está configurado incorretamente.
Use outras ferramentas de diagnóstico como `Get-HgsClientConfiguration` em um host protegido para testar se um host passou por atestado.
