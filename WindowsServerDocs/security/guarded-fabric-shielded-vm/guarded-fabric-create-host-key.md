---
title: Criar uma chave de host e adicioná-lo ao HGS
ms.custom: na
ms.prod: windows-server-threshold
ms.topic: article
ms.assetid: a12c8494-388c-4523-8d70-df9400bbc2c0
manager: dongill
author: rpsqrd
ms.technology: security-guarded-fabric
ms.date: 08/29/2018
ms.openlocfilehash: 0526831fb0648e7f8f6fb1a081180f2e2aa9f09f
ms.sourcegitcommit: eaf071249b6eb6b1a758b38579a2d87710abfb54
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 05/31/2019
ms.locfileid: "66447493"
---
# <a name="create-a-host-key-and-add-it-to-hgs"></a>Criar uma chave de host e adicioná-lo ao HGS

>Aplica-se a: Windows Server 2019


Este tópico aborda como preparar os hosts do Hyper-V para se tornam hosts protegidos usando o atestado de chave de host (modo de chave). Você vai criar um par de chaves de host (ou usar um certificado existente) e adicione a metade pública da chave ao HGS.

## <a name="create-a-host-key"></a>Criar uma chave de host

1.  Instale o Windows Server 2019 no seu computador de host do Hyper-V.
2.  Instale os recursos do Hyper-V e o suporte de Hyper-V de guardião de Host:

    ```powershell
    Install-WindowsFeature Hyper-V, HostGuardian -IncludeManagementTools -Restart
    ``` 

3.  Gerar uma chave de host automaticamente, ou selecione um certificado existente. Se você estiver usando um certificado personalizado, ele deve ter pelo menos uma chave RSA de 2048 bits, o EKU de autenticação de cliente e o uso de chave de Assinatura Digital.

    ```powershell
    Set-HgsClientHostKey
    ```

    Como alternativa, você pode especificar uma impressão digital, se você quiser usar seu próprio certificado. 
    Isso pode ser útil se você quiser compartilhar um certificado em vários computadores ou usar um certificado associado a um TPM ou um HSM. Aqui está um exemplo de como criar um certificado de TPM associado (que impede de ter a chave privada roubado e usado em outro computador e requer um TPM 1.2):

    ```powershell
    $tpmBoundCert = New-SelfSignedCertificate -Subject “Host Key Attestation ($env:computername)” -Provider “Microsoft Platform Crypto Provider”
    Set-HgsClientHostKey -Thumbprint $tpmBoundCert.Thumbprint
    ```

4.  Obter a metade pública da chave para fornecer ao servidor HGS. Você pode usar o cmdlet a seguir ou, se você tiver o certificado armazenado em outro lugar, fornecer um arquivo. cer que contém o público em metade da chave. Observe que estamos apenas armazenar e validando a chave pública em HGS; não mantemos nenhuma informação do certificado nem validarmos a data de expiração ou de cadeia de certificado.

    ```powershell
    Get-HgsClientHostKey -Path "C:\temp\$env:hostname-HostKey.cer"
    ```

5.  Copie o arquivo. cer para seu servidor HGS.

## <a name="add-the-host-key-to-the-attestation-service"></a>Adicionar a chave de host para o serviço de Atestado

Esta etapa é feita no servidor HGS e permite que o host executar VMs blindadas. É recomendável que você definir o nome para o FQDN ou identificador de recurso da máquina host, portanto, você pode facilmente consultar qual host a chave está instalado.

```powershell
Add-HgsAttestationHostKey -Name MyHost01 -Path "C:\temp\MyHost01-HostKey.cer"
``` 

## <a name="next-step"></a>Próximas etapas

> [!div class="nextstepaction"]
> [Confirmar hosts podem atestar com êxito](guarded-fabric-confirm-hosts-can-attest-successfully.md)

## <a name="see-also"></a>Consulte também

- [Implantar o serviço guardião de Host para hosts protegidos e VMs blindadas](guarded-fabric-deploying-hgs-overview.md)
