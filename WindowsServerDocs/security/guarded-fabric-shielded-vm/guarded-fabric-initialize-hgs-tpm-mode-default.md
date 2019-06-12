---
title: Inicializar o cluster HGS usando o modo TPM em uma floresta nova dedicada (padrão)
ms.custom: na
ms.prod: windows-server-threshold
ms.topic: article
manager: dongill
author: rpsqrd
ms.technology: security-guarded-fabric
ms.date: 08/29/2018
ms.openlocfilehash: f2fb970cdc215df06dd9dee2e20b5466d7e42dcb
ms.sourcegitcommit: eaf071249b6eb6b1a758b38579a2d87710abfb54
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 05/31/2019
ms.locfileid: "66447417"
---
# <a name="initialize-the-hgs-cluster-using-tpm-mode-in-a-new-dedicated-forest-default"></a>Inicializar o cluster HGS usando o modo TPM em uma floresta nova dedicada (padrão)

>Aplica-se a: Windows Server 2019, Windows Server (canal semestral), Windows Server 2016

1.  [!INCLUDE [Initialize HGS](../../../includes/guarded-fabric-initialize-hgs-default-step-one.md)]

2.  [!INCLUDE [Obtain certificates for HGS](../../../includes/guarded-fabric-initialize-hgs-default-step-two.md)]

3.  Execute [Initialize HgsServer](https://technet.microsoft.com/library/mt652185.aspx) em uma janela elevada do PowerShell no primeiro nó HGS. A sintaxe desse cmdlet oferece suporte a muitas entradas diferentes, mas as 2 invocações mais comuns estão abaixo:

    -   Se você estiver usando arquivos PFX para certificados de assinatura e criptografia, execute os seguintes comandos:

        ```powershell
        $signingCertPass = Read-Host -AsSecureString -Prompt "Signing certificate password"
        $encryptionCertPass = Read-Host -AsSecureString -Prompt "Encryption certificate password"

        Initialize-HgsServer -HgsServiceName 'MyHgsDNN' -SigningCertificatePath '.\signCert.pfx' -SigningCertificatePassword $signingCertPass -EncryptionCertificatePath '.\encCert.pfx' -EncryptionCertificatePassword $encryptionCertPass -TrustTpm
        ```

    -   Se você estiver usando não exportável certificados instalados no repositório de certificados local, execute o comando a seguir. Se você não souber as impressões digitais dos certificados, você pode listar os certificados disponíveis executando `Get-ChildItem Cert:\LocalMachine\My`.

        ```powershell
        Initialize-HgsServer -HgsServiceName 'MyHgsDNN' -SigningCertificateThumbprint '1A2B3C4D5E6F...' -EncryptionCertificateThumbprint '0F9E8D7C6B5A...' -TrustTpm
        ```

4.  [!INCLUDE [Initialize HGS](../../../includes/guarded-fabric-initialize-hgs-default-step-four.md)]

5.  [!INCLUDE [Initialize HGS](../../../includes/guarded-fabric-initialize-hgs-default-step-five.md)]

## <a name="next-step"></a>Próximas etapas

> [!div class="nextstepaction"]
> [Instalar certificados de raiz do TPM](guarded-fabric-install-trusted-tpm-root-certificates.md)
  