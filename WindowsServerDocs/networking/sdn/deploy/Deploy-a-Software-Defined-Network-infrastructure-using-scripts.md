---
title: Implantar uma infraestrutura de rede definida de Software usando Scripts
description: Este tópico aborda como implantar uma infraestrutura de rede Microsoft Software Defined (SDN) usando scripts no Windows Server 2016.
manager: dougkim
ms.prod: windows-server-threshold
ms.service: virtual-network
ms.technology: networking-sdn
ms.topic: get-started-article
ms.assetid: 5ba5bb37-ece0-45cb-971b-f7149f658d19
ms.author: pashort
author: shortpatti
ms.date: 08/23/2018
ms.openlocfilehash: dabfe3de4cc307723ff7e614fb73e3903e74aeb2
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59844617"
---
# <a name="deploy-a-software-defined-network-infrastructure-using-scripts"></a>Implantar uma infraestrutura de rede definida pelo software usando scripts

>Aplica-se a: Windows Server (canal semestral), Windows Server 2016

Neste tópico, você deve implantar uma infraestrutura de rede Microsoft Software Defined (SDN) usando scripts. A infraestrutura inclui um controlador de rede (HA) altamente disponível, uma alta disponibilidade do Software SLB balanceador de carga () / MUX, redes virtuais e associados a listas de controle de acesso (ACLs). Além disso, outro script implanta uma carga de trabalho de locatário para validar sua infraestrutura SDN.  
  
Se você quiser que suas cargas de trabalho de locatário para se comunicar fora de suas redes virtuais, você pode configurar regras de NAT do SLB, túneis de Gateway Site a Site ou encaminhamento de camada 3 para o roteamento entre as cargas de trabalho virtuais e físicas.  
  
Você também pode implantar uma infraestrutura SDN usando o Virtual Machine Manager (VMM). Para obter mais informações, consulte [configurar uma infraestrutura de rede definida pelo Software (SDN) na malha do VMM](https://technet.microsoft.com/system-center-docs/vmm/scenario/sdn-overview).  

  
## <a name="pre-deployment"></a>Pré-implantação  
  
> [!IMPORTANT]  
> Antes de começar a implantação, você deve planejar e configurar os hosts e infraestrutura de rede física. Para obter mais informações, consulte [Planejar a infraestrutura de rede definida por software](../../sdn/plan/Plan-a-Software-Defined-Network-Infrastructure.md).  
  
Todos os hosts do Hyper-V devem ter o Windows Server 2016 instalado.  
  
## <a name="deployment-steps"></a>Etapas de implantação  
Comece Configurando o comutador virtual do Hyper-V (servidores físicos) e a atribuição de endereço IP do host do Hyper-V. Qualquer tipo de armazenamento que é compatível com o Hyper-V, local ou compartilhado pode ser usado.  

### <a name="install-host-networking"></a>Instalar o sistema de rede do host  

1. Instale os drivers de rede mais recentes disponíveis para o seu hardware NIC.  
2. Instalar a função Hyper-V em todos os hosts (para obter mais informações, consulte [Introdução ao Hyper-V no Windows Server 2016](https://docs.microsoft.com/windows-server/virtualization/hyper-v/get-started/Get-started-with-Hyper-V-on-Windows).   
  
   ```PowerShell
   Install-WindowsFeature -Name Hyper-V -ComputerName <computer_name> -IncludeManagementTools -Restart
   ```  
    
3. Crie o comutador virtual do Hyper-V.<p>Usar o mesmo nome de comutador para todos os hosts, por exemplo, **sdnSwitch**. Configure pelo menos um adaptador de rede ou, se usando o conjunto, configure pelo menos dois adaptadores de rede. Difusão de entrada máximo ocorre ao usar duas NICs.  

   ```PowerShell
   New-VMSwitch "<switch name>" -NetAdapterName "<NetAdapter1>" [, "<NetAdapter2>" -EnableEmbeddedTeaming $True] -AllowManagementOS $True
   ```  
   >[!TIP] 
   >Se você tiver NICs separadas do gerenciamento, você pode ignorar as etapas 4 e 5.

3. Consulte o tópico de planejamento ([planejar uma infraestrutura de rede definida pelo Software](../../sdn/plan/../../sdn/plan/../../sdn/plan/Plan-a-Software-Defined-Network-Infrastructure.md)) e trabalhe com seu administrador de rede para obter a ID de VLAN da VLAN de gerenciamento. Anexe a vNIC de gerenciamento do comutador Virtual recém-criado para a VLAN de gerenciamento. Esta etapa pode ser omitida se seu ambiente de não usar marcas de VLAN.  
   
   ```PowerShell
   Set-VMNetworkAdapterIsolation -ManagementOS -IsolationMode Vlan -DefaultIsolationID <Management VLAN> -AllowUntaggedTraffic $True
   ```  
 
4. Consulte o tópico de planejamento ([planejar uma infraestrutura de rede definida pelo Software](../../sdn/plan/../../sdn/plan/../../sdn/plan/Plan-a-Software-Defined-Network-Infrastructure.md)) e trabalhe com seu administrador de rede para usar o DHCP ou atribuições de IP estáticas para atribuir um endereço IP para a vNIC de gerenciamento de recém-criado vSwitch. O exemplo a seguir mostra como criar um endereço IP estático e atribuí-lo para a vNIC de gerenciamento do vSwitch:  
 
   ```PowerShell
   New-NetIPAddress -InterfaceAlias "vEthernet (<switch name>)" -IPAddress <IP> -DefaultGateway <Gateway IP> -AddressFamily IPv4 -PrefixLength <Length of Subnet Mask - for example: 24>
   ```  
      
5. [Opcional] Implantar uma máquina virtual para hospedar os serviços de domínio do Active Directory ([instalar o Active Directory Domain Services (nível 100)](https://technet.microsoft.com/library/hh472162.aspx) e um servidor DNS.  
   
    a. Conecte-se a máquina virtual de servidor DNS/Active Directory para a VLAN de gerenciamento:
    
       ```PowerShell
       Set-VMNetworkAdapterIsolation -VMName "<VM Name>" -Access -VlanId <Management VLAN> -AllowUntaggedTraffic $True  
       ```   

   b. Instale o DNS e Active Directory Domain Services.  

   >[!NOTE]
   >O controlador de rede dá suporte a certificados Kerberos e X.509 para autenticação. Este guia usa ambos os mecanismos de autenticação para finalidades diferentes (embora apenas uma for necessária).  
        
6. Junte-se todos os hosts do Hyper-V para o domínio. Verifique se a entrada do servidor DNS para o adaptador de rede que tem um endereço IP atribuído para os pontos de gerenciamento de rede para um servidor DNS que pode resolver o nome de domínio. 

   ```PowerShell   
   Set-DnsClientServerAddress -InterfaceAlias "vEthernet (<switch name>)" -ServerAddresses <DNS Server IP>  
   ```
   
   a. Clique com botão direito **inicie**, clique em **sistema**e, em seguida, clique em **alterar configurações**.  
   b. Clique em **Alterar**.  
   c. Clique em **domínio** e especifique o nome de domínio.  
   d. Clique em **OK**.  
   e. Digite os nome e senha de credenciais do usuário quando solicitado.  
   f. Reinicie o servidor.  
  
### <a name="validation"></a>Validação  
Use as etapas a seguir para validar que o host de rede está configurado corretamente.  

1. Certifique-se de que o comutador de VM foi criado com êxito:  
      
   ```PowerShell
   Get-VMSwitch "<switch name>"
   ```  

2. Verifique se que a vNIC de gerenciamento no comutador de VM está conectada à VLAN de gerenciamento:  

   >[!NOTE]
   >Relevante apenas se o tráfego de gerenciamento e locatário compartilham a mesma NIC.    
      
   ```PowerShell
   Get-VMNetworkAdapterIsolation -ManagementOS
   ```

3. Valide todos os hosts do Hyper-V e recursos de gerenciamento externo, por exemplo, os servidores DNS.<p>Certifique-se de que eles são acessíveis por meio de ping usando seu endereço IP de gerenciamento e/ou nome de domínio totalmente qualificado (FQDN).   
      
   ``ping <Hyper-V Host IP>``  
   ``ping <Hyper-V Host FQDN>``  

4. Execute o seguinte comando no host de implantação e especifique o FQDN de cada host Hyper-V para garantir que as credenciais do Kerberos usadas fornece acesso a todos os servidores.  
      
   ``winrm id -r:<Hyper-V Host FQDN>``  
      
### <a name="nano-installation-requirements-and-notes"></a>Observações e requisitos de instalação do Nano  

Se você usar o Nano como seus hosts do Hyper-V (servidores físicos) para a implantação, serão os seguintes requisitos adicionais:  

1. Todos os nós do Nano precisam ter o pacote de DSC instalado com o pacote de idiomas:  
   
    - Microsoft-NanoServer-DSC-Package.cab  
    - Microsoft-NanoServer-DSC-Package_en-us.cab
   
    ``dism /online /add-package /packagepath:<Path> /loglevel:4``  

2. Os scripts de SDN Express devem ser executados a partir de um host não Nano (Server Core do Windows ou Windows Server com GUI). Não há suporte para fluxos de trabalho do PowerShell no Nano.  

3.  Invocar a API NorthBound do controlador de rede usando o PowerShell ou os Wrappers de REST do NC (que dependem de Invoke-WebRequest e Invoke-RestMethod) deve ser feito de um host não Nano.  
   
         
### <a name="run-sdn-express-scripts"></a>Executar scripts de SDN Express  
  
1. Vá para o [repositório Microsoft SDN GitHub](https://github.com/Microsoft/SDN.git) dos arquivos de instalação.

2. Baixe os arquivos de instalação do repositório para o computador designado de implantação. Clique em **clonar ou baixar** e, em seguida, clique em **baixar ZIP**.  
 
   >[!NOTE]
   >O computador designado de implantação deve estar executando o Windows Server 2016 ou posterior.
 
3. Expanda o arquivo zip e copie a **SDNExpress** pasta no computador de implantação `C:\` pasta.  
  
4. Compartilhamento de `C:\SDNExpress` pasta como "**SDNExpress**" com permissão para **todos** para **leitura/gravação**.  
  
5. Navegue até o `C:\SDNExpress` pasta.<p>Você verá as seguintes pastas:  

   |Nome da pasta|Descrição|  
   |---------------|---------------|  
   |AgentConf|Mantém cópias atualizadas dos esquemas OVSDB usados pelo agente de Host de SDN em cada host do Windows Server 2016 Hyper-V à diretiva de rede do programa.|  
   |Certificados|Local temporário compartilhado para o arquivo de certificado do NC.|  
   |Imagens|Esvaziar, coloque sua imagem de vhdx do Windows Server 2016 aqui|  
   |Ferramentas|Utilitários para solução de problemas e depuração.  Copiado para os hosts e máquinas virtuais.  É recomendável que você coloque o Monitor de rede ou o Wireshark aqui para que ele esteja disponível, se necessário.|  
   |Scripts|Scripts de implantação.<br /><br />-   **SDNExpress.ps1**<br />    Implanta e configura a malha, incluindo máquinas virtuais do controlador de rede, máquinas virtuais SLB Mux, pools de gateway e as máquinas de virtuais de gateway HNV correspondente para o pool (s).<br />-   **FabricConfig.psd1**<br />    Um modelo de arquivo de configuração para o script SDNExpress.  Você irá personalizar isso para o seu ambiente.<br />-   **SDNExpressTenant.ps1**<br />    Implanta uma carga de trabalho de locatário de exemplo em uma rede virtual com um VIP com balanceamento de carga.<br />    Também provisiona uma ou mais conexões de rede (a VPN S2S IPSec, GRE, L3) em gateways de borda de provedor do serviço que estão conectados à carga de trabalho do locatário criada anteriormente. Os gateways de IPSec e GRE estão disponíveis para conectividade sobre o endereço IP VIP correspondente e o gateway de encaminhamento L3 ao longo do pool de endereços correspondente.<br />    Esse script pode ser usado para excluir a configuração correspondente com uma opção de desfazer.<br />-   **TenantConfig.psd1**<br />    Um arquivo de configuração de modelo para a carga de trabalho de locatário e configuração de S2S do gateway.<br />-   **SDNExpressUndo.ps1**<br />    Limpa o ambiente de malha e ele será redefinido para um estado inicial.<br />-   **SDNExpressEnterpriseExample.ps1**<br />    Provisiona um ou mais ambientes de site de empresa com um Gateway de acesso remoto e (opcionalmente) uma máquina de virtual enterprise correspondente por site. Os gateways enterprise IPSec ou GRE conecta-se para o endereço IP VIP correspondente do gateway de provedor de serviço para estabelecer os túneis S2S. O Gateway de encaminhamento L3 conecta-se o endereço de IP de par correspondente. <br />            Esse script pode ser usado para excluir a configuração correspondente com uma opção de desfazer.<br />-   **EnterpriseConfig.psd1**<br />    Um arquivo de configuração de modelo para o gateway do site a site corporativo e a configuração de VM do cliente.|  
   |TenantApps|Arquivos usados para implantar cargas de trabalho de locatário de exemplo.|  
   ---
  
6. Verifique se o arquivo VHDX do Windows Server 2016 está na **imagens** pasta.  
  
7. Personalizar o arquivo SDNExpress\scripts\FabricConfig.psd1 alterando a **<< substituir >>** marcas com valores específicos de acordo com sua infra-estrutura de laboratório, incluindo nomes de host, nomes de domínio, nomes de usuário e senhas, e informações de rede para as redes listadas no tópico de planejamento de rede.  

8. Crie um registro Host A no DNS para o NetworkControllerRestIP e NetworkControllerRestName (FQDN).  

9. Execute o script como um usuário com credenciais de administrador de domínio:  
      
   ``SDNExpress\scripts\SDNExpress.ps1 -ConfigurationDataFile FabricConfig.psd1 -Verbose``  
      
10. Para desfazer todas as operações, execute o seguinte comando:  
      
    ``SDNExpress\scripts\SDNExpressUndo.ps1 -ConfigurationDataFile FabricConfig.psd1 -Verbose``  
      
#### <a name="validation"></a>Validação  

Supondo que o script SDN Express executou até a conclusão sem relatar erros, você pode executar a etapa a seguir para garantir que os recursos de malha tem sido implantados corretamente e estão disponíveis para implantação do locatário.  

Use [ferramentas de diagnóstico](https://docs.microsoft.com/windows-server/networking/sdn/troubleshoot/troubleshoot-windows-server-software-defined-networking-stack) para garantir que não há nenhum erro em quaisquer recursos de malha no controlador de rede.  
      
   ``Debug-NetworkControllerConfigurationState -NetworkController <FQDN of Network Controller Rest Name>``  
        
   
### <a name="deploy-a-sample-tenant-workload-with-the-software-load-balancer"></a>Implantar uma carga de trabalho de locatário de exemplo com o balanceador de carga de software  
    
Agora que os recursos de malha tem sido implantados, você pode validar seu SDN implantação ponta a ponta ao implantar uma carga de trabalho de locatário de exemplo. Essa carga de trabalho de locatário consiste em duas sub-redes virtuais (camada da web e camada de banco de dados) protegidas por meio de regras de lista de controle de acesso (ACL) usando o firewall distribuído de SDN. A sub-rede virtual da camada da web é acessível por meio do SLB/MUX usando um endereço IP Virtual (VIP). O script implanta duas máquinas virtuais da camada da web e a máquina de virtual de camada de um banco de dados e conecta-se para as sub-redes virtuais automaticamente.  
  
1.  Personalizar o arquivo SDNExpress\scripts\TenantConfig.psd1 alterando a **<< substituir >>** marcas com valores específicos (por exemplo: Nome da imagem VHD, nome REST do controlador de rede, nome do vSwitch, etc. definido anteriormente no arquivo FabricConfig.psd1)  

2.  Execute o script. Por exemplo:   

    ``SDNExpress\scripts\SDNExpressTenant.ps1 -ConfigurationDataFile TenantConfig.psd1 -Verbose``  

3.  Para desfazer a configuração, execute o mesmo script com o **desfazer** parâmetro. Por exemplo:  

    ``SDNExpress\scripts\SDNExpressTenant.ps1 -Undo -ConfigurationDataFile TenantConfig.psd1 -Verbose``  

#### <a name="validation"></a>Validação  

Para validar que a implantação do locatário foi bem-sucedida, faça o seguinte:

1. Faça logon na máquina virtual da camada do banco de dados e tente executar ping no endereço IP de uma das máquinas de virtuais de camada da web (certifique-se de que o Firewall do Windows estiver desativado nas máquinas virtuais da camada da web).  

2. Verifique os recursos do locatário do controlador de rede para todos os erros. Execute o seguinte em qualquer host Hyper-V com conectividade de camada 3 para o controlador de rede:  
      
   ``Debug-NetworkControllerConfigurationState -NetworkController <FQDN of Network Controller REST Name>``

3. Para verificar o balanceador de carga está sendo executado corretamente, execute o seguinte de qualquer host Hyper-V:
    
   ``wget <VIP IP address>/unique.htm -disablekeepalive -usebasicparsing``
   
   onde `<VIP IP address>` é o endereço IP VIP configurado no arquivo TenantConfig.psd1 da camada da web. 

   >[!TIP]
   >Pesquise o `VIPIP` variável TenantConfig.psd1.

   Execute este vezes vários para ver o balanceador de carga alternar entre as quedas disponíveis. Você também pode observar esse comportamento usando um navegador da web. Navegue para `<VIP IP address>/unique.htm`. Feche o navegador e abra uma nova instância e procure novamente. Você verá a página azul e a página verde alternativa, exceto quando o navegador armazena em cache a página antes que o cache expire.

---