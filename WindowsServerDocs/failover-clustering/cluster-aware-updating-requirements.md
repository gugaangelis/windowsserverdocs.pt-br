---
ms.assetid: 75cc1d24-fa2f-45bd-8f3b-1bbd4a1aead0
title: Requisitos de atualização com suporte a cluster e práticas recomendadas
ms.prod: windows-server
ms.topic: article
ms.manager: dongill
author: JasonGerend
ms.author: jgerend
ms.technology: storage-failover-clustering
ms.date: 08/06/2018
description: Requisitos para usar a atualização com suporte a cluster para instalar atualizações em clusters que executam o Windows Server.
ms.openlocfilehash: 501969fad2455195bca485bd8124911d6d75378e
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71361318"
---
# <a name="cluster-aware-updating-requirements-and-best-practices"></a>Requisitos de atualização com suporte a cluster e práticas recomendadas

>Aplica-se a: Windows Server 2019, Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Esta seção descreve os requisitos e as dependências necessários para usar a cau ( [atualização com suporte a cluster](cluster-aware-updating.md) ) para aplicar atualizações a um cluster de failover que executa o Windows Server.

> [!NOTE]  
> Talvez seja necessário validar de forma independente se o seu ambiente de cluster está pronto para aplicar atualizações se você usar um plug @ no__t-0in diferente de **Microsoft. WindowsUpdatePlugin**. Se você estiver usando um plug @ no__t-1in que não seja @ no__t-0Microsoft, entre em contato com o Publicador para obter mais informações. Para obter mais informações sobre o plug @ no__t-0ins, consulte [como o plug @ no__t-2Ins funciona](cluster-aware-updating-plug-ins.md).   

## <a name="BKMK_REQ_CLUS"></a>Instalar o recurso de clustering de failover e as ferramentas de clustering de failover  
O CAU requer uma instalação do recurso de Cluster de failover e as Ferramentas de cluster de failover. As ferramentas de clustering de failover incluem as ferramentas de CAU @no__t -0clusterawareupdating. dll @ no__t-1, os cmdlets de clustering de failover e outros componentes necessários para operações de CAU. Para saber as etapas de instalação do recurso de Cluster de failover, consulte [Installing the Failover Clustering Feature and Tools (Instalando o recurso e as ferramentas de cluster de failover)](create-failover-cluster.md#install-the-failover-clustering-feature).  

Os requisitos de instalação exatos para as ferramentas de clustering de failover dependem se a CAU coordena atualizações como uma função clusterizada no cluster de failover \(by usando o modo Self @ no__t-1updating @ no__t-2 ou de um computador remoto. O modo Self @ no__t-0updating da CAU também requer a instalação da função clusterizada CAU no cluster de failover usando as ferramentas de CAU.    

A tabela a seguir resume os requisitos de instalação do recurso CAU para dois modos de atualização do CAU.  

|Componente instalado|Modo Self @ no__t-0updating|Modo @ no__t-0updating remoto|  
|-----------------------|-----------------------|-------------------------|  
|Recurso de cluster de failover|Necessário em todos os nós de cluster|Necessário em todos os nós de cluster|  
|Ferramentas de cluster de failover|Necessário em todos os nós de cluster|-Necessário no computador remoto @ no__t-0updating<br />-Necessário em todos os nós de cluster para executar o cmdlet [Save-CauDebugTrace](https://docs.microsoft.com/powershell/module/clusterawareupdating/Save-CauDebugTrace?view=win10-ps)|  
|Função clusterizada do CAU|Obrigatório|Não necessário|  

## <a name="obtain-an-administrator-account"></a>Obter uma conta de administrador  
Os requisitos de administrador a seguir são necessários para usar recursos de CAU.  

-   Para visualizar ou aplicar ações de atualização usando a interface do usuário da CAU \(UI @ no__t-1 ou os cmdlets de atualização com suporte a cluster, você deve usar uma conta de domínio que tenha direitos de administrador local e permissões em todos os nós de cluster. Se a conta não tiver privilégios suficientes em todos os nós, você será solicitado na janela de atualização com suporte a cluster para fornecer as credenciais necessárias ao executar essas ações. Para usar os cmdlets de [atualização com suporte a cluster](https://docs.microsoft.com/powershell/module/clusterawareupdating/?view=win10-ps) , você pode fornecer as credenciais necessárias como um parâmetro de cmdlet.  

-   Se você usar a CAU no modo Remote @ no__t-0updating quando estiver conectado com uma conta que não tenha direitos de administrador local e permissões nos nós de cluster, deverá executar as ferramentas de CAU como administrador usando uma conta de administrador local na atualização O computador coordenador ou usando uma conta que tenha o direito de usuário **representar um cliente após a autenticação** . 

-   Para executar o Analisador de Práticas Recomendadas CAU, você deve usar uma conta que tenha privilégios administrativos nos nós de cluster e privilégios administrativos locais no computador usado para executar o cmdlet [Test-CauSetup](https://docs.microsoft.com/powershell/module/clusterawareupdating/Test-CauSetup?view=win10-ps) ou para analisar a atualização do cluster preparação usando a janela de atualização com suporte a cluster. Para mais informações, consulte [Testar preparação de atualização do cluster](#BKMK_BPA).  

## <a name="verify-the-cluster-configuration"></a>Verificar a configuração do cluster  
A seguir estão os requisitos gerais de um cluster de failover para suporte de atualizações usando CAU. Os requisitos de configuração adicional para gerenciamento remoto nos nós estão listados em [Configurar os nós para gerenciamento remoto](#BKMK_NODE_CONFIG) mais adiante neste tópico.  

-   Deve haver nós de cluster suficientes online para que o cluster tenha quorum.  

-   Todos os nós de cluster devem estar no mesmo domínio do Active Directory.  

-   O nome do cluster deve ser resolvido na rede usando DNS.  

-   Se a CAU é usada no modo @ no__t-0updating remoto, o computador do coordenador de atualização deve ter conectividade de rede com os nós de cluster de failover e deve estar no mesmo domínio Active Directory que o cluster de failover.  

-   O serviço de cluster deve estar executando em todos os nós de cluster. Por padrão, esse serviço é instalado em todos os nós de cluster e está configurado para iniciar automaticamente.  

-   Para usar os scripts do PowerShell pre @ no__t-0update ou post @ no__t-1Update durante uma execução de atualização da CAU, verifique se os scripts estão instalados em todos os nós de cluster ou se eles estão acessíveis a todos os nós, por exemplo, em um compartilhamento de arquivos de rede altamente disponível. Se os scripts forem salvos em um compartilhamento de arquivos de rede, configure a pasta para permissão de Leitura para o grupo Todos.  

## <a name="BKMK_NODE_CONFIG"></a>Configurar os nós para gerenciamento remoto  
Para usar a atualização com suporte a cluster, todos os nós do cluster devem ser configurados para gerenciamento remoto. Por padrão, a única tarefa que você deve executar para configurar os nós para gerenciamento remoto é [habilitar uma regra de firewall para permitir reinicializações automáticas](#BKMK_FW). 

A tabela a seguir lista os requisitos completos de gerenciamento remoto, caso seu ambiente seja disverge dos padrões.

Esses requisitos são adicionais aos requisitos de instalação para a [Instalação do recurso de Cluster de failover e Ferramentas de cluster de failover](#BKMK_REQ_CLUS) e os requisitos de cluster gerais descritos nas seções anteriores neste tópico.  

|Requisito|Estado padrão|Modo Self @ no__t-0updating|Modo @ no__t-0updating remoto|  
|---------------|---|-----------------------|-------------------------|  
|[Habilitar uma regra de firewall para permitir reinicializações automáticas](#BKMK_FW)|Desabilitado|Necessário em todos os nós de cluster se um firewall estiver em uso|Necessário em todos os nós de cluster se um firewall estiver em uso|  
|[Habilitar Instrumentação de Gerenciamento do Windows](#BKMK_WMI)|Enabled|Necessário em todos os nós de cluster|Necessário em todos os nós de cluster|  
|[Habilitar o Windows PowerShell 3,0 ou 4,0 e a comunicação remota do Windows PowerShell](#BKMK_PS)|Enabled|Necessário em todos os nós de cluster|Necessário em todos os nós de cluster para executar o seguinte:<br /><br />-O cmdlet [Save-CauDebugTrace](https://docs.microsoft.com/powershell/module/clusterawareupdating/Save-CauDebugTrace?view=win10-ps)<br />-PowerShell pre @ no__t-0update e post @ no__t-1Update scripts durante uma execução de atualização<br />-Testes de preparação de atualização de cluster usando a janela de atualização com suporte a cluster ou o cmdlet [Test @ no__t-1CauSetup](https://docs.microsoft.com/powershell/module/clusterawareupdating/Test-CauSetup?view=win10-ps) do Windows PowerShell|  
|[Instalar o .NET Framework 4,6 ou 4,5](#BKMK_NET)|Enabled|Necessário em todos os nós de cluster|Necessário em todos os nós de cluster para executar o seguinte:<br /><br />-O cmdlet [Save-CauDebugTrace](https://docs.microsoft.com/powershell/module/clusterawareupdating/Save-CauDebugTrace?view=win10-ps)<br />-PowerShell pre @ no__t-0update e post @ no__t-1Update scripts durante uma execução de atualização<br />-Testes de preparação de atualização de cluster usando a janela de atualização com suporte a cluster ou o cmdlet [Test @ no__t-1CauSetup](https://docs.microsoft.com/powershell/module/clusterawareupdating/Test-CauSetup?view=win10-ps) do Windows PowerShell|  

### <a name="BKMK_FW"></a>Habilitar uma regra de firewall para permitir reinicializações automáticas  
Para permitir reinicializações automáticas após a aplicação das atualizações \(if a instalação de uma atualização requer uma reinicialização @ no__t-1, se o Firewall do Windows ou um firewall que não seja @ no__t-2Microsoft estiver em uso nos nós do cluster, uma regra de firewall deverá ser habilitada em cada nó que permitir o seguinte tráfego:  

-   Protocolo: TCP  

-   Direção: entrada  

-   Programa: wininit.exe  

-   Portas: Portas Dinâmicas RPC  

-   Perfil: Domínio  

Se o Firewall do Windows for usado nos nós de cluster, você pode fazer isso habilitando o grupo de regras do Firewall do Windows **Desligamento Remoto** em cada nó de cluster. Quando você usa a janela de atualização com suporte a cluster para aplicar atualizações e para configurar as opções Self @ no__t-0updating, o grupo de regras de firewall do Windows de **desligamento remoto** é habilitado automaticamente em cada nó de cluster.  

> [!NOTE]  
> O grupo de regras do Firewall do Windows **Remote Shutdown** não pode ser habilitado quando ele está em conflito com as definições de Política de Grupo configuradas para o Firewall do Windows.    

O grupo de regras de firewall de **desligamento remoto** também é habilitado especificando o parâmetro **– EnableFirewallRules** ao executar os seguintes cmdlets de Cau: [Add-CauClusterRole](https://docs.microsoft.com/powershell/module/clusterawareupdating/Add-CauClusterRole?view=win10-ps), [Invoke-CauRun](https://docs.microsoft.com/powershell/module/clusterawareupdating/Invoke-CauRun?view=win10-ps)e [SetCauClusterRole](https://docs.microsoft.com/powershell/module/clusterawareupdating/Set-CauClusterRole?view=win10-ps).  

O exemplo do PowerShell a seguir mostra um método adicional para habilitar reinicializações automáticas em um nó de cluster.  

```PowerShell  
Set-NetFirewallRule -Group "@firewallapi.dll,-36751" -Profile Domain -Enabled true  
```  

### <a name="BKMK_WMI"></a>Habilitar Instrumentação de Gerenciamento do Windows (WMI) 
Todos os nós de cluster devem ser configurados para gerenciamento remoto usando Instrumentação de Gerenciamento do Windows \(WMI @ no__t-1. Isso é habilitado por padrão.  

Para habilitar manualmente o gerenciamento remoto, execute o seguinte:  

1.  No console de Serviços, inicie o serviço **Gerenciamento remoto do Windows** e defina o tipo de inicialização **Automática**.  

2.  Execute o cmdlet [Set-WSManQuickConfig](https://docs.microsoft.com/powershell/module/Microsoft.WsMan.Management/Set-WSManQuickConfig?view=powershell-6) ou execute o seguinte comando em um prompt de comando elevado:  

    ```PowerShell  
    winrm quickconfig -q  
    ```  

Para dar suporte à comunicação remota do WMI, se o Firewall do Windows estiver em uso nos nós do cluster, a regra de firewall de entrada para **Gerenciamento Remoto do Windows \(http @ no__t-2in @ no__t-3** deverá ser habilitada em cada nó.  Por padrão, essa regra está habilitada.  

### <a name="BKMK_PS"></a>Habilitar o Windows PowerShell e a comunicação remota do Windows PowerShell  
Para habilitar o modo Self @ no__t-0updating e determinados recursos de CAU no modo Remote @ no__t-1updating, o PowerShell deve ser instalado e habilitado para executar comandos remotos em todos os nós de cluster. Por padrão, o PowerShell é instalado e habilitado para comunicação remota.  

Para habilitar a comunicação remota do PowerShell, use um dos seguintes métodos:  

-   Execute o cmdlet [Enable-PSRemoting](https://docs.microsoft.com/powershell/module/Microsoft.PowerShell.Core/Enable-PSRemoting) .  

-   Configure uma configuração de Política de Grupo de domínio @ no__t-0level para Gerenciamento Remoto do Windows \(WinRM @ no__t-2.  

Para obter mais informações sobre como habilitar a comunicação remota do PowerShell, consulte [sobre requisitos remotos](https://docs.microsoft.com/powershell/module/microsoft.powershell.core/about/about_remote_requirements?view=powershell-6).  

### <a name="BKMK_NET"></a>Instalar o .NET Framework 4,6 ou 4,5  
Para habilitar o modo Self @ no__t-0updating e determinados recursos de CAU no modo Remote @ no__t-1updating,. NET Framework 4,6 ou .NET Framework 4,5 (no Windows Server 2012 R2) deve ser instalado em todos os nós de cluster. Por padrão, o .NET Framework é instalado.  

Para instalar .NET Framework 4,6 (ou 4,5) usando o PowerShell, se ele ainda não estiver instalado, use o seguinte comando:

```PowerShell
Install-WindowsFeature -Name NET-Framework-45-Core
```

## <a name="BKMK_BEST_PRAC"></a>Recomendações de práticas recomendadas para usar a atualização com suporte a cluster 

### <a name="BKMK_BP_WUA"></a>Recomendações para a aplicação de atualizações da Microsoft

Recomendamos que, quando você começar a usar a CAU para aplicar atualizações com o padrão **Microsoft. WindowsUpdatePlugin** plug @ no__t-1in em um cluster, você pare de usar outros métodos para instalar atualizações de software da Microsoft nos nós de cluster.  

> [!CAUTION]  
> Combinar a CAU com métodos que atualizam nós individuais automaticamente \(on uma agenda de tempo fixa @ no__t-1 pode causar resultados imprevisíveis, incluindo interrupções no serviço e tempo de inatividade não planejado.  

Convém seguir estas diretrizes:  

-   Para melhores resultados, recomendamos desabilitar as configurações nos nós do cluster para atualização automática, por exemplo, por meio das configurações de Atualizações automáticas no Painel de controle, ou nas configurações definidas usando a Política de Grupo.  

    > [!CAUTION]  
    > A instalação automática de atualizações nos nós de cluster pode interferir na instalação de atualizações pelo CAU e causar falhas no CAU.  

    Se elas forem necessárias, as seguintes Atualizações automáticas são compatíveis com CAU, porque o administrador pode controlar o sincronismo da instalação da atualização:  

    -   Configurações para notificar antes do download das atualizações e para notificar antes da instalação  

    -   Configurações para baixar as atualizações automaticamente e notificar antes da instalação  

    No entanto, se as Atualizações Automáticas estiverem baixando atualizações ao mesmo tempo em que é executada uma atualização do CAU, a execução da atualização poderá levar mais tempo para ser concluída.  

-   Não configure um sistema de atualização, como Windows Server Update Services \(WSUS @ no__t-1 para aplicar atualizações automaticamente \(on uma agenda de tempo fixa @ no__t-3 a nós de cluster.  

-   Todos os nós de cluster devem ser configurados uniformemente para usar a mesma fonte de atualização, por exemplo, um servidor WSUS, Windows Update ou Microsoft Update.  

-   Se você usar um sistema de gerenciamento de configurações para aplicar atualizações de software a computadores na rede, exclua os nós do cluster de todas as atualizações necessárias ou automáticas. Exemplos de sistemas de gerenciamento de configurações incluem o Microsoft System Center Configuration Manager 2007 e o Microsoft System Center Virtual Machine Manager 2008.  

-   Se os servidores de distribuição de software internos \(for exemplo, os servidores do WSUS @ no__t-1 serão usados para conter e implantar as atualizações, verifique se esses servidores identificaram corretamente as atualizações aprovadas para os nós de cluster.  

#### <a name="BKMK_PROXY"></a>Aplicar atualizações da Microsoft em cenários de filial  
Para baixar atualizações da Microsoft do Microsoft Update ou Windows Update para nós de cluster em determinados cenários de filiais, pode ser necessário configurar definições de proxy para a conta de Sistema Local em cada nó. Por exemplo, isso pode ser necessário se os clusters da filial acessarem o Microsoft Update ou o Windows Update para baixar atualizações usando um servidor de proxy local.  

Se necessário, defina as configurações de proxy do WinHTTP em cada nó para especificar um servidor proxy local e configurar exceções de endereço local \(that é, uma lista de bypass para endereços locais @ no__t-1. Para isso, você pode executar o seguinte comando em cada nó de cluster em um prompt de comandos com privilégios elevados:  

```  
netsh winhttp set proxy <ProxyServerFQDN >:<port> "<local>"  
```  

em que <*ProxyServerFQDN*> é o nome de domínio totalmente qualificado para o servidor proxy e a*porta*< > é a porta pela qual se comunicar (geralmente a porta 443).  

Por exemplo, para definir as configurações de proxy do WinHTTP para a conta do sistema local especificando o servidor proxy *MyProxy.contoso.com*, com a porta 443 e as exceções de endereço local, digite o seguinte comando:  

```  
netsh winhttp set proxy MyProxy.CONTOSO.com:443 "<local>"  
```  

### <a name="BKMK_BP_HF"></a>Recomendações para usar o Microsoft. HotfixPlugin  

-   Recomendamos configurar permissões na pasta raiz de hotfix e o arquivo de configuração de hotfix para restringir o acesso de Gravação aos administradores locais somente nos computadores que são usados para armazenar esses arquivos. Isso ajuda a evitar possíveis violações desses arquivos por usuários não autorizados que poderiam comprometer a funcionalidade do cluster de failover quando os hotfixes forem aplicados.  

-   Para ajudar a garantir a integridade dos dados para o protocolo de mensagens de servidor \(SMB @ no__t-1 que são usadas para acessar a pasta raiz do hotfix, você deve configurar a criptografia SMB na pasta compartilhada SMB, se for possível configurá-la. O **Microsoft.HotfixPlugin** requer que a assinatura SMB ou Criptografia SMB seja configurada para ajudar a garantir a integridade dos dados para as conexões SMB. 

    Para obter mais informações, consulte [restringir o acesso à pasta raiz do hotfix e ao arquivo de configuração do hotfix](cluster-aware-updating-plug-ins.md#BKMK_ACL).

### <a name="additional-recommendations"></a>Recomendações adicionais  

-   Para evitar interferir em uma Execução de atualização CAU que pode estar agendada ao mesmo tempo, não agende alterações de senha para objetos de nome de cluster durante as janelas de manutenção agendadas.  

-   Você deve definir as permissões apropriadas nos scripts Pre @ no__t-0update e post @ no__t-1Update que são salvos em pastas compartilhadas de rede para evitar possíveis violações desses arquivos por usuários não autorizados.  

-   Para configurar a CAU no modo Self @ no__t-0updating, um objeto de computador virtual \(VCO @ no__t-2 para a função clusterizada CAU deve ser criado em Active Directory. O CAU pode criar esse objeto automaticamente no momento em que a função clusterizada do CAU for adicionada e o cluster de failover tiver permissões suficientes. Entretanto, por causa das políticas de segurança em certas organizações, pode ser necessário pré-configurar o objeto no Active Directory. Para obter um procedimento para isso, consulte [Etapas para pré-preparar a conta para uma função em cluster](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/cc731002\(v=ws.10\)#steps-for-prestaging-the-cluster-name-account).  

-   Para salvar e reutilizar as configurações de Executar atualização nos clusters de failover com necessidades de atualização similar na organização de TI, você pode criar Perfis de Execução de Atualização. Além disso, dependendo do modo de atualização, você pode salvar e gerenciar os Perfis de Execução de Atualização em um compartilhamento de arquivo que está acessível a todos os computadores do Coordenador de Atualização remoto ou clusters de failover. Para obter mais informações, consulte [Opções avançadas e atualizando perfis de execução para a cau](cluster-aware-updating-options.md).  

## <a name="BKMK_BPA"></a>Preparação da atualização do cluster de teste  
Você pode executar o modelo de Analisador de Práticas Recomendadas CAU \(BPA @ no__t-1 para testar se um cluster de failover e o ambiente de rede atendem a muitos dos requisitos para que as atualizações de software sejam aplicadas pela CAU. Muitos dos testes verificam o ambiente quanto à preparação para aplicar as atualizações da Microsoft usando o plug-in padrão do tipo @ no__t-0in, **Microsoft. WindowsUpdatePlugin**.  

> [!NOTE]  
> Talvez seja necessário validar de forma independente que o ambiente de cluster está pronto para aplicar as atualizações de software usando um plug @ no__t-0in diferente de **Microsoft. WindowsUpdatePlugin**. Se você estiver usando um plug @ no__t-1in que não seja @ no__t-0Microsoft, como um fornecido pelo fabricante do hardware, entre em contato com o editor para obter mais informações.  

É possível executar o BPA nas duas seguintes maneiras:  

1.  Selecione **Analisar preparação da atualização do cluster** no console de CAU. Depois que o BPA concluir os testes de preparação, um relatório de teste será exibido. Se forem detectados problemas nos nós de cluster, os problemas específicos e os nós nos quais os problemas aparecem são identificados, para que você possa executar a ação corretiva. Os testes podem levar vários minutos para serem concluídos.  

2.  Execute o cmdlet [Test-CauSetup](https://docs.microsoft.com/powershell/module/clusterawareupdating/Test-CauSetup) . Você pode executar o cmdlet em um computador local ou remoto no qual o módulo clustering de failover do Windows PowerShell (parte das ferramentas de clustering de failover) está instalado. Também é possível executar o cmdlet em um nó do cluster de failover.  

> [!NOTE]  
> -   Você deve usar uma conta que tenha privilégios administrativos nos nós de cluster e privilégios administrativos locais no computador usado para executar o cmdlet **Test @ no__t-1CauSetup** ou para analisar a preparação da atualização do cluster usando o reconhecimento de cluster Atualizando janela. Para executar os testes usando a janela de atualização com suporte a cluster, você deve estar conectado ao computador com as credenciais necessárias.  
> -   Os testes assumem que as ferramentas do CAU usadas para visualizar e aplicar as atualizações de software são executadas a partir do mesmo computador e com as mesmas credenciais de usuário que as usadas para testar a preparação de atualização do cluster.  

> [!IMPORTANT]  
> É altamente recomendável que você teste o cluster quanto à preparação para atualização nas seguintes situações:  
>   
> -   Antes de usar o CAU pela primeira vez para aplicar atualizações de software.  
> -   Após adicionar um nó ao cluster ou executar outras alterações de hardware no cluster, que requerem a execução do Assistente de Validação de Cluster.  
> -   Depois de alterar uma origem de atualização, ou alterar configurações de atualização ou configurações \(other que a CAU @ no__t-1 que pode afetar a aplicação de atualizações nos nós.  

### <a name="tests-for-cluster-updating-readiness"></a>Testes para preparação de atualização do cluster  
A tabela a seguir lista os testes de preparação de atualização do cluster, alguns problemas comuns e etapas de resolução.  


|                                                      Testar                                                      |                                                                                                                                               Possíveis problemas e impactos                                                                                                                                               |                                                                                                                                                                                         Etapas de resolução                                                                                                                                                                                         |
|----------------------------------------------------------------------------------------------------------------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
|                                     O cluster de failover deve estar disponível                                     |                                                                                       Não é possível resolver o nome do cluster de failover, ou um ou mais nós de cluster não podem ser acessados. O BPA não pode executar testes de preparação do cluster.                                                                                        |                                                                   -Verifique a ortografia do nome do cluster especificado durante a execução do BPA.<br />-Verifique se todos os nós do cluster estão online e em execução.<br />-Verifique se o assistente para validar uma configuração pode ser executado com êxito no cluster de failover.                                                                    |
|                    Os nós de cluster de failover devem estar habilitados para gerenciamento remoto via WMI                    |                                                Um ou mais nós de cluster de failover não estão habilitados para gerenciamento remoto usando Instrumentação de Gerenciamento do Windows \(WMI @ no__t-1. O CAU não pode atualizar os nós de cluster, se os nós não estiverem configurados para o gerenciamento remoto.                                                 |                                                                                                  Certifique-se de que todos os nós de cluster de failover estejam habilitados para o gerenciamento remoto por meio de WMI. Para mais informações, consulte [Configurar os nós para o gerenciamento remoto](#BKMK_NODE_CONFIG) neste tópico.                                                                                                   |
|                      A comunicação remota do PowerShell deve ser habilitada em cada nó de cluster de failover                       |                                                           O PowerShell não está instalado ou não está habilitado para comunicação remota em um ou mais nós de cluster de failover. A CAU não pode ser configurada para o modo Self @ no__t-0updating ou usar determinados recursos no modo Remote @ no__t-1updating.                                                            |                                                                                             Verifique se o PowerShell está instalado em todos os nós de cluster e se está habilitado para comunicação remota.<br /><br />Para mais informações, consulte [Configurar os nós para o gerenciamento remoto](#BKMK_NODE_CONFIG) neste tópico.                                                                                             |
|                                            Versão de cluster de failover                                            |                                                                            Um ou mais nós no cluster de failover não executam o Windows Server 2016, o Windows Server 2012 R2 ou o Windows Server 2012. O CAU não pode atualizar o cluster de failover.                                                                             |                                                                   Verifique se o cluster de failover especificado durante a execução do BPA está executando o Windows Server 2016, o Windows Server 2012 R2 ou o Windows Server 2012.<br /><br />Para obter mais informações, consulte [Verificar a configuração do cluster](#BKMK_REQ_CLUS) neste tópico.                                                                   |
| As versões necessárias do .NET Framework e do Windows PowerShell devem ser instaladas em todos os nós de cluster de failover |                                                                                              .NET Framework 4,6, 4,5 ou Windows PowerShell não está instalado em um ou mais nós de cluster. Alguns recursos de CAU podem não funcionar.                                                                                              |                                                                            Verifique se o .NET Framework 4,6 ou 4,5 e o Windows PowerShell estão instalados em todos os nós de cluster, se forem necessários.<br /><br />Para mais informações, consulte [Configurar os nós para o gerenciamento remoto](#BKMK_NODE_CONFIG) neste tópico.                                                                             |
|                           O serviço de cluster deve estar em execução em todos os nós de cluster                           |                                                                                                            O serviço de cluster não está sendo executado em um ou mais nós. O CAU não pode atualizar o cluster de failover.                                                                                                             |                        -Verifique se a Serviço de cluster \(clussvc @ no__t-1 foi iniciada em todos os nós no cluster e se está configurada para iniciar automaticamente.<br />-Verifique se o assistente para validar uma configuração pode ser executado com êxito no cluster de failover.<br /><br />Para obter mais informações, consulte [Verificar a configuração do cluster](#BKMK_REQ_CLUS) neste tópico.                         |
|     As atualizações automáticas não devem estar configuradas para instalar automaticamente as atualizações em nenhum nó de cluster de failover     |                                           As atualizações automáticas devem estar configuradas em, no mínimo, um nó de cluster de failover para instalar automaticamente as atualizações da Microsoft nesse nó. Combinar o CAU com outros métodos de atualização pode resultar em tempo de inatividade não planejado ou resultados imprevisíveis.                                            |                                                     Se a funcionalidade do Windows Update estiver configurada para atualizações automáticas em um ou mais nós de cluster, certifique-se de que as atualizações automáticas não estejam configuradas para instalar as atualizações automaticamente.<br /><br />Para obter mais informações, consulte [Recomendações para aplicar atualizações da Microsoft](#BKMK_BP_WUA).                                                     |
|                          Os nós de cluster de failover devem usar a mesma origem de atualização                          |                                                    Um ou mais nós de cluster de failover estão configurados para usar uma origem de atualização para as atualizações da Microsoft que é diferente do restante dos nós. As atualizações podem não ser aplicadas de maneira uniforme nos nós de cluster pelo CAU.                                                    |                                                                        Certifique-se de que todos os nós de cluster estejam configurados para usar a mesma origem de atualização, por exemplo, um servidor WSUS, Windows Update ou Microsoft Update.<br /><br />Para obter mais informações, consulte [Recomendações para aplicar atualizações da Microsoft](#BKMK_BP_WUA).                                                                         |
|       Uma regra de firewall que permite o encerramento remoto deve ser habilitada em todos os nós no cluster de failover       |                 Um ou mais nós de cluster de failover não possui uma regra de firewall habilitada que permita o encerramento remoto ou uma configuração de Política de Grupo que previna que essa regra seja habilitada. Uma Execução de Atualização que aplica atualizações que requerem a reinicialização automática dos nós pode não ser concluída de maneira adequada.                  |                                                                    Se o Firewall do Windows ou um firewall não no__t-0Microsoft estiver em uso nos nós do cluster, configure uma regra de firewall que permita o desligamento remoto.<br /><br />Para obter mais informações, consulte [Habilitar uma função de firewall para permitir reinícios automáticos](#BKMK_FW) neste tópico.                                                                    |
|          A configuração de servidor de proxy em cada nó de cluster de failover deve ser definida em um servidor proxy local          |                             Um ou mais nós de cluster de failover têm uma configuração incorreta de servidor proxy.<br /><br />Se um servidor proxy local estiver em uso, a configuração de servidor proxy em cada nó deve ser configurada corretamente para o cluster acessar o Microsoft Update ou o Windows Update.                              |                                            Certifique-se de que as configurações de proxy do WinHTTP em cada nó de cluster estejam definidas em um servidor proxy local, caso necessário. Se um servidor proxy não estiver em uso em seu ambiente, esse aviso pode ser ignorado.<br /><br />Para mais informações, consulte [Aplicar atualizações em cenários de filiais](#BKMK_PROXY) neste tópico.                                            |
|        A função clusterizada CAU deve ser instalada no cluster de failover para habilitar o modo Self @ no__t-0updating        |                                                                                                   A função clusterizada de CAU não está instalada nesse cluster de failover. Essa função é necessária para o próprio @ no__t-0updating do cluster.                                                                                                   |      Para usar a CAU no modo Self @ no__t-0updating, adicione a função clusterizada CAU no cluster de failover de uma das seguintes maneiras:<br /><br />-Execute o cmdlet do PowerShell [Add-CauClusterRole](https://docs.microsoft.com/powershell/module/clusterawareupdating/Add-CauClusterRole) .<br />-Selecione a ação **Configurar opções do cluster Self @ no__t-1updating** na janela de atualização com suporte a cluster.      |
|         A função clusterizada CAU deve ser habilitada no cluster de failover para habilitar o modo Self @ no__t-0updating         | A função clusterizada de CAU é desabilitada. Por exemplo, a função clusterizada CAU não está instalada ou foi desabilitada usando o cmdlet [Disable @ no__t-1CauClusterRole](https://docs.microsoft.com/powershell/module/clusterawareupdating/Disable-CauClusterRole) do PowerShell. Essa função é necessária para o próprio @ no__t-0updating do cluster. | Para usar a CAU no modo Self @ no__t-0updating, habilite a função clusterizada CAU nesse cluster de failover de uma das seguintes maneiras:<br /><br />-Execute o cmdlet [Enable-CauClusterRole](https://docs.microsoft.com/powershell/module/clusterawareupdating/Enable-CauClusterRole) do PowerShell.<br />-Selecione a ação **Configurar opções do cluster Self @ no__t-1updating** na janela de atualização com suporte a cluster. |
|      O plug-0in da CAU configurado para o modo Self @ no__t-1updating deve ser registrado em todos os nós de cluster de failover      |                                                              A função clusterizada CAU em um ou mais nós deste cluster de failover não pode acessar o módulo de plug @ no__t-0in da CAU que está configurado nas opções Self @ no__t-1updating. Uma execução de @ no__t-0updating pode falhar.                                                              |           -Verifique se o plug-in CAU (no__t-0in) configurado está instalado em todos os nós de cluster seguindo o procedimento de instalação do produto que fornece o plug-in CAU @ no__t-1in.<br />-Execute o cmdlet [Register @ no__t-1CauPlugin](https://docs.microsoft.com/powershell/module/clusterawareupdating/Register-CauPlugin) do PowerShell para registrar o plug @ no__t-2in nos nós de cluster necessários.           |
|                Todos os nós de cluster de failover devem ter o mesmo conjunto de conectores CAU @ no__t-0ins registrado                 |                                                                             Uma execução de autono__t-0updating poderá falhar se o plug @ no__t-1in configurado para ser usado em uma execução de atualização for alterado para um que não esteja disponível em todos os nós de cluster.                                                                              |           -Verifique se o plug-in CAU (no__t-0in) configurado está instalado em todos os nós de cluster seguindo o procedimento de instalação do produto que fornece o plug-in CAU @ no__t-1in.<br />-Execute o cmdlet [Register @ no__t-1CauPlugin](https://docs.microsoft.com/powershell/module/clusterawareupdating/Register-CauPlugin) do PowerShell para registrar o plug @ no__t-2in nos nós de cluster necessários.           |
|                               As opções configuradas de Execução de Atualização devem ser válidas                                |                                                                          As opções de execução de agendamento e atualização do self @ no__t-0updating que estão configuradas para este cluster de failover estão incompletas ou não são válidas. Uma execução de @ no__t-0updating pode falhar.                                                                           |                                                            Configure um agendamento Self @ no__t-0updating válido e um conjunto de opções de execução de atualização. Por exemplo, você pode usar o cmdlet [set @ no__t-1CauClusterRole](https://docs.microsoft.com/powershell/module/clusterawareupdating/Set-CauClusterRole) do PowerShell para configurar a função clusterizada Cau.                                                            |
|                  Pelo menos dois nós de cluster de failover devem ser proprietários da função clusterizada de CAU                  |                                                                                        Uma execução de atualização iniciada no modo Self @ no__t-0updating falhará porque a função clusterizada CAU não tem um nó de proprietário possível para o qual mover.                                                                                         |                                                                                                                Use as Ferramentas de cluster de failover para garantir que todos os nós de cluster sejam configurados como possíveis proprietários da função clusterizada de CAU. Essa é a configuração padrão.                                                                                                                |
|                  Todos os nós de cluster de failover devem poder acessar os scripts do Windows PowerShell                  |                                                                        Nem todos os nós proprietários possíveis da função clusterizada CAU podem acessar os scripts do Windows PowerShell pre @ no__t-0update e post @ no__t-1Update configurados. Uma execução de no__t Self-0updating falhará.                                                                        |                                                                                                                    Verifique se todos os nós proprietários possíveis da função clusterizada CAU têm permissões para acessar os scripts do PowerShell pre @ no__t-0update e post @ no__t-1Update configurados.                                                                                                                     |
|                   Todos os nós de cluster de failover devem usar scripts de Windows PowerShell idênticos                   |                                                     Nem todos os nós proprietários possíveis da função clusterizada CAU usam a mesma cópia dos scripts do Windows PowerShell pre @ no__t-0update e post @ no__t-1Update especificados. Uma execução do @ @ no__t-0updating pode falhar ou mostrar um comportamento inesperado.                                                     |                                                                                                                                   Verifique se todos os nós proprietários possíveis da função clusterizada CAU usam os mesmos scripts Pre @ no__t-0update e post @ no__t-1Update do PowerShell.                                                                                                                                   |
|         A configuração de WarnAfter especificada para a Execução de Atualização deve ser inferior à configuração de StopAfter         |                                                                           Os valores de tempo limite Execução de Atualização de CAU fazem com que o aviso de tempo limite não seja efetivo. Uma Execução de Atualização pode ser cancelada antes que um log de evento de avisos possa ser gerado.                                                                            |                                                                                                                                      Nas opções de Execução de Atualização, configure um valor de opção **WarnAfter** que seja inferior ao valor de opção **StopAfter** .                                                                                                                                       |

## <a name="see-also"></a>Consulte também  

-   [Visão geral da atualização com suporte a cluster](cluster-aware-updating.md)