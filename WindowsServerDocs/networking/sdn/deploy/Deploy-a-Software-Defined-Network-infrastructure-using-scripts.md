---
title: Implante uma infraestrutura de rede definidos Software usando Scripts
description: Este tópico aborda como implantar uma infraestrutura de rede de definido de Software da Microsoft (SDN) usando scripts no Windows Server 2016.
manager: brianlic
ms.prod: windows-server-threshold
ms.service: virtual-network
ms.technology: networking-sdn
ms.topic: get-started-article
ms.assetid: 5ba5bb37-ece0-45cb-971b-f7149f658d19
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 4428ad73ab8933510d5a759ec4fa7377ea222ebd
ms.sourcegitcommit: 19d9da87d87c9eefbca7a3443d2b1df486b0b010
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/28/2018
---
# <a name="deploy-a-software-defined-network-infrastructure-using-scripts"></a>Implante uma infraestrutura de rede definidos Software usando Scripts

>Aplica-se a: Windows Server (anual por canal), Windows Server 2016

Este tópico aborda como implantar uma infraestrutura de rede de definido de Software da Microsoft (SDN) usando scripts. A infraestrutura inclui um controlador de rede (HA) altamente disponível, um HA Software Load balanceador (SLB) / Multiplexador, redes virtuais e associadas a listas de controle de acesso (ACLs). Além disso, o outro script implanta uma carga de trabalho de locatário para validar sua infraestrutura SDN.  
  
Se você quiser que as cargas de trabalho de locatário para se comunicar fora suas redes virtuais, você pode configurar as regras SLB NAT, túneis Gateway to-Site ou encaminhamento de camada 3 rotear entre as cargas de trabalho virtuais e físicas.  
  
Você também pode implantar uma infraestrutura SDN usando Virtual Machine Manager (VMM). Para obter mais informações, consulte [configurar uma infraestrutura de rede de definido de Software (SDN) na malha VMM](https://technet.microsoft.com/system-center-docs/vmm/scenario/sdn-overview).  
  
## <a name="pre-deployment"></a>Antes da implantação  
  
> [!IMPORTANT]  
> Antes de começar a implantação, você deve planejar e configurar seu hosts e infraestrutura de rede física. Para obter mais informações, consulte [planejar uma infraestrutura de rede definidos do Software](../../sdn/plan/Plan-a-Software-Defined-Network-Infrastructure.md).  
  
Todos os hosts Hyper-V devem ter o Windows Server 2016 instalado.  
  
## <a name="deployment-steps"></a>Etapas de implantação  
Comece definindo o comutador virtual do Hyper-V (servidores físicos) e a atribuição de endereço IP do host do Hyper-V. Qualquer tipo de armazenamento que é compatível com o Hyper-V, compartilhado ou local pode ser usado.  
### <a name="install-host-networking"></a>Instalar o host de rede  
1. Instale os drivers de rede mais recentes disponíveis para seu hardware da placa de rede.  
2. Instale a função Hyper-V em todos os hosts (para obter mais informações, consulte [começar com o Hyper-V no Windows Server 2016](https://technet.microsoft.com/en-us/library/mt126159.aspx).   
  
   Em um prompt do Windows PowerShellcommand com privilégios elevados:  
   ``Install-WindowsFeature -Name Hyper-V -ComputerName <computer_name> -IncludeManagementTools -Restart``  
    
    um. Criar o comutador virtual Hyper-V (use o mesmo nome de alternar para todos os hosts. Por exemplo: **sdnSwitch**). Configure pelo menos um adaptador de rede ou, se usando a opção Embedded agrupamento, configure pelo menos dois adaptadores de rede. Distribuindo entrada máximo ocorre ao usar dois NICs.  
 `` New-VMSwitch "<switch name>" -NetAdapterName "<NetAdapter1>" [, "<NetAdapter2>" -EnableEmbeddedTeaming $True] -AllowManagementOS $True``  
 
 >[!NOTE] 
 >Você pode ignorar as etapas 3 e 4, se você tiver separado NICs de gerenciamento.

3. Consulte o tópico de planejamento ([planejar uma infraestrutura de rede definidos do Software](../../sdn/plan/../../sdn/plan/../../sdn/plan/Plan-a-Software-Defined-Network-Infrastructure.md)) e funcionam com o administrador de rede para obter a ID de VLAN da VLAN gerenciamento. Anexe o vNIC de gerenciamento do Switch Virtual recém-criada obtenham o gerenciamento. Esta etapa pode ser omitida se seu ambiente não usar tags de VLAN.  
 `` Set-VMNetworkAdapterIsolation -ManagementOS -IsolationMode Vlan -DefaultIsolationID <Management VLAN> -AllowUntaggedTraffic $True``  
 
4. Consulte o tópico de planejamento ([planejar uma infraestrutura de rede definidos do Software](../../sdn/plan/../../sdn/plan/../../sdn/plan/Plan-a-Software-Defined-Network-Infrastructure.md)) e funcionam com o administrador de rede para usar o DHCP ou atribuições de IP estáticas para atribuir um endereço IP a vNIC o gerenciamento de vSwitch recém-criado. O exemplo a seguir mostra como criar um endereço IP estático e atribuí-lo a vNIC o gerenciamento de vSwitch:  
 ``New-NetIPAddress -InterfaceAlias "vEthernet (<switch name>)" -IPAddress <IP> -DefaultGateway <Gateway IP> -AddressFamily IPv4 -PrefixLength <Length of Subnet Mask - for example: 24>``  
      
5. [Opcional] Implantar uma máquina virtual de hospedar serviços de domínio do Active Directory ([instalar o Active Directory Domain Services (nível 100)](https://technet.microsoft.com/library/hh472162.aspx) e um servidor DNS.  
   
    um. Conectar-se a máquina virtual de servidor do Active Directory/DNS obtenham o gerenciamento:
    
            Set-VMNetworkAdapterIsolation -VMName "<VM Name>" -Access -VlanId <Management VLAN> -AllowUntaggedTraffic $True  
   
   b. Instale o DNS e serviços de domínio do Active Directory.  
      >[!NOTE]
      >O controlador de rede oferece suporte a Kerberos e x. 509 certificados para autenticação. Este guia usa dois mecanismos de autenticação para finalidades diferentes (embora apenas um é necessário).  
        
6. Junte-se todos os hosts Hyper-V para o domínio. Certifique-se a entrada de servidor DNS para o adaptador de rede que tenha um endereço IP atribuído a pontos de rede de gerenciamento para um servidor DNS que pode resolver o nome de domínio. Por exemplo:

        Set-DnsClientServerAddress -InterfaceAlias "vEthernet (<switch name>)" -ServerAddresses <DNS Server IP>  
   
   um. Clique com botão direito **iniciar**, clique em **sistema**e clique em **alterar configurações**.  
   b. Clique em **alteração**.  
   c. Clique em **domínio** e especifique o nome de domínio.  
   d. Clique em **Okey**.  
   e. Digite os nome e a senha credenciais do usuário quando solicitado.  
   f. Reinicie o servidor.  
  
### <a name="validation"></a>Validação  
Use as etapas a seguir para validar esse host de rede está configurado corretamente.  
1. Certifique-se de que a opção de VM foi criada com êxito:  
      
    ``Get-VMSwitch "<switch name>"``  
2. Verifique se o vNIC gerenciamento no Switch VM está conectado obtenham o gerenciamento:  
    >[!NOTE]
    >Relevantes apenas se o tráfego de gerenciamento e locatário compartilhar a mesma placa de rede.    
      
    ``Get-VMNetworkAdapterIsolation -ManagementOS``  
3. Valide que todos os hosts Hyper-V (e recursos de gerenciamento externo, por exemplo: servidores DNS) são acessíveis por meio do ping usando seu endereço IP de gerenciamento e/ou o nome de domínio totalmente qualificado (FQDN).   
      
   ``ping <Hyper-V Host IP>``  
   ``ping <Hyper-V Host FQDN>``  
4. Execute o seguinte comando no host de implantação e especifique o FQDN de cada host do Hyper-V para garantir que as credenciais de Kerberos usadas fornece acesso a todos os servidores.  
      
   ``winrm id -r:<Hyper-V Host FQDN>``  
      
### <a name="nano-installation-requirements-and-notes"></a>Notas e os requisitos de instalação Nano  
Se você usar Nano como seu hosts Hyper-V (servidores físicos) para a implantação, estes são os requisitos adicionais:  
1. Todos os nós Nano precisam ter o pacote de DSC instalado com o pacote de idiomas:  
   
   * Microsoft-NanoServer-DSC-Package.cab  
   * Microsoft-NanoServer-DSC-Package_en-us.cab
   
        ``dism /online /add-package /packagepath:<Path> /loglevel:4``  
2. Os scripts SDN Express devem ser executados em host não Nano (Server Core do Windows ou Windows Server com GUI). Fluxos de trabalho do PowerShell não são suportados no Nano.  
3.  Chamar a API em sentido norte do controlador de rede usando o PowerShell ou NC REST Wrappers (que dependem de Invoke-WebRequest e Invoke-RestMethod) deve ser feito em host não Nano.  
   
         
### <a name="run-sdn-express-scripts"></a>Executar Scripts Express SDN  
  
1.  Os arquivos de instalação estão localizados no GitHub. Baixar o arquivo zip do [Microsoft SDN GitHub repositório](https://github.com/Microsoft/SDN.git). Na página do repositório SDN Microsoft, clique em **Clone ou baixar** e, em seguida, clique em **Download ZIP**.  
  
2.  Designe um computador como o computador de implantação.  Esse computador deve estar executando o Windows Server 2016. Expanda o arquivo zip e copiar o **SDNExpress** pasta no computador de implantação `C:\`pasta.  
  
3.  Compartilhar o `C:\SDNExpress`pasta como "**SDNExpress**" com permissão para **todos** para **leitura/gravação **.  
  
4.  Navegue até o `C:\SDNExpress`pasta.

 Você verá as seguintes pastas:  

|Nome da pasta|Descrição|  
|---------------|---------------|  
|AgentConf|Mantém atualizadas cópias dos esquemas OVSDB usadas pelo agente de Host SDN em cada host do Windows Server 2016 Hyper-V para política de rede do programa.|  
|Certificados|Local compartilhado temporário para o arquivo de certificado NC.|  
|Imagens|Esvaziar, coloque a imagem do Windows Server 2016 vhdx aqui|  
|Ferramentas|Utilitários para solução de problemas e depuração.  Copiada para a hosts e máquinas virtuais.  Recomendamos que você coloque o Monitor de rede ou Wireshark aqui para que ele está disponível, se necessário.|  
|Scripts|Scripts de implantação.<br /><br />-   **SDNExpress.ps1**<br />    Implanta e configura a malha, incluindo as máquinas de virtuais de controlador de rede, máquinas virtuais de multiplexador SLB, pools de gateway e as máquinas de virtuais de gateway HNV correspondente para os grupos.<br />-   **FabricConfig.psd1**<br />    Um modelo de arquivo de configuração para o script SDNExpress.  Você personalizará isso para seu ambiente.<br />-   **SDNExpressTenant.ps1**<br />    Implanta uma carga de trabalho de locatário de amostra em uma rede virtual com um VIP de balanceamento de carga.<br />    Também fornece uma ou mais conexões de rede (IPSec S2S VPN, GRE, L3) sobre os gateways de borda de provedor de serviço que estão conectados à carga de trabalho criado anteriormente locatário. Os gateways IPSec e GRE estão disponíveis para conectividade sobre o endereço de IP VIP correspondente e o gateway de encaminhamento L3 sobre o pool de endereço correspondente.<br />    Esse script pode ser usado para excluir a configuração correspondente com uma opção de desfazer também.<br />-   **TenantConfig.psd1**<br />    Um arquivo de configuração do modelo de carga de trabalho de locatário e configuração de gateway S2S.<br />-   **SDNExpressUndo.ps1**<br />    Limpa o ambiente de malha e restaura um estado inicial.<br />-   **SDNExpressEnterpriseExample.ps1**<br />    Provisiona ambientes corporativos um ou mais de site com um Gateway de acesso remoto e (opcionalmente) uma máquina de virtual enterprise correspondente por site. Os gateways enterprise IPSec ou GRE conecta-se para o endereço IP VIP correspondente do gateway de provedor de serviço para estabelecer os túneis S2S. O Gateway de encaminhamento L3 conecta o endereço de IP de par correspondente. <br />            Esse script pode ser usado para excluir a configuração correspondente com uma opção de desfazer também.<br />-   **EnterpriseConfig.psd1**<br />    Um arquivo de configuração do modelo para o gateway do Enterprise-to-site e a configuração de cliente VM.|  
|TenantApps|Arquivos usados para implantar cargas de trabalho de locatário de exemplo.|  
  
5.  Verifique se o arquivo do Windows Server 2016 VHDX está sendo o **imagens** pasta.  
  
6. Personalizar o arquivo SDNExpress\scripts\FabricConfig.psd1, alterando a **<< substituir >>** marcas com valores específicos ajustar à sua infraestrutura de laboratório, incluindo nomes de host, nomes de domínio, nomes de usuário e senhas e informações de rede para as redes são listadas no tópico de planejamento de rede.  
7. Crie um registro de Host A no DNS para a NetworkControllerRestName (FQDN) e NetworkControllerRestIP.  
8. Execute o script como um usuário com credenciais de administrador do domínio:  
      
    ``SDNExpress\scripts\SDNExpress.ps1 -ConfigurationDataFile FabricConfig.psd1 -Verbose``  
      
9.  Para desfazer todas as operações, execute o seguinte comando:  
      
    ``SDNExpress\scripts\SDNExpressUndo.ps1 -ConfigurationDataFile FabricConfig.psd1 -Verbose``  
      
#### <a name="validation"></a>Validação  
Supondo que o script SDN Express executou a conclusão sem informar os erros, você pode realizar a etapa a seguir para garantir que os recursos de malha implantaram corretamente e estão disponíveis para a implantação de locatário.  

- Use [ferramentas de diagnóstico](https://docs.microsoft.com/windows-server/networking/sdn/troubleshoot/troubleshoot-windows-server-software-defined-networking-stack) para garantir que não existam erros em todos os recursos malha no controlador de rede.  
      
    ``Debug-NetworkControllerConfigurationState -NetworkController <FQDN of Network Controller Rest Name>``  
        
   
### <a name="deploy-a-sample-tenant-workload-with-the-software-load-balancer"></a>Implantar uma carga de trabalho de locatário de exemplo com o balanceador de carga de software  
    
Agora que os recursos de malha for implantados, você pode validar seu SDN implantação ponta a ponta Implantando uma carga de trabalho de locatário do exemplo. Essa carga de trabalho do locatário consiste em duas virtuais sub-redes (web e camadas de banco de dados) protegidos por meio de regras de lista de controle de acesso (ACL) usando o firewall SDN distribuído. Sub-rede virtual da camada da web é acessível por meio de SLB/Multiplexador usando um endereço IP Virtual (VIP). O script automaticamente implanta duas máquinas de virtuais de nível da web e um banco de dados camada virtual machine e conecta esses para as sub-redes virtuais.  
  
1.  Personalizar o arquivo SDNExpress\scripts\TenantConfig.psd1, alterando a **<< substituir >>** marcas com valores específicos (por exemplo: de imagem VHD nome, nome da rede controlador REST, vSwitch nome, conforme indicado anteriormente no arquivo FabricConfig.psd1 etc.)  
2.  Execute o script. Por exemplo:  
``SDNExpress\scripts\SDNExpressTenant.ps1 -ConfigurationDataFile TenantConfig.psd1 -Verbose``  
3.  Para desfazer a configuração, execute o script mesmo com o **desfazer** parâmetro. Por exemplo:  
``SDNExpress\scripts\SDNExpressTenant.ps1 -Undo -ConfigurationDataFile TenantConfig.psd1 -Verbose``  

#### <a name="validation"></a>Validação  
Para validar que a implantação de locatário foi bem-sucedida, faça o seguinte:
1.  Faça logon na máquina virtual banco de dados camada e tente ping no endereço IP de um das máquinas de virtuais de nível da web (certifique-se de que Firewall do Windows estiver desativado nas máquinas de virtuais de nível da web).  
2.  Verifique os recursos de locatário do controlador de rede para quaisquer erros. Execute o seguinte em qualquer host do Hyper-V Layer 3 conectividade com o controlador de rede:  
      
    ``Debug-NetworkControllerConfigurationState -NetworkController <FQDN of Network Controller REST Name>``
3. Para verificar o balanceador está sendo executado corretamente, execute o seguinte em qualquer host do Hyper-V:
    
        wget <VIP IP address>/unique.htm -disablekeepalive -usebasicparsing
   
   Onde `<VIP IP address>`é a camada da web endereço IP VIP configuradas no arquivo TenantConfig.psd1. Procure o `VIPIP`variável em TenantConfig.psd1.

   Execute esta vezes muliple para ver o balanceador alternar entre os DIPs disponíveis. Você também pode observar esse comportamento usando um navegador da web. Navegue até `<VIP IP address>/unique.htm`. Feche o navegador e abra uma nova instância e procure novamente. Você verá a página azul e verde página alternativa, exceto quando o navegador armazena em cache da página para que o cache de apagamento.