---
title: Configurar nós adicionais do HGS
ms.custom: na
ms.prod: windows-server-threshold
ms.topic: article
ms.assetid: 227f723b-acb2-42a7-bbe3-44e82f930e35
manager: dongill
author: rpsqrd
ms.technology: security-guarded-fabric
ms.date: 10/22/2018
ms.openlocfilehash: a89337457cc71ffee78e3f73fecc2262f1fb38e9
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59855197"
---
# <a name="configure-additional-hgs-nodes"></a>Configurar nós adicionais do HGS

>Aplica-se a: Windows Server 2019, Windows Server (canal semestral), Windows Server 2016

Em ambientes de produção, HGS devem ser configurado em um cluster de alta disponibilidade para garantir que as VMs blindadas podem ser ligadas, mesmo se um nó HGS fica inativo. Para ambientes de teste, nós HGS secundários não são necessários.

Use um dos seguintes métodos para adicionar nós HGS, como a mais adequadas para seu ambiente.

|                |                         |                              | 
|----------------|-------------------------|------------------------------|
|Nova floresta HGS  | [Usando arquivos PFX](#dedicated-hgs-forest-with-pfx-certificates) | [Usando as impressões digitais de certificado](#dedicated-hgs-forest-with-certificate-thumbprints) |
|Floresta de bastiões existente |  [Usando arquivos PFX](#existing-bastion-forest-with-pfx-certificates) | [Usando as impressões digitais de certificado](#existing-bastion-forest-with-certificate-thumbprints) |

## <a name="prerequisites"></a>Pré-requisitos

Certifique-se de que cada nó adicional: 
- Tem a mesma configuração de hardware e software como o nó principal 
- Está conectado à mesma rede que os outros servidores HGS
- Pode resolver os outros servidores HGS por seus nomes DNS

## <a name="dedicated-hgs-forest-with-pfx-certificates"></a>Floresta HGS dedicada com os certificados PFX

1. Promover o controlador de domínio do nó de HGS
2. Inicializar o servidor HGS

### <a name="promote-the-hgs-node-to-a-domain-controller"></a>Promover o controlador de domínio do nó de HGS

[!INCLUDE [Promote to a domain controller](../../../includes/guarded-fabric-promote-domain-controller.md)] 

### <a name="initialize-the-hgs-server"></a>Inicializar o servidor HGS

[!INCLUDE [Initialize the HGS server](../../../includes/guarded-fabric-initialize-hgs-on-the-node.md)] 

## <a name="dedicated-hgs-forest-with-certificate-thumbprints"></a>Floresta dedicada de HGS com impressões digitais de certificado
 
1. Promover o controlador de domínio do nó de HGS
2. Inicializar o servidor HGS
3. Instalar as chaves privadas para os certificados

### <a name="promote-the-hgs-node-to-a-domain-controller"></a>Promover o controlador de domínio do nó de HGS

[!INCLUDE [Promote to a domain controller](../../../includes/guarded-fabric-promote-domain-controller.md)] 

### <a name="initialize-the-hgs-server"></a>Inicializar o servidor HGS

[!INCLUDE [Initialize the HGS server](../../../includes/guarded-fabric-initialize-hgs-on-the-node.md)] 

### <a name="install-the-private-keys-for-the-certificates"></a>Instalar as chaves privadas para os certificados

[!INCLUDE [Install private keys](../../../includes/guarded-fabric-install-private-keys.md)]

## <a name="existing-bastion-forest-with-pfx-certificates"></a>Floresta de bastiões existente com os certificados PFX

1. Adicionar o nó a domínio existente
2. Conceder os direitos de máquina para recuperar a senha de gMSA e executar Install-ADServiceAccount
3. Inicializar o servidor HGS

### <a name="join-the-node-to-the-existing-domain"></a>Adicionar o nó a domínio existente

1. Certifique-se de que pelo menos uma NIC no nó está configurada para usar o servidor DNS no primeiro servidor HGS.
2. Junte-se o novo nó HGS ao mesmo domínio como o primeiro nó HGS. 

### <a name="grant-the-machine-rights-to-retrieve-gmsa-password-and-run-install-adserviceaccount"></a>Conceder os direitos de máquina para recuperar a senha de gMSA e executar Install-ADServiceAccount

[!INCLUDE [Grant the machine rights to retrieve the group MSA](../../../includes/guarded-fabric-grant-machine-rights-to-retrieve-gmsa.md)] 

### <a name="initialize-the-hgs-server"></a>Inicializar o servidor HGS

[!INCLUDE [Initialize the HGS server](../../../includes/guarded-fabric-initialize-hgs-on-the-node.md)] 

## <a name="existing-bastion-forest-with-certificate-thumbprints"></a>Floresta de bastiões existente com as impressões digitais de certificado

1. Adicionar o nó a domínio existente
2. Conceder os direitos de máquina para recuperar a senha de gMSA e executar Install-ADServiceAccount
3. Inicializar o servidor HGS
4. Instalar as chaves privadas para os certificados

### <a name="join-the-node-to-the-existing-domain"></a>Adicionar o nó a domínio existente

1. Certifique-se de que pelo menos uma NIC no nó está configurada para usar o servidor DNS no primeiro servidor HGS.
2. Junte-se o novo nó HGS ao mesmo domínio como o primeiro nó HGS. 

### <a name="grant-the-machine-rights-to-retrieve-gmsa-password-and-run-install-adserviceaccount"></a>Conceder os direitos de máquina para recuperar a senha de gMSA e executar Install-ADServiceAccount

[!INCLUDE [Grant the machine rights to retrieve the group MSA](../../../includes/guarded-fabric-grant-machine-rights-to-retrieve-gmsa.md)] 

### <a name="initialize-the-hgs-server"></a>Inicializar o servidor HGS

[!INCLUDE [Initialize the HGS server](../../../includes/guarded-fabric-initialize-hgs-on-the-node.md)] 

Levará até 10 minutos para a criptografia e certificados de autenticação do primeiro servidor HGS para replicar a este nó.

### <a name="install-the-private-keys-for-the-certificates"></a>Instalar as chaves privadas para os certificados

[!INCLUDE [Install private keys](../../../includes/guarded-fabric-install-private-keys.md)]

## <a name="configure-hgs-for-https-communications"></a>Configurar o HGS para comunicações HTTPS

Se você quiser proteger pontos de extremidade HGS com um certificado SSL, você deve configurar o certificado SSL nesse nó, bem como todos os outros nós no cluster HGS.
Certificados SSL *não são* replicado pelo HGS e não é necessário usar as mesmas chaves para cada nó (ou seja, você pode ter diferentes certificados SSL para cada nó).

Ao solicitar um certificado SSL, verifique se o nome de domínio totalmente qualificado do cluster (conforme mostrado na saída do `Get-HgsServer`) é qualquer o nome comum da entidade do certificado ou incluído como um nome DNS alternativo do assunto.
Quando você tiver obtido um certificado da autoridade de certificação, você pode configurar o HGS para usá-lo com [HgsServer conjunto](https://technet.microsoft.com/itpro/powershell/windows/hgsserver/set-hgsserver).

```powershell
$sslPassword = Read-Host -AsSecureString -Prompt "SSL Certificate Password"
Set-HgsServer -Http -Https -HttpsCertificatePath 'C:\temp\HgsSSLCertificate.pfx' -HttpsCertificatePassword $sslPassword
```

Se você já instalou o certificado no repositório de certificados local e deseja fazer referência a ele por impressão digital, execute o seguinte comando:

```powershell
Set-HgsServer -Http -Https -HttpsCertificateThumbprint 'A1B2C3D4E5F6...'
```

HGS sempre irá expor as portas HTTP e HTTPS para comunicação.
Não há suporte para remover a associação de HTTP no IIS, no entanto, você pode usar o Firewall do Windows ou outras tecnologias de firewall de rede para bloquear as comunicações pela porta 80.

## <a name="decommission-an-hgs-node"></a>Desativar um nó HGS

Para desativar um nó HGS:

1. [Limpar a configuração de HGS](guarded-fabric-manage-hgs.md#clearing-the-hgs-configuration).

   Isso remove o nó do cluster e desinstala os serviços de proteção de chave e Atestado. 
   Se ele é o último nó do cluster, - Force é necessário para indicar que você deseja remover o último nó e destruir o cluster no Active Directory. 
   
   Se o HGS está implantado em uma floresta de bastiões (padrão), que é a única etapa. 
   Você pode, opcionalmente, desassocie o computador do domínio e remover a conta gMSA do Active Directory.

1. Se o HGS criou seu próprio domínio, você deve também [desinstalar HGS](guarded-fabric-manage-hgs.md#clearing-the-hgs-configuration) para sair do domínio e rebaixar o controlador de domínio.



## <a name="next-step"></a>Próximas etapas

>[!div class="nextstepaction"]
[Validar a configuração de HGS](guarded-fabric-verify-hgs-configuration.md)

