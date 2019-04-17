---
title: Segurança de controlador de rede
description: Você pode usar este tópico para saber como configurar a segurança de toda a comunicação entre o controlador de rede e outros softwares e dispositivos.
manager: brianlic
ms.prod: windows-server-threshold
ms.technology: networking-sdn
ms.topic: article
ms.assetid: bc625de9-ee31-40a4-9ad2-7448bfbfb6e6
ms.author: pashort
author: shortpatti
ms.openlocfilehash: ab04d3f528beb037988e6390fe85ad9af219266b
ms.sourcegitcommit: 19d9da87d87c9eefbca7a3443d2b1df486b0b010
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/28/2018
---
# <a name="network-controller-security"></a>Segurança de controlador de rede

Você pode usar este tópico para saber como configurar a segurança de toda a comunicação entre o controlador de rede e outros softwares e dispositivos. 

>[!NOTE]
>Para obter uma visão geral do controlador de rede, consulte [rede controlador](../technologies/network-controller/network-controller.md).

Os caminhos de comunicação que você pode proteger incluem comunicação em sentido norte no plano de gerenciamento, comunicação de cluster entre o controlador de rede máquinas virtuais \(VMs\) em um cluster e comunicação Southbound no plano de dados.

1. **Comunicação em sentido norte**. Controlador de rede se comunica com o plano de gerenciamento com um software de gerenciamento compatível com SDN\ como o Windows PowerShell e o System Center Virtual Machine Manager \(SCVMM\). Essas ferramentas de gerenciamento fornecem a capacidade de definir a política de rede e criar um estado de meta para a rede, em relação ao qual você pode comparar a configuração real da rede para trazer a configuração real em paridade com o estado de meta.

2. **Comunicação de Cluster de controlador de rede**. Quando você configura três ou mais VMs como nós de cluster de controlador de rede, esses nós se comunicar uns com os outros. Essa comunicação pode estar relacionada a sincronização e replicação de dados em todos os nós ou específica comunicação entre os serviços de controlador de rede.

3.  **Comunicação southbound**. Controlador de rede se comunica com o plano de dados com SDN infraestrutura e outros dispositivos como balanceadores de carga de software, gateways e máquinas de host. Você pode usar o controlador de rede para configurar e gerenciar esses dispositivos southbound para que eles mantêm o estado de meta que você configurou para a rede.

## <a name="northbound-communication"></a>Comunicação em sentido norte

Controlador de rede oferece suporte a autenticação, autorização e criptografia para comunicação em sentido norte. As seções a seguir fornecem informações sobre como configurar essas configurações de segurança.

**Autenticação**

Quando você configura a autenticação para comunicação de rede controlador em sentido norte, você permitir que nós de cluster de controlador de rede e clientes de gerenciamento para verificar a identidade do dispositivo com o qual estão se comunicando.

Controlador de rede compatível com os seguintes modos de autenticação entre nós de controlador de rede e clientes de gerenciamento.

>[!Note]
>Se você estiver implantando o controlador de rede com o System Center Virtual Machine Manager, apenas **Kerberos** modo tem suporte.

1. **Kerberos**. Você pode usar a autenticação Kerberos quando ambos os cliente de gerenciamento, como o computador que executa SCVMM e todos os nós de cluster de controlador de rede são tiver ingressado em um domínio do Active Directory, com contas de domínio usadas para autenticação.

2. **X509**. X509 é a autenticação baseada em certificate\. Você pode usar X509 autenticação quando os clientes de gerenciamento não tenham ingressados em um domínio do Active Directory. Para usar X509, você deve registrar certificados para todos os nós de cluster de controlador de rede e clientes de gerenciamento e todos os nós e clientes de gerenciamento devem confiar uns dos outros ' certificados.

3. **Nenhum**. Quando você escolhe nesse modo, não há nenhuma autenticação seja executada entre clientes de gerenciamento e controlador de rede. Esse modo é fornecido apenas para fins de teste e não é recomendado para uso em um ambiente de produção. 

Você pode configurar o modo de autenticação para comunicação em sentido norte usando o comando do Windows PowerShell **instalar NetworkController** com o **ClientAuthentication** parâmetro. 

Para obter mais informações, consulte os tópicos a seguir. 

- [Install-NetworkController](https://technet.microsoft.com/itpro/powershell/windows/network-controller/install-networkcontroller)
- [Set-NetworkController](https://technet.microsoft.com/itpro/powershell/windows/network-controller/set-networkcontroller)

**Autorização**

Quando você configura a autorização para comunicação de rede controlador em sentido norte, você permitir que nós de cluster de controlador de rede e clientes de gerenciamento para verificar se o dispositivo com o qual estão se comunicando é confiável e tem permissão para participar da comunicação.

Para cada um dos modos de autenticação compatíveis com o controlador de rede, os seguintes métodos de autorização são usados.

1.  **Kerberos**. Quando você estiver usando o método de autenticação Kerberos, você deve definir os usuários e computadores que estão autorizados a se comunicar com o controlador de rede criando um grupo de segurança no Active Directory e, em seguida, adicionando a usuários autorizados e computadores ao grupo. Você pode configurar o controlador de rede para usar o grupo de segurança para autorização usando o **ClientSecurityGroup** parâmetro do **instalar NetworkController** comando do Windows PowerShell. Após a instalação do controlador de rede, você pode alterar o grupo de segurança usando o [conjunto NetworkController](https://technet.microsoft.com/itpro/powershell/windows/network-controller/set-networkcontroller) comando com o parâmetro **- ClientSecurityGroup**. Se você estiver usando SCVMM, você deve fornecer o grupo de segurança como um parâmetro durante a implantação.

2.  **X509**. Quando você estiver usando o X509 método de autenticação, controlador de rede só aceita solicitações de clientes de gerenciamento cujos impressões digitais de certificados são conhecidos por controlador de rede. Você pode configurar esses impressões digitais usando o **ClientCertificateThumbprint** parâmetro do **instalar NetworkController** comando do Windows PowerShell. Você pode adicionar outras impressões digitais cliente a qualquer momento usando o **conjunto NetworkController** comando.

3.  **Nenhum**. Quando você escolhe nesse modo, não há nenhuma autorização realizada para tentativas de comunicação entre os nós do controlador de rede e clientes de gerenciamento. Esse modo é fornecido apenas para fins de teste e não é recomendado para uso em um ambiente de produção. 

Para obter mais informações, consulte os tópicos a seguir. 

- [Install-NetworkController](https://technet.microsoft.com/itpro/powershell/windows/network-controller/install-networkcontroller)
- [Set-NetworkController](https://technet.microsoft.com/itpro/powershell/windows/network-controller/set-networkcontroller)

**Criptografia**

Comunicação em sentido norte usa Secure Sockets Layer \(SSL\) para criar um canal criptografado entre nós de controlador de rede e clientes de gerenciamento. Criptografia de SSL para comunicação em sentido norte inclui os requisitos a seguir.

- Todos os nós de controlador de rede devem ter um certificado idêntico que inclui a finalidade de autenticação de cliente e servidor de autenticação no uso avançado de chave \(EKU\) extensões. 

- O URI usado pelos clientes de gerenciamento para se comunicar com o controlador de rede deve ser o nome do requerente do certificado. Nome do requerente do certificado deve conter o nome de domínio totalmente qualificado (FQDN) ou o endereço IP do ponto de extremidade de REST de controlador de rede.

- Se nós de controlador de rede estão localizados em várias sub-redes, o nome do requerente do seus certificados deve ser igual ao valor que você usa para o **RestName** parâmetro no **instalar NetworkController** comando do Windows PowerShell. 

- O certificado SSL deve ser confiável pelos todos os clientes de gerenciamento.

### <a name="ssl-certificate-enrollment-and-configuration"></a>Configuração e registro de certificado SSL

Você deve registrar o certificado SSL em nós de controlador de rede manualmente.

Depois que o certificado é registrado, você pode configurar o controlador de rede para usar o certificado com o **- ServerCertificate** parâmetro do **instalar NetworkController** comando do Windows PowerShell. Se você já tiver instalado o controlador de rede, você pode atualizar a configuração a qualquer momento usando o **conjunto NetworkController** comando.

>[!NOTE]
>Se você estiver usando SCVMM, você deve adicionar o certificado como um recurso de biblioteca. Para obter mais informações, consulte [configurar um controlador de rede SDN na malha VMM](https://technet.microsoft.com/system-center-docs/vmm/scenario/sdn-network-controller).

## <a name="network-controller-cluster-communication"></a>Comunicação de Cluster de controlador de rede

Controlador de rede oferece suporte a autenticação, autorização e criptografia para comunicação entre os nós de controlador de rede. A comunicação é sobre [Windows Communication Foundation](https://msdn.microsoft.com/library/ms731082.aspx) \(WCF\) e TCP.

Você pode configurar esse modo com o **ClusterAuthentication** parâmetro do **instalar NetworkControllerCluster** comando do Windows PowerShell. 

Para obter mais informações, consulte [instalar NetworkControllerCluster](https://technet.microsoft.com/itpro/powershell/windows/network-controller/install-networkcontrollercluster).

**Autenticação**

Quando você configura a autenticação para comunicação de rede controlador Cluster, você permitir que nós de cluster de controlador de rede verificar a identidade dos outros nós com a qual estão se comunicando.

Controlador de rede compatível com os seguintes modos de autenticação entre nós de controlador de rede.

>[!NOTE]
>Se você implantar o controlador de rede usando SCVMM, apenas **Kerberos** modo tem suporte.

1. **Kerberos**. Você pode usar a autenticação Kerberos quando todos os nós de cluster de controlador de rede são tiver ingressados em um domínio do Active Directory, com contas de domínio usadas para autenticação.

2. **X509**. X509 é a autenticação baseada em certificate\. Você pode usar X509 autenticação quando nós de cluster de controlador de rede não estiver conectado a um domínio do Active Directory. Para usar X509, você deve registrar certificados para todos os nós de cluster de controlador de rede, e todos os nós devem confiar nos certificados. Além disso, o nome do requerente do certificado que é registrado em cada nó deve ser o mesmo que o nome DNS do nó.

3. **Nenhum**. Quando você escolhe nesse modo, não há nenhuma autenticação seja executada entre nós de controlador de rede. Esse modo é fornecido apenas para fins de teste e não é recomendado para uso em um ambiente de produção.

**Autorização**

Quando você configura a autorização para comunicação de rede controlador Cluster, você permitir que nós de cluster de controlador de rede verificar se os nós com a qual estão se comunicando são confiáveis e têm permissão para participar da comunicação.

Para cada um dos modos de autenticação compatíveis com o controlador de rede, os seguintes métodos de autorização são usados.

1. **Kerberos**. Nós de controlador de rede aceitam solicitações de comunicação apenas de outras contas de computador do controlador de rede. Você pode configurar essas contas quando você implanta o controlador de rede usando o **nome** parâmetro do [nova NetworkControllerNodeObject](https://technet.microsoft.com/itpro/powershell/windows/network-controller/new-networkcontrollernodeobject) comando do Windows PowerShell.

2. **X509**. Nós de controlador de rede aceitam solicitações de comunicação apenas de outras contas de computador do controlador de rede. Você pode configurar essas contas quando você implanta o controlador de rede usando o **nome** parâmetro do [nova NetworkControllerNodeObject](https://technet.microsoft.com/itpro/powershell/windows/network-controller/new-networkcontrollernodeobject) comando do Windows PowerShell.

3. **Nenhum**. Quando você escolhe nesse modo, não há nenhuma autorização realizada entre nós de controlador de rede. Esse modo é fornecido apenas para fins de teste e não é recomendado para uso em um ambiente de produção.

**Criptografia**

Comunicação entre os nós de controlador de rede é criptografada usando a criptografia no nível do transporte de WCF. Essa forma de criptografia é usada quando os métodos de autenticação e autorização são Kerberos ou X509 certificados. Para obter mais informações, consulte os tópicos a seguir.

- [Como: segura um serviço com credenciais do Windows](https://msdn.microsoft.com/library/ms734673.aspx)
- [Como: segura um serviço com certificados x. 509](https://msdn.microsoft.com/library/ms788968.aspx).

## <a name="southbound-communication"></a>Comunicação southbound

Controlador de rede interage com diferentes tipos de dispositivos para comunicação Southbound. Essas interações usam diferentes protocolos. Por isso, há requisitos diferentes para autenticação, autorização e criptografia dependendo do tipo de dispositivo e o protocolo usado pelo controlador de rede para se comunicar com o dispositivo.

A tabela a seguir fornece informações sobre a interação de controlador de rede com dispositivos southbound diferentes.

| Southbound/serviço de dispositivo | Protocolo              | Autenticação usado    |
|---------------------------|-----------------------|------------------------|
| Software balanceador    | WCF (Multiplexador), TCP (Host) | Certificados           |
| Firewall                  | OVSDB                 | Certificados           |
| Gateway                   | WinRM                 | Kerberos, certificados |
| Rede virtual        | OVSDB, WCF            | Certificados           |
| Roteamento definida pelo usuário      | OVSDB                 | Certificados           |

Para cada um desses protocolos, o mecanismo de comunicação é descrito na seção a seguir.

**Autenticação**

Para comunicação Southbound, as seguintes protocolos e os métodos de autenticação são usados.

1. **TCP/WCF/OVSDB**. Esses protocolos, a autenticação é executada usando X509 certificados. Controlador de rede e o par de balanceamento de carga de Software \(SLB\) Multiplexer \ (MUX\) / computadores host apresentam seus certificados uns aos outros para autenticação mútua. Cada certificado deve ser confiável para o ponto remoto.

    Para a autenticação southbound, você pode usar o mesmo certificado SSL que esteja configurado para criptografar a comunicação com os clientes em sentido norte. Você também deve configurar um certificado no SLB MUX e dispositivos de host. Nome do requerente do certificado deve ser igual ao nome DNS do dispositivo.

2. **WinRM**. Para esse protocolo, autenticação é realizada usando Kerberos \ (para machines\ ingressado no domínio) e usando certificados \ (para fora do domínio associado machines\).

**Autorização**

Para comunicação Southbound, as seguintes protocolos e os métodos de autorização são usados.

1. **TCP do WCF**. Esses protocolos, autorização com base no nome do assunto da entidade par. Controlador de rede armazena o nome DNS do dispositivo par e a usa para autorização. Esse nome DNS deve coincidir com o nome do requerente do dispositivo no certificado. Da mesma forma, o certificado de controlador de rede deve corresponder ao nome de rede controlador DNS armazenado no dispositivo par.

2. **WinRM**. Se estiver sendo usado Kerberos, a conta do cliente WinRM deverá estar presente em um grupo predefinido no Active Directory ou no grupo Administradores Local no servidor. Se certificados estão sendo usados, o cliente apresenta um certificado para o servidor que o servidor autoriza usando o nome/emissor do assunto e o servidor usa uma conta de usuário mapeadas para realizar autenticação.

3.  **OVSDB**. Não há nenhuma autorização fornecida para esse protocolo.

**Criptografia**

Para comunicação Southbound, os seguintes métodos de criptografia são usados para os protocolos.

1. **TCP/WCF/OVSDB**. Esses protocolos, a criptografia é executada usando o certificado que é registrado no cliente ou servidor.

2. **WinRM**. WinRM tráfego é criptografado por padrão usando o provedor de suporte de segurança Kerberos \(SSP\). Você pode configurar criptografia adicionais, na forma de SSL, no servidor WinRM.
