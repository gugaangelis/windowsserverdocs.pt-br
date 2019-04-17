---
ms.assetid: 75cc1d24-fa2f-45bd-8f3b-1bbd4a1aead0
title: "Suporte a cluster atualizando requisitos e as práticas recomendadas"
ms.prod: windows-server-threshold
ms.topic: article
ms.manager: dongill
author: JasonGerend
ms.author: jgerend
ms.technology: storage-failover-clustering
ms.date: 4/28/2017
description: "Requisitos para usar o suporte a Cluster atualizando para instalar atualizações no clusters executando o Windows Server."
ms.openlocfilehash: 8517466d58345077af446c1b2c1e2aeb3b17aaa1
ms.sourcegitcommit: 583355400f6b0d880dc0ac6bc06f0efb50d674f7
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 10/17/2017
---
# <a name="cluster-aware-updating-requirements-and-best-practices"></a>Suporte a cluster atualizando requisitos e as práticas recomendadas

>Aplica-se a: Windows Server (anual por canal), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Esta seção descreve os requisitos e as dependências que são necessárias para usar [atualizando suporte a Cluster](cluster-aware-updating.md) (CAU) para aplicar atualizações a um cluster de failover executando o Windows Server.
  
> [!NOTE]  
> Talvez você precise validar independente que seu ambiente de cluster está pronto para aplicar atualizações, se você usar um plug\-in diferente **Microsoft.WindowsUpdatePlugin**. Se você estiver usando um Microsoft non\ plug\-in, contate o fornecedor para obter mais informações. Para saber mais sobre plug\-ins, consulte [funcionamento Plug\-ins](cluster-aware-updating-plug-ins.md).   
  
## <a name="BKMK_REQ_CLUS"></a>Instalar o recurso de cluster de Failover e as ferramentas de cluster de Failover  
CAU requer uma instalação do recurso cluster de Failover e as ferramentas de cluster de Failover. As ferramentas de cluster de Failover incluem o CAU ferramentas \(clusterawareupdating.dll\), os cmdlets de cluster de Failover e outros componentes necessários para operações de CAU. Para obter etapas instalar o recurso de cluster de Failover, consulte [instalar as ferramentas e o recurso de cluster de Failover](http://go.microsoft.com/fwlink/p/?LinkId=253342).  
  
Os requisitos de instalação exato para as ferramentas de cluster de Failover dependem se CAU organize atualizações como uma função clusterizada no cluster de failover \ (usando modo \ atualizando self\) ou de um computador remoto. Além disso, o modo de atualização self\ de CAU requer a instalação da função clusterizada CAU no cluster de failover usando as ferramentas CAU.    
  
A tabela a seguir resume os requisitos de instalação do recurso CAU para os dois modos de atualização CAU.  
  
|Componente instalado|Modo de atualização Self\|Modo de atualização Remote\|  
|-----------------------|-----------------------|-------------------------|  
|Recurso de cluster de failover|Necessário em todos os nós de cluster|Necessário em todos os nós de cluster|  
|Ferramentas de cluster de failover|Necessário em todos os nós de cluster|-Necessários no computador atualizando remote\<br />-Necessários em todos os nós de cluster para executar o [salvar CauDebugTrace](https://technet.microsoft.com/itpro/powershell/windows/cluster-aware-updating/save-caudebugtrace) cmdlet|  
|Função clusterizada CAU|Necessário|Não é necessária|  
  
## <a name="obtain-an-administrator-account"></a>Obter uma conta de administrador  
Os seguintes requisitos de administrador são necessários para usar recursos CAU.  
  
-   Para visualizar ou aplicar ações de atualização usando a interface do usuário CAU \(UI\) ou os cmdlets atualizando suporte a Cluster, você deve usar uma conta de domínio que tenha direitos de administrador local e as permissões em todos os nós de cluster. Se a conta não tiver privilégios suficientes em cada nó, você precisará na janela do suporte a Cluster atualizando fornecer as credenciais necessárias quando você executar essas ações. Para usar o [atualizando suporte a Cluster](https://technet.microsoft.com/itpro/powershell/windows/cluster-aware-updating) cmdlets, você pode fornecer as credenciais necessárias como um parâmetro de cmdlet.  
  
-   Se você usar CAU no modo de atualização remote\ quando você estiver conectado com uma conta que não tem direitos de administrador local e permissões em nós de cluster, você deve executar as ferramentas CAU como administrador usando uma conta de administrador local no computador de coordenador de atualização ou usando uma conta que tenha o **representar um cliente após autenticação** direito de usuário. 
  
-   Para executar o analisador de práticas recomendadas CAU, você deve usar uma conta que tenha privilégios administrativos em nós de cluster e privilégios administrativos locais no computador em que é usada para executar o [teste CauSetup](https://technet.microsoft.com/itpro/powershell/windows/cluster-aware-updating/test-causetup) cmdlet ou para analisar o cluster usando a janela atualizar suporte a Cluster preparação para a atualização. Para obter mais informações, consulte [preparação para a atualização de cluster de teste](#BKMK_BPA).  
  
## <a name="verify-the-cluster-configuration"></a>Verifique se a configuração de cluster  
A seguir estão os requisitos gerais para um cluster de failover dar suporte a atualizações usando CAU. Requisitos de configuração adicionais para o gerenciamento remoto em nós estão listados em [configurar os nós de gerenciamento remoto](#BKMK_NODE_CONFIG) posteriormente neste tópico.  
  
-   Nós de cluster suficientes devem estar online para que ele tiver quorum.  
  
-   Todos os nós de cluster devem estar no mesmo domínio do Active Directory.  
  
-   O nome do cluster deve ser resolvido na rede usando DNS.  
  
-   Se CAU é usado no modo de atualização remote\, o computador de coordenador de atualização deve ter conectividade de rede para os nós de cluster de failover, e ele deve estar no mesmo domínio do Active Directory do cluster de failover.  
  
-   O serviço de Cluster deve ser executado em todos os nós de cluster. Por padrão, esse serviço é instalado em todos os nós de cluster e está configurado para iniciar automaticamente.  
  
-   Para usar scripts do PowerShell pre\ atualização ou atualização post\ durante uma CAU atualizando executar, certifique-se de que os scripts estão instalados em todos os nós de cluster ou que estão acessíveis a todos os nós, por exemplo, em um compartilhamento de arquivo altamente disponíveis na rede. Se scripts são salvos em um compartilhamento de arquivos de rede, configure a pasta de permissão de leitura para todos grupo.  
  
## <a name="BKMK_NODE_CONFIG"></a>Configurar os nós para o gerenciamento remoto  
Para usar o suporte a Cluster atualizando, todos os nós de cluster devem ser configurados para o gerenciamento remoto. Por padrão, a única tarefa que você deve executar para configurar os nós para gerenciamento remoto é [habilitar uma regra de firewall para permitir reinicializações automáticas](#BKMK_FW). 

A tabela a seguir lista os requisitos de gerenciamento remoto completo, no caso de seu ambiente é divergente dos padrões.

Esses requisitos são além dos requisitos de instalação para o [instalar o recurso de cluster de Failover e as ferramentas de cluster de Failover](#BKMK_REQ_CLUS) e geral clustering requisitos descritos nas seções anteriores neste tópico.  
  
|Requisito|Estado padrão|Modo de atualização Self\|Modo de atualização Remote\|  
|---------------|---|-----------------------|-------------------------|  
|[Habilitar uma regra de firewall para permitir reinicializações automáticas](#BKMK_FW)|Desabilitado|Necessário em todos os nós de cluster se um firewall está em uso|Necessário em todos os nós de cluster se um firewall está em uso|  
|[Habilitar a instrumentação de gerenciamento do Windows](#BKMK_WMI)|Habilitado|Necessário em todos os nós de cluster|Necessário em todos os nós de cluster|  
|[Permitir que o Windows PowerShell 3.0 ou 4.0 e Windows PowerShell remoto](#BKMK_PS)|Habilitado|Necessário em todos os nós de cluster|Necessário em todos os nós de cluster para executar o seguinte:<br /><br />-Os [salvar CauDebugTrace](https://technet.microsoft.com/itpro/powershell/windows/cluster-aware-updating/save-caudebugtrace) cmdlet<br />-Scripts de PowerShell pre\ update e update post\ durante uma atualização executar<br />-Testes de cluster usando a janela atualizar suporte a Cluster preparação para a atualização ou o [Test\ CauSetup](http://go.microsoft.com/fwlink/p/?LinkId=242384) cmdlet do Windows PowerShell|  
|[Instalar o .NET Framework 4.6 ou 4.5](#BKMK_NET)|Habilitado|Necessário em todos os nós de cluster|Necessário em todos os nós de cluster para executar o seguinte:<br /><br />-Os [salvar CauDebugTrace](https://technet.microsoft.com/itpro/powershell/windows/cluster-aware-updating/save-caudebugtrace) cmdlet<br />-Scripts de PowerShell pre\ update e update post\ durante uma atualização executar<br />-Testes de cluster usando a janela atualizar suporte a Cluster preparação para a atualização ou o [Test\ CauSetup](http://go.microsoft.com/fwlink/p/?LinkId=242384) cmdlet do Windows PowerShell|  

### <a name="BKMK_FW"></a>Habilitar uma regra de firewall para permitir reinicializações automáticas  
Para permitir reinicializações automáticas após as atualizações são aplicadas \ (se a instalação de uma atualização requer um restart\), se o Firewall do Windows ou um firewall non\ da Microsoft está em uso em nós de cluster, uma regra de firewall deve ser habilitada em cada nó que permite que o tráfego a seguir:  
  
-   Protocolo: TCP  
  
-   Direção: entrada  
  
-   Programa: wininit.exe  
  
-   Portas: Portas de dinâmicas de RPC  
  
-   Perfil: domínio  
  
Se o Firewall do Windows é usado em nós de cluster, você pode fazer isso habilitando o **desligamento remoto** grupo de regra de Firewall do Windows em cada nó de cluster. Quando você usa a janela atualizar suporte a Cluster para aplicar atualizações e para configurar as opções de atualização self\, o **desligamento remoto** grupo de regra de Firewall do Windows é habilitado automaticamente em cada nó de cluster.  
  
> [!NOTE]  
> O **desligamento remoto** grupo de regra de Firewall do Windows não pode ser habilitado quando ele entra em conflito com as configurações de política de grupo que estão configuradas para o Firewall do Windows.    
  
O **desligamento remoto** grupo de regra de firewall também estiver habilitado, especificando o **– EnableFirewallRules** parâmetro quando executando os seguintes cmdlets CAU: [adicionar CauClusterRole](https://technet.microsoft.com/itpro/powershell/windows/cluster-aware-updating/add-cauclusterrole), [Invoke CauRun](https://technet.microsoft.com/itpro/powershell/windows/cluster-aware-updating/invoke-causcan), e [SetCauClusterRole](https://technet.microsoft.com/itpro/powershell/windows/cluster-aware-updating/set-cauclusterrole).  
  
O exemplo de PowerShell a seguir mostra um método adicional para habilitar reinicializações automáticas em um nó de cluster.  
  
```PowerShell  
Set-NetFirewallRule -Group "@firewallapi.dll,-36751" -Profile Domain -Enabled true  
```  

### <a name="BKMK_WMI"></a>Habilitar o Windows Management Instrumentation (WMI) 
Todos os nós de cluster devem ser configurados para o gerenciamento remoto usando o Windows Management Instrumentation \(WMI\). Isso é habilitado por padrão.  
  
Para ativar manualmente o gerenciamento remoto, faça o seguinte:  
  
1.  No console Serviços, inicie o **gerenciamento remoto do Windows** serviço e defina o tipo de inicialização como **automática**.  
  
2.  Execute o [conjunto WSManQuickConfig](https://msdn.microsoft.com/powershell/reference/5.1/microsoft.wsman.management/set-wsmanquickconfig) solicitar cmdlet ou executar o comando a seguir de um comando com privilégios elevados:  
  
    ```PowerShell  
    winrm quickconfig -q  
    ```  
  
Para suportar remoting WMI, se o Firewall do Windows está em uso em nós de cluster, o firewall de entrada de regras para **gerenciamento remoto do Windows \(HTTP\-In\)** deve estar habilitado em cada nó.  Por padrão, essa regra está ativada.  
  
### <a name="BKMK_PS"></a>Habilitar o Windows PowerShell e o Windows PowerShell remoto  
Para habilitar o modo de atualização self\ e determinados recursos CAU no modo de atualização remote\, PowerShell deve ser instalado e habilitado a executar comandos remotos em todos os nós de cluster. Por padrão, o PowerShell é instalado e habilitado para remoto.  
  
Para habilitar a conexão remota do PowerShell, use um dos seguintes métodos:  
  
-   Execute o [habilitar PSRemoting](https://msdn.microsoft.com/powershell/reference/5.1/microsoft.powershell.core/enable-psremoting) cmdlet.  
  
-   Defina uma configuração de política de grupo do nível de domain\ para \(WinRM\) gerenciamento remoto do Windows.  
  
Para obter mais informações sobre como habilitar a conexão remota do PowerShell, consulte [about\_Remote\_Requirements](https://msdn.microsoft.com/powershell/reference/5.1/microsoft.powershell.core/about/about_remote_requirements).  
  
### <a name="BKMK_NET"></a>Instalar o .NET Framework 4.6 ou 4.5  
Para habilitar o modo de atualização self\ e determinados recursos CAU no modo de atualização remote\, .NET Framework 4.6 ou .NET Framework 4.5 (no Windows Server 2012 R2) deve ser instalado em todos os nós de cluster. Por padrão, o .NET Framework é instalado.  

Para instalar o .NET Framework 4.6 (ou 4.5) usando o PowerShell se ele ainda não estiver instalado, use o seguinte comando:

```PowerShell
Install-WindowsFeature -Name NET-Framework-45-Core
```

## <a name="BKMK_BEST_PRAC"></a>Recomendações para usar o suporte a Cluster atualizando de práticas recomendadas 
  
### <a name="BKMK_BP_WUA"></a>Recomendações para aplicar atualizações da Microsoft

Nós recomendamos que, quando você começar a usar CAU para aplicar atualizações com o padrão **Microsoft.WindowsUpdatePlugin** plug\-in em um cluster, você parar de usar outros métodos para instalar as atualizações de software da Microsoft em nós de cluster.  
  
> [!CAUTION]  
> Combinando CAU com os métodos que atualizam automaticamente nós individuais \ (em um schedule\ de tempo fixo) podem causar resultados imprevisíveis, inclusive interrupções no serviço e tempo de inatividade planejado.  
  
Nós recomendamos que você siga estas diretrizes:  
  
-   Para obter melhores resultados, recomendamos que você desative configurações em nós de cluster de atualização automática, por exemplo, por meio das configurações de atualizações automáticas no painel de controle ou nas configurações que estão configuradas usando a política de grupo.  
  
    > [!CAUTION]  
    > Instalação automática de atualizações em nós de cluster pode interferir com a instalação das atualizações por CAU e pode causar falhas CAU.  
  
    Se eles forem necessárias, as seguintes configurações de atualizações automáticas são compatíveis com CAU, porque o administrador pode controlar o tempo de instalação da atualização:  
  
    -   Configurações para notificá-lo antes de baixar atualizações e notificar antes da instalação  
  
    -   Configurações para baixar atualizações automaticamente e notificar antes da instalação  
  
    No entanto, se as atualizações automáticas estiver baixando atualizações ao mesmo tempo como um CAU atualizando executar, execute a atualização pode levar mais tempo para concluir.  
  
-   Não definir um sistema de atualização, como Windows Server Update Services \(WSUS\) para aplicar atualizações automaticamente \ (em um schedule\ de tempo fixo) para nós de cluster.  
  
-   Todos os nós de cluster devem ser configurados de maneira uniforme para usar a mesma fonte de atualização, por exemplo, um WSUS server, Windows Update ou o Microsoft Update.  
  
-   Se você usar um sistema de gerenciamento de configuração para aplicar atualizações de software para computadores na rede, exclua nós de cluster de todas as atualizações necessárias ou automáticas. Exemplos de sistemas de gerenciamento de configuração incluem o Microsoft System Center Configuration Manager 2007 e Microsoft System Center Virtual Machine Manager 2008.  
  
-   Se os servidores de distribuição de software internos \ (por exemplo, o WSUS servers\) são usados para conter e implantar as atualizações, certifique-se de que os servidores corretamente identificam as atualizações aprovadas para os nós de cluster.  
  
#### <a name="BKMK_PROXY"></a>Aplicar atualizações da Microsoft em cenários de filial  
Para baixar atualizações do Microsoft pelo Microsoft Update ou o Windows Update para nós de cluster em determinados cenários de filial, talvez seja necessário definir as configurações de proxy para a conta Sistema Local em cada nó. Por exemplo, talvez seja necessário fazer isso se seu clusters de office ramificação acessem o Microsoft Update ou o Windows Update para baixar atualizações usando um servidor proxy local.  
  
Se necessário, definir as configurações de proxy de WinHTTP em cada nó para especificar um servidor proxy local e configurar exceções de endereço local \ (ou seja, uma lista de proxies para addresses\ local). Para fazer isso, você pode executar o comando a seguir em cada nó de cluster em um prompt de comando com privilégios elevados:  
  
```  
netsh winhttp set proxy <ProxyServerFQDN >:<port> "<local>"  
```  
  
Onde <*ProxyServerFQDN*> é o nome de domínio totalmente qualificado para o servidor proxy e <*porta*> é a porta por meio da qual se comunique (geralmente, a porta 443).  
  
Por exemplo, para definir as configurações de proxy WinHTTP para a conta Sistema Local especificando o servidor proxy *MyProxy.CONTOSO.com*, com a porta 443 e exceções de endereço local, digite o seguinte comando:  
  
```  
netsh winhttp set proxy MyProxy.CONTOSO.com:443 "<local>"  
```  
  
### <a name="BKMK_BP_HF"></a>Recomendações para usar o Microsoft.HotfixPlugin  
  
-   Recomendamos que você configure as permissões na pasta raiz de hotfix e arquivo de configuração de hotfix para restringir o acesso de gravação aos administradores locais somente nos computadores que são usados para armazenar esses arquivos. Isso ajuda a evitar a violação com esses arquivos por usuários não autorizados que poderiam comprometer a funcionalidade de cluster de failover quando hotfixes são aplicados.  
  
-   Para ajudar a garantir a integridade de dados para as conexões de \(SMB\) de bloco de mensagem do servidor que são usados para acessar a pasta raiz de hotfix, você deve configurar a criptografia SMB na pasta compartilhada SMB, se é possível configurá-lo. O **Microsoft.HotfixPlugin** exige que SMB criptografia ou assinatura SMB esteja configurada para ajudar a garantir a integridade de dados para as conexões de SMB. 
  
    Para obter mais informações, consulte [restringir o acesso à pasta raiz de hotfix e arquivo de configuração de hotfix](cluster-aware-updating-plug-ins.md#BKMK_ACL).
  
### <a name="additional-recommendations"></a>Recomendações adicionais  
  
-   Para evitar interferir em um CAU atualizando executar que podem ser agendados ao mesmo tempo, não agende alterações de senha para objetos de nome do cluster e computador virtual durante a janelas de manutenção agendadas.  
  
-   Você deve definir permissões apropriadas no pre\ update e update post\ scripts que são salvos em pastas compartilhadas na rede para evitar possíveis adulterações com esses arquivos por usuários não autorizados.  
  
-   Para configurar CAU no modo de atualização self\, um objeto de computador virtual \(VCO\) para a função clusterizada CAU deve ser criada no Active Directory. CAU pode criar esse objeto automaticamente no momento em que a função clusterizada CAU é adicionada, se o cluster de failover tiver permissões suficientes. No entanto, devido as políticas de segurança em determinados organizações, pode ser necessário inserir o objeto no Active Directory. Para obter um procedimento para fazer isso, consulte [etapas para inserir uma conta para uma função clusterizada](http://go.microsoft.com/fwlink/p/?LinkId=237624).  
  
-   Para salvar e reutilizar configurações atualizando executar em clusters de failover com semelhante atualizando necessidades na organização de TI, você pode criar perfis de executar a atualização. Além disso, dependendo do modo de atualização, você pode salvar e gerenciar os perfis de executar a atualização em um compartilhamento de arquivo que seja acessível para todos os computadores remotos de coordenador de atualização ou clusters de failover. Para obter mais informações, consulte [opções avançadas e atualizar perfis executar para CAU](cluster-aware-updating-options.md).  
  
## <a name="BKMK_BPA"></a>Preparação para a atualização de cluster de teste  
Você pode executar o modelo \(BPA\) CAU analisador de práticas recomendadas para testar se um cluster de failover e o ambiente de rede conhecer muitos dos requisitos de software as atualizações aplicadas pelo CAU. Muitos dos testes verificar o ambiente para preparação para aplicar atualizações da Microsoft usando o padrão plug\-in, **Microsoft.WindowsUpdatePlugin**.  
  
> [!NOTE]  
> Talvez você precise validar independente que seu ambiente de cluster está pronto para aplicar atualizações de software usando um plug\-in diferente **Microsoft.WindowsUpdatePlugin**. Se você estiver usando um Microsoft non\ plug\-in, como um fornecido pelo fabricante do hardware, contate o fornecedor para obter mais informações.  
  
Você pode executar a ferramenta BPA nas duas formas a seguir:  
  
1.  Selecione **preparação para a atualização de cluster de analisar** no console CAU. Após o BPA conclusão dos testes de preparação, aparece um relatório de teste. Se forem detectados problemas em nós de cluster, os problemas específicos e os nós onde aparecem os problemas são identificados para que você pode executar uma ação corretiva. Os testes podem levar vários minutos para ser concluída.  
  
2.  Execute o [teste CauSetup](https://technet.microsoft.com/itpro/powershell/windows/cluster-aware-updating/test-causetup) cmdlet. Você pode executar o cmdlet em um computador local ou remoto em que o Failover Clustering módulo para Windows PowerShell (parte das ferramentas de cluster de Failover) está instalado. Você também pode executar o cmdlet em um nó de cluster de failover.  
  
> [!NOTE]  
> -   Você deve usar uma conta que tenha privilégios administrativos em nós de cluster e privilégios administrativos locais no computador em que é usada para executar o **Test\ CauSetup** cmdlet ou para analisar o cluster usando a janela atualizar suporte a Cluster preparação para a atualização. Para executar os testes usando a janela atualizar suporte a Cluster, você deve estar conectado ao computador com as credenciais necessárias.  
> -   Os testes suponha que as ferramentas CAU que são usadas para visualizar e aplicar as atualizações de software executadas no mesmo computador e com as mesmas credenciais de usuário que são usados para testar a preparação para a atualização de cluster.  
  
> [!IMPORTANT]  
> É altamente recomendável que você teste o cluster para atualizar preparação nas seguintes situações:  
>   
> -   Antes de usar CAU pela primeira vez para aplicar atualizações de software.  
> -   Depois que você adicione um nó de cluster ou fazer outras alterações de hardware no cluster que exigem a execução validar um Assistente de Cluster.  
> -   Depois que você alterar uma fonte de atualização ou alterar configurações de atualização ou configurações \(other than CAU\) que podem afetar a aplicação das atualizações em nós.  
  
### <a name="tests-for-cluster-updating-readiness"></a>Testes para preparação de atualização de cluster  
A tabela a seguir lista o cluster etapas de resolução, alguns problemas comuns e testes de preparação para a atualização.  
  
|Teste|Impactos e possíveis problemas|Etapas de resolução|  
|--------|-------------------------------|--------------------|  
|O cluster de failover deve estar disponível|Não é possível resolver que o nome de cluster de failover, ou um ou mais nós de cluster não podem ser acessado. O BPA não pode executar os testes de preparação do cluster.|-Verifica a ortografia do nome do cluster especificado durante o BPA executar.<br />-Verifique se todos os nós de cluster estão online e em execução.<br />-Verifique se a validar um Assistente de configuração podem executar com êxito no cluster de failover.|  
|Os nós de cluster de failover devem estar habilitados para o gerenciamento remoto por meio de WMI|Um ou mais nós de cluster de failover não estão habilitados para o gerenciamento remoto usando o Windows Management Instrumentation \(WMI\). CAU não pode atualizar os nós de cluster se os nós não estão configurados para o gerenciamento remoto.|Certifique-se de que todos os nós de cluster de failover estão habilitados para o gerenciamento remoto por meio de WMI. Para obter mais informações, consulte [configurar os nós de gerenciamento remoto](#BKMK_NODE_CONFIG) neste tópico.|  
|Conexão remota do PowerShell deve ser habilitada em cada nó de cluster de failover|PowerShell não está instalado ou não está habilitado para acesso remoto em um ou mais nós de cluster de failover. CAU não pode ser configurado para o modo de atualização self\ ou usar determinados recursos no modo de atualização remote\.|Certifique-se de que o PowerShell está instalado em todos os nós de cluster e está habilitado para remoto.<br /><br />Para obter mais informações, consulte [configurar os nós de gerenciamento remoto](#BKMK_NODE_CONFIG) neste tópico.|  
|Versão de cluster de failover|Um ou mais nós no cluster de failover não execute o Windows Server 2012, Windows Server 2012 R2 ou Windows Server 2016. CAU não poderá atualizar o cluster de failover.|Verificar se o cluster de failover especificado durante o BPA executar está funcionando Windows Server 2012, Windows Server 2012 R2 ou Windows Server 2016.<br /><br />Para obter mais informações, consulte [verificar a configuração de cluster](#BKMK_REQ_CLUS) neste tópico.|  
|As versões necessárias do .NET Framework e do Windows PowerShell devem ser instaladas em todos os nós de cluster de failover|.NET framework 4.6, 4.5 ou o Windows PowerShell não estiver instalado em um ou mais nós de cluster. Alguns recursos CAU talvez não funcionem.|Certifique-se de que o .NET Framework 4.6 ou 4.5 e do Windows PowerShell são instalados em todos os nós de cluster, se eles forem necessários.<br /><br />Para obter mais informações, consulte [configurar os nós de gerenciamento remoto](#BKMK_NODE_CONFIG) neste tópico.|  
|O serviço de Cluster deve ser executado em todos os nós de cluster|O serviço de Cluster não está executando em um ou mais nós. CAU não poderá atualizar o cluster de failover.|-Verifique se o Cluster serviço \(clussvc\) é iniciado em todos os nós do cluster, e ele está configurado para iniciar automaticamente.<br />-Verifique se a validar um Assistente de configuração podem executar com êxito no cluster de failover.<br /><br />Para obter mais informações, consulte [verificar a configuração de cluster](#BKMK_REQ_CLUS) neste tópico.|  
|As atualizações automáticas não devem ser configuradas para instalar atualizações automaticamente em qualquer nó de cluster de failover|Em pelo menos um nó de cluster de failover, as atualizações automáticas está configurada para instalar automaticamente as atualizações da Microsoft no nó. Combinando CAU com outros métodos de atualização pode resultar em tempo de inatividade não planejado ou resultados imprevisíveis.|Se a funcionalidade do Windows Update está configurada para atualizações automáticas em um ou mais nós de cluster, certifique-se de que as atualizações automáticas não está configurada para instalar atualizações automaticamente.<br /><br />Para obter mais informações, consulte [recomendações para aplicar atualizações do Microsoft](#BKMK_BP_WUA).|  
|Os nós de cluster de failover devem usar a mesma fonte de atualização|Um ou mais nós de cluster de failover são configurados para usar uma fonte de atualização para atualizações da Microsoft que é diferente do resto de nós. Atualizações não podem ser aplicadas uniformemente em nós de cluster pelo CAU.|Certifique-se de que todos os nós de cluster está configurado para usar a mesma fonte de atualização, por exemplo, um servidor WSUS, Windows Update ou o Microsoft Update.<br /><br />Para obter mais informações, consulte [recomendações para aplicar atualizações do Microsoft](#BKMK_BP_WUA).|  
|Uma regra de firewall que permite o desligamento remoto deve ser habilitada em cada nó no cluster de failover|Um ou mais nós de cluster de failover não tenha habilitada uma regra de firewall que permite o desligamento remoto ou uma configuração de política de grupo impede que esta regra sendo ativado. Uma atualização executar que se aplica atualizações que exigem reiniciar automaticamente os nós poderá não ser concluída corretamente.|Se o Firewall do Windows ou um firewall non\ da Microsoft está em uso em nós de cluster, configure uma regra de firewall que permite o desligamento remoto.<br /><br />Para obter mais informações, consulte [habilitar uma regra de firewall para permitir reinicializações automáticas](#BKMK_FW) neste tópico.|  
|A configuração do servidor proxy em cada nó de cluster de failover deve ser definida como um servidor proxy local|Um ou mais nós de cluster de failover tem uma configuração do servidor proxy incorreto.<br /><br />Se um servidor proxy local estiver em uso, a configuração do servidor proxy em cada nó deve ser configurada corretamente para o cluster acessar o Microsoft Update ou o Windows Update.|Certifique-se de que as configurações de proxy WinHTTP em cada nó de cluster são definidas como um servidor proxy local se ele é necessário. Se um servidor proxy não está em uso em seu ambiente, esse aviso pode ser ignorado.<br /><br />Para obter mais informações, consulte [aplicar atualizações em cenários de filial](#BKMK_PROXY) neste tópico.|  
|A função clusterizada CAU deve ser instalada no cluster de failover para habilitar o modo de atualização self\|A função clusterizada CAU não está instalada neste cluster de failover. Essa função é necessária para atualizar self\ de cluster.|Para usar CAU no modo de atualização self\, adicione a função clusterizada CAU no cluster de failover em uma das seguintes maneiras:<br /><br />-Executar o [CauClusterRole adicionar](https://technet.microsoft.com/itpro/powershell/windows/cluster-aware-updating/add-cauclusterrole) cmdlet do PowerShell.<br />-Selecione o **configurar as opções de atualização self\ cluster** ação na janela atualizar suporte a Cluster.|  
|A função clusterizada CAU deve ser habilitada no cluster de failover para habilitar o modo de atualização self\|A função clusterizada CAU está desabilitada. Por exemplo, a função clusterizada CAU não está instalada, ou ele foi desativado, usando o [Disable\ CauClusterRole](https://technet.microsoft.com/itpro/powershell/windows/cluster-aware-updating/disable-cauclusterrole) cmdlet do PowerShell. Essa função é necessária para atualizar self\ de cluster.|Para usar CAU no modo de atualização self\, habilite a função de cluster CAU neste cluster de failover em uma das seguintes maneiras:<br /><br />-Executar o [habilitar CauClusterRole](https://technet.microsoft.com/itpro/powershell/windows/cluster-aware-updating/enable-cauclusterrole) cmdlet do PowerShell.<br />-Selecione o **configurar as opções de atualização self\ cluster** ação na janela atualizar suporte a Cluster.|  
|O CAU configurado plug\-in para o modo de atualização self\ deve ser registrado em todos os nós de cluster de failover|A função clusterizada CAU em um ou mais nós desse cluster de failover não pode acessar o CAU plug\ no módulo configurado nas opções de atualização self\. Um self\-atualizando executar poderá falhar.|-Verifique se o CAU configurado plug\-in é instalado em todos os nós de cluster seguindo o procedimento de instalação do produto que fornece o CAU plug\-in.<br />-Executar o [Register\ CauPlugin](https://technet.microsoft.com/itpro/powershell/windows/cluster-aware-updating/register-cauplugin) cmdlet do PowerShell para registrar o plug\-in em nós de cluster necessários.|  
|Todos os nós de cluster de failover devem ter o mesmo conjunto de marcas CAU plug\-ins|Um self\-atualizando executar poderá falhar se o plug\-in que está configurado para ser usado em executar uma atualização for alterado para uma que não está disponível em todos os nós de cluster.|-Verifique se o CAU configurado plug\-in é instalado em todos os nós de cluster seguindo o procedimento de instalação do produto que fornece o CAU plug\-in.<br />-Executar o [Register\ CauPlugin](https://technet.microsoft.com/itpro/powershell/windows/cluster-aware-updating/register-cauplugin) cmdlet do PowerShell para registrar o plug\-in em nós de cluster necessários.|  
|As opções de atualização executar configuradas devem ser válidas|O agendamento de atualização self\ e atualizando executar opções que estão configuradas para esse cluster de failover incompletas ou não são válidas. Um self\-atualizando executar poderá falhar.|Configure um agendamento de atualização self\ válido e conjunto de opções de executar a atualização. Por exemplo, você pode usar o [Set \ CauClusterRole](https://technet.microsoft.com/itpro/powershell/windows/cluster-aware-updating/set-cauclusterrole) cmdlet do PowerShell para configurar o CAU clusterizado função.|  
|Pelo menos dois nós de cluster de failover deve ser proprietários da função clusterizado CAU|Uma atualização executar iniciado no modo de atualização self\ falhará porque a função clusterizada CAU não tem um nó de proprietário possíveis para mover para.|Use as ferramentas de cluster de Failover para garantir que todos os nós de cluster são configurados como possíveis proprietários do CAU agrupado função. Essa é a configuração padrão.||  
|Todos os nós de cluster de failover devem ser capazes de acessar scripts do Windows PowerShell|Nem todos os nós de proprietário possíveis da função clusterizado CAU podem acessar scripts configurados do Windows PowerShell pre\ update e update post\. Um self\-atualizando executar falhará.|Certifique-se de que todos os nós de proprietário possíveis da função clusterizado CAU tem permissões para acessar os scripts de pre\ update e update post\ de PowerShell configurados.|  
|Todos os nós de cluster de failover devem usar idênticos scripts do Windows PowerShell|Nem todos os nós de proprietário possíveis da função clusterizado CAU usarem a mesma cópia dos scripts especificados do Windows PowerShell pre\ update e update post\. Executar uma atualização self\ pode falhar ou mostrar um comportamento inesperado.|Certifique-se de que todos os nós de proprietário possíveis da função clusterizado CAU usam os mesmos PowerShell pre\ update e update post\ scripts.|  
|A configuração de WarnAfter especificada para executar a atualização deve ser menor que a configuração de StopAfter|Os valores de tempo limite especificados CAU atualizando executar Verifique o tempo limite de aviso ineficaz. Execute uma atualização pode ser cancelada antes de um log de eventos de aviso podem ser gerado.|Em Opções de atualização executar, configurar um **WarnAfter** opção valor que seja menor do que o **StopAfter** valor de opção.|  
  
## <a name="see-also"></a>Consulte também  
  
-   [Visão geral do suporte a cluster atualizando](cluster-aware-updating.md)
  
