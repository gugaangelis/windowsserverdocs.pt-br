---
title: Solução de problemas usando a ferramenta de diagnóstico de malha protegida
ms.custom: na
ms.prod: windows-server-threshold
ms.topic: article
ms.assetid: 07691d5b-046c-45ea-8570-a0a85c3f2d22
manager: dongill
author: huu
ms.technology: security-guarded-fabric
ms.openlocfilehash: c102fa0503e6aac279235e1243b55e0e3cf81e1d
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59812407"
---
# <a name="troubleshooting-using-the-guarded-fabric-diagnostic-tool"></a>Solução de problemas usando a ferramenta de diagnóstico de malha protegida

>Aplica-se a: Windows Server 2019, Windows Server (canal semestral), Windows Server 2016

Este tópico descreve o uso da ferramenta Diagnóstico de malha protegida para identificar e corrigir falhas comuns na implantação, configuração e operação em andamento da infraestrutura de malha protegida. Isso inclui o serviço de guardião de Host (HGS), todos os hosts protegidos e os serviços de suporte, como DNS e Active Directory. A ferramenta de diagnóstico pode ser usada para executar um primeiro passo na separação de uma malha protegida de falha, fornecendo aos administradores com um ponto de partida para resolver a interrupções e identificando ativos configurados incorretamente. A ferramenta não é uma substituição para uma melhor compreensão de som de operar uma malha protegida e só serve para verificar rapidamente os problemas mais comuns encontrados durante as operações diárias.

Documentação dos cmdlets usados neste tópico pode ser encontrada no [TechNet](https://technet.microsoft.com/library/mt718834.aspx).

[!INCLUDE [Guarded fabric diagnostics tool](../../../includes/guarded-fabric-diagnostics-tool.md)] 

# <a name="quick-start"></a>Início Rápido

Você pode diagnosticar um host protegido ou um nó HGS a seguinte chamada de uma sessão do Windows PowerShell com privilégios de administrador local:
```PowerShell
Get-HgsTrace -RunDiagnostics -Detailed
```
Isso detectará automaticamente a função do host atual e diagnosticar problemas relevantes que podem ser detectados automaticamente.  Todos os resultados gerados durante esse processo são exibidos devido à presença do `-Detailed` alternar.

O restante deste tópico fornecem uma explicação detalhada sobre o uso avançado do `Get-HgsTrace` para fazer coisas como diagnosticar diversos hosts ao mesmo tempo e detectar erros de configuração de nó cruzado complexos.

## <a name="diagnostics-overview"></a>Visão geral do diagnóstico
Malha protegida de diagnóstico está disponível em qualquer host de máquina virtual blindada relacionados a ferramentas e recursos instalados, incluindo hosts que executam o Server Core.  No momento, o diagnóstico é incluído com os seguintes recursos/pacotes:

1. Função de serviço guardião de host
2. Suporte do Hyper-V ao Guardião de Host
3. Ferramentas de Blindagem de VM para Gerenciamento de Malha
4. Ferramentas de Administração de Servidor Remoto (RSAT)

Isso significa que esteja disponíveis em todos os hosts protegidos, nós HGS, determinados servidores de gerenciamento de malha e as estações de trabalho do Windows 10 com ferramentas de diagnóstico [RSAT](https://www.microsoft.com/download/details.aspx?id=45520) instalado.  Diagnóstico pode ser chamado de qualquer uma das máquinas acima com a intenção de diagnosticar qualquer host protegido ou o nó HGS em uma malha protegida; usar destinos de rastreamento remoto, o diagnóstico pode localizar e conectar-se aos hosts que não seja o computador que executa o diagnóstico.

Cada host direcionado pelo diagnóstico é conhecido como "target de rastreamento".  Destinos de rastreamento são identificados por seus nomes de host e funções.  As funções descrevem a função que executa a um destino de rastreamento fornecida em uma malha protegida.  No momento, o rastreamento tem como alvo o suporte `HostGuardianService` e `GuardedHost` funções.  Observe que é possível que um host ocupar várias funções ao mesmo tempo, e isso também é suportado pelo diagnóstico, no entanto, isso não deve ser feito em ambientes de produção.  Os hosts Hyper-V e HGS devem ser mantidos separados e distintos em todos os momentos.

Os administradores podem começar as tarefas de diagnóstico, executando `Get-HgsTrace`.  Esse comando executa duas funções distintas com base nos switches fornecidos em tempo de execução: diagnóstico e coleta de rastreamento.  Esses dois combinados formam a totalidade da ferramenta de diagnóstico de malha protegida.  Embora não explicitamente necessárias, o diagnóstico mais úteis exige rastreamentos que só podem ser coletados com credenciais de administrador no destino do rastreamento.  Se os privilégios insuficientes são mantidos pelo usuário que está executando a coleta de rastreamento, os rastreamentos que exigem elevação falharão enquanto todos os outros serão passados.  Isso permite que o diagnóstico parcial no caso de um operador com privilégios insuficientes está realizando triagem. 

### <a name="trace-collection"></a>Coleta de rastreamento
Por padrão, `Get-HgsTrace` coletará apenas os rastreamentos e salvá-los em uma pasta temporária.  Rastreamentos de assumem a forma de uma pasta nomeada com o host de destino, preenchido com arquivos especialmente formatados que descrevem como o host está configurado.  Os rastreamentos também contêm metadados que descrevem como o diagnóstico foram chamado para coletar os rastreamentos.  Esses dados são usados pelo diagnóstico para reidratar informações sobre o host ao executar Diagnóstico manual.

Se necessário, os rastreamentos podem ser examinados manualmente.  Todos os formatos são legíveis (XML) ou podem ser inspecionados prontamente usando as ferramentas padrão (por exemplo, X509 certificados e as extensões de Shell do Windows Crypto).  Observe entretanto que rastreamentos não são projetados para Diagnóstico manual e é sempre mais eficiente processar os rastreamentos com os recursos de diagnóstico do `Get-HgsTrace`.

Os resultados da execução de coleta de rastreamento não façam qualquer indicação quanto a integridade de um determinado host.  Eles simplesmente indicam que os rastreamentos foram coletados com êxito.  É necessário usar os recursos de diagnóstico do `Get-HgsTrace` para determinar se os rastreamentos indicam um ambiente com falhas.

Usando o `-Diagnostic` parâmetro, você pode restringir a coleta de rastreamento para apenas esses rastreamentos necessário para operar o diagnóstico especificado.  Isso reduz a quantidade de dados coletados, bem como as permissões necessárias para invocar o diagnóstico.

### <a name="diagnosis"></a>Diagnóstico
Rastreamentos coletados podem ser diagnosticados ao fornecido `Get-HgsTrace` o local dos rastreamentos por meio de `-Path` parâmetro e especificando o `-RunDiagnostics` alternar.  Além disso, `Get-HgsTrace` pode executar o diagnóstico e coleção em uma única passagem, fornecendo o `-RunDiagnostics` switch e uma lista de destinos de rastreamento.  Se nenhum destino de rastreamento forem fornecido, o computador atual é usado como um destino de implícito, com sua função inferida inspecionando os módulos instalados do Windows PowerShell.

Diagnóstico fornecerá resultados em um formato hierárquico mostrando quais destinos de rastreamento, conjuntos de diagnóstico e diagnóstico individual é responsável por uma falha específica.  Falhas incluem atualizações e recomendações de resolução se uma decisão pode ser feita sobre que ação deve ser realizada em Avançar.  Por padrão, passando e irrelevantes resultados estão ocultos.  Para ver tudo testado o diagnóstico, especifique o `-Detailed` alternar.  Isso fará com que todos os resultados sejam exibidos independentemente de seu status.

É possível restringir o conjunto de diagnósticos que são executados usando o `-Diagnostic` parâmetro.  Isso permite que você especifique quais classes de diagnóstico devem ser executados em relação aos destinos de rastreamento e suprimindo todas as outras pessoas.  Exemplos de classes de diagnóstico disponíveis incluem rede, práticas recomendadas e cliente hardware.  Consulte a [documentação do cmdlet](https://technet.microsoft.com/library/mt718831.aspx) para encontrar uma lista atualizada de diagnóstico disponível.

> [!WARNING]
> Diagnóstico não é uma substituição para um monitoramento strong e o pipeline de resposta a incidentes.  Há um pacote do System Center Operations Manager para monitorar as malhas protegidas, bem como vários canais de log de eventos que podem ser monitorados para detectar problemas rapidamente.  O diagnóstico, em seguida, pode ser usado para essas falhas de triagem rápida e estabelecer um curso de ação.

## <a name="targeting-diagnostics"></a>Direcionamento de diagnóstico

`Get-HgsTrace` opera em relação aos destinos de rastreamento.  Um destino de rastreamento é um objeto que corresponde a um nó HGS ou um host protegido dentro de uma malha protegida.  Ele pode ser pensado como uma extensão para um `PSSession` que inclui informações necessárias somente para diagnóstico, como a função do host na malha.  Destinos podem ser gerados implicitamente (por exemplo, local ou manual de diagnóstico) ou explicitamente com o `New-HgsTraceTarget` comando.

### <a name="local-diagnosis"></a>Diagnóstico local

Por padrão, `Get-HgsTrace` têm como destino o host local (ou seja, em que o cmdlet está sendo invocado).  Isso é conhecido como o destino local implícito.  O destino local implícito é usado somente quando nenhum destino é fornecido na `-Target` parâmetro e nenhum rastreamento já existente é encontrado no `-Path`.

O destino local implícito usa a inferência de tipos de função para determinar qual função o host atual desempenha na malha protegida.  Isso se baseia em módulos do PowerShell do Windows instalados que correspondem grosseiramente a quais recursos foram instalados no sistema.  A presença do `HgsServer` módulo fará com que o destino do rastreamento assumir o papel `HostGuardianService` e a presença da `HgsClient` módulo fará com que o destino do rastreamento colocar a função `GuardedHost`.  É possível que um determinado host ter ambos os módulos presentes nesse caso, ele será tratado como um `HostGuardianService` e um `GuardedHost`.

Portanto, a invocação de padrão de diagnóstico para coletar rastreamentos localmente:
```PowerShell
Get-HgsTrace
```
... é equivalente ao seguinte:
```PowerShell
New-HgsTraceTarget -Local | Get-HgsTrace
```
> [!TIP]
> `Get-HgsTrace` pode aceitar destinos por meio do pipeline ou diretamente por meio de `-Target` parâmetro.  Operacionalmente, há nenhuma diferença entre os dois.

### <a name="remote-diagnosis-using-trace-targets"></a>Destinos de rastreamento usando o diagnóstico remoto

É possível diagnosticar remotamente um host por meio da geração de destinos de rastreamento com informações de conexão remota.  Tudo o que é necessário é o nome do host e um conjunto de credenciais capazes de se conectar usando a comunicação remota do Windows PowerShell.
```PowerShell
$server = New-HgsTraceTarget -HostName "hgs-01.secure.contoso.com" -Role HostGuardianService -Credential (Enter-Credential)
Get-HgsTrace -RunDiagnostics -Target $server
```
Este exemplo irá gerar um prompt para coletar as credenciais de usuário remoto e, em seguida, o diagnóstico será executado usando o host remoto no `hgs-01.secure.contoso.com` para concluir a coleta de rastreamento.  Os rastreamentos resultantes são baixados o localhost e, em seguida, diagnosticados.  Os resultados do diagnóstico são apresentados igual ao realizar [diagnóstico local](#local-diagnosis).  Da mesma forma, não é necessário especificar uma função, como pode ser deduzido com base no Windows PowerShell módulos instalados no sistema remoto.

Diagnóstico remoto utiliza a comunicação remota do Windows PowerShell para todos os acessos ao host remoto.  Portanto, é um pré-requisito que o destino do rastreamento têm a comunicação remota do Windows PowerShell habilitada (consulte [PSRemoting habilitar](https://technet.microsoft.com/library/hh849694.aspx)) e se o localhost está corretamente configurado para iniciar conexões com o destino.

> [!NOTE]
> Na maioria dos casos, somente é necessário que o localhost fazer parte da mesma floresta do Active Directory e que um nome de host DNS válida seja usada.  Se seu ambiente utiliza um modelo mais complicado de Federação ou para usar endereços IP diretos para conectividade, você talvez precise executar uma configuração adicional, como configurar o WinRM [hosts confiáveis](https://technet.microsoft.com/library/ff700227.aspx).

Você pode verificar o destino de rastreamento é instanciado e configurado para aceitar conexões usando corretamente a `Test-HgsTraceTarget` cmdlet:
```PowerShell
$server = New-HgsTraceTarget -HostName "hgs-01.secure.contoso.com" -Role HostGuardianService -Credential (Enter-Credential)
$server | Test-HgsTraceTarget
```
Esse comando retornará `$True` somente se `Get-HgsTrace` seria capaz de estabelecer uma sessão de diagnóstico remota com o destino do rastreamento.  Em caso de falha, esse cmdlet retornará informações de status relevantes para solução de problemas de conexão de comunicação remota do Windows PowerShell.

#### <a name="implicit-credentials"></a>Credenciais implícitas

Ao executar o diagnóstico remoto de um usuário com privilégios suficientes para conectar-se remotamente para o destino do rastreamento, não é necessário fornecer credenciais para `New-HgsTraceTarget`.  O `Get-HgsTrace` cmdlet irá reutilizar automaticamente as credenciais do usuário que invocou o cmdlet, ao abrir uma conexão.

> [!WARNING]
> Algumas restrições se aplicam à reutilização de credenciais, especialmente ao executar o que é conhecido como um "segundo salto."  Isso ocorre quando a tentativa de reutilizar as credenciais de dentro de uma sessão remota para outro computador.  É necessário [configurar o CredSSP](https://technet.microsoft.com/library/hh849872.aspx) dar suporte a esse cenário, mas isso está fora do escopo de gerenciamento de malha protegida e solução de problemas.

#### <a name="using-windows-powershell-just-enough-administration-jea-and-diagnostics"></a>Usando o Windows PowerShell Just Enough (JEA) administração e diagnóstico

Diagnóstico remoto oferece suporte ao uso de pontos de extremidade JEA restrito o Windows PowerShell. Por padrão, os destinos de rastreamento remoto se conectarão usando o padrão `microsoft.powershell` ponto de extremidade.  Se o destino do rastreamento tem o `HostGuardianService` função, ele também tenta usar o `microsoft.windows.hgs` ponto de extremidade que é configurado quando HGS está instalado.

Se você quiser usar um ponto de extremidade personalizado, você deve especificar o nome da configuração de sessão ao construir o destino de rastreamento usando o `-PSSessionConfigurationName` parâmetro, como abaixo:

```PowerShell
New-HgsTraceTarget -HostName "hgs-01.secure.contoso.com" -Role HostGuardianService -Credential (Enter-Credential) -PSSessionConfigurationName "microsoft.windows.hgs"
```

#### <a name="diagnosing-multiple-hosts"></a>Diagnóstico de vários Hosts

Você pode passar vários destinos de rastreamento para `Get-HgsTrace` ao mesmo tempo.  Isso inclui uma combinação de destinos locais e remotos.  Cada destino será rastreado por sua vez e, em seguida, os rastreamentos de cada destino serão ser diagnosticados simultaneamente.  A ferramenta de diagnóstico pode usar o conhecimento de aumento da sua implantação para identificar complexas configurações incorretas de nó cruzado não seriam detectáveis.  Usando esse recurso requer apenas o fornecimento de rastreamentos de vários hosts simultaneamente (no caso de Diagnóstico manual) ou direcionamento de vários hosts ao chamar `Get-HgsTrace` (no caso de diagnóstico remoto).

Aqui está um exemplo de como usar o diagnóstico remoto para uma malha é composta de dois nós HGS de triagem e dois hosts protegidos, onde um dos hosts protegidos está sendo usado para iniciar `Get-HgsTrace`.

```PowerShell
$hgs01 = New-HgsTraceTarget -HostName "hgs-01.secure.contoso.com" -Credential (Enter-Credential)
$hgs02 = New-HgsTraceTarget -HostName "hgs-02.secure.contoso.com" -Credential (Enter-Credential)
$gh01 = New-HgsTraceTarget -Local
$gh02 = New-HgsTraceTarget -HostName "guardedhost-02.contoso.com"
Get-HgsTrace -Target $hgs01,$hgs02,$gh01,$gh02 -RunDiagnostics
```

> [!NOTE]
> Você não precisa diagnosticar seu toda a malha protegida durante o diagnóstico de vários nós.  Em muitos casos, é suficiente para incluir todos os nós que podem estar envolvidos em uma determinada condição de falha.  Isso geralmente é um subconjunto dos hosts protegidos e um número de nós do cluster HGS.

## <a name="manual-diagnosis-using-saved-traces"></a>Diagnóstico manual usando rastreamentos salvos

Às vezes, você talvez queira executar novamente o diagnóstico sem coletar rastreamentos novamente, ou talvez você não tenha as credenciais necessárias para diagnosticar todos os hosts em sua malha de simultaneamente remotamente.  Diagnóstico manual é um mecanismo pelo qual você ainda poderá realizar uma triagem de malha todo usando `Get-HgsTrace`, mas sem usar a coleta de rastreamento remoto.

Antes de executar o Diagnóstico manual, você precisará garantir que os administradores de cada host na malha do será priorizada estiver pronto e disposto a executar comandos em seu nome.  Saída de rastreamento de diagnóstico não expõe todas as informações que geralmente são visualizadas como confidencial, no entanto, cabe ao usuário determinar se é seguro para expor essas informações para outras pessoas.

> [!NOTE]
> Os rastreamentos não são anônimas e revelam a configuração de rede, configurações de PKI e outra configuração que, às vezes, é considerada informações particulares.  Portanto, os rastreamentos só devem ser transmitido para entidades confiáveis dentro de uma organização e nunca postadas publicamente.

Etapas para executar um diagnóstico manual são da seguinte maneira:

1. Solicitação que executam cada administrador de host `Get-HgsTrace` especificando um conhecido `-Path` e a lista de diagnóstico que você pretende executar em relação os rastreamentos resultantes.  Por exemplo: 

 ```PowerShell
 Get-HgsTrace -Path C:\Traces -Diagnostic Networking,BestPractices
 ```
2. Solicite que cada administrador de host a pasta resultante de rastreamentos de pacote e enviá-lo para você.  Esse processo pode ser acionado por email, por meio de compartilhamentos de arquivos ou qualquer outro mecanismo com base nas políticas de operação e procedimentos estabelecidos pela sua organização.

3. Mescle todos os rastreamentos recebidos em uma única pasta, com nenhum outro conteúdo ou pastas.

    * Por exemplo, suponha que você teve seu envio administradores você rastreia coletados de quatro máquinas chamadas HGS-01, 02 de HGS, RR1N2608-12 e 13 RR1N2608.  Cada administrador teria enviado é uma pasta com o mesmo nome.  Você deve montar uma estrutura de diretórios que aparece da seguinte maneira:

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

4. Execute diagnósticos, fornecendo o caminho para a pasta de rastreamento montado sobre o `-Path` parâmetro e especificando o `-RunDiagnostics` alternar, bem como esse para os quais você solicitado seus administradores para coletar rastreamentos de diagnóstico.  Diagnóstico assumirá que ele não é possível acessar os hosts encontrados dentro do caminho e, portanto, tentar usar apenas os rastreamentos previamente coletados.  Se os rastreamentos estiverem faltando ou danificado, diagnóstico falhará somente os testes afetados e prosseguir normalmente.  Por exemplo: 

 ```PowerShell
 Get-HgsTrace -RunDiagnostics -Diagnostic Networking,BestPractices -Path ".\FabricTraces"
 ```

### <a name="mixing-saved-traces-with-additional-targets"></a>Misturar salvo rastreamentos com destinos adicionais

Em alguns casos, você pode ter um conjunto de rastreamentos coletados previamente que você deseja aumentar com rastreamentos de host adicionais.  É possível misturar rastreamentos previamente coletados com destinos adicionais que serão rastreados e diagnosticados em uma única chamada de diagnóstico.

Seguindo as instruções para coletar e montar uma pasta de rastreamento especificada acima, chamar `Get-HgsTrace` com destinos de rastreamento adicionais não encontrados na pasta de rastreamento previamente coletados:

```PowerShell
$hgs03 = New-HgsTraceTarget -HostName "hgs-03.secure.contoso.com" -Credential (Enter-Credential)
Get-HgsTrace -RunDiagnostics -Target $hgs03 -Path .\FabricTraces
``` 

O cmdlet de diagnóstico identificará todos os hosts previamente coletados e um host adicional que ainda precisa ser rastreado e executará o rastreamento necessário.  A soma de todos os rastreamentos coletados recentemente e previamente coletados, em seguida, serão ser diagnosticada.  A pasta de rastreamento resultante conterá os rastreamentos novos e antigos.
