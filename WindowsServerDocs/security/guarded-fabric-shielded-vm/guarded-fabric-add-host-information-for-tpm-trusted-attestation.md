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
ms.openlocfilehash: cfc1d0d2b99a79e6c1deb013fab350e3abc6167c
ms.sourcegitcommit: acfdb7b2ad283d74f526972b47c371de903d2a3d
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/05/2020
ms.locfileid: "87769664"
---
# <a name="add-host-information-for-tpm-trusted-attestation"></a>Adicionar informações do host para o atestado confiável do TPM

> Aplica-se a: Windows Server 2019, Windows Server (canal semestral), Windows Server 2016

Para o modo TPM, o administrador de malha captura três tipos de informações de host, cada um deles precisa ser adicionado à configuração do HGS:

- Um identificador TPM (EKpub) para cada host Hyper-V
- Políticas de integridade de código, uma lista branca de binários permitidos para os hosts do Hyper-V
- Uma linha de base do TPM (medidas de inicialização) que representa um conjunto de hosts Hyper-V que são executados na mesma classe de hardware

Depois que o administrador da malha capturar as informações, adicione-as à configuração do HGS, conforme descrito no procedimento a seguir.

1. Obtenha os arquivos XML que contêm as informações de EKpub e copie-os para um servidor HGS. Haverá um arquivo XML por host. Em seguida, em um console do Windows PowerShell com privilégios elevados em um servidor HGS, execute o comando a seguir. Repita o comando para cada um dos arquivos XML.

    ```powershell
    Add-HgsAttestationTpmHost -Path <Path><Filename>.xml -Name <HostName>
    ```

    > [!NOTE]
    > Se você encontrar um erro ao adicionar um identificador de TPM em relação a um certificado de chave de endosso não confiável (EKCert), verifique se os [certificados raiz do TPM confiável foram adicionados](guarded-fabric-install-trusted-tpm-root-certificates.md) ao nó HgS.
    > Além disso, alguns fornecedores de TPM não usam EKCerts.
    > Você pode verificar se um EKCert está ausente abrindo o arquivo XML em um editor como o bloco de notas e verificando se há uma mensagem de erro indicando que nenhum EKCert foi encontrado.
    > Se esse for o caso e você confiar que o TPM em seu computador é autêntico, você poderá usar o `-Force` sinalizador para substituir essa verificação de segurança e adicionar o identificador de host ao HgS.

2. Obtenha a política de integridade de código que o administrador de malha criou para os hosts, em formato binário ( \* . p7b). Copie-o para um servidor HGS. Em seguida, execute o comando a seguir.

    Para `<PolicyName>` , especifique um nome para a política CI que descreve o tipo de host ao qual se aplica. Uma prática recomendada é nomeá-la após a marca/modelo de seu computador e qualquer configuração de software especial em execução nela.<br>Para `<Path>` , especifique o caminho e o nome do arquivo da política de integridade de código.

    ```powershell
    Add-HgsAttestationCIPolicy -Path <Path> -Name '<PolicyName>'
    ```

    > [!NOTE]
    > Se você estiver usando uma política de integridade de código assinada, registre uma cópia não assinada da mesma política com o HGS.
    > A assinatura nas políticas de integridade de código é usada para controlar as atualizações da política, mas não é medida no TPM do host e, portanto, não pode ser atestada pelo HGS.

3. Obtenha o arquivo de log do TCG que o administrador de malha capturou de um host de referência. Copie o arquivo para um servidor HGS. Em seguida, execute o comando a seguir. Normalmente, você nomeará a política após a classe de hardware que ela representa (por exemplo, "revisão do modelo do fabricante").

    ```powershell
    Add-HgsAttestationTpmPolicy -Path <Filename>.tcglog -Name '<PolicyName>'
    ```

Isso conclui o processo de configuração de um cluster HGS para o modo TPM. O administrador de malha pode precisar que você forneça duas URLs do HGS para que a configuração possa ser concluída para os hosts. Para obter essas URLs, em um servidor HGS, execute [Get-HgsServer](https://docs.microsoft.com/powershell/module/hgsserver/get-hgsserver?view=win10-ps).

## <a name="next-step"></a>Próxima etapa

> [Confirmar atestado](guarded-fabric-confirm-hosts-can-attest-successfully.md)
