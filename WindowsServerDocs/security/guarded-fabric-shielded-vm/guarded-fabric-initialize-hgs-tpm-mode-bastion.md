---
title: Inicializar o cluster HGS usando o modo TPM em uma floresta de bastiões
ms.custom: na
ms.prod: windows-server-threshold
ms.topic: article
manager: dongill
author: rpsqrd
ms.technology: security-guarded-fabric
ms.date: 08/29/2018
ms.openlocfilehash: ac6407f9f23780dc62ff7d99fe0daf0f81f79f72
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59832137"
---
# <a name="initialize-the-hgs-cluster-using-tpm-mode-in-an-existing-bastion-forest"></a>Inicializar o cluster HGS usando o modo TPM em uma floresta de bastiões existente

>Aplica-se a: Windows Server 2019, Windows Server (canal semestral), Windows Server 2016

Active Directory Domain Services será instalado no computador, mas devem permanecer não configurado.

[!INCLUDE [Obtain certificates for HGS](../../../includes/guarded-fabric-initialize-hgs-default-step-two.md)]

Antes de continuar, verifique se seus objetos de cluster para o serviço guardião de Host de pré-teste e concedidos usuário conectado **controle total** sobre os objetos VCO e o CNO no Active Directory.
O nome do objeto de computador virtual deve ser passado para o `-HgsServiceName` parâmetro e o nome do cluster para o `-ClusterName` parâmetro.

> [!TIP]
> Verifique os controladores de domínio do AD para garantir que seus objetos de cluster foram replicadas para todos os controladores de domínio antes de continuar.

Se você estiver usando certificados de PFX, execute os seguintes comandos no servidor HGS:

```powershell
$signingCertPass = Read-Host -AsSecureString -Prompt "Signing certificate password"
$encryptionCertPass = Read-Host -AsSecureString -Prompt "Encryption certificate password"

Install-ADServiceAccount -Identity 'HGSgMSA'

Initialize-HgsServer -UseExistingDomain -ServiceAccount 'HGSgMSA' -JeaReviewersGroup 'HgsJeaReviewers' -JeaAdministratorsGroup 'HgsJeaAdmins' -HgsServiceName 'HgsService' -SigningCertificatePath '.\signCert.pfx' -SigningCertificatePassword $signPass -EncryptionCertificatePath '.\encCert.pfx' -EncryptionCertificatePassword $encryptionCertPass -TrustTpm
```

Se você estiver usando certificados instalados no computador local (como backup de HSM certificados e certificados não exportável), use o `-SigningCertificateThumbprint` e `-EncryptionCertificateThumbprint` parâmetros em vez disso.

## <a name="next-step"></a>Próximas etapas

>[!div class="nextstepaction"]
[Instalar certificados de raiz do TPM](guarded-fabric-install-trusted-tpm-root-certificates.md)