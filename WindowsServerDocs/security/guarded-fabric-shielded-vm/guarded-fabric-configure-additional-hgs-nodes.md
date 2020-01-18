---
title: Configurar nós adicionais do HGS
ms.custom: na
ms.prod: windows-server
ms.topic: article
ms.assetid: 227f723b-acb2-42a7-bbe3-44e82f930e35
manager: dongill
author: rpsqrd
ms.technology: security-guarded-fabric
ms.date: 01/14/2020
ms.openlocfilehash: ece005617c4a2faac41c2be15967b2f43951517e
ms.sourcegitcommit: c5709021aa98abd075d7a8f912d4fd2263db8803
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 01/18/2020
ms.locfileid: "76265858"
---
# <a name="configure-additional-hgs-nodes"></a>Configurar nós adicionais do HGS

>Aplica-se a: Windows Server 2019, Windows Server (canal semestral), Windows Server 2016

Em ambientes de produção, o HGS deve ser configurado em um cluster de alta disponibilidade para garantir que as VMs blindadas possam ser ligadas mesmo se um nó HGS ficar inativo. Para ambientes de teste, nós HGS secundários não são necessários.

Use um desses métodos para adicionar nós HGS, conforme mais adequado para o seu ambiente.

|                |                         |                              | 
|----------------|-------------------------|------------------------------|
|Nova floresta HGS  | [Usando arquivos PFX](#dedicated-hgs-forest-with-pfx-certificates) | [Usando impressões digitais de certificado](#dedicated-hgs-forest-with-certificate-thumbprints) |
|Floresta de bastiões existente |  [Usando arquivos PFX](#existing-bastion-forest-with-pfx-certificates) | [Usando impressões digitais de certificado](#existing-bastion-forest-with-certificate-thumbprints) |

## <a name="prerequisites"></a>Pré-requisitos

Certifique-se de que cada nó adicional: 
- Tem a mesma configuração de hardware e software que o nó primário 
- Está conectado à mesma rede que os outros servidores HGS
- Pode resolver os outros servidores HGS por seus nomes DNS

## <a name="dedicated-hgs-forest-with-pfx-certificates"></a>Floresta HGS dedicada com certificados PFX

1. Promover o nó HGS para um controlador de domínio
2. Inicializar o servidor HGS

### <a name="promote-the-hgs-node-to-a-domain-controller"></a>Promover o nó HGS para um controlador de domínio

[!INCLUDE [Promote to a domain controller](../../../includes/guarded-fabric-promote-domain-controller.md)] 

### <a name="initialize-the-hgs-server"></a>Inicializar o servidor HGS

[!INCLUDE [Initialize the HGS server](../../../includes/guarded-fabric-initialize-hgs-on-the-node.md)] 

## <a name="dedicated-hgs-forest-with-certificate-thumbprints"></a>Floresta HGS dedicada com impressões digitais de certificado
 
1. Promover o nó HGS para um controlador de domínio
2. Inicializar o servidor HGS
3. Instalar as chaves privadas para os certificados

### <a name="promote-the-hgs-node-to-a-domain-controller"></a>Promover o nó HGS para um controlador de domínio

[!INCLUDE [Promote to a domain controller](../../../includes/guarded-fabric-promote-domain-controller.md)] 

### <a name="initialize-the-hgs-server"></a>Inicializar o servidor HGS

[!INCLUDE [Initialize the HGS server](../../../includes/guarded-fabric-initialize-hgs-on-the-node.md)] 

### <a name="install-the-private-keys-for-the-certificates"></a>Instalar as chaves privadas para os certificados

[!INCLUDE [Install private keys](../../../includes/guarded-fabric-install-private-keys.md)]

## <a name="existing-bastion-forest-with-pfx-certificates"></a>Floresta de bastiões existente com certificados PFX

1. Unir o nó ao domínio existente
2. Conceda aos direitos de computador para recuperar a senha do gMSA e executar Install-ADServiceAccount
3. Inicializar o servidor HGS

### <a name="join-the-node-to-the-existing-domain"></a>Unir o nó ao domínio existente

1. Verifique se pelo menos uma NIC no nó está configurada para usar o servidor DNS em seu primeiro servidor HGS.
2. Ingresse o novo nó HGS no mesmo domínio que o seu primeiro nó HGS. 

### <a name="grant-the-machine-rights-to-retrieve-gmsa-password-and-run-install-adserviceaccount"></a>Conceda aos direitos de computador para recuperar a senha do gMSA e executar Install-ADServiceAccount

[!INCLUDE [Grant the machine rights to retrieve the group MSA](../../../includes/guarded-fabric-grant-machine-rights-to-retrieve-gmsa.md)] 

### <a name="initialize-the-hgs-server"></a>Inicializar o servidor HGS

[!INCLUDE [Initialize the HGS server](../../../includes/guarded-fabric-initialize-hgs-on-the-node.md)] 

## <a name="existing-bastion-forest-with-certificate-thumbprints"></a>Floresta de bastiões existente com impressões digitais de certificado

1. Unir o nó ao domínio existente
2. Conceda aos direitos de computador para recuperar a senha do gMSA e executar Install-ADServiceAccount
3. Inicializar o servidor HGS
4. Instalar as chaves privadas para os certificados

### <a name="join-the-node-to-the-existing-domain"></a>Unir o nó ao domínio existente

1. Verifique se pelo menos uma NIC no nó está configurada para usar o servidor DNS em seu primeiro servidor HGS.
2. Ingresse o novo nó HGS no mesmo domínio que o seu primeiro nó HGS. 

### <a name="grant-the-machine-rights-to-retrieve-gmsa-password-and-run-install-adserviceaccount"></a>Conceda aos direitos de computador para recuperar a senha do gMSA e executar Install-ADServiceAccount

[!INCLUDE [Grant the machine rights to retrieve the group MSA](../../../includes/guarded-fabric-grant-machine-rights-to-retrieve-gmsa.md)] 

### <a name="initialize-the-hgs-server"></a>Inicializar o servidor HGS

[!INCLUDE [Initialize the HGS server](../../../includes/guarded-fabric-initialize-hgs-on-the-node.md)] 

Levará até 10 minutos para que os certificados de criptografia e autenticação do primeiro servidor HGS sejam replicados para esse nó.

### <a name="install-the-private-keys-for-the-certificates"></a>Instalar as chaves privadas para os certificados

[!INCLUDE [Install private keys](../../../includes/guarded-fabric-install-private-keys.md)]

## <a name="configure-hgs-for-https-communications"></a>Configurar o HGS para comunicações HTTPS

Se você quiser proteger pontos de extremidade HGS com um certificado SSL, deverá configurar o certificado SSL nesse nó, bem como todos os outros nós no cluster HGS.
Os certificados SSL *não são* replicados pelo HgS e não precisam usar as mesmas chaves para cada nó (ou seja, você pode ter diferentes certificados SSL para cada nó).

Ao solicitar um certificado SSL, certifique-se de que o nome de domínio totalmente qualificado do cluster (conforme mostrado na saída de `Get-HgsServer`) seja o nome comum da entidade do certificado ou seja incluído como um nome DNS alternativo da entidade.
Depois de obter um certificado de sua autoridade de certificação, você pode configurar o HGS para usá-lo com [set-HgsServer](https://technet.microsoft.com/itpro/powershell/windows/hgsserver/set-hgsserver).

```powershell
$sslPassword = Read-Host -AsSecureString -Prompt "SSL Certificate Password"
Set-HgsServer -Http -Https -HttpsCertificatePath 'C:\temp\HgsSSLCertificate.pfx' -HttpsCertificatePassword $sslPassword
```

Se você já tiver instalado o certificado no repositório de certificados local e quiser fazer referência a ele por impressão digital, execute o seguinte comando em vez disso:

```powershell
Set-HgsServer -Http -Https -HttpsCertificateThumbprint 'A1B2C3D4E5F6...'
```

O HGS sempre vai expor as portas HTTP e HTTPS para comunicação.
Não há suporte para remover a associação HTTP no IIS, no entanto, você pode usar o Firewall do Windows ou outras tecnologias de firewall de rede para bloquear as comunicações pela porta 80.

## <a name="decommission-an-hgs-node"></a>Desativar um nó HGS

Para desativar um nó HGS:

1. [Limpe a configuração do HgS](guarded-fabric-manage-hgs.md#clearing-the-hgs-configuration).

   Isso remove o nó do cluster e desinstala os serviços de atestado e proteção de chave. 
   Se for o último nó no cluster,-Force é necessário para significar que você deseja remover o último nó e destruir o cluster no Active Directory. 

   Se o HGS for implantado em uma floresta de bastiões (padrão), essa será a única etapa. 
   Opcionalmente, você pode desassociar o computador do domínio e remover a conta gMSA da Active Directory.

2. Se o HGS criou seu próprio domínio, você também deve [desinstalar o HgS](guarded-fabric-manage-hgs.md#clearing-the-hgs-configuration) para desassociar o domínio e rebaixar o controlador de domínio.
