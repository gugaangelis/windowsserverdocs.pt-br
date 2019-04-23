---
title: Inicializar o cluster HGS usando modo AD em uma floresta nova dedicada (padrão)
ms.custom: na
ms.prod: windows-server-threshold
ms.topic: article
manager: dongill
author: rpsqrd
ms.technology: security-guarded-fabric
ms.date: 08/29/2018
ms.openlocfilehash: 4ef70ab9ce8150a9a3ed774d6df407da097880e9
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59865667"
---
# <a name="initialize-the-hgs-cluster-using-ad-mode-in-a-new-dedicated-forest-default"></a>Inicializar o cluster HGS usando modo AD em uma floresta nova dedicada (padrão)

>Aplica-se a: Windows Server (canal semestral), Windows Server 2016

>[!IMPORTANT]
>Atestado de Admin confiável (modo AD) é substituído, começando com o Windows Server 2019. Para ambientes em que o atestado de TPM não for possível, configure [atestado de chaves de host](guarded-fabric-initialize-hgs-key-mode-default.md). Atestado de chaves do host fornece garantia semelhante para o modo do AD e é mais simples de configurar. 

1.  [!INCLUDE [Initialize HGS](../../../includes/guarded-fabric-initialize-hgs-default-step-one.md)] 
2.  [!INCLUDE [Obtain certificates for HGS](../../../includes/guarded-fabric-initialize-hgs-default-step-two.md)]

3.  Execute [Initialize HgsServer](https://technet.microsoft.com/library/mt652185.aspx) em uma janela elevada do PowerShell no primeiro nó HGS. A sintaxe desse cmdlet oferece suporte a muitas entradas diferentes, mas as 2 invocações mais comuns estão abaixo:

    -   Se você estiver usando arquivos PFX para certificados de assinatura e criptografia, execute os seguintes comandos:

        ```powershell
        $signingCertPass = Read-Host -AsSecureString -Prompt "Signing certificate password"
        $encryptionCertPass = Read-Host -AsSecureString -Prompt "Encryption certificate password"

        Initialize-HgsServer -HgsServiceName 'MyHgsDNN' -SigningCertificatePath '.\signCert.pfx' -SigningCertificatePassword $signingCertPass -EncryptionCertificatePath '.\encCert.pfx' -EncryptionCertificatePassword $encryptionCertPass -TrustActiveDirectory
        ```

    -   Se você estiver usando não exportável certificados instalados no repositório de certificados local, execute o comando a seguir. Se você não souber as impressões digitais dos certificados, você pode listar os certificados disponíveis executando `Get-ChildItem Cert:\LocalMachine\My`.

        ```powershell
        Initialize-HgsServer -HgsServiceName 'MyHgsDNN' -SigningCertificateThumbprint '1A2B3C4D5E6F...' -EncryptionCertificateThumbprint '0F9E8D7C6B5A...' --TrustActiveDirectory
        ```

4.  [!INCLUDE [Initialize HGS](../../../includes/guarded-fabric-initialize-hgs-default-step-four.md)]  

5.  [!INCLUDE [Initialize HGS](../../../includes/guarded-fabric-initialize-hgs-default-step-five.md)]

## <a name="next-step"></a>Próximas etapas

>[!div class="nextstepaction"]
[Configurar DNS de malha](guarded-fabric-configuring-fabric-dns-ad.md)