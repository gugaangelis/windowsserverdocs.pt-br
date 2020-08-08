---
title: Implantar uma infraestrutura de rede definida pelo software usando scripts
description: Este tópico aborda como implantar uma infraestrutura de SDN (rede definida pelo software) da Microsoft usando scripts no Windows Server 2016.
manager: grcusanz
ms.topic: get-started-article
ms.assetid: 5ba5bb37-ece0-45cb-971b-f7149f658d19
ms.author: anpaul
author: AnirbanPaul
ms.date: 08/23/2018
ms.openlocfilehash: 7fcf8b095479ec21c045a60244917b09883a6162
ms.sourcegitcommit: 68444968565667f86ee0586ed4c43da4ab24aaed
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/07/2020
ms.locfileid: "87993771"
---
# <a name="deploy-a-software-defined-network-infrastructure-using-scripts"></a>Implantar uma infraestrutura de rede definida pelo software usando scripts

>Aplica-se a: Windows Server (canal semestral), Windows Server 2016 neste tópico, você implanta uma infraestrutura de SDN (rede definida pelo software) da Microsoft usando scripts. A infraestrutura inclui um controlador de rede altamente disponível (HA), um Load Balancer de software de alta disponibilidade (SLB)/MUX, redes virtuais e ACLs (listas de controle de acesso) associadas. Além disso, outro script implanta uma carga de trabalho de locatário para validar sua infraestrutura de SDN.

Se você quiser que suas cargas de trabalho de locatário se comuniquem fora de suas redes virtuais, você pode configurar regras de NAT de SLB, túneis de gateway de site a site ou encaminhamento de camada 3 para rotear entre cargas de trabalho físicas e virtuais.

Você também pode implantar uma infraestrutura SDN usando o Virtual Machine Manager (VMM). Para obter mais informações, consulte [Configurar uma infraestrutura de Sdn (rede definida pelo software) na malha do VMM](/system-center/vmm/deploy-sdn?view=sc-vmm-2019).

## <a name="pre-deployment"></a>Pré-implantação

> [!IMPORTANT]
> Antes de começar a implantação, você deve planejar e configurar os hosts e a infraestrutura da rede física. Para obter mais informações, confira [Planejar a infraestrutura de rede definida por software](../../sdn/plan/Plan-a-Software-Defined-Network-Infrastructure.md).

Todos os hosts Hyper-V devem ter o Windows Server 2016 instalado.

## <a name="deployment-steps"></a>Etapas de implantação.
Comece Configurando o comutador virtual do Hyper-v (servidores físicos) e a atribuição de endereço IP do host Hyper-V. Qualquer tipo de armazenamento compatível com o Hyper-V, compartilhado ou local pode ser usado.

### <a name="install-host-networking"></a>Instalar rede do host

1. Instale os drivers de rede mais recentes disponíveis para o hardware da NIC.
2. Instale a função Hyper-V em todos os hosts (para obter mais informações, consulte [introdução ao Hyper-v no Windows Server 2016](../../../virtualization/hyper-v/get-started/get-started-with-hyper-v-on-windows.md).

   ```PowerShell
   Install-WindowsFeature -Name Hyper-V -ComputerName <computer_name> -IncludeManagementTools -Restart
   ```

3. Crie o comutador virtual do Hyper-V.<p>Use o mesmo nome de comutador para todos os hosts, por exemplo, **sdnSwitch**. Configure pelo menos um adaptador de rede ou, se estiver usando SET, configure pelo menos dois adaptadores de rede. A difusão de entrada máxima ocorre ao usar duas NICs.

   ```PowerShell
   New-VMSwitch "<switch name>" -NetAdapterName "<NetAdapter1>" [, "<NetAdapter2>" -EnableEmbeddedTeaming $True] -AllowManagementOS $True
   ```

   >[!TIP]
   >Você pode ignorar as etapas 4 e 5 se tiver NICs de gerenciamento separadas.

3. Consulte o tópico de planejamento ([planejar uma infraestrutura de rede definida pelo software](../../sdn/plan/../../sdn/plan/../../sdn/plan/Plan-a-Software-Defined-Network-Infrastructure.md)) e trabalhe com o administrador da rede para obter a ID de VLAN da VLAN de gerenciamento. Anexe o vNIC de gerenciamento do comutador virtual criado recentemente à VLAN de gerenciamento. Esta etapa poderá ser omitida se o ambiente não usar marcas de VLAN.

   ```PowerShell
   Set-VMNetworkAdapterIsolation -ManagementOS -IsolationMode Vlan -DefaultIsolationID <Management VLAN> -AllowUntaggedTraffic $True
   ```

4. Consulte o tópico de planejamento ([planejar uma infraestrutura de rede definida pelo software](../../sdn/plan/../../sdn/plan/../../sdn/plan/Plan-a-Software-Defined-Network-Infrastructure.md)) e trabalhe com o administrador da rede para usar as atribuições de IP estático ou DHCP para atribuir um endereço IP ao vNIC de gerenciamento do vSwitch recém-criado. O exemplo a seguir mostra como criar um endereço IP estático e atribuí-lo ao vNIC de gerenciamento do vSwitch:

   ```PowerShell
   New-NetIPAddress -InterfaceAlias "vEthernet (<switch name>)" -IPAddress <IP> -DefaultGateway <Gateway IP> -AddressFamily IPv4 -PrefixLength <Length of Subnet Mask - for example: 24>
   ```

5. Adicional Implante uma máquina virtual no host Active Directory Domain Services ([instale Active Directory Domain Services (nível 100)](../../../identity/ad-ds/deploy/install-active-directory-domain-services--level-100-.md) e um servidor DNS.

    a. Conecte a máquina virtual do servidor Active Directory/DNS à VLAN de gerenciamento:

      ```PowerShell
      Set-VMNetworkAdapterIsolation -VMName "<VM Name>" -Access -VlanId <Management VLAN> -AllowUntaggedTraffic $True
      ```

   b. Instale o Active Directory Domain Services e o DNS.

   >[!NOTE]
   >O controlador de rede dá suporte a certificados Kerberos e X. 509 para autenticação. Este guia usa os dois mecanismos de autenticação para finalidades diferentes (embora apenas um seja necessário).

6. Ingresse todos os hosts Hyper-V no domínio. Verifique se a entrada do servidor DNS para o adaptador de rede que tem um endereço IP atribuído à rede de gerenciamento aponta para um servidor DNS que pode resolver o nome de domínio.

   ```PowerShell
   Set-DnsClientServerAddress -InterfaceAlias "vEthernet (<switch name>)" -ServerAddresses <DNS Server IP>
   ```

   a. Clique com o botão direito do mouse em **Iniciar**, clique em **sistema**e em **alterar configurações**.
   b. Clique em **Alterar**.
   c. Clique em **domínio** e especifique o nome de domínio.  "" "d. Clique em **OK**.
   e. Digite as credenciais de nome de usuário e senha quando solicitado.
   f. Reinicie o servidor.

### <a name="validation"></a>Validação
Use as etapas a seguir para validar que a rede do host está configurada corretamente.

1. Verifique se o comutador de VM foi criado com êxito:

   ```PowerShell
   Get-VMSwitch "<switch name>"
   ```

2. Verifique se o vNIC de gerenciamento no comutador de VM está conectado à VLAN de gerenciamento:

   >[!NOTE]
   >Relevante somente se o gerenciamento e o tráfego do locatário compartilharem a mesma NIC.

   ```PowerShell
   Get-VMNetworkAdapterIsolation -ManagementOS
   ```

3. Valide todos os hosts Hyper-V e recursos de gerenciamento externo, por exemplo, servidores DNS.<p>Verifique se eles estão acessíveis por meio de ping usando seu endereço IP de gerenciamento e/ou FQDN (nome de domínio totalmente qualificado).

   ``ping <Hyper-V Host IP>``
   ``ping <Hyper-V Host FQDN>``

4. Execute o comando a seguir no host de implantação e especifique o FQDN de cada host Hyper-V para garantir que as credenciais Kerberos usadas forneçam acesso a todos os servidores.

   ``winrm id -r:<Hyper-V Host FQDN>``

### <a name="nano-installation-requirements-and-notes"></a>Observações e requisitos de instalação do nano

Se você usar o nano como seus hosts do Hyper-V (servidores físicos) para a implantação, os requisitos adicionais a seguir serão:

1. Todos os nós do nano precisam ter o pacote DSC instalado com o pacote de idiomas:

   - Microsoft-NanoServer-DSC-Package.cab
   - Microsoft-NanoServer-DSC-Package_en-us.cab

     ``dism /online /add-package /packagepath:<Path> /loglevel:4``

2. Os scripts do SDN Express devem ser executados de um host não nano (Windows Server Core ou Windows Server c/GUI). Não há suporte para fluxos de trabalho do PowerShell no nano.

3. Invocar a API NorthBound do controlador de rede usando os wrappers do PowerShell ou do NC REST (que dependem de Invoke-WebRequest e Invoke-RestMethod) deve ser feito em um host não nano.

### <a name="run-sdn-express-scripts"></a>Executar scripts do SDN Express

1. Vá para o [repositório GitHub do Microsoft Sdn](https://github.com/Microsoft/SDN.git) para os arquivos de instalação.

2. Baixe os arquivos de instalação do repositório para o computador de implantação designado. Clique em **clonar ou baixar** e, em seguida, clique em **baixar zip**.

   >[!NOTE]
   >O computador de implantação designado deve estar executando o Windows Server 2016 ou posterior.

3. Expanda o arquivo zip e copie a pasta **SDNExpress** para a pasta do computador de implantação `C:\` .

4. Compartilhe a `C:\SDNExpress` pasta como "**SDNExpress**" com permissão para **todos** para **leitura/gravação**.

5. Navegue até a pasta `C:\SDNExpress`.<p>Você verá as seguintes pastas:

   | Nome da Pasta | Descrição |
   |--|--|
   | AgentConf | Contém cópias atualizadas dos esquemas OVSDB usados pelo agente de host SDN em cada host do Hyper-V do Windows Server 2016 para programar a política de rede. |
   | Certificados | Local compartilhado temporário para o arquivo de certificado NC. |
   | Imagens | Vazio, coloque a imagem do vhdx do Windows Server 2016 aqui |
   | Ferramentas | Utilitários para solução de problemas e depuração.  Copiado para os hosts e máquinas virtuais.  Recomendamos que você coloque Monitor de Rede ou o Wireshark aqui para que ele esteja disponível, se necessário. |
   | Scripts | Scripts de implantação.<p>- **SDNExpress.ps1**<br>Implanta e configura a malha, incluindo as máquinas virtuais do controlador de rede, máquinas virtuais SLB MUX, pools de gateway e as máquinas virtuais do gateway HNV correspondentes aos pools.<br />-   **FabricConfig.psd1**<br>Um modelo de arquivo de configuração para o script SDNExpress.  Você personalizará isso para o seu ambiente.<br />-   **SDNExpressTenant.ps1**<br>Implanta uma carga de trabalho de locatário de exemplo em uma rede virtual com um VIP com balanceamento de carga.<br>O também provisiona uma ou mais conexões de rede (VPN S2S IPSec, GRE, L3) nos gateways de borda do provedor de serviços que estão conectados à carga de trabalho de locatário criada anteriormente. Os gateways IPSec e GRE estão disponíveis para conectividade sobre o endereço IP VIP correspondente e o gateway de encaminhamento L3 sobre o pool de endereços correspondente.<br>Esse script também pode ser usado para excluir a configuração correspondente com uma opção de desfazer.<br />- **TenantConfig.psd1**<br>Um arquivo de configuração de modelo para carga de trabalho de locatário e configuração de gateway S2S.<br />- **SDNExpressUndo.ps1**<br>Limpa o ambiente de malha e o redefine para um estado inicial.<br />- **SDNExpressEnterpriseExample.ps1**<br>Provisiona um ou mais ambientes de site corporativo com um gateway de acesso remoto e (opcionalmente) uma máquina virtual corporativa correspondente por site. Os gateways corporativos IPSec ou GRE se conectam ao endereço IP VIP correspondente do gateway do provedor de serviços para estabelecer os túneis S2S. O gateway de encaminhamento L3 se conecta ao endereço IP do par correspondente. <br> Esse script também pode ser usado para excluir a configuração correspondente com uma opção de desfazer.<br />- **EnterpriseConfig.psd1**<br>Um arquivo de configuração de modelo para a configuração de VM de cliente e gateway de site a site corporativo. |
   | TenantApps | Arquivos usados para implantar cargas de trabalho de locatário de exemplo. |

6. Verifique se o arquivo VHDX do Windows Server 2016 está na pasta **imagens** .

7. Personalize o arquivo SDNExpress\scripts\FabricConfig.psd1 alterando as marcas **<< substituir >>** com valores específicos para se ajustar à sua infraestrutura de laboratório, incluindo nomes de host, nomes de domínio, nomes de dados e senhas e informações de rede para as redes listadas no tópico Planejamento de rede.

8. Crie um registro de host A no DNS para o NetworkControllerRestName (FQDN) e NetworkControllerRestIP.

9. Execute o script como um usuário com credenciais de administrador de domínio:

   ``SDNExpress\scripts\SDNExpress.ps1 -ConfigurationDataFile FabricConfig.psd1 -Verbose``

10. Para desfazer todas as operações, execute o seguinte comando:

    ``SDNExpress\scripts\SDNExpressUndo.ps1 -ConfigurationDataFile FabricConfig.psd1 -Verbose``

#### <a name="validation"></a>Validação

Supondo que o script SDN Express foi executado até a conclusão sem relatar erros, você pode executar a etapa a seguir para garantir que os recursos de malha tenham sido implantados corretamente e estejam disponíveis para implantação de locatário.

Use [ferramentas de diagnóstico](../troubleshoot/troubleshoot-windows-server-software-defined-networking-stack.md) para garantir que não haja nenhum erro em nenhum recurso de malha no controlador de rede.

   ``Debug-NetworkControllerConfigurationState -NetworkController <FQDN of Network Controller Rest Name>``


### <a name="deploy-a-sample-tenant-workload-with-the-software-load-balancer"></a>Implantar uma carga de trabalho de locatário de exemplo com o balanceador de carga de software

Agora que os recursos de malha foram implantados, você pode validar sua implantação de SDN de ponta a ponta implantando uma carga de trabalho de locatário de exemplo. Essa carga de trabalho de locatário consiste em duas sub-redes virtuais (camada da Web e camada de banco de dados) protegidas por meio de regras de ACL (lista de controle de acesso) usando o firewall distribuído do SDN. A sub-rede virtual da camada da Web pode ser acessada por meio do SLB/MUX usando um endereço IP virtual (VIP). O script implanta automaticamente duas máquinas virtuais de camada da Web e uma máquina virtual de camada de banco de dados e as conecta às sub-redes virtuais.

1.  Personalize o arquivo SDNExpress\scripts\TenantConfig.psd1 alterando as marcas **<< substituir >>** com valores específicos (por exemplo: nome da imagem do VHD, nome de REST do controlador de rede, nome do vSwitch, etc. conforme definido anteriormente no arquivo FabricConfig.psd1)

2.  Execute o script. Por exemplo:

    ``SDNExpress\scripts\SDNExpressTenant.ps1 -ConfigurationDataFile TenantConfig.psd1 -Verbose``

3.  Para desfazer a configuração, execute o mesmo script com o parâmetro **Undo** . Por exemplo:

    ``SDNExpress\scripts\SDNExpressTenant.ps1 -Undo -ConfigurationDataFile TenantConfig.psd1 -Verbose``

#### <a name="validation"></a>Validação

Para validar se a implantação do locatário foi bem-sucedida, faça o seguinte:

1. Faça logon na máquina virtual da camada de banco de dados e tente executar o ping no endereço IP de uma das máquinas virtuais da camada da Web (verifique se o Firewall do Windows está desativado em máquinas virtuais da camada da Web).

2. Verifique se há erros nos recursos de locatário do controlador de rede. Execute o seguinte em qualquer host Hyper-V com conectividade de camada 3 para o controlador de rede:

   ``Debug-NetworkControllerConfigurationState -NetworkController <FQDN of Network Controller REST Name>``

3. Para verificar se o balanceador de carga está sendo executado corretamente, execute o seguinte em qualquer host Hyper-V:

   ``wget <VIP IP address>/unique.htm -disablekeepalive -usebasicparsing``

   em que `<VIP IP address>` é o endereço IP VIP da camada da Web que você configurou no arquivo TenantConfig.psd1.

   >[!TIP]
   >Procure a `VIPIP` variável no TenantConfig.psd1.

   Execute este vários vezes para ver a alternância do balanceador de carga entre os DIPs disponíveis. Você também pode observar esse comportamento usando um navegador da Web. Navegue até `<VIP IP address>/unique.htm`. Feche o brower e abra uma nova instância e navegue novamente. Você verá a página azul e a página verde alternativa, exceto quando o navegador armazenar a página em cache antes do tempo limite do cache.