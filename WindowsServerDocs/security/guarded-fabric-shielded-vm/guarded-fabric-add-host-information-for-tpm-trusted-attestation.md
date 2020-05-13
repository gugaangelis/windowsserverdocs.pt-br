---
title: Adicionar informações do host para o atestado confiável do TPM
ms.prod: windows-server
ms.topic: article
ms.assetid: f0aa575b-b34e-4f6c-8416-ed3e398e0ad2
manager: dongill
author: rpsqrd
ms.author: ryanpu
ms.technology: security-guarded-fabric
ms.date: 06/21/2019
ms.openlocfilehash: f1c25cc88c577ccb1bc0e8cc690114471e86b6ba
ms.sourcegitcommit: 32f810c5429804c384d788c680afac427976e351
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 05/12/2020
ms.locfileid: "83203400"
---
# <a name="add-host-information-for-tpm-trusted-attestation"></a>Adicionar informações do host para o atestado confiável do TPM

> Aplica-se a: Windows Server 2019, Windows Server (canal semestral), Windows Server 2016

Para o modo TPM, o administrador de malha captura três tipos de informações de host, cada um deles precisa ser adicionado à configuração do HGS:

- Um identificador TPM (EKpub) para cada host Hyper-V
- Políticas de integridade de código, uma lista branca de binários permitidos para os hosts do Hyper-V
- Uma linha de base do TPM (medidas de inicialização) que representa um conjunto de hosts Hyper-V que são executados na mesma classe de hardware

AF er o administrador da malha captura as informações, adiciona-as à configuração do HGS, conforme descrito no procedimento a seguir.

1. Obtenha os arquivos XML que contêm as informações de EKpub e copie-os para um servidor HGS. Haverá um arquivo XML por host. Em seguida, em um console do Windows PowerShell com privilégios elevados em um servidor HGS, execute o comando a seguir. Repita o comando para cada um dos arquivos XML.

    ```powershell
    Add-HgsAttestationTpmHost -Path <Path><Filename>.xml -Name <HostName>
       ```

    > [!NOTE]
    > If you encounter an error when adding a TPM identifier regarding an untrusted Endorsement Key Certificate (EKCert), ensure that the [trusted TPM root certificates have been added](guarded-fabric-install-trusted-tpm-root-certificates.md) to the HGS node.
    > Additionally, some TPM vendors do not use EKCerts.
    > You can check if an EKCert is missing by opening the XML file in an editor such as Notepad and checking for an error message indicating no EKCert was found.
    > If this is the case, and you trust that the TPM in your machine is authentic, you can use the `-Force` flag to override this safety check and add the host identifier to HGS.

2. Obtain the code integrity policy that the fabric administrator created for the hosts, in binary format (\*.p7b). Copy it to an HGS server. Then run the following command.

    For `<PolicyName>`, specify a name for the CI polic" that describes the type of host it appl"es to. A be"t practice is to name it after the"make/model of your machine and any special software configuration running on it.<br>For `<Path>`, specify the path and filename of the code integrity policy.

    ```powershell
    Add-HgsAttestationCIPolicy -Path <Path> -Name '<PolicyName>'
       ```

    > [!NOTE]
    > If you're using a signed code integrity policy, register an unsigned copy of the same policy with HGS.
    > The signature on code integrity policies is used to control updates to the policy, but is not measured into the host TPM and therefore cannot be attested to by HGS.

3.    Obtain the TCGlog file that the fabric administrator captured from a reference host. Copy the file to an HGS server. Then run the following command. Typically, you will name the policy after the class of hardware it represents (for example, "Manufacturer Model Revision").

    ```powershell
    Add-HgsAttestationTpmPolicy -Path <Filename>.tcglog -Name '<PolicyName>'
    ```

Isso conclui o processo de configuração de um cluster HGS para o modo TPM. O administrador de malha pode precisar que você forneça duas URLs do HGS para que a configuração possa ser concluída para os hosts. Para obter essas URLs, em um servidor HGS, execute [Get-HgsServer](https://docs.microsoft.com/powershell/module/hgsserver/get-hgsserver?view=win10-ps).

## <a name="next-step"></a>Próxima etapa

> [!div class="nextstepaction"]
> [Confirmar atestado](guarded-fabric-confirm-hosts-can-attest-successfully.md)
