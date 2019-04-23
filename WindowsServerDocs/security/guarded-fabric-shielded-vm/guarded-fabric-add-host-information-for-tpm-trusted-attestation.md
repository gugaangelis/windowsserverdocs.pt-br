---
title: Adicionar informações de host para atestado de TPM confiável
ms.custom: na
ms.prod: windows-server-threshold
ms.topic: article
ms.assetid: f0aa575b-b34e-4f6c-8416-ed3e398e0ad2
manager: dongill
author: rpsqrd
ms.technology: security-guarded-fabric
ms.date: 08/29/2018
ms.openlocfilehash: 215764f48fc8cfa2b28f4b5f6ca7dfeeb53b9cce
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59851267"
---
>Aplica-se a: Windows Server 2019, Windows Server (canal semestral), Windows Server 2016

### <a name="add-host-information-for-tpm-trusted-attestation"></a>Adicionar informações de host para atestado de TPM confiável

Para o modo TPM, o administrador de malha captura três tipos de informações do host, cada um deles precisa ser adicionada à configuração de HGS:

- Um identificador TPM (EKpub) para cada host Hyper-V
- Políticas de integridade de código, uma lista de permissões de binários permitidos para os hosts do Hyper-V
- Uma linha de base do TPM (medições de inicialização) que representa um conjunto de Hyper-V hospeda que são executados na mesma classe de hardware

Depois de malha do administrador captura as informações, adicione-o à configuração do HGS, conforme descrito no procedimento a seguir.

1.  Obtém os arquivos XML que contêm as informações de EKpub e copiá-los para um servidor HGS. Haverá um arquivo XML por host. Em seguida, em um console do Windows PowerShell com privilégios elevados em um servidor HGS, execute o comando a seguir. Repita o comando para cada um dos arquivos XML.

    ```powershell
    Add-HgsAttestationTpmHost -Path <Path><Filename>.xml -Name <HostName>
    ```

    > [!NOTE]
    > Se você encontrar um erro ao adicionar um identificador TPM sobre um certificado de chave de endosso não confiável (EKCert), certifique-se de que o [confiáveis foram adicionados certificados de raiz TPM](guarded-fabric-install-trusted-tpm-root-certificates.md) para o nó HGS.
    > Além disso, alguns fornecedores TPM não usam EKCerts.
    > Você pode verificar se um EKCert está ausente, abrindo o arquivo XML em um editor como o bloco de notas e verificando uma mensagem de erro que indica nenhuma EKCert foi encontrado.
    > Se esse for o caso e você confia que o TPM no seu computador é autêntico, você pode usar o `-Force` sinalizador para substituir essa verificação de segurança e adicionar o identificador do host ao HGS.

2. Obter a política de integridade de código criado pelo administrador de malha para hosts, no formato binário (p7b). Copie-o para um servidor HGS. Em seguida, execute o comando a seguir.

    Para `<PolicyName>`, especifique um nome para a política de CI que descreve o tipo de host que ele se aplica ao. Uma prática recomendada é nomeá-lo após o marca/modelo do seu computador e qualquer configuração especial de software em execução nele.<br>Para `<Path>`, especifique o caminho e nome de arquivo de política de integridade de código.

    ```powershell
    Add-HgsAttestationCIPolicy -Path <Path> -Name '<PolicyName>'
    ```

3. Obtenha o arquivo de TCGlog que o administrador de malha capturado de um host de referência. Copie o arquivo para um servidor HGS. Em seguida, execute o comando a seguir. Normalmente, será nomear a política após a classe de hardware, que ele representa (por exemplo, "Revisão de modelo do fabricante").

    ```powershell
    Add-HgsAttestationTpmPolicy -Path <Filename>.tcglog -Name '<PolicyName>'
    ```

Isso conclui o processo de configuração de um cluster HGS para o modo TPM. O administrador de malha talvez seja necessário fornecer duas URLs do HGS antes de concluir a configuração para os hosts. Para obter essas URLs, em um servidor HGS, execute [Get-HgsServer](https://docs.microsoft.com/powershell/module/hgsserver/get-hgsserver?view=win10-ps).

## <a name="next-step"></a>Próximas etapas

>[!div class="nextstepaction"]
[Confirmar Atestado](guarded-fabric-confirm-hosts-can-attest-successfully.md)