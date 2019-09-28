---
title: Segurança do controlador de rede
description: Você pode usar este tópico para aprender a configurar a segurança de toda a comunicação entre o controlador de rede e outros dispositivos e software.
manager: dougkim
ms.prod: windows-server
ms.technology: networking-sdn
ms.topic: article
ms.assetid: bc625de9-ee31-40a4-9ad2-7448bfbfb6e6
ms.author: pashort
author: shortpatti
ms.date: 08/30/2018
ms.openlocfilehash: bd44b4d696fef3c167c7bcd4ffbc7ca79009cebc
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71405972"
---
# <a name="secure-the-network-controller"></a>Proteger o controlador de rede

Neste tópico, você aprenderá a configurar a segurança para toda a comunicação entre o [controlador de rede](../technologies/network-controller/network-controller.md) e outros softwares e dispositivos. 

Os caminhos de comunicação que você pode proteger incluem comunicação Northbound no plano de gerenciamento, comunicação de cluster entre VMs \(\) de máquinas virtuais de controlador de rede em um cluster e comunicação Southbound nos dados aérea.

1. **Comunicação Northbound**. O controlador de rede se comunica no plano de gerenciamento\-com software de gerenciamento compatível com Sdn, \(como\)o Windows PowerShell e System Center Virtual Machine Manager SCVMM. Essas ferramentas de gerenciamento fornecem a capacidade de definir a diretiva de rede e criar um estado de meta para a rede, no qual você pode comparar a configuração de rede real para colocar a configuração real em paridade com o estado da meta.

2. **Comunicação de cluster do controlador de rede**. Quando você configura três ou mais VMs como nós de cluster do controlador de rede, esses nós se comunicam entre si. Essa comunicação pode estar relacionada à sincronização e à replicação de dados entre nós ou comunicação específica entre os serviços do controlador de rede.

3.  **Comunicação Southbound**. O controlador de rede se comunica no plano de dados com a infraestrutura de SDN e outros dispositivos, como balanceadores de carga de software, gateways e máquinas de host. Você pode usar o controlador de rede para configurar e gerenciar esses dispositivos Southbound para que eles mantenham o estado da meta que você configurou para a rede.


## <a name="northbound-communication"></a>Comunicação Northbound

O controlador de rede dá suporte à autenticação, autorização e criptografia para comunicação northbound. As seções a seguir fornecem informações sobre como definir essas configurações de segurança.

### <a name="authentication"></a>Autenticação

Ao configurar a autenticação para a comunicação Northbound do controlador de rede, você permite que nós de cluster do controlador de rede e clientes de gerenciamento verifiquem a identidade do dispositivo com o qual estão se comunicando.

O controlador de rede dá suporte aos três modos de autenticação a seguir entre os clientes de gerenciamento e os nós do controlador de rede.

>[!Note]
>Se você estiver implantando o controlador de rede com o System Center Virtual Machine Manager, somente o modo **Kerberos** terá suporte.

1. **Kerberos**. Use a autenticação Kerberos ao unir o cliente de gerenciamento e todos os nós de cluster do controlador de rede a um domínio Active Directory. O domínio de Active Directory deve ter contas de domínio usadas para autenticação.

2. **X509**. Use o X509 para\-autenticação baseada em certificado para clientes de gerenciamento que não ingressaram em um domínio Active Directory. Você deve registrar certificados para todos os nós de cluster do controlador de rede e clientes de gerenciamento. Além disso, todos os nós e clientes de gerenciamento devem confiar nos certificados dos outros.

3. **Nenhum**. Use nenhum para fins de teste em um ambiente de teste e, portanto, não é recomendado para uso em um ambiente de produção. Quando você escolhe esse modo, não há nenhuma autenticação executada entre nós e clientes de gerenciamento.

Você pode configurar o modo de autenticação para comunicação Northbound usando o comando do Windows PowerShell **[install-NetworkController](https://technet.microsoft.com/itpro/powershell/windows/network-controller/install-networkcontroller)** com o parâmetro _clientauthentication válido_ . 


### <a name="authorization"></a>Autorização

Ao configurar a autorização para a comunicação Northbound do controlador de rede, você permite que nós de cluster do controlador de rede e clientes de gerenciamento verifiquem se o dispositivo com o qual estão se comunicando é confiável e tem permissão para participar do comunicação.

Use os métodos de autorização a seguir para cada um dos modos de autenticação com suporte do controlador de rede.

1.  **Kerberos**. Ao usar o método de autenticação Kerberos, você define os usuários e computadores autorizados a se comunicar com o controlador de rede criando um grupo de segurança no Active Directory e, em seguida, adicionando os usuários e computadores autorizados ao grupo. Você pode configurar o controlador de rede para usar o grupo de segurança para autorização usando o parâmetro _ClientSecurityGroup_ do comando **[install-NetworkController](https://technet.microsoft.com/itpro/powershell/windows/network-controller/install-networkcontroller)** do Windows PowerShell. Depois de instalar o controlador de rede, você pode alterar o grupo de segurança usando o comando **[set-NetworkController](https://technet.microsoft.com/itpro/powershell/windows/network-controller/set-networkcontroller)** com o parâmetro _-ClientSecurityGroup_. Se estiver usando o SCVMM, você deverá fornecer o grupo de segurança como um parâmetro durante a implantação.

2.  **X509**. Quando você estiver usando o método de autenticação X509, o controlador de rede só aceita solicitações de clientes de gerenciamento cujas impressões digitais de certificado são conhecidas pelo controlador de rede. Você pode configurar essas impressões digitais usando o parâmetro _ClientCertificateThumbprint_ do comando **[install-NetworkController](https://technet.microsoft.com/itpro/powershell/windows/network-controller/install-networkcontroller)** do Windows PowerShell. Você pode adicionar outras impressões digitais do cliente a qualquer momento usando o comando **[set-NetworkController](https://technet.microsoft.com/itpro/powershell/windows/network-controller/set-networkcontroller)** .

3.  **Nenhum**. Quando você escolhe esse modo, não há nenhuma autenticação executada entre nós e clientes de gerenciamento. Use nenhum para fins de teste em um ambiente de teste e, portanto, não é recomendado para uso em um ambiente de produção. 


### <a name="encryption"></a>Criptografia

A comunicação Northbound usa \(protocolo SSL\) SSL para criar um canal criptografado entre os clientes de gerenciamento e os nós do controlador de rede. A criptografia SSL para comunicação Northbound inclui os seguintes requisitos:

- Todos os nós do controlador de rede devem ter um certificado idêntico que inclua a autenticação do servidor e as finalidades \(de\) autenticação do cliente nas extensões EKU do uso avançado de chave. 

- O URI usado pelos clientes de gerenciamento para se comunicar com o controlador de rede deve ser o nome da entidade do certificado. O nome da entidade do certificado deve conter o nome de domínio totalmente qualificado (FQDN) ou o endereço IP do ponto de extremidade REST do controlador de rede.

- Se os nós do controlador de rede estiverem em sub-redes diferentes, o nome da entidade de seus certificados deverá ser o mesmo que o valor usado para o parâmetro _REST_ no comando **install-NetworkController** do Windows PowerShell. 

- Todos os clientes de gerenciamento devem confiar no certificado SSL.


### <a name="ssl-certificate-enrollment-and-configuration"></a>Registro e configuração de certificado SSL

Você deve registrar manualmente o certificado SSL em nós do controlador de rede.

Depois que o certificado for registrado, você poderá configurar o controlador de rede para usar o certificado com o parâmetro **-serverCertificate** do comando **install-NetworkController** do Windows PowerShell. Se você já tiver instalado o controlador de rede, poderá atualizar a configuração a qualquer momento usando o comando **set-NetworkController** .

>[!NOTE]
>Se você estiver usando o SCVMM, deverá adicionar o certificado como um recurso de biblioteca. Para obter mais informações, consulte [configurar um controlador de rede Sdn na malha do VMM](https://technet.microsoft.com/system-center-docs/vmm/scenario/sdn-network-controller).

## <a name="network-controller-cluster-communication"></a>Comunicação de cluster do controlador de rede

O controlador de rede dá suporte à autenticação, autorização e criptografia para comunicação entre nós do controlador de rede. A comunicação é sobre [Windows Communication Foundation](https://msdn.microsoft.com/library/ms731082.aspx) \(WCF\) e TCP.

Você pode configurar esse modo com o parâmetro **ClusterAuthentication** do comando **install-NetworkControllerCluster** do Windows PowerShell. 

Para obter mais informações, consulte [install-NetworkControllerCluster](https://technet.microsoft.com/itpro/powershell/windows/network-controller/install-networkcontrollercluster).

### <a name="authentication"></a>Autenticação

Ao configurar a autenticação para comunicação de cluster do controlador de rede, você permite que nós de cluster do controlador de rede verifiquem a identidade dos outros nós com os quais estão se comunicando.

O controlador de rede dá suporte aos três modos de autenticação a seguir entre os nós do controlador de rede.

>[!NOTE]
>Se você implantar o controlador de rede usando o SCVMM, somente o modo **Kerberos** terá suporte.

1. **Kerberos**. Você pode usar a autenticação Kerberos quando todos os nós de cluster do controlador de rede são ingressados em um domínio Active Directory, com contas de domínio usadas para autenticação.

2. **X509**. O X509 é\-uma autenticação baseada em certificado. Você pode usar a autenticação X509 quando os nós de cluster do controlador de rede não tiverem ingressado em um domínio Active Directory. Para usar o X509, você deve registrar certificados em todos os nós de cluster do controlador de rede e todos os nós devem confiar nos certificados. Além disso, o nome da entidade do certificado registrado em cada nó deve ser o mesmo que o nome DNS do nó.

3. **Nenhum**. Quando você escolhe esse modo, não há nenhuma autenticação executada entre nós do controlador de rede. Esse modo é fornecido apenas para fins de teste e não é recomendado para uso em um ambiente de produção.

### <a name="authorization"></a>Autorização

Ao configurar a autorização para comunicação de cluster do controlador de rede, você permite que os nós de cluster do controlador de rede verifiquem se os nós com os quais estão se comunicando são confiáveis e têm permissão para participar da comunicação.

Para cada um dos modos de autenticação com suporte do controlador de rede, os métodos de autorização a seguir são usados.

1. **Kerberos**. Os nós do controlador de rede aceitam solicitações de comunicação somente de outras contas de computador do controlador de rede. Você pode configurar essas contas ao implantar o controlador de rede usando o parâmetro **Name** do comando [New-NetworkControllerNodeObject](https://technet.microsoft.com/itpro/powershell/windows/network-controller/new-networkcontrollernodeobject) do Windows PowerShell.

2. **X509**. Os nós do controlador de rede aceitam solicitações de comunicação somente de outras contas de computador do controlador de rede. Você pode configurar essas contas ao implantar o controlador de rede usando o parâmetro **Name** do comando [New-NetworkControllerNodeObject](https://technet.microsoft.com/itpro/powershell/windows/network-controller/new-networkcontrollernodeobject) do Windows PowerShell.

3. **Nenhum**. Quando você escolhe esse modo, não há nenhuma autorização executada entre nós do controlador de rede. Esse modo é fornecido apenas para fins de teste e não é recomendado para uso em um ambiente de produção.

### <a name="encryption"></a>Criptografia

A comunicação entre nós do controlador de rede é criptografada usando a criptografia do nível de transporte do WCF. Essa forma de criptografia é usada quando os métodos de autenticação e autorização são certificados Kerberos ou X509. Para obter mais informações, consulte estes tópicos.

- [Como: Proteger um serviço com credenciais do Windows](https://msdn.microsoft.com/library/ms734673.aspx)
- [Como: Proteger um serviço com certificados](https://msdn.microsoft.com/library/ms788968.aspx)X. 509.

## <a name="southbound-communication"></a>Comunicação Southbound

O controlador de rede interage com diferentes tipos de dispositivos para comunicação Southbound. Essas interações usam protocolos diferentes. Por isso, há requisitos diferentes para autenticação, autorização e criptografia, dependendo do tipo de dispositivo e protocolo usados pelo controlador de rede para se comunicar com o dispositivo.

A tabela a seguir fornece informações sobre a interação do controlador de rede com diferentes dispositivos Southbound.

| Dispositivo/serviço Southbound | Protocol              | Autenticação usada    |
|---------------------------|-----------------------|------------------------|
| Balanceador de Carga de Software    | WCF (MUX), TCP (host) | Certificados           |
| Firewall                  | OVSDB                 | Certificados           |
| Gateway                   | WinRM                 | Kerberos, certificados |
| Rede virtual        | OVSDB, WCF            | Certificados           |
| Roteamento definido pelo usuário      | OVSDB                 | Certificados           |

Para cada um desses protocolos, o mecanismo de comunicação é descrito na seção a seguir.

### <a name="authentication"></a>Autenticação

Para a comunicação Southbound, são usados os seguintes protocolos e métodos de autenticação.

1. **WCF/TCP/OVSDB**. Para esses protocolos, a autenticação é executada usando certificados X509. Tanto o controlador de rede quanto o par de \(balanceamento\) de carga de\)software de pares SLB multiplexador \(MUX máquinas de e/ou computadores de mesmo nível, apresentam seus certificados para autenticação mútua. Cada certificado deve ser confiável pelo par remoto.

    Para a autenticação Southbound, você pode usar o mesmo certificado SSL configurado para criptografar a comunicação com os clientes do northbound. Você também deve configurar um certificado no MUX SLB e em dispositivos host. O nome da entidade do certificado deve ser o mesmo que o nome DNS do dispositivo.

2. **WinRM**. Para esse protocolo, a autenticação é executada usando o \(Kerberos para computadores\) ingressados no domínio e \(usando certificados para computadores\)não ingressados no domínio.

### <a name="authorization"></a>Autorização

Para a comunicação Southbound, são usados os seguintes protocolos e métodos de autorização.

1. **WCF/TCP**. Para esses protocolos, a autorização baseia-se no nome da entidade do par. O controlador de rede armazena o nome DNS do dispositivo par e o usa para autorização. Esse nome DNS deve corresponder ao nome da entidade do dispositivo no certificado. Da mesma forma, o certificado do controlador de rede deve corresponder ao nome DNS do controlador de rede armazenado no dispositivo par.

2. **WinRM**. Se o Kerberos estiver sendo usado, a conta do cliente WinRM deverá estar presente em um grupo predefinido no Active Directory ou no grupo local de administradores no servidor. Se os certificados estiverem sendo usados, o cliente apresentará um certificado ao servidor que o servidor autoriza usando o nome/emissor da entidade e o servidor usará uma conta de usuário mapeada para executar a autenticação.

3.  **OVSDB**. Não há autorização fornecida para esse protocolo.

### <a name="encryption"></a>Criptografia

Para a comunicação Southbound, os métodos de criptografia a seguir são usados para protocolos.

1. **WCF/TCP/OVSDB**. Para esses protocolos, a criptografia é executada usando o certificado registrado no cliente ou servidor.

2. **WinRM**. O tráfego do WinRM é criptografado por padrão usando o \(SSP\)do provedor de suporte de segurança Kerberos. Você pode configurar a criptografia adicional, na forma de SSL, no servidor WinRM.
