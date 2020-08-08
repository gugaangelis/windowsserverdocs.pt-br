---
ms.assetid: 75cc1d24-fa2f-45bd-8f3b-1bbd4a1aead0
title: Requisitos e práticas recomendadas para Atualização com Suporte a Cluster
ms.topic: article
manager: lizross
author: JasonGerend
ms.author: jgerend
ms.date: 08/06/2018
description: Requisitos para usar a atualização com suporte a cluster para instalar atualizações em clusters que executam o Windows Server.
ms.openlocfilehash: f306ee80873e7082eef9195494f5c509167b596a
ms.sourcegitcommit: 68444968565667f86ee0586ed4c43da4ab24aaed
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/07/2020
ms.locfileid: "87990819"
---
# <a name="cluster-aware-updating-requirements-and-best-practices"></a>Requisitos e práticas recomendadas para Atualização com Suporte a Cluster

> Aplica-se a: Windows Server 2019, Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Esta seção descreve os requisitos e as dependências necessários para usar a cau ( [atualização com suporte a cluster](cluster-aware-updating.md) ) para aplicar atualizações a um cluster de failover que executa o Windows Server.

> [!NOTE]
> Talvez seja necessário validar de forma independente se o seu ambiente de cluster está pronto para aplicar atualizações se você usar um plug- \- in diferente do **Microsoft. WindowsUpdatePlugin**. Se você estiver usando um \- plug não Microsoft \- , entre em contato com o editor para obter mais informações. Para obter mais informações sobre plug- \- ins, consulte [como os plug- \- ins funcionam](cluster-aware-updating-plug-ins.md).

## <a name="install-the-failover-clustering-feature-and-the-failover-clustering-tools"></a><a name="BKMK_REQ_CLUS"></a>Instale o recurso de Cluster de failover e as ferramentas de Cluster de failover
O CAU requer uma instalação do recurso de Cluster de failover e as Ferramentas de cluster de failover. As ferramentas de clustering de failover incluem as ferramentas de CAU \(clusterawareupdating.dll\) , os cmdlets de clustering de failover e outros componentes necessários para operações de Cau. Para saber as etapas de instalação do recurso de Cluster de failover, consulte [Installing the Failover Clustering Feature and Tools (Instalando o recurso e as ferramentas de cluster de failover)](create-failover-cluster.md#install-the-failover-clustering-feature).

Os requisitos de instalação exatos para as ferramentas de clustering de failover dependem se a CAU coordena atualizações como uma função clusterizada no cluster de failover \( usando o \- modo de autoatualização \) ou de um computador remoto. O \- modo de autoatualização da cau exige, além disso, a instalação da função clusterizada cau no cluster de failover usando as ferramentas de Cau.

A tabela a seguir resume os requisitos de instalação do recurso CAU para dois modos de atualização do CAU.

|Componente instalado|\-Modo de autoatualização|\-Modo de atualização remota|
|-----------------------|-----------------------|-------------------------|
|Recurso de cluster de failover|Necessário em todos os nós de cluster|Necessário em todos os nós de cluster|
|Ferramentas de cluster de failover|Necessário em todos os nós de cluster|-Necessário no \- computador de atualização remota<br />-Necessário em todos os nós de cluster para executar o cmdlet [Save-CauDebugTrace](/powershell/module/clusterawareupdating/Save-CauDebugTrace?view=win10-ps)|
|Função clusterizada do CAU|Necessária|Não é necessária|

## <a name="obtain-an-administrator-account"></a>Obter uma conta de administrador
Os requisitos de administrador a seguir são necessários para usar recursos de CAU.

-   Para visualizar ou aplicar as ações de atualização usando a interface do usuário do CAU \( \) ou os cmdlets de atualização com suporte a cluster, você deve usar uma conta de domínio que tenha direitos e permissões de administrador local em todos os nós de cluster. Se a conta não tiver privilégios suficientes em todos os nós, você será solicitado na janela de atualização com suporte a cluster para fornecer as credenciais necessárias ao executar essas ações. Para usar os cmdlets de [atualização com suporte a cluster](/powershell/module/clusterawareupdating/?view=win10-ps) , você pode fornecer as credenciais necessárias como um parâmetro de cmdlet.

-   Se você usar a CAU no \- modo de atualização remota quando estiver conectado com uma conta que não tem direitos de administrador local e permissões nos nós de cluster, execute as ferramentas de Cau como administrador usando uma conta de administrador local no computador do coordenador de atualização ou usando uma conta que tenha o direito de usuário **representar um cliente após a autenticação** .

-   Para executar o Analisador de Práticas Recomendadas CAU, você deve usar uma conta que tenha privilégios administrativos nos nós de cluster e privilégios administrativos locais no computador usado para executar o cmdlet [Test-CauSetup](/powershell/module/clusterawareupdating/Test-CauSetup?view=win10-ps) ou para analisar a preparação da atualização do cluster usando a janela de atualização com suporte a cluster. Para mais informações, consulte [Testar preparação de atualização do cluster](#BKMK_BPA).

## <a name="verify-the-cluster-configuration"></a>Verificar a configuração do cluster
A seguir estão os requisitos gerais de um cluster de failover para suporte de atualizações usando CAU. Os requisitos de configuração adicional para gerenciamento remoto nos nós estão listados em [Configurar os nós para gerenciamento remoto](#BKMK_NODE_CONFIG) mais adiante neste tópico.

-   Deve haver nós de cluster suficientes online para que o cluster tenha quorum.

-   Todos os nós de cluster devem estar no mesmo domínio do Active Directory.

-   O nome do cluster deve ser resolvido na rede usando DNS.

-   Se a CAU for usada no \- modo de atualização remota, o computador do coordenador de atualização deve ter conectividade de rede com os nós de cluster de failover e deve estar no mesmo domínio Active Directory que o cluster de failover.

-   O serviço de cluster deve estar executando em todos os nós de cluster. Por padrão, esse serviço é instalado em todos os nós de cluster e está configurado para iniciar automaticamente.

-   Para usar o PowerShell pre \- Update ou postar \- scripts de atualização durante uma execução de atualização da cau, verifique se os scripts estão instalados em todos os nós de cluster ou se eles estão acessíveis a todos os nós, por exemplo, em um compartilhamento de arquivos de rede altamente disponível. Se os scripts forem salvos em um compartilhamento de arquivos de rede, configure a pasta para permissão de Leitura para o grupo Todos.

## <a name="configure-the-nodes-for-remote-management"></a><a name="BKMK_NODE_CONFIG"></a>Configurar os nós para gerenciamento remoto
Para usar a atualização com suporte a cluster, todos os nós do cluster devem ser configurados para gerenciamento remoto. Por padrão, a única tarefa que você deve executar para configurar os nós para gerenciamento remoto é [habilitar uma regra de firewall para permitir reinicializações automáticas](#BKMK_FW).

A tabela a seguir lista os requisitos completos de gerenciamento remoto, caso seu ambiente seja disverge dos padrões.

Esses requisitos são adicionais aos requisitos de instalação para a [Instalação do recurso de Cluster de failover e Ferramentas de cluster de failover](#BKMK_REQ_CLUS) e os requisitos de cluster gerais descritos nas seções anteriores neste tópico.

|Requisito|Estado padrão|\-Modo de autoatualização|\-Modo de atualização remota|
|---------------|---|-----------------------|-------------------------|
|[Habilitar uma função de firewall para permitir reinícios automáticos](#BKMK_FW)|Desabilitado|Necessário em todos os nós de cluster se um firewall estiver em uso|Necessário em todos os nós de cluster se um firewall estiver em uso|
|[Habilitar a Instrumentação de Gerenciamento do Windows](#BKMK_WMI)|Habilitada|Necessário em todos os nós de cluster|Necessário em todos os nós de cluster|
|[Habilitar o Windows PowerShell 3.0 ou 4.0 e o Windows PowerShell remotamente](#BKMK_PS)|Habilitada|Necessário em todos os nós de cluster|Necessário em todos os nós de cluster para executar o seguinte:<p>-O cmdlet [Save-CauDebugTrace](/powershell/module/clusterawareupdating/Save-CauDebugTrace?view=win10-ps)<br />-Pré- \- atualização do PowerShell e pós- \- atualização de scripts durante uma execução de atualização<br />-Testes de preparação de atualização de cluster usando a janela de atualização com suporte a cluster ou o cmdlet [test \- CauSetup](/powershell/module/clusterawareupdating/Test-CauSetup?view=win10-ps) do Windows PowerShell|
|[Instalar o .NET Framework 4,6 ou 4,5](#BKMK_NET)|Habilitada|Necessário em todos os nós de cluster|Necessário em todos os nós de cluster para executar o seguinte:<p>-O cmdlet [Save-CauDebugTrace](/powershell/module/clusterawareupdating/Save-CauDebugTrace?view=win10-ps)<br />-Pré- \- atualização do PowerShell e pós- \- atualização de scripts durante uma execução de atualização<br />-Testes de preparação de atualização de cluster usando a janela de atualização com suporte a cluster ou o cmdlet [test \- CauSetup](/powershell/module/clusterawareupdating/Test-CauSetup?view=win10-ps) do Windows PowerShell|

### <a name="enable-a-firewall-rule-to-allow-automatic-restarts"></a><a name="BKMK_FW"></a>Habilitar uma função de firewall para permitir reinícios automáticos
Para permitir reinicializações automáticas depois que as atualizações forem aplicadas \( se a instalação de uma atualização exigir uma reinicialização \) , se o Firewall do Windows ou um \- Firewall não Microsoft estiver em uso nos nós do cluster, uma regra de firewall deverá ser habilitada em cada nó que permita o seguinte tráfego:

-   Protocolo: TCP

-   Direção: entrada

-   Programa: wininit.exe

-   Portas: Portas Dinâmicas RPC

-   Perfil: Domínio

Se o Firewall do Windows for usado nos nós de cluster, você pode fazer isso habilitando o grupo de regras do Firewall do Windows **Desligamento Remoto** em cada nó de cluster. Quando você usa a janela de atualização com suporte a cluster para aplicar atualizações e configurar \- Opções de atualização automática, o grupo de regras de firewall do Windows de **desligamento remoto** é habilitado automaticamente em cada nó de cluster.

> [!NOTE]
> O grupo de regras do Firewall do Windows **Remote Shutdown** não pode ser habilitado quando ele está em conflito com as definições de Política de Grupo configuradas para o Firewall do Windows.

O grupo de regras de firewall de **desligamento remoto** também é habilitado especificando o parâmetro **– EnableFirewallRules** ao executar os seguintes cmdlets de Cau: [Add-CauClusterRole](/powershell/module/clusterawareupdating/Add-CauClusterRole?view=win10-ps), [Invoke-CauRun](/powershell/module/clusterawareupdating/Invoke-CauRun?view=win10-ps)e [SetCauClusterRole](/powershell/module/clusterawareupdating/Set-CauClusterRole?view=win10-ps).

O exemplo do PowerShell a seguir mostra um método adicional para habilitar reinicializações automáticas em um nó de cluster.

```PowerShell
Set-NetFirewallRule -Group "@firewallapi.dll,-36751" -Profile Domain -Enabled true
```

### <a name="enable-windows-management-instrumentation-wmi"></a><a name="BKMK_WMI"></a>Habilitar Instrumentação de Gerenciamento do Windows (WMI)
Todos os nós de cluster devem ser configurados para gerenciamento remoto usando Instrumentação de Gerenciamento do Windows \( WMI \) . Isso é habilitado por padrão.

Para habilitar manualmente o gerenciamento remoto, execute o seguinte:

1.  No console de Serviços, inicie o serviço **Gerenciamento remoto do Windows** e defina o tipo de inicialização **Automática**.

2.  Execute o cmdlet [Set-WSManQuickConfig](/powershell/module/Microsoft.WsMan.Management/Set-WSManQuickConfig?view=powershell-6) , ou execute o seguinte comando em um prompt de comandos com privilégios elevados:

    ```PowerShell
    winrm quickconfig -q
    ```

Para dar suporte à comunicação remota do WMI, se o Firewall do Windows estiver em uso nos nós do cluster, a regra de firewall de entrada para o **gerenciamento remoto do Windows \( http \- no \) ** deve ser habilitada em cada nó.  Por padrão, essa regra está habilitada.

### <a name="enable-windows-powershell-and-windows-powershell-remoting"></a><a name="BKMK_PS"></a>Habilitar o Windows PowerShell e o Windows PowerShell remotamente
Para habilitar o \- modo de autoatualização e determinados recursos de cau no \- modo de atualização remota, o PowerShell deve ser instalado e habilitado para executar comandos remotos em todos os nós de cluster. Por padrão, o PowerShell é instalado e habilitado para comunicação remota.

Para habilitar a comunicação remota do PowerShell, use um dos seguintes métodos:

-   Execute o cmdlet [Enable-PSRemoting](/powershell/module/Microsoft.PowerShell.Core/Enable-PSRemoting) .

-   Defina uma \- configuração de política de grupo de nível de domínio para gerenciamento remoto do Windows \( WinRM \) .

Para obter mais informações sobre como habilitar a comunicação remota do PowerShell, consulte [sobre requisitos remotos](/powershell/module/microsoft.powershell.core/about/about_remote_requirements?view=powershell-6).

### <a name="install-net-framework-46-or-45"></a><a name="BKMK_NET"></a>Instalar o .NET Framework 4,6 ou 4,5
Para habilitar o \- modo de autoatualização e determinados recursos de cau no \- modo de atualização remota, o. NET Framework 4,6 ou o .NET Framework 4,5 (no Windows Server 2012 R2) deve ser instalado em todos os nós de cluster. Por padrão, o .NET Framework é instalado.

Para instalar .NET Framework 4,6 (ou 4,5) usando o PowerShell, se ele ainda não estiver instalado, use o seguinte comando:

```PowerShell
Install-WindowsFeature -Name NET-Framework-45-Core
```

## <a name="best-practices-recommendations-for-using-cluster-aware-updating"></a><a name="BKMK_BEST_PRAC"></a>Recomendações de práticas recomendadas para usar a atualização com suporte a cluster

### <a name="recommendations-for-applying-microsoft-updates"></a><a name="BKMK_BP_WUA"></a>Recomendações para aplicar atualizações da Microsoft

Recomendamos que, quando você começar a usar a CAU para aplicar atualizações com o plug-in **Microsoft. WindowsUpdatePlugin** padrão em \- um cluster, você pare de usar outros métodos para instalar atualizações de software da Microsoft nos nós de cluster.

> [!CAUTION]
> Combinar a CAU com métodos que atualizam nós individuais automaticamente \( em um agendamento de tempo fixo \) pode causar resultados imprevisíveis, incluindo interrupções no serviço e tempo de inatividade não planejado.

Convém seguir estas diretrizes:

-   Para melhores resultados, recomendamos desabilitar as configurações nos nós do cluster para atualização automática, por exemplo, por meio das configurações de Atualizações automáticas no Painel de controle, ou nas configurações definidas usando a Política de Grupo.

    > [!CAUTION]
    > A instalação automática de atualizações nos nós de cluster pode interferir na instalação de atualizações pelo CAU e causar falhas no CAU.

    Se elas forem necessárias, as seguintes Atualizações automáticas são compatíveis com CAU, porque o administrador pode controlar o sincronismo da instalação da atualização:

    -   Configurações para notificar antes do download das atualizações e para notificar antes da instalação

    -   Configurações para baixar as atualizações automaticamente e notificar antes da instalação

    No entanto, se as Atualizações Automáticas estiverem baixando atualizações ao mesmo tempo em que é executada uma atualização do CAU, a execução da atualização poderá levar mais tempo para ser concluída.

-   Não configure um sistema de atualização, como Windows Server Update Services o \( WSUS \) para aplicar atualizações automaticamente \( em um agendamento de tempo fixo \) para nós de cluster.

-   Todos os nós de cluster devem ser configurados uniformemente para usar a mesma fonte de atualização, por exemplo, um servidor WSUS, Windows Update ou Microsoft Update.

-   Se você usar um sistema de gerenciamento de configurações para aplicar atualizações de software a computadores na rede, exclua os nós do cluster de todas as atualizações necessárias ou automáticas. Exemplos de sistemas de gerenciamento de configuração incluem o Microsoft Endpoint Configuration Manager e o Microsoft System Center Virtual Machine Manager 2008.

-   Se os servidores de distribuição \( de software internos por exemplo, os servidores do WSUS \) forem usados para conter e implantar as atualizações, certifique-se de que esses servidores identifiquem corretamente as atualizações aprovadas para os nós de cluster.

#### <a name="apply-microsoft-updates-in-branch-office-scenarios"></a><a name="BKMK_PROXY"></a>Aplicar as atualizações da Microsoft em cenários de filiais
Para baixar atualizações da Microsoft do Microsoft Update ou Windows Update para nós de cluster em determinados cenários de filiais, pode ser necessário configurar definições de proxy para a conta de Sistema Local em cada nó. Por exemplo, isso pode ser necessário se os clusters da filial acessarem o Microsoft Update ou o Windows Update para baixar atualizações usando um servidor de proxy local.

Se necessário, defina as configurações de proxy do WinHTTP em cada nó para especificar um servidor proxy local e configurar as exceções de endereço local \( que são, uma lista de bypass para endereços locais \) . Para isso, você pode executar o seguinte comando em cada nó de cluster em um prompt de comandos com privilégios elevados:

```
netsh winhttp set proxy <ProxyServerFQDN >:<port> "<local>"
```

em que <*ProxyServerFQDN*> é o nome de domínio totalmente qualificado para o servidor proxy e a *porta* <> é a porta pela qual se comunicar (geralmente a porta 443).

Por exemplo, para definir as configurações de proxy do WinHTTP para a conta do sistema local especificando o servidor proxy *MyProxy.contoso.com*, com a porta 443 e as exceções de endereço local, digite o seguinte comando:

```
netsh winhttp set proxy MyProxy.CONTOSO.com:443 "<local>"
```

### <a name="recommendations-for-using-the-microsofthotfixplugin"></a><a name="BKMK_BP_HF"></a>Recomendações para usar o Microsoft.HotfixPlugin

-   Recomendamos configurar permissões na pasta raiz de hotfix e o arquivo de configuração de hotfix para restringir o acesso de Gravação aos administradores locais somente nos computadores que são usados para armazenar esses arquivos. Isso ajuda a evitar possíveis violações desses arquivos por usuários não autorizados que poderiam comprometer a funcionalidade do cluster de failover quando os hotfixes forem aplicados.

-   Para ajudar a garantir a integridade dos dados para as conexões SMB do bloco de mensagens do servidor \( \) que são usadas para acessar a pasta raiz do hotfix, você deve configurar a criptografia SMB na pasta compartilhada SMB, se for possível configurá-la. O **Microsoft.HotfixPlugin** requer que a assinatura SMB ou Criptografia SMB seja configurada para ajudar a garantir a integridade dos dados para as conexões SMB.

    Para obter mais informações, consulte [restringir o acesso à pasta raiz do hotfix e ao arquivo de configuração do hotfix](cluster-aware-updating-plug-ins.md#BKMK_ACL).

### <a name="additional-recommendations"></a>Recomendações adicionais

-   Para evitar interferir em uma Execução de atualização CAU que pode estar agendada ao mesmo tempo, não agende alterações de senha para objetos de nome de cluster durante as janelas de manutenção agendadas.

-   Você deve definir as permissões apropriadas nos \- scripts pré-atualização e post \- atualizados que são salvos em pastas compartilhadas de rede para evitar a violação potencial desses arquivos por usuários não autorizados.

-   Para configurar a CAU no \- modo de autoatualização, um objeto de computador virtual \( VCO \) para a função clusterizada Cau deve ser criado no Active Directory. O CAU pode criar esse objeto automaticamente no momento em que a função clusterizada do CAU for adicionada e o cluster de failover tiver permissões suficientes. Entretanto, por causa das políticas de segurança em certas organizações, pode ser necessário pré-configurar o objeto no Active Directory. Para obter um procedimento para isso, consulte [Etapas para pré-preparar a conta para uma função em cluster](/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/cc731002\(v=ws.10\)#steps-for-prestaging-the-cluster-name-account).

-   Para salvar e reutilizar as configurações de Executar atualização nos clusters de failover com necessidades de atualização similar na organização de TI, você pode criar Perfis de Execução de Atualização. Além disso, dependendo do modo de atualização, você pode salvar e gerenciar os Perfis de Execução de Atualização em um compartilhamento de arquivo que está acessível a todos os computadores do Coordenador de Atualização remoto ou clusters de failover. Para obter mais informações, consulte [Opções avançadas e atualizando perfis de execução para a cau](cluster-aware-updating-options.md).

## <a name="test-cluster-updating-readiness"></a><a name="BKMK_BPA"></a>Teste de preparação de atualização do cluster
Você pode executar o modelo de BPA Analisador de Práticas Recomendadas do CAU \( \) para testar se um cluster de failover e o ambiente de rede atendem a muitos dos requisitos para que as atualizações de software sejam aplicadas pela Cau. Muitos dos testes verificam o ambiente quanto à preparação para aplicar as atualizações da Microsoft usando o plug- \- in padrão, **Microsoft. WindowsUpdatePlugin**.

> [!NOTE]
> Talvez seja necessário validar de forma independente se o ambiente de cluster está pronto para aplicar as atualizações de software usando um plug- \- in diferente do **Microsoft. WindowsUpdatePlugin**. Se você estiver usando um \- plug não Microsoft \- , como um fornecido pelo fabricante do hardware, entre em contato com o editor para obter mais informações.

É possível executar o BPA nas duas seguintes maneiras:

1.  Selecione **Analisar preparação da atualização do cluster** no console de CAU. Depois que o BPA concluir os testes de preparação, um relatório de teste será exibido. Se forem detectados problemas nos nós de cluster, os problemas específicos e os nós nos quais os problemas aparecem são identificados, para que você possa executar a ação corretiva. Os testes podem levar vários minutos para serem concluídos.

2.  Execute o cmdlet [Test-CauSetup](/powershell/module/clusterawareupdating/Test-CauSetup) . Você pode executar o cmdlet em um computador local ou remoto no qual o módulo clustering de failover do Windows PowerShell (parte das ferramentas de clustering de failover) está instalado. Também é possível executar o cmdlet em um nó do cluster de failover.

> [!NOTE]
> -   Você deve usar uma conta que tenha privilégios administrativos nos nós de cluster e privilégios administrativos locais no computador que é usado para executar o cmdlet **test \- CauSetup** ou para analisar a preparação da atualização de cluster usando a janela de atualização com suporte a cluster. Para executar os testes usando a janela de atualização com suporte a cluster, você deve estar conectado ao computador com as credenciais necessárias.
> -   Os testes assumem que as ferramentas do CAU usadas para visualizar e aplicar as atualizações de software são executadas a partir do mesmo computador e com as mesmas credenciais de usuário que as usadas para testar a preparação de atualização do cluster.

> [!IMPORTANT]
> É altamente recomendável que você teste o cluster quanto à preparação para atualização nas seguintes situações:
>
> -   Antes de usar o CAU pela primeira vez para aplicar atualizações de software.
> -   Após adicionar um nó ao cluster ou executar outras alterações de hardware no cluster, que requerem a execução do Assistente de Validação de Cluster.
> -   Depois de alterar uma origem de atualização ou alterar configurações de atualização ou configurações \( diferentes da cau \) que podem afetar a aplicação de atualizações nos nós.

### <a name="tests-for-cluster-updating-readiness"></a>Testes para preparação de atualização do cluster
A tabela a seguir lista os testes de preparação de atualização do cluster, alguns problemas comuns e etapas de resolução.


|                                                      Teste                                                      |                                                                                                                                               Possíveis problemas e impactos                                                                                                                                               |                                                                                                                                                                                         Etapas de resolução                                                                                                                                                                                         |
|----------------------------------------------------------------------------------------------------------------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
|                                     O cluster de failover deve estar disponível                                     |                                                                                       Não é possível resolver o nome do cluster de failover, ou um ou mais nós de cluster não podem ser acessados. O BPA não pode executar testes de preparação do cluster.                                                                                        |                                                                   -Verifique a ortografia do nome do cluster especificado durante a execução do BPA.<br />-Verifique se todos os nós do cluster estão online e em execução.<br />-Verifique se o assistente para validar uma configuração pode ser executado com êxito no cluster de failover.                                                                    |
|                    Os nós de cluster de failover devem estar habilitados para gerenciamento remoto via WMI                    |                                                Um ou mais nós de cluster de failover não estão habilitados para gerenciamento remoto usando Instrumentação de Gerenciamento do Windows \( WMI \) . O CAU não pode atualizar os nós de cluster, se os nós não estiverem configurados para o gerenciamento remoto.                                                 |                                                                                                  Certifique-se de que todos os nós de cluster de failover estejam habilitados para o gerenciamento remoto por meio de WMI. Para mais informações, consulte [Configurar os nós para o gerenciamento remoto](#BKMK_NODE_CONFIG) neste tópico.                                                                                                   |
|                      A comunicação remota do PowerShell deve ser habilitada em cada nó de cluster de failover                       |                                                           O PowerShell não está instalado ou não está habilitado para comunicação remota em um ou mais nós de cluster de failover. A CAU não pode ser configurada para o \- modo de atualização automática ou usar determinados recursos no \- modo de atualização remota.                                                            |                                                                                             Verifique se o PowerShell está instalado em todos os nós de cluster e se está habilitado para comunicação remota.<p>Para mais informações, consulte [Configurar os nós para o gerenciamento remoto](#BKMK_NODE_CONFIG) neste tópico.                                                                                             |
|                                            Versão de cluster de failover                                            |                                                                            Um ou mais nós no cluster de failover não executam o Windows Server 2016, o Windows Server 2012 R2 ou o Windows Server 2012. O CAU não pode atualizar o cluster de failover.                                                                             |                                                                   Verifique se o cluster de failover especificado durante a execução do BPA está executando o Windows Server 2016, o Windows Server 2012 R2 ou o Windows Server 2012.<p>Para mais informações, consulte [Verificar a configuração do cluster](#BKMK_REQ_CLUS) nesse tópico.                                                                   |
| As versões necessárias do .NET Framework e do Windows PowerShell devem ser instaladas em todos os nós de cluster de failover |                                                                                              .NET Framework 4,6, 4,5 ou Windows PowerShell não está instalado em um ou mais nós de cluster. Alguns recursos de CAU podem não funcionar.                                                                                              |                                                                            Verifique se o .NET Framework 4,6 ou 4,5 e o Windows PowerShell estão instalados em todos os nós de cluster, se forem necessários.<p>Para mais informações, consulte [Configurar os nós para o gerenciamento remoto](#BKMK_NODE_CONFIG) neste tópico.                                                                             |
|                           O serviço de cluster deve estar em execução em todos os nós de cluster                           |                                                                                                            O serviço de cluster não está sendo executado em um ou mais nós. O CAU não pode atualizar o cluster de failover.                                                                                                             |                        -Verifique se o Serviço de cluster \( ClusSvc \) é iniciado em todos os nós no cluster e se ele está configurado para iniciar automaticamente.<br />-Verifique se o assistente para validar uma configuração pode ser executado com êxito no cluster de failover.<p>Para mais informações, consulte [Verificar a configuração do cluster](#BKMK_REQ_CLUS) nesse tópico.                         |
|     As atualizações automáticas não devem estar configuradas para instalar automaticamente as atualizações em nenhum nó de cluster de failover     |                                           As atualizações automáticas devem estar configuradas em, no mínimo, um nó de cluster de failover para instalar automaticamente as atualizações da Microsoft nesse nó. Combinar o CAU com outros métodos de atualização pode resultar em tempo de inatividade não planejado ou resultados imprevisíveis.                                            |                                                     Se a funcionalidade do Windows Update estiver configurada para atualizações automáticas em um ou mais nós de cluster, certifique-se de que as atualizações automáticas não estejam configuradas para instalar as atualizações automaticamente.<p>Para mais informações, consulte [Recomendações para aplicação de atualizações da Microsoft](#BKMK_BP_WUA).                                                     |
|                          Os nós de cluster de failover devem usar a mesma origem de atualização                          |                                                    Um ou mais nós de cluster de failover estão configurados para usar uma origem de atualização para as atualizações da Microsoft que é diferente do restante dos nós. As atualizações podem não ser aplicadas de maneira uniforme nos nós de cluster pelo CAU.                                                    |                                                                        Certifique-se de que todos os nós de cluster estejam configurados para usar a mesma origem de atualização, por exemplo, um servidor WSUS, Windows Update ou Microsoft Update.<p>Para mais informações, consulte [Recomendações para aplicação de atualizações da Microsoft](#BKMK_BP_WUA).                                                                         |
|       Uma regra de firewall que permite o encerramento remoto deve ser habilitada em todos os nós no cluster de failover       |                 Um ou mais nós de cluster de failover não possui uma regra de firewall habilitada que permita o encerramento remoto ou uma configuração de Política de Grupo que previna que essa regra seja habilitada. Uma Execução de Atualização que aplica atualizações que requerem a reinicialização automática dos nós pode não ser concluída de maneira adequada.                  |                                                                    Se o Firewall do Windows ou um \- Firewall não Microsoft estiver em uso nos nós do cluster, configure uma regra de firewall que permita o desligamento remoto.<p>Para mais informações, consulte [Ativar uma regra de firewall para permitir reinicializações automáticas](#BKMK_FW) neste tópico.                                                                    |
|          A configuração de servidor de proxy em cada nó de cluster de failover deve ser definida em um servidor proxy local          |                             Um ou mais nós de cluster de failover têm uma configuração incorreta de servidor proxy.<p>Se um servidor proxy local estiver em uso, a configuração de servidor proxy em cada nó deve ser configurada corretamente para o cluster acessar o Microsoft Update ou o Windows Update.                              |                                            Certifique-se de que as configurações de proxy do WinHTTP em cada nó de cluster estejam definidas em um servidor proxy local, caso necessário. Se um servidor proxy não estiver em uso em seu ambiente, esse aviso pode ser ignorado.<p>Para mais informações, consulte [Aplicar atualizações em cenários de filiais](#BKMK_PROXY) neste tópico.                                            |
|        A função clusterizada CAU deve ser instalada no cluster de failover para habilitar o \- modo de autoatualização        |                                                                                                   A função clusterizada de CAU não está instalada nesse cluster de failover. Essa função é necessária para a atualização automática do cluster \- .                                                                                                   |      Para usar a CAU no \- modo de autoatualização, adicione a função clusterizada cau no cluster de failover de uma das seguintes maneiras:<p>-Execute o cmdlet do PowerShell [Add-CauClusterRole](/powershell/module/clusterawareupdating/Add-CauClusterRole) .<br />-Selecione a ação **Configurar \- Opções de atualização automática de cluster** na janela de atualização com suporte a cluster.      |
|         A função clusterizada CAU deve ser habilitada no cluster de failover para habilitar o \- modo de autoatualização         | A função clusterizada de CAU é desabilitada. Por exemplo, a função clusterizada CAU não está instalada ou foi desabilitada usando o cmdlet [Disable \- CauClusterRole](/powershell/module/clusterawareupdating/Disable-CauClusterRole) do PowerShell. Essa função é necessária para a atualização automática do cluster \- . | Para usar a CAU no \- modo de autoatualização, habilite a função clusterizada Cau nesse cluster de failover de uma das seguintes maneiras:<p>-Execute o cmdlet [Enable-CauClusterRole](/powershell/module/clusterawareupdating/Enable-CauClusterRole) do PowerShell.<br />-Selecione a ação **Configurar \- Opções de atualização automática de cluster** na janela de atualização com suporte a cluster. |
|      O plug-in CAU configurado \- para o \- modo de autoatualização deve ser registrado em todos os nós de cluster de failover      |                                                              A função clusterizada CAU em um ou mais nós deste cluster de failover não pode acessar o módulo de plug- \- in Cau configurado nas \- Opções de atualização automática. Uma \- execução de autoatualização pode falhar.                                                              |           -Verifique se o plug-in CAU configurado \- está instalado em todos os nós de cluster seguindo o procedimento de instalação para o produto que fornece o plug- \- in Cau.<br />-Execute o cmdlet [Register \- CauPlugin](/powershell/module/clusterawareupdating/Register-CauPlugin) do PowerShell para registrar o plug- \- in nos nós de cluster necessários.           |
|                Todos os nós de cluster de failover devem ter o mesmo conjunto de plug-ins de CAU registrados \-                 |                                                                             Uma \- execução de autoatualização poderá falhar se o plug- \- in configurado para ser usado em uma execução de atualização for alterado para um que não esteja disponível em todos os nós de cluster.                                                                              |           -Verifique se o plug-in CAU configurado \- está instalado em todos os nós de cluster seguindo o procedimento de instalação para o produto que fornece o plug- \- in Cau.<br />-Execute o cmdlet [Register \- CauPlugin](/powershell/module/clusterawareupdating/Register-CauPlugin) do PowerShell para registrar o plug- \- in nos nós de cluster necessários.           |
|                               As opções configuradas de Execução de Atualização devem ser válidas                                |                                                                          As opções de agendamento de atualização automática \- e de execução de atualização configuradas para este cluster de failover estão incompletas ou não são válidas. Uma \- execução de autoatualização pode falhar.                                                                           |                                                            Configure uma agenda válida \- de autoatualização e conjunto de opções de execução de atualização. Por exemplo, você pode usar o cmdlet [set \- CauClusterRole](/powershell/module/clusterawareupdating/Set-CauClusterRole) do PowerShell para configurar a função clusterizada Cau.                                                            |
|                  Pelo menos dois nós de cluster de failover devem ser proprietários da função clusterizada de CAU                  |                                                                                        Uma execução de atualização iniciada no \- modo de autoatualização falhará, pois a função clusterizada Cau não tem um nó de proprietário possível para o qual mover.                                                                                         |                                                                                                                Use as Ferramentas de cluster de failover para garantir que todos os nós de cluster sejam configurados como possíveis proprietários da função clusterizada de CAU. Essa é a configuração padrão.                                                                                                                |
|                  Todos os nós de cluster de failover devem poder acessar os scripts do Windows PowerShell                  |                                                                        Nem todos os nós de proprietário possíveis da função clusterizada CAU podem acessar os scripts do Windows PowerShell pre \- Update e post update configurados \- . Uma \- execução de autoatualização falhará.                                                                        |                                                                                                                    Verifique se todos os nós proprietários possíveis da função clusterizada CAU têm permissões para acessar os scripts de pré-atualização do PowerShell e pós-atualização configurados \- \- .                                                                                                                     |
|                   Todos os nós de cluster de failover devem usar scripts de Windows PowerShell idênticos                   |                                                     Nem todos os nós proprietários possíveis da função clusterizada CAU usam a mesma cópia dos scripts do Windows PowerShell pre \- Update e post \- Update especificados. Uma \- execução de autoatualização pode falhar ou mostrar um comportamento inesperado.                                                     |                                                                                                                                   Verifique se todos os nós proprietários possíveis da função clusterizada CAU usam os mesmos scripts Pre- \- Update e post update do PowerShell \- .                                                                                                                                   |
|         A configuração de WarnAfter especificada para a Execução de Atualização deve ser inferior à configuração de StopAfter         |                                                                           Os valores de tempo limite Execução de Atualização de CAU fazem com que o aviso de tempo limite não seja efetivo. Uma Execução de Atualização pode ser cancelada antes que um log de evento de avisos possa ser gerado.                                                                            |                                                                                                                                      Nas opções de Execução de Atualização, configure um valor de opção **WarnAfter** que seja inferior ao valor de opção **StopAfter**.                                                                                                                                       |

## <a name="additional-references"></a>Referências adicionais

-   [Visão geral da Atualização com Suporte a Cluster](cluster-aware-updating.md)