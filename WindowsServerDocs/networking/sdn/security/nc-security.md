---
title: Segurança do controlador de rede
description: Você pode usar este tópico para aprender a configurar a segurança de toda a comunicação entre o controlador de rede e outros softwares e dispositivos.
manager: dougkim
ms.prod: windows-server-threshold
ms.technology: networking-sdn
ms.topic: article
ms.assetid: bc625de9-ee31-40a4-9ad2-7448bfbfb6e6
ms.author: pashort
author: shortpatti
ms.date: 08/30/2018
ms.openlocfilehash: fccd6ee1ba734d72629264bd5b3bb7d93ddcdb70
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59884757"
---
# <a name="secure-the-network-controller"></a>Proteger o controlador de rede

Neste tópico, você aprenderá a configurar a segurança de toda a comunicação entre [controlador de rede](../technologies/network-controller/network-controller.md) e outros softwares e dispositivos. 

Os caminhos de comunicação que você pode proteger incluem comunicação Northbound no plano de gerenciamento, comunicação de cluster entre máquinas virtuais do controlador de rede \(VMs\) em um cluster e comunicação Southbound nos dados plano.

1. **Comunicação northbound**. Controlador de rede se comunica com o plano de gerenciamento com SDN\-software com capacidade de gerenciamento, como o Windows PowerShell e o System Center Virtual Machine Manager \(SCVMM\). Essas ferramentas de gerenciamento fornecem a capacidade de definir a política de rede e para criar um estado de meta para a rede, em relação ao qual você pode comparar a configuração de rede reais para colocar a configuração real em paridade com o estado de meta.

2. **Comunicação de Cluster de controlador de rede**. Quando você configura três ou mais máquinas virtuais como nós de cluster de controlador de rede, esses nós se comunicam entre si. Essa comunicação pode estar relacionada à sincronização e replicação de dados em nós ou de comunicação específicos entre os serviços de controlador de rede.

3.  **Comunicação southbound**. Controlador de rede se comunica com o plano de dados com a infraestrutura de SDN e outros dispositivos como máquinas de host, gateways e balanceadores de carga de software. Você pode usar o controlador de rede para configurar e gerenciar esses dispositivos southbound para que eles mantêm o estado de meta que você configurou para a rede.


## <a name="northbound-communication"></a>Comunicação northbound

Oferece suporte à autenticação, autorização e criptografia para comunicação Northbound do controlador de rede. As seções a seguir fornecem informações sobre como configurar essas configurações de segurança.

### <a name="authentication"></a>Autenticação

Quando você configurar a autenticação para comunicação Northbound de controlador de rede, você permitir que nós de cluster de controlador de rede e clientes de gerenciamento para verificar a identidade do dispositivo com o qual eles estão se comunicando.

Controlador de rede dá suporte a três seguintes modos de autenticação entre os clientes de gerenciamento e os nós de controlador de rede.

>[!Note]
>Se você estiver implantando o controlador de rede com o System Center Virtual Machine Manager, apenas **Kerberos** modo tem suporte.

1. **Kerberos**. Use a autenticação Kerberos ao unir o cliente de gerenciamento e todos os nós de cluster de controlador de rede em um domínio do Active Directory. Domínio do Active Directory deve ter contas de domínio usadas para autenticação.

2. **X509**. Use X509 para certificados\-com base em autenticação para clientes de gerenciamento não tiver ingressado em um domínio do Active Directory. Você deve registrar certificados para todos os nós de cluster de controlador de rede e clientes de gerenciamento. Além disso, todos os nós e os clientes de gerenciamento devem confiar em certificados uns dos outros.

3. **Nenhum**. Use Nenhum para o teste fins em um ambiente de teste e, portanto, não recomendado para uso em um ambiente de produção. Quando você escolhe esse modo, não há nenhuma autenticação executada entre nós e os clientes de gerenciamento.

Você pode configurar o modo de autenticação para comunicação Northbound usando o comando do Windows PowerShell **[Install NetworkController](https://technet.microsoft.com/itpro/powershell/windows/network-controller/install-networkcontroller)** com o _ClientAuthentication_ parâmetro. 


### <a name="authorization"></a>Autorização

Quando você configura a autorização para comunicação Northbound de controlador de rede, você permite que nós de cluster de controlador de rede e clientes de gerenciamento para verificar se o dispositivo com o qual eles estão se comunicando é confiável e tem permissão para participar de comunicação.

Use os seguintes métodos de autorização para cada um dos modos de autenticação com suporte pelo controlador de rede.

1.  **Kerberos**. Quando você estiver usando o método de autenticação Kerberos, você deve definir os usuários e computadores autorizados a se comunicar com o controlador de rede, criando um grupo de segurança no Active Directory e, em seguida, adicionar os usuários autorizados e os computadores ao grupo. Você pode configurar o controlador de rede para usar o grupo de segurança para autorização usando o _ClientSecurityGroup_ parâmetro do **[Install NetworkController](https://technet.microsoft.com/itpro/powershell/windows/network-controller/install-networkcontroller)** Comando do Windows PowerShell. Depois de instalar o controlador de rede, você pode alterar o grupo de segurança usando o **[NetworkController conjunto](https://technet.microsoft.com/itpro/powershell/windows/network-controller/set-networkcontroller)** comando com o parâmetro _- ClientSecurityGroup_ . Se usando o SCVMM, você deve fornecer o grupo de segurança como um parâmetro durante a implantação.

2.  **X509**. Quando você estiver usando o X509 método de autenticação, o controlador de rede só aceita solicitações de clientes de gerenciamento cujas impressões digitais de certificado são conhecidas por controlador de rede. Você pode configurar essas impressões digitais usando o _ClientCertificateThumbprint_ parâmetro do **[Install NetworkController](https://technet.microsoft.com/itpro/powershell/windows/network-controller/install-networkcontroller)** comando do Windows PowerShell. Você pode adicionar outras as impressões digitais do cliente a qualquer momento usando o **[NetworkController conjunto](https://technet.microsoft.com/itpro/powershell/windows/network-controller/set-networkcontroller)** comando.

3.  **Nenhum**. Quando você escolhe esse modo, não há nenhuma autenticação executada entre nós e os clientes de gerenciamento. Use Nenhum para o teste fins em um ambiente de teste e, portanto, não recomendado para uso em um ambiente de produção. 


### <a name="encryption"></a>Criptografia

Comunicação northbound usa Secure Sockets Layer \(SSL\) para criar um canal criptografado entre clientes de gerenciamento e os nós de controlador de rede. A criptografia SSL para comunicação Northbound inclui os seguintes requisitos:

- Todos os nós de controlador de rede devem ter um certificado idêntico que inclui as finalidades de autenticação de servidor e autenticação de cliente em uso avançado de chave \(EKU\) extensões. 

- O URI usado pelos clientes de gerenciamento para se comunicar com o controlador de rede deve ser o nome de assunto do certificado. O nome de assunto do certificado deve conter o nome de domínio totalmente qualificado (FQDN) ou o endereço IP do ponto de extremidade de REST do controlador de rede.

- Se nós de controlador de rede estiverem em sub-redes diferentes, o nome da entidade de seus certificados deve ser o mesmo que o valor usado para o _RestName_ parâmetro na **Install-NetworkController** Windows Comando do PowerShell. 

- Todos os clientes de gerenciamento devem confiar no certificado SSL.


### <a name="ssl-certificate-enrollment-and-configuration"></a>Configuração e registro de certificado SSL

Você deve registrar manualmente o certificado SSL em nós de controlador de rede.

Depois que o certificado for registrado, você pode configurar o controlador de rede para usar o certificado com o **- ServerCertificate** parâmetro do **Install-NetworkController** comando do Windows PowerShell . Se você já tiver instalado o controlador de rede, você pode atualizar a configuração a qualquer momento usando o **NetworkController conjunto** comando.

>[!NOTE]
>Se você estiver usando o SCVMM, você deve adicionar o certificado como um recurso de biblioteca. Para obter mais informações, consulte [configurar um controlador de rede SDN na malha do VMM](https://technet.microsoft.com/system-center-docs/vmm/scenario/sdn-network-controller).

## <a name="network-controller-cluster-communication"></a>Comunicação de Cluster de controlador de rede

Controlador de rede dá suporte a autenticação, autorização e criptografia para comunicação entre nós de controlador de rede. A comunicação está acima [Windows Communication Foundation](https://msdn.microsoft.com/library/ms731082.aspx) \(WCF\) e TCP.

Você pode configurar esse modo com o **ClusterAuthentication** parâmetro do **Install-NetworkControllerCluster** comando do Windows PowerShell. 

Para obter mais informações, consulte [Install-NetworkControllerCluster](https://technet.microsoft.com/itpro/powershell/windows/network-controller/install-networkcontrollercluster).

### <a name="authentication"></a>Autenticação

Quando você configurar a autenticação para a comunicação de Cluster de controlador de rede, permitem que nós de cluster de controlador de rede verificar a identidade dos outros nós com os quais eles estão se comunicando.

Controlador de rede dá suporte a três seguintes modos de autenticação entre os nós de controlador de rede.

>[!NOTE]
>Se você implanta o controlador de rede usando o SCVMM, apenas **Kerberos** modo tem suporte.

1. **Kerberos**. Você pode usar a autenticação Kerberos quando todos os nós de cluster de controlador de rede são associados a um domínio do Active Directory, contas de domínio usadas para autenticação.

2. **X509**. X509 é certificado\-com base em autenticação. Você pode usar X509 autenticação quando nós de cluster de controlador de rede não fazem parte de um domínio do Active Directory. Para usar X509, você deve registrar certificados para todos os nós de cluster de controlador de rede e todos os nós devem confiar em certificados. Além disso, o nome da entidade do certificado que é registrado em cada nó deve ser o mesmo que o nome DNS do nó.

3. **Nenhum**. Quando você escolhe esse modo, não há nenhuma autenticação seja executada entre os nós de controlador de rede. Esse modo é fornecido somente para fins de teste e não é recomendado para uso em um ambiente de produção.

### <a name="authorization"></a>Autorização

Quando você configura a autorização para comunicação de Cluster de controlador de rede, permitem que nós de cluster de controlador de rede para verificar se os nós com os quais eles estão se comunicando são confiáveis e ter permissão para participar da comunicação.

Para cada um dos modos de autenticação com suporte pelo controlador de rede, os seguintes métodos de autorização são usados.

1. **Kerberos**. Nós de controlador de rede aceitam comunicação somente solicitações de outras contas de computador do controlador de rede. Você pode configurar essas contas quando você implanta um controlador de rede usando o **nome** parâmetro do [New NetworkControllerNodeObject](https://technet.microsoft.com/itpro/powershell/windows/network-controller/new-networkcontrollernodeobject) comando do Windows PowerShell.

2. **X509**. Nós de controlador de rede aceitam comunicação somente solicitações de outras contas de computador do controlador de rede. Você pode configurar essas contas quando você implanta um controlador de rede usando o **nome** parâmetro do [New NetworkControllerNodeObject](https://technet.microsoft.com/itpro/powershell/windows/network-controller/new-networkcontrollernodeobject) comando do Windows PowerShell.

3. **Nenhum**. Quando você escolhe esse modo, não há nenhuma autorização executada entre os nós de controlador de rede. Esse modo é fornecido somente para fins de teste e não é recomendado para uso em um ambiente de produção.

### <a name="encryption"></a>Criptografia

Comunicação entre os nós de controlador de rede é criptografada usando criptografia no nível do transporte WCF. Essa forma de criptografia é usada quando os métodos de autenticação e autorização são Kerberos ou X509 certificados. Para obter mais informações, consulte estes tópicos.

- [Como: Proteger um serviço com credenciais do Windows](https://msdn.microsoft.com/library/ms734673.aspx)
- [Como: Proteger um serviço com certificados x. 509](https://msdn.microsoft.com/library/ms788968.aspx).

## <a name="southbound-communication"></a>Comunicação southbound

Controlador de rede interage com diferentes tipos de dispositivos para a comunicação Southbound. Essas interações usam diferentes protocolos. Por isso, há diferentes requisitos de autenticação, autorização e criptografia, dependendo do tipo de dispositivo e o protocolo usado pelo controlador de rede para se comunicar com o dispositivo.

A tabela a seguir fornece informações sobre a interação do controlador de rede com diferentes dispositivos southbound.

| Dispositivo southbound/serviço | Protocolo              | Autenticação usada    |
|---------------------------|-----------------------|------------------------|
| Balanceador de Carga de Software    | WCF (MUX), TCP (Host) | Certificados           |
| Firewall                  | OVSDB                 | Certificados           |
| Gateway                   | WinRM                 | Kerberos, certificados |
| Rede virtual        | OVSDB, WCF            | Certificados           |
| Roteamento definido pelo usuário      | OVSDB                 | Certificados           |

Para cada um desses protocolos, o mecanismo de comunicação é descrito na seção a seguir.

### <a name="authentication"></a>Autenticação

Para a comunicação Southbound, são usados os seguintes protocolos e métodos de autenticação.

1. **WCF/TCP/OVSDB**. Para esses protocolos, autenticação é realizada usando X509 certificados. Controlador de rede e o par de balanceamento de carga de Software \(SLB\) Multiplexer \(MUX \) /máquinas host apresentam seus certificados uns aos outros para autenticação mútua. Cada certificado deve ser confiável pelo par remoto.

    Para a autenticação southbound, você pode usar o mesmo certificado SSL que é configurado para criptografar a comunicação com os clientes Northbound. Você também deve configurar um certificado no SLB MUX e dispositivos de host. O nome de assunto do certificado deve ser o mesmo que o nome DNS do dispositivo.

2. **WinRM**. Para esse protocolo, a autenticação é realizada usando o Kerberos \(para o domínio computadores associados ao\) e usando certificados \(para fora do domínio computadores associados ao\).

### <a name="authorization"></a>Autorização

Para a comunicação Southbound, são usados os seguintes protocolos e métodos de autorização.

1. **WCF/TCP**. Para esses protocolos, a autorização baseia-se no nome do assunto da entidade do par. Controlador de rede armazena o nome DNS do dispositivo de ponto a ponto e usa-o para autorização. Esse nome DNS deve corresponder ao nome de assunto do dispositivo no certificado. Da mesma forma, o certificado do controlador de rede deve corresponder ao nome de DNS do controlador de rede armazenado no dispositivo de ponto a ponto.

2. **WinRM**. Se Kerberos estiver sendo usado, a conta do cliente WinRM deve estar presente em um grupo predefinido no Active Directory ou no grupo de administradores locais no servidor. Se os certificados estiverem sendo usados, o cliente apresenta um certificado para o servidor que autoriza o servidor usando o nome de assunto/emissor e o servidor usa uma conta de usuário mapeado para realizar a autenticação.

3.  **OVSDB**. Não há nenhuma autorização fornecida para esse protocolo.

### <a name="encryption"></a>Criptografia

Para comunicação Southbound, os seguintes métodos de criptografia são usados para protocolos.

1. **WCF/TCP/OVSDB**. Para esses protocolos, a criptografia é executada usando o certificado que está registrado no cliente ou servidor.

2. **WinRM**. O tráfego de WinRM é criptografado por padrão usando o provedor de suporte de segurança do Kerberos \(SSP\). Você pode configurar a criptografia adicional, na forma de SSL, no servidor do WinRM.
