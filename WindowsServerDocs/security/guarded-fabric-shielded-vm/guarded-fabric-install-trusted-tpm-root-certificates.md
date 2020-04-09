---
title: Instalar certificados raiz de TPM confiáveis
ms.prod: windows-server
ms.topic: article
manager: dongill
author: rpsqrd
ms.author: ryanpu
ms.technology: security-guarded-fabric
ms.date: 06/27/2019
ms.openlocfilehash: 096a40f422f308a036b8062e4515ebe698c31f08
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80856569"
---
# <a name="install-trusted-tpm-root-certificates"></a>Instalar certificados raiz de TPM confiáveis

>Aplica-se a: Windows Server 2019, Windows Server (canal semestral), Windows Server 2016

Ao configurar o HGS para usar o atestado de TPM, você também precisa configurar o HGS para confiar nos fornecedores do TPMs em seus servidores.
Esse processo de verificação adicional garante que apenas TPMs confiáveis e de confiança sejam capazes de atestar com seu HGS.
Se você tentar registrar um TPM não confiável com `Add-HgsAttestationTpmHost`, receberá um erro indicando que o fornecedor do TPM não é confiável.

Para confiar em seu TPMs, os certificados de assinatura raiz e intermediário usados para assinar a chave de endosso em seus servidores ' TPMs precisam ser instalados no HGS.
Se você usar mais de um modelo TPM em seu datacenter, talvez seja necessário instalar certificados diferentes para cada modelo.
O HGS examinará os repositórios de certificados "TrustedTPM_RootCA" e "TrustedTPM_IntermediateCA" para os certificados do fornecedor.

> [!NOTE]
> Os certificados de fornecedor do TPM são diferentes daqueles instalados por padrão no Windows e representam a raiz específica e os certificados intermediários usados pelos fornecedores do TPM.

Uma coleção de certificados raiz TPM confiáveis e intermediários é publicada pela Microsoft para sua conveniência.
Você pode usar as etapas abaixo para instalar esses certificados.
Se os certificados do TPM não estiverem incluídos no pacote abaixo, entre em contato com o fornecedor do TPM ou com o OEM do servidor para obter os certificados raiz e intermediário para seu modelo de TPM específico.

Repita as seguintes etapas em **todos os servidores HgS**:

1.  Baixe o pacote mais recente de [https://go.microsoft.com/fwlink/?linkid=2097925](https://go.microsoft.com/fwlink/?linkid=2097925).

2.  Verifique a assinatura do arquivo CAB para garantir sua autenticidade. Não prossiga se a assinatura não for válida.

    ```powershell
    Get-AuthenticodeSignature .\TrustedTpm.cab
    ```
    
    Veja a seguir um exemplo de saída:
    
    ```
    Directory: C:\Users\Administrator\Downloads
        
    SignerCertificate                         Status                                 Path
    -----------------                         ------                                 ----
    0DD6D4D4F46C0C7C2671962C4D361D607E370940  Valid                                  TrustedTpm.cab
    ```

2.  Expanda o arquivo CAB.

    ```
    mkdir .\TrustedTPM
    expand.exe -F:* <Path-To-TrustedTpm.cab> .\TrustedTPM
    ```

3.  Por padrão, o script de configuração instalará certificados para cada fornecedor do TPM. Se você quiser apenas importar certificados para seu fornecedor específico do TPM, exclua as pastas de fornecedores de TPM não confiáveis para sua organização.

4.  Instale o pacote de certificado confiável executando o script de instalação na pasta expandida.

    ```
    cd .\TrustedTPM
    .\setup.cmd
    ```

Para adicionar novos certificados ou aqueles que foram intencionalmente ignorados durante uma instalação anterior, basta repetir as etapas acima em cada nó em seu cluster HGS.
Os certificados existentes permanecerão confiáveis, mas novos certificados encontrados no arquivo CAB expandido serão adicionados aos repositórios TPM confiáveis.

## <a name="next-step"></a>Próximas etapas

> [!div class="nextstepaction"]
> [Configurar DNS da malha](guarded-fabric-configuring-fabric-dns-tpm.md)



