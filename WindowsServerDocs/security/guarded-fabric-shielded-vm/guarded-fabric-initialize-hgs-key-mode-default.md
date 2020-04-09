---
title: Inicializar o cluster HGS usando o modo de chave em uma nova floresta dedicada (padrão)
ms.prod: windows-server
ms.topic: article
manager: dongill
author: rpsqrd
ms.author: ryanpu
ms.technology: security-guarded-fabric
ms.date: 08/29/2018
ms.openlocfilehash: feae5230eec6b3f3c0471f75199970a47016a583
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80856659"
---
# <a name="initialize-the-hgs-cluster-using-key-mode-in-a-new-dedicated-forest-default"></a>Inicializar o cluster HGS usando o modo de chave em uma nova floresta dedicada (padrão)

>Aplica-se a: Windows Server (canal semestral), Windows Server 2019, Windows Server 2016


1.  [!INCLUDE [Initialize HGS](../../../includes/guarded-fabric-initialize-hgs-default-step-one.md)] 
2.  [!INCLUDE [Obtain certificates for HGS](../../../includes/guarded-fabric-initialize-hgs-default-step-two.md)]

3.  Execute [Initialize-HgsServer](https://technet.microsoft.com/library/mt652185.aspx) em uma janela do PowerShell com privilégios elevados no primeiro nó HgS. A sintaxe desse cmdlet dá suporte a várias entradas diferentes, mas as duas invocações mais comuns estão abaixo:

    -   Se você estiver usando arquivos PFX para seus certificados de assinatura e criptografia, execute os seguintes comandos:

        ```powershell
        $signingCertPass = Read-Host -AsSecureString -Prompt "Signing certificate password"
        $encryptionCertPass = Read-Host -AsSecureString -Prompt "Encryption certificate password"

        Initialize-HgsServer -HgsServiceName 'MyHgsDNN' -SigningCertificatePath '.\signCert.pfx' -SigningCertificatePassword $signingCertPass -EncryptionCertificatePath '.\encCert.pfx' -EncryptionCertificatePassword $encryptionCertPass -TrustHostkey
        ```

    -   Se você estiver usando certificados não exportáveis instalados no repositório de certificados local, execute o comando a seguir. Se você não souber as impressões digitais de seus certificados, poderá listar os certificados disponíveis executando `Get-ChildItem Cert:\LocalMachine\My`.

        ```powershell
        Initialize-HgsServer -HgsServiceName 'MyHgsDNN' -SigningCertificateThumbprint '1A2B3C4D5E6F...' -EncryptionCertificateThumbprint '0F9E8D7C6B5A...' --TrustHostKey
        ```

4.  [!INCLUDE [Initialize HGS](../../../includes/guarded-fabric-initialize-hgs-default-step-four.md)]  

5.  [!INCLUDE [Initialize HGS](../../../includes/guarded-fabric-initialize-hgs-default-step-five.md)]


## <a name="next-step"></a>Próximas etapas

> [!div class="nextstepaction"]
> [Criar chave de host](guarded-fabric-create-host-key.md)