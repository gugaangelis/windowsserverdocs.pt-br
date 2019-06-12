---
ms.assetid: 75cc1d24-fa2f-45bd-8f3b-1bbd4a1aead0
title: Cluster-Aware Updating requisitos e práticas recomendadas
ms.prod: windows-server-threshold
ms.topic: article
ms.manager: dongill
author: JasonGerend
ms.author: jgerend
ms.technology: storage-failover-clustering
ms.date: 08/06/2018
description: Requisitos para usar a atualização com suporte a Cluster para instalar atualizações em clusters que executam o Windows Server.
ms.openlocfilehash: 32fa77ca2c24d75347d61f9bdeb4c5c93a20d89d
ms.sourcegitcommit: eaf071249b6eb6b1a758b38579a2d87710abfb54
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 05/31/2019
ms.locfileid: "66439686"
---
# <a name="cluster-aware-updating-requirements-and-best-practices"></a>Cluster-Aware Updating requisitos e práticas recomendadas

>Aplica-se a: Windows Server 2019, Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Esta seção descreve os requisitos e as dependências necessárias para usar [Cluster-Aware Updating](cluster-aware-updating.md) (CAU) para aplicar atualizações a um cluster de failover executando o Windows Server.

> [!NOTE]  
> Talvez você precise verificar independentemente se o seu ambiente de cluster está preparado para aplicar atualizações se você usar um plug\-em que **windowsupdateplugin**. Se você estiver usando um não\-plug Microsoft\-, entre em contato com o publicador para obter mais informações. Para obter mais informações sobre a plug\-ins, consulte [como conecte\-ins funcionam](cluster-aware-updating-plug-ins.md).   

## <a name="BKMK_REQ_CLUS"></a>Instale o recurso de Clustering de Failover e as ferramentas de Clustering de Failover  
O CAU requer uma instalação do recurso de Cluster de failover e as Ferramentas de cluster de failover. As ferramentas de Clustering de Failover incluem ferramentas do CAU \(clusterawareupdating\), os cmdlets de Clustering de Failover e outros componentes necessários para operações de CAU. Para saber as etapas de instalação do recurso de Cluster de failover, consulte [Installing the Failover Clustering Feature and Tools (Instalando o recurso e as ferramentas de cluster de failover)](create-failover-cluster.md#install-the-failover-clustering-feature).  

Os requisitos exatos de instalação para as ferramentas de Clustering de Failover dependem se o CAU coordena atualizações como uma função clusterizada no cluster de failover \(por meio de self\-modo de atualização\) ou de um computador remoto. O self\-adicionalmente o modo da CAU de atualização requer a instalação da função clusterizada de CAU no cluster de failover usando as ferramentas CAU.    

A tabela a seguir resume os requisitos de instalação do recurso CAU para dois modos de atualização do CAU.  

|Componente instalado|Self\-modo de atualização|Remoto\-modo de atualização|  
|-----------------------|-----------------------|-------------------------|  
|Recurso de cluster de failover|Necessário em todos os nós de cluster|Necessário em todos os nós de cluster|  
|Ferramentas de cluster de failover|Necessário em todos os nós de cluster|-Required remoto\-atualizar computador<br />-Necessário em todos os nós de cluster para executar o [Save-CauDebugTrace](https://docs.microsoft.com/powershell/module/clusterawareupdating/Save-CauDebugTrace?view=win10-ps) cmdlet|  
|Função clusterizada do CAU|Obrigatório|Não necessário|  

## <a name="obtain-an-administrator-account"></a>Obter uma conta de administrador  
Os requisitos de administrador a seguir são necessários para usar recursos de CAU.  

-   Para visualizar ou aplicar ações atualizadas usando a interface de usuário da CAU \(interface do usuário\) ou os cmdlets de Cluster-Aware Updating, você deve usar uma conta de domínio que tenha permissões e direitos de administrador local em todos os nós de cluster. Se a conta não tem privilégios suficientes em cada nó, você precisará na janela do Cluster-Aware Updating fornecer as credenciais necessárias quando você executar essas ações. Para usar o [Cluster-Aware Updating](https://docs.microsoft.com/powershell/module/clusterawareupdating/?view=win10-ps) cmdlets, você pode fornecer as credenciais necessárias como um parâmetro de cmdlet.  

-   Se você utiliza CAU no remoto\-atualizar modo quando você está conectado com uma conta que não tenha permissões e direitos de administrador local em nós de cluster, você deve executar as ferramentas do CAU como um administrador usando uma conta de administrador local no Computador coordenador de atualização ou usando uma conta que tenha o **representar um cliente após autenticação** direito de usuário. 

-   Para executar o analisador de práticas recomendadas do CAU, você deve usar uma conta que tenha privilégios administrativos em nós de cluster e privilégios administrativos locais no computador que é usado para executar o [Test-CauSetup](https://docs.microsoft.com/powershell/module/clusterawareupdating/Test-CauSetup?view=win10-ps) cmdlet ou para analisar usando a janela de Cluster-Aware Updating a prontidão de atualização do cluster. Para mais informações, consulte [Testar preparação de atualização do cluster](#BKMK_BPA).  

## <a name="verify-the-cluster-configuration"></a>Verificar a configuração do cluster  
A seguir estão os requisitos gerais de um cluster de failover para suporte de atualizações usando CAU. Os requisitos de configuração adicional para gerenciamento remoto nos nós estão listados em [Configurar os nós para gerenciamento remoto](#BKMK_NODE_CONFIG) mais adiante neste tópico.  

-   Deve haver nós de cluster suficientes online para que o cluster tenha quorum.  

-   Todos os nós de cluster devem estar no mesmo domínio do Active Directory.  

-   O nome do cluster deve ser resolvido na rede usando DNS.  

-   Se for usado CAU no remoto\-modo de atualização, o computador de coordenador de atualização deve ter conectividade de rede para os nós de cluster de failover e ela deve estar no mesmo domínio do Active Directory que o cluster de failover.  

-   O serviço de cluster deve estar executando em todos os nós de cluster. Por padrão, esse serviço é instalado em todos os nós de cluster e está configurado para iniciar automaticamente.  

-   Para usar o PowerShell pré\-atualizar ou postar\-scripts durante uma execução de atualização CAU, assegure que os scripts são instalados em todos os nós de cluster ou que são acessíveis a todos os nós, por exemplo, em um compartilhamento de arquivos de rede altamente disponível. Se os scripts forem salvos em um compartilhamento de arquivos de rede, configure a pasta para permissão de Leitura para o grupo Todos.  

## <a name="BKMK_NODE_CONFIG"></a>Configurar os nós para gerenciamento remoto  
Para usar o Cluster-Aware Updating, todos os nós do cluster devem ser configurados para gerenciamento remoto. Por padrão, a única tarefa você deve executar para configurar os nós para gerenciamento remoto é [habilitar uma regra de firewall permitir reinícios automáticos](#BKMK_FW). 

A tabela a seguir lista os requisitos de gerenciamento remoto completa, no caso de seu ambiente divergirá dos padrões.

Esses requisitos são adicionais aos requisitos de instalação para a [Instalação do recurso de Cluster de failover e Ferramentas de cluster de failover](#BKMK_REQ_CLUS) e os requisitos de cluster gerais descritos nas seções anteriores neste tópico.  

|Requisito|Estado padrão|Self\-modo de atualização|Remoto\-modo de atualização|  
|---------------|---|-----------------------|-------------------------|  
|[Habilitar uma regra de firewall permitir reinícios automáticos](#BKMK_FW)|Desabilitada|Necessário em todos os nós de cluster se um firewall estiver em uso|Necessário em todos os nós de cluster se um firewall estiver em uso|  
|[Habilitar a instrumentação de gerenciamento do Windows](#BKMK_WMI)|Enabled|Necessário em todos os nós de cluster|Necessário em todos os nós de cluster|  
|[Habilitar o Windows PowerShell 3.0 ou 4.0 e comunicação remota do Windows PowerShell](#BKMK_PS)|Enabled|Necessário em todos os nós de cluster|Necessário em todos os nós de cluster para executar o seguinte:<br /><br />-A [Save-CauDebugTrace](https://docs.microsoft.com/powershell/module/clusterawareupdating/Save-CauDebugTrace?view=win10-ps) cmdlet<br />-Pré PowerShell\-atualizar e lançar\-durante uma execução de atualização<br />-Testes de preparação de atualização usando a janela de atualização com suporte a Cluster do cluster ou o [teste\-CauSetup](https://docs.microsoft.com/powershell/module/clusterawareupdating/Test-CauSetup?view=win10-ps) cmdlet do Windows PowerShell|  
|[Instalar o .NET Framework 4.6 ou 4.5](#BKMK_NET)|Enabled|Necessário em todos os nós de cluster|Necessário em todos os nós de cluster para executar o seguinte:<br /><br />-A [Save-CauDebugTrace](https://docs.microsoft.com/powershell/module/clusterawareupdating/Save-CauDebugTrace?view=win10-ps) cmdlet<br />-Pré PowerShell\-atualizar e lançar\-durante uma execução de atualização<br />-Testes de preparação de atualização usando a janela de atualização com suporte a Cluster do cluster ou o [teste\-CauSetup](https://docs.microsoft.com/powershell/module/clusterawareupdating/Test-CauSetup?view=win10-ps) cmdlet do Windows PowerShell|  

### <a name="BKMK_FW"></a>Habilitar uma regra de firewall permitir reinícios automáticos  
Para permitir reinícios automáticos após as atualizações são aplicadas \(se a instalação de uma atualização requer uma reinicialização\), se o Firewall do Windows ou um não\-Microsoft firewall está em uso em nós de cluster, uma regra de firewall deve ser habilitada no cada nó que permite o tráfego a seguir:  

-   Protocolo: TCP  

-   Direção: entrada  

-   Programa: wininit.exe  

-   Portas: Portas Dinâmicas RPC  

-   Perfil: Domínio  

Se o Firewall do Windows for usado nos nós de cluster, você pode fazer isso habilitando o grupo de regras do Firewall do Windows **Desligamento Remoto** em cada nó de cluster. Quando você usa a janela de atualização com suporte a Cluster para aplicar atualizações e configurar a autoatendimento\-atualizando opções, o **desligamento remoto** grupo de regras de Firewall do Windows é habilitado automaticamente em cada nó do cluster.  

> [!NOTE]  
> O grupo de regras do Firewall do Windows **Remote Shutdown** não pode ser habilitado quando ele está em conflito com as definições de Política de Grupo configuradas para o Firewall do Windows.    

O **Remote Shutdown** grupo de regras de firewall também é habilitado especificando as **– EnableFirewallRules** parâmetro ao executar os seguintes cmdlets do CAU: [Add-CauClusterRole](https://docs.microsoft.com/powershell/module/clusterawareupdating/Add-CauClusterRole?view=win10-ps), [Invoke-CauRun](https://docs.microsoft.com/powershell/module/clusterawareupdating/Invoke-CauRun?view=win10-ps), e [SetCauClusterRole](https://docs.microsoft.com/powershell/module/clusterawareupdating/Set-CauClusterRole?view=win10-ps).  

O exemplo de PowerShell a seguir mostra um método adicional para permitir reinícios automáticos em um nó de cluster.  

```PowerShell  
Set-NetFirewallRule -Group "@firewallapi.dll,-36751" -Profile Domain -Enabled true  
```  

### <a name="BKMK_WMI"></a>Habilitar a instrumentação de gerenciamento do Windows (WMI) 
Todos os nós de cluster devem ser configurados para o gerenciamento remoto usando o Windows Management Instrumentation \(WMI\). Isso é habilitado por padrão.  

Para habilitar manualmente o gerenciamento remoto, execute o seguinte:  

1.  No console de Serviços, inicie o serviço **Gerenciamento remoto do Windows** e defina o tipo de inicialização **Automática**.  

2.  Execute o [Set-WSManQuickConfig](https://docs.microsoft.com/powershell/module/Microsoft.WsMan.Management/Set-WSManQuickConfig?view=powershell-6) prompt cmdlet, ou execute o seguinte comando em um comando com privilégios elevados:  

    ```PowerShell  
    winrm quickconfig -q  
    ```  

Para dar suporte à comunicação remota do WMI, se o Firewall do Windows está em uso em nós de cluster, regra de firewall de entrada para **gerenciamento remoto do Windows \(HTTP\-na\)**  deve estar habilitado em cada nó.  Por padrão, essa regra está habilitada.  

### <a name="BKMK_PS"></a>Habilitar o Windows PowerShell e a comunicação remota do Windows PowerShell  
Para habilitar o self\-atualização de modo e certos recursos do CAU no remoto\-modo de atualização, PowerShell deve ser instalado e habilitado para executar comandos remotos em todos os nós de cluster. Por padrão, o PowerShell é instalado e habilitado remotamente.  

Para habilitar a comunicação remota do PowerShell, use um dos seguintes métodos:  

-   Execute o [Enable-PSRemoting](https://docs.microsoft.com/powershell/module/Microsoft.PowerShell.Core/Enable-PSRemoting) cmdlet.  

-   Configurar um domínio\-configuração de diretiva de grupo para o gerenciamento remoto do Windows no nível \(WinRM\).  

Para obter mais informações sobre como habilitar a comunicação remota do PowerShell, consulte [sobre requisitos remotos](https://docs.microsoft.com/powershell/module/microsoft.powershell.core/about/about_remote_requirements?view=powershell-6).  

### <a name="BKMK_NET"></a>Instalar o .NET Framework 4.6 ou 4.5  
Para habilitar o self\-atualização de modo e certos recursos do CAU no remoto\-atualizar modo, .NET Framework 4.6 ou .NET Framework 4.5 (no Windows Server 2012 R2) deve ser instalado em todos os nós de cluster. Por padrão, o .NET Framework está instalado.  

Para instalar o .NET Framework 4.6 (ou 4.5) usando o PowerShell se ele ainda não estiver instalado, use o seguinte comando:

```PowerShell
Install-WindowsFeature -Name NET-Framework-45-Core
```

## <a name="BKMK_BEST_PRAC"></a>Práticas recomendadas para usar a atualização com suporte a Cluster 

### <a name="BKMK_BP_WUA"></a>Recomendações para aplicar atualizações da Microsoft

É recomendável que, quando você começar a usar o CAU para aplicar atualizações com o padrão **windowsupdateplugin** conecte\-em um cluster, você parar de usar outros métodos para instalar as atualizações de software da Microsoft no cluster nós.  

> [!CAUTION]  
> Combinar o CAU com métodos que atualizam nós individuais automaticamente \(em um horário fixo\) pode causar resultados imprevisíveis, incluindo interrupções no serviço e o tempo de inatividade não planejado.  

Convém seguir estas diretrizes:  

-   Para melhores resultados, recomendamos desabilitar as configurações nos nós do cluster para atualização automática, por exemplo, por meio das configurações de Atualizações automáticas no Painel de controle, ou nas configurações definidas usando a Política de Grupo.  

    > [!CAUTION]  
    > A instalação automática de atualizações nos nós de cluster pode interferir na instalação de atualizações pelo CAU e causar falhas no CAU.  

    Se elas forem necessárias, as seguintes Atualizações automáticas são compatíveis com CAU, porque o administrador pode controlar o sincronismo da instalação da atualização:  

    -   Configurações para notificar antes do download das atualizações e para notificar antes da instalação  

    -   Configurações para baixar as atualizações automaticamente e notificar antes da instalação  

    No entanto, se as Atualizações Automáticas estiverem baixando atualizações ao mesmo tempo em que é executada uma atualização do CAU, a execução da atualização poderá levar mais tempo para ser concluída.  

-   Não configure um sistema de atualização, como o Windows Server Update Services \(WSUS\) para aplicar atualizações automaticamente \(em um horário fixo\) a nós do cluster.  

-   Todos os nós de cluster devem ser configurados uniformemente para usar a mesma fonte de atualização, por exemplo, um servidor WSUS, Windows Update ou Microsoft Update.  

-   Se você usar um sistema de gerenciamento de configurações para aplicar atualizações de software a computadores na rede, exclua os nós do cluster de todas as atualizações necessárias ou automáticas. Exemplos de sistemas de gerenciamento de configurações incluem o Microsoft System Center Configuration Manager 2007 e o Microsoft System Center Virtual Machine Manager 2008.  

-   Se os servidores de distribuição de software interno \(por exemplo, servidores do WSUS\) são usados para conter e implantar as atualizações, certifique-se de que esses servidores identificam corretamente as atualizações aprovadas para os nós de cluster.  

#### <a name="BKMK_PROXY"></a>Aplicar atualizações da Microsoft em cenários de filiais  
Para baixar atualizações da Microsoft do Microsoft Update ou Windows Update para nós de cluster em determinados cenários de filiais, pode ser necessário configurar definições de proxy para a conta de Sistema Local em cada nó. Por exemplo, isso pode ser necessário se os clusters da filial acessarem o Microsoft Update ou o Windows Update para baixar atualizações usando um servidor de proxy local.  

Se necessário, configure as configurações de proxy do WinHTTP em cada nó para especificar um servidor proxy local e configurar exceções de endereço local \(ou seja, uma lista de bypass para endereços locais\). Para isso, você pode executar o seguinte comando em cada nó de cluster em um prompt de comandos com privilégios elevados:  

```  
netsh winhttp set proxy <ProxyServerFQDN >:<port> "<local>"  
```  

onde <*ProxyServerFQDN*> é o nome de domínio totalmente qualificado para o servidor proxy e <*porta*> é a porta pela qual se comunicar (geralmente a porta 443).  

Por exemplo, para definir configurações de proxy WinHTTP para a conta Sistema Local especificando o servidor proxy *MyProxy.CONTOSO.com*, com a porta 443 e exceções de endereço local, digite o seguinte comando:  

```  
netsh winhttp set proxy MyProxy.CONTOSO.com:443 "<local>"  
```  

### <a name="BKMK_BP_HF"></a>Recomendações para usar o Microsoft. hotfixplugin  

-   Recomendamos configurar permissões na pasta raiz de hotfix e o arquivo de configuração de hotfix para restringir o acesso de Gravação aos administradores locais somente nos computadores que são usados para armazenar esses arquivos. Isso ajuda a evitar possíveis violações desses arquivos por usuários não autorizados que poderiam comprometer a funcionalidade do cluster de failover quando os hotfixes forem aplicados.  

-   Para ajudar a garantir a integridade dos dados para o bloco de mensagens do servidor \(SMB\) conexões que são usadas para acessar a pasta raiz de hotfix, você deve configurar a criptografia SMB na pasta compartilhada de SMB, se for possível configurá-lo. O **Microsoft.HotfixPlugin** requer que a assinatura SMB ou Criptografia SMB seja configurada para ajudar a garantir a integridade dos dados para as conexões SMB. 

    Para obter mais informações, consulte [restringir o acesso para a pasta raiz de hotfix e o arquivo de configuração de hotfix](cluster-aware-updating-plug-ins.md#BKMK_ACL).

### <a name="additional-recommendations"></a>Recomendações adicionais  

-   Para evitar interferir em uma Execução de atualização CAU que pode estar agendada ao mesmo tempo, não agende alterações de senha para objetos de nome de cluster durante as janelas de manutenção agendadas.  

-   Você deve definir as permissões apropriadas no pré\-atualizar e lançar\-scripts de atualização que são salvos na rede compartilhada pastas para evitar possíveis violações desses arquivos por usuários não autorizados.  

-   Para configurar o CAU no self\-atualizando modo, um objeto de computador virtual \(VCO\) para CAU função clusterizada deve ser criada no Active Directory. O CAU pode criar esse objeto automaticamente no momento em que a função clusterizada do CAU for adicionada e o cluster de failover tiver permissões suficientes. Entretanto, por causa das políticas de segurança em certas organizações, pode ser necessário pré-configurar o objeto no Active Directory. Para obter um procedimento para isso, consulte [Etapas para pré-preparar a conta para uma função em cluster](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/cc731002\(v=ws.10\)#steps-for-prestaging-the-cluster-name-account).  

-   Para salvar e reutilizar as configurações de Executar atualização nos clusters de failover com necessidades de atualização similar na organização de TI, você pode criar Perfis de Execução de Atualização. Além disso, dependendo do modo de atualização, você pode salvar e gerenciar os Perfis de Execução de Atualização em um compartilhamento de arquivo que está acessível a todos os computadores do Coordenador de Atualização remoto ou clusters de failover. Para obter mais informações, consulte [opções avançadas e perfis de executar atualização da CAU](cluster-aware-updating-options.md).  

## <a name="BKMK_BPA"></a>Preparação de atualização de cluster de teste  
Você pode executar o analisador de práticas recomendadas do CAU \(BPA\) modelo para testar se um cluster de failover e o ambiente de rede atenderam aos diversos requisitos para que as atualizações de software aplicadas pelo CAU. Vários testes verificam a preparação aplicar as atualizações da Microsoft, usando o plugue padrão do ambiente\-no, **windowsupdateplugin**.  

> [!NOTE]  
> Talvez você precise verificar independentemente se o seu ambiente de cluster está pronto para aplicar atualizações de software usando um plug\-em que **windowsupdateplugin**. Se você estiver usando um não\-plug Microsoft\-, por exemplo, um fornecido pelo fabricante do hardware, entre em contato com o publicador para obter mais informações.  

É possível executar o BPA nas duas seguintes maneiras:  

1.  Selecione **Analisar preparação da atualização do cluster** no console de CAU. Depois que o BPA ter concluído os testes de preparação, um relatório de teste é exibido. Se forem detectados problemas nos nós de cluster, os problemas específicos e os nós nos quais os problemas aparecem são identificados, para que você possa executar a ação corretiva. Os testes podem levar vários minutos para serem concluídos.  

2.  Execute o [Test-CauSetup](https://docs.microsoft.com/powershell/module/clusterawareupdating/Test-CauSetup) cmdlet. Você pode executar o cmdlet em um computador local ou remoto no qual o Failover Clustering de módulo para Windows PowerShell (parte das ferramentas de Clustering de Failover) está instalado. Também é possível executar o cmdlet em um nó do cluster de failover.  

> [!NOTE]  
> -   Você deve usar uma conta que tenha privilégios administrativos em nós de cluster e privilégios administrativos locais no computador que é usado para executar o **teste\-CauSetup** cmdlet ou ao analisar a preparação de atualização do cluster usando a janela de atualização com suporte a Cluster. Para executar os testes usando a janela de atualização com suporte a Cluster, você deve estar conectado ao computador com as credenciais necessárias.  
> -   Os testes assumem que as ferramentas do CAU usadas para visualizar e aplicar as atualizações de software são executadas a partir do mesmo computador e com as mesmas credenciais de usuário que as usadas para testar a preparação de atualização do cluster.  

> [!IMPORTANT]  
> É altamente recomendável que você teste o cluster quanto à preparação para atualização nas seguintes situações:  
>   
> -   Antes de usar o CAU pela primeira vez para aplicar atualizações de software.  
> -   Após adicionar um nó ao cluster ou executar outras alterações de hardware no cluster, que requerem a execução do Assistente de Validação de Cluster.  
> -   Depois que você alterar uma origem de atualização, ou alterar configurações ou configurações de atualização \(diferentes de CAU\) que podem afetar a aplicação das atualizações em nós.  

### <a name="tests-for-cluster-updating-readiness"></a>Testes para preparação de atualização do cluster  
A tabela a seguir lista os testes de preparação de atualização do cluster, alguns problemas comuns e etapas de resolução.  


|                                                      Testar                                                      |                                                                                                                                               Possíveis problemas e impactos                                                                                                                                               |                                                                                                                                                                                         Etapas de resolução                                                                                                                                                                                         |
|----------------------------------------------------------------------------------------------------------------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
|                                     O cluster de failover deve estar disponível                                     |                                                                                       Não é possível resolver o nome do cluster de failover, ou um ou mais nós de cluster não podem ser acessados. O BPA não pode executar testes de preparação do cluster.                                                                                        |                                                                   -Verifique a ortografia do nome do cluster especificado durante a execução de BPA.<br />-Certifique-se de que todos os nós do cluster estão online e em execução.<br />-Verifique se o validar um Assistente de configuração podem executar com êxito no cluster de failover.                                                                    |
|                    Os nós de cluster de failover devem estar habilitados para gerenciamento remoto via WMI                    |                                                Um ou mais nós de cluster de failover não estão habilitados para o gerenciamento remoto usando o Windows Management Instrumentation \(WMI\). O CAU não pode atualizar os nós de cluster, se os nós não estiverem configurados para o gerenciamento remoto.                                                 |                                                                                                  Certifique-se de que todos os nós de cluster de failover estejam habilitados para o gerenciamento remoto por meio de WMI. Para mais informações, consulte [Configurar os nós para o gerenciamento remoto](#BKMK_NODE_CONFIG) neste tópico.                                                                                                   |
|                      Comunicação remota do PowerShell deve ser habilitada em cada nó de cluster de failover                       |                                                           PowerShell não está instalado ou não está habilitada para uso remoto em um ou mais nós de cluster de failover. A CAU não pode ser configurado para self\-modo de atualização ou usar determinados recursos no remoto\-modo de atualização.                                                            |                                                                                             Certifique-se de que o PowerShell é instalado em todos os nós de cluster e habilitado remotamente.<br /><br />Para mais informações, consulte [Configurar os nós para o gerenciamento remoto](#BKMK_NODE_CONFIG) neste tópico.                                                                                             |
|                                            Versão de cluster de failover                                            |                                                                            Um ou mais nós no cluster de failover não executam o Windows Server 2016, Windows Server 2012 R2 ou Windows Server 2012. O CAU não pode atualizar o cluster de failover.                                                                             |                                                                   Verifique se o cluster de failover que é especificado durante a execução de BPA está executando o Windows Server 2016, Windows Server 2012 R2 ou Windows Server 2012.<br /><br />Para obter mais informações, consulte [Verificar a configuração do cluster](#BKMK_REQ_CLUS) neste tópico.                                                                   |
| As versões necessárias do .NET Framework e do Windows PowerShell devem ser instaladas em todos os nós de cluster de failover |                                                                                              .NET framework 4.6, 4.5 ou Windows PowerShell não está instalado em um ou mais nós do cluster. Alguns recursos de CAU podem não funcionar.                                                                                              |                                                                            Certifique-se de que o .NET Framework 4.6 ou 4.5 e Windows PowerShell são instalados em todos os nós de cluster, se forem necessários.<br /><br />Para mais informações, consulte [Configurar os nós para o gerenciamento remoto](#BKMK_NODE_CONFIG) neste tópico.                                                                             |
|                           O serviço de cluster deve estar em execução em todos os nós de cluster                           |                                                                                                            O serviço de cluster não está sendo executado em um ou mais nós. O CAU não pode atualizar o cluster de failover.                                                                                                             |                        -Certifique-se de que o serviço de Cluster \(clussvc\) é iniciado em todos os nós no cluster, e ele está configurado para iniciar automaticamente.<br />-Verifique se o validar um Assistente de configuração podem executar com êxito no cluster de failover.<br /><br />Para obter mais informações, consulte [Verificar a configuração do cluster](#BKMK_REQ_CLUS) neste tópico.                         |
|     As atualizações automáticas não devem estar configuradas para instalar automaticamente as atualizações em nenhum nó de cluster de failover     |                                           As atualizações automáticas devem estar configuradas em, no mínimo, um nó de cluster de failover para instalar automaticamente as atualizações da Microsoft nesse nó. Combinar o CAU com outros métodos de atualização pode resultar em tempo de inatividade não planejado ou resultados imprevisíveis.                                            |                                                     Se a funcionalidade do Windows Update estiver configurada para atualizações automáticas em um ou mais nós de cluster, certifique-se de que as atualizações automáticas não estejam configuradas para instalar as atualizações automaticamente.<br /><br />Para obter mais informações, consulte [Recomendações para aplicar atualizações da Microsoft](#BKMK_BP_WUA).                                                     |
|                          Os nós de cluster de failover devem usar a mesma origem de atualização                          |                                                    Um ou mais nós de cluster de failover estão configurados para usar uma origem de atualização para as atualizações da Microsoft que é diferente do restante dos nós. As atualizações podem não ser aplicadas de maneira uniforme nos nós de cluster pelo CAU.                                                    |                                                                        Certifique-se de que todos os nós de cluster estejam configurados para usar a mesma origem de atualização, por exemplo, um servidor WSUS, Windows Update ou Microsoft Update.<br /><br />Para obter mais informações, consulte [Recomendações para aplicar atualizações da Microsoft](#BKMK_BP_WUA).                                                                         |
|       Uma regra de firewall que permite o encerramento remoto deve ser habilitada em todos os nós no cluster de failover       |                 Um ou mais nós de cluster de failover não possui uma regra de firewall habilitada que permita o encerramento remoto ou uma configuração de Política de Grupo que previna que essa regra seja habilitada. Uma Execução de Atualização que aplica atualizações que requerem a reinicialização automática dos nós pode não ser concluída de maneira adequada.                  |                                                                    Se o Firewall do Windows ou um não\-Microsoft firewall está em uso em nós de cluster, configure uma regra de firewall que permita o desligamento remoto.<br /><br />Para obter mais informações, consulte [Habilitar uma função de firewall para permitir reinícios automáticos](#BKMK_FW) neste tópico.                                                                    |
|          A configuração de servidor de proxy em cada nó de cluster de failover deve ser definida em um servidor proxy local          |                             Um ou mais nós de cluster de failover têm uma configuração incorreta de servidor proxy.<br /><br />Se um servidor proxy local estiver em uso, a configuração de servidor proxy em cada nó deve ser configurada corretamente para o cluster acessar o Microsoft Update ou o Windows Update.                              |                                            Certifique-se de que as configurações de proxy do WinHTTP em cada nó de cluster estejam definidas em um servidor proxy local, caso necessário. Se um servidor proxy não estiver em uso em seu ambiente, esse aviso pode ser ignorado.<br /><br />Para mais informações, consulte [Aplicar atualizações em cenários de filiais](#BKMK_PROXY) neste tópico.                                            |
|        A função clusterizada de CAU deve ser instalada no cluster de failover para habilitar o self\-modo de atualização        |                                                                                                   A função clusterizada de CAU não está instalada nesse cluster de failover. Essa função é necessária para o cluster self\-atualizando.                                                                                                   |      Para usar o CAU no próprio\-atualizando modo, adicione a função clusterizada CAU no cluster de failover em uma das seguintes maneiras:<br /><br />-Execute o [Add-CauClusterRole](https://docs.microsoft.com/powershell/module/clusterawareupdating/Add-CauClusterRole) cmdlet do PowerShell.<br />-Selecione a **configurar cluster self\-opções de atualização** ação na janela de atualização com suporte a Cluster.      |
|         A função clusterizada de CAU deve ser habilitada no cluster de failover para habilitar o self\-modo de atualização         | A função clusterizada de CAU é desabilitada. Por exemplo, a função clusterizada de CAU não está instalada ou foi desabilitada usando o [desabilite\-CauClusterRole](https://docs.microsoft.com/powershell/module/clusterawareupdating/Disable-CauClusterRole) cmdlet do PowerShell. Essa função é necessária para o cluster self\-atualizando. | Para usar o CAU no próprio\-atualizando modo, habilitar a função clusterizada de CAU nesse cluster de failover de uma das seguintes maneiras:<br /><br />-Execute o [Enable-CauClusterRole](https://docs.microsoft.com/powershell/module/clusterawareupdating/Enable-CauClusterRole) cmdlet do PowerShell.<br />-Selecione a **configurar cluster self\-opções de atualização** ação na janela de atualização com suporte a Cluster. |
|      O plugue CAU configurado\-para si mesmo\-modo de atualização deve ser registrado em todos os nós de cluster de failover      |                                                              A função clusterizada de CAU em um ou mais nós desse cluster de failover não é possível acessar o plugue CAU\-no módulo que está configurado no self\-opções de atualização. Um self\-executar a atualização poderá falhar.                                                              |           -Certifique-se de que o plugue CAU configurado\-in estiver instalado em todos os nós de cluster, seguindo o procedimento de instalação para o produto que fornece o plugue CAU\-no.<br />-Execute o [registre\-CauPlugin](https://docs.microsoft.com/powershell/module/clusterawareupdating/Register-CauPlugin) cmdlet do PowerShell para registrar o plug\-em nós de cluster necessários.           |
|                Todos os nós de cluster de failover devem ter o mesmo conjunto de plugue CAU registrado\-ins                 |                                                                             Um self\-executar a atualização poderá falhar se o plugue\-no que é configurado para ser usado em uma execução de atualização é alterado para um que não está disponível em todos os nós de cluster.                                                                              |           -Certifique-se de que o plugue CAU configurado\-in estiver instalado em todos os nós de cluster, seguindo o procedimento de instalação para o produto que fornece o plugue CAU\-no.<br />-Execute o [registre\-CauPlugin](https://docs.microsoft.com/powershell/module/clusterawareupdating/Register-CauPlugin) cmdlet do PowerShell para registrar o plug\-em nós de cluster necessários.           |
|                               As opções configuradas de Execução de Atualização devem ser válidas                                |                                                                          O self\-atualização de agendamento e execução de atualização opções que estão configuradas para esse cluster de failover estão incompletas ou não são válidas. Um self\-executar a atualização poderá falhar.                                                                           |                                                            Configurar um válido\-atualizando a agenda e o conjunto de opções de execução de atualização. Por exemplo, você pode usar o [definir\-CauClusterRole](https://docs.microsoft.com/powershell/module/clusterawareupdating/Set-CauClusterRole) função clusterizada do cmdlet do PowerShell para configurar o CAU.                                                            |
|                  Pelo menos dois nós de cluster de failover devem ser proprietários da função clusterizada de CAU                  |                                                                                        Uma execução de atualização iniciado no autoatendimento\-modo de atualização falhará porque a função clusterizada de CAU não tem um nó de possível proprietário para mover para.                                                                                         |                                                                                                                Use as Ferramentas de cluster de failover para garantir que todos os nós de cluster sejam configurados como possíveis proprietários da função clusterizada de CAU. Essa é a configuração padrão.                                                                                                                |
|                  Todos os nós de cluster de failover devem poder acessar os scripts do Windows PowerShell                  |                                                                        Nem todos os possíveis nós do proprietário da função clusterizada CAU podem acessar o pre do Windows PowerShell configurado\-atualizar e lançar\-scripts de atualização. Um self\-executar a atualização falhará.                                                                        |                                                                                                                    Certifique-se de que todos os possíveis nós do proprietário da função clusterizada CAU tem permissões para acessar o pré PowerShell configurado\-atualizar e lançar\-scripts de atualização.                                                                                                                     |
|                   Todos os nós de cluster de failover devem usar scripts de Windows PowerShell idênticos                   |                                                     Nem todos os possíveis nós do proprietário da função clusterizada CAU usam a mesma cópia do pré especificado do Windows PowerShell\-atualizar e lançar\-scripts de atualização. Um self\-executar a atualização pode falhar ou apresentar um comportamento inesperado.                                                     |                                                                                                                                   Certifique-se de que todos os possíveis nós do proprietário da função clusterizada CAU usam a mesma pré PowerShell\-atualizar e lançar\-scripts de atualização.                                                                                                                                   |
|         A configuração de WarnAfter especificada para a Execução de Atualização deve ser inferior à configuração de StopAfter         |                                                                           Os valores de tempo limite Execução de Atualização de CAU fazem com que o aviso de tempo limite não seja efetivo. Uma Execução de Atualização pode ser cancelada antes que um log de evento de avisos possa ser gerado.                                                                            |                                                                                                                                      Nas opções de Execução de Atualização, configure um valor de opção **WarnAfter** que seja inferior ao valor de opção **StopAfter** .                                                                                                                                       |

## <a name="see-also"></a>Consulte também  

-   [Visão geral da atualização com suporte a cluster](cluster-aware-updating.md)