---
title: Inicializar o cluster HGS usando modo AD em uma floresta de bastiões
ms.custom: na
ms.prod: windows-server-threshold
ms.topic: article
manager: dongill
author: rpsqrd
ms.technology: security-guarded-fabric
ms.date: 08/29/2018
ms.openlocfilehash: 887fc8655a6ff3e862fa04b5b450456b04c55718
ms.sourcegitcommit: eaf071249b6eb6b1a758b38579a2d87710abfb54
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 05/31/2019
ms.locfileid: "66447459"
---
# <a name="initialize-the-hgs-cluster-using-ad-mode-in-an-existing-bastion-forest"></a>Inicializar o cluster HGS usando modo AD em uma floresta de bastiões existente

>Aplica-se a: Windows Server (canal semestral), Windows Server 2016


>[!IMPORTANT]
>Atestado de Admin confiável (modo AD) é substituído, começando com o Windows Server 2019. Para ambientes em que o atestado de TPM não for possível, configure [atestado de chaves de host](guarded-fabric-initialize-hgs-key-mode-bastion.md). Atestado de chaves do host fornece garantia semelhante para o modo do AD e é mais simples de configurar. 

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

Initialize-HgsServer -UseExistingDomain -ServiceAccount 'HGSgMSA' -JeaReviewersGroup 'HgsJeaReviewers' -JeaAdministratorsGroup 'HgsJeaAdmins' -HgsServiceName 'HgsService' -ClusterName 'HgsCluster' -SigningCertificatePath '.\signCert.pfx' -SigningCertificatePassword $signPass -EncryptionCertificatePath '.\encCert.pfx' -EncryptionCertificatePassword $encryptionCertPass -TrustActiveDirectory
```

Se você estiver usando certificados instalados no computador local (como backup de HSM certificados e certificados não exportável), use o `-SigningCertificateThumbprint` e `-EncryptionCertificateThumbprint` parâmetros em vez disso.

## <a name="next-step"></a>Próximas etapas

> [!div class="nextstepaction"]
> [Configurar DNS da malha](guarded-fabric-configuring-fabric-dns-ad.md)

