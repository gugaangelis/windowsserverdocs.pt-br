---
title: Criar uma chave de host e adicioná-la ao HGS
ms.prod: windows-server
ms.topic: article
ms.assetid: a12c8494-388c-4523-8d70-df9400bbc2c0
manager: dongill
author: rpsqrd
ms.author: ryanpu
ms.technology: security-guarded-fabric
ms.date: 08/29/2018
ms.openlocfilehash: 3c234ab3d4925f4b03e252307aa905845fbb6d0d
ms.sourcegitcommit: acfdb7b2ad283d74f526972b47c371de903d2a3d
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/05/2020
ms.locfileid: "87769094"
---
# <a name="create-a-host-key-and-add-it-to-hgs"></a>Criar uma chave de host e adicioná-la ao HGS

>Aplica-se a: Windows Server 2019

Este tópico aborda como preparar hosts Hyper-V para se tornarem hosts protegidos usando o atestado de chave de host (modo de chave). Você criará um par de chaves de host (ou usará um certificado existente) e adicionará a metade pública da chave ao HGS.

## <a name="create-a-host-key"></a>Criar uma chave de host

1. Instale o Windows Server 2019 em seu computador host Hyper-V.
2. Instale os recursos de suporte do Hyper-v do Hyper-V e do guardião de host:

    ```powershell
    Install-WindowsFeature Hyper-V, HostGuardian -IncludeManagementTools -Restart
    ```

3. Gere uma chave de host automaticamente ou selecione um certificado existente. Se você estiver usando um certificado personalizado, ele deverá ter pelo menos uma chave RSA de 2048 bits, EKU de autenticação de cliente e uso de chave de assinatura digital.

    ```powershell
    Set-HgsClientHostKey
    ```

    Como alternativa, você pode especificar uma impressão digital se quiser usar seu próprio certificado.
    Isso pode ser útil se você quiser compartilhar um certificado em vários computadores ou usar um certificado associado a um TPM ou a um HSM. Aqui está um exemplo de criação de um certificado associado ao TPM (que impede que ele tenha a chave privada roubada e usada em outro computador e requer apenas um TPM 1,2):

    ```powershell
    $tpmBoundCert = New-SelfSignedCertificate -Subject "Host Key Attestation ($env:computername)" -Provider "Microsoft Platform Crypto Provider"
    Set-HgsClientHostKey -Thumbprint $tpmBoundCert.Thumbprint
    ```

4. Obtenha a metade pública da chave para fornecer ao servidor HGS. Você pode usar o cmdlet a seguir ou, se tiver o certificado armazenado em outro lugar, forneça uma. cer contendo a metade pública da chave. Observe que estamos apenas armazenando e validando a chave pública no HGS; Não mantemos nenhuma informação de certificado nem validamos a cadeia de certificados ou a data de validade.

    ```powershell
    Get-HgsClientHostKey -Path "C:\temp\$env:hostname-HostKey.cer"
    ```

5. Copie o arquivo. cer para o servidor HGS.

## <a name="add-the-host-key-to-the-attestation-service"></a>Adicionar a chave de host ao serviço de atestado

Essa etapa é feita no servidor HGS e permite que o host execute VMs blindadas. É recomendável que você defina o nome para o FQDN ou o identificador de recurso do computador host, para que você possa se referir facilmente a qual host a chave está instalada.

```powershell
Add-HgsAttestationHostKey -Name MyHost01 -Path "C:\temp\MyHost01-HostKey.cer"
```

## <a name="next-step"></a>Próxima etapa

- [Confirmar se hosts podem atestar com êxito](guarded-fabric-confirm-hosts-can-attest-successfully.md)

## <a name="additional-references"></a>Referências adicionais

- [Implantar o Serviço Guardião de Host para hosts protegidos e VMs blindadas](guarded-fabric-deploying-hgs-overview.md)
