---
title: Instalar certificados de raiz confiáveis do TPM
ms.custom: na
ms.prod: windows-server-threshold
ms.topic: article
manager: dongill
author: rpsqrd
ms.technology: security-guarded-fabric
ms.date: 09/11/2018
ms.openlocfilehash: 2721bf13a7f6d3c90f9d055835fba51138ef75dd
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59841557"
---
# <a name="install-trusted-tpm-root-certificates"></a>Instalar certificados de raiz confiáveis do TPM

>Aplica-se a: Windows Server 2019, Windows Server (canal semestral), Windows Server 2016

Quando você configura o HGS usar atestado de TPM, você também precisará configurar o HGS para confiar os fornecedores dos TPMs em seus servidores.
Esse processo de verificação extra garante que somente os TPMs autênticos e confiáveis são capazes de atestar com o HGS.
Se você tentar registrar um TPM não confiável com `Add-HgsAttestationTpmHost`, você receberá um erro indicando que o fornecedor do TPM não é confiável.

Para confiar em sua TPMs, a raiz e intermediários certificados de autenticação usados para assinar a chave de endosso TPMs de seus servidores precisam ser instalados em HGS.
Se você usar mais de um modelo do TPM no seu datacenter, você talvez precise instalar certificados diferentes para cada modelo.
HGS ficará em "TrustedTPM_RootCA" e "TrustedTPM_IntermediateCA" certificado armazena os certificados de fornecedores.

> [!NOTE]
> Os certificados de fornecedor do TPM são diferentes daqueles instalado por padrão no Windows e representam a raiz específica e os certificados intermediários usados por fornecedores de TPM.

Uma coleção de certificados intermediários e raiz confiável de TPM é publicada pela Microsoft para sua conveniência.
Você pode usar as etapas abaixo para instalar esses certificados.
Se seus certificados TPM não estão incluídos no pacote abaixo, entre em contato com seu fornecedor TPM ou servidor OEM para obter os certificados raiz e intermediários para seu modelo específico do TPM.

Repita as etapas a seguir em **cada servidor HGS**:

1.  Baixe o pacote mais recente do [ https://tpmsec.microsoft.com/OnPremisesDHA/TrustedTPM.cab ](https://tpmsec.microsoft.com/OnPremisesDHA/TrustedTPM.cab).

2.  Expanda o arquivo cab.

    ```
    mkdir .\TrustedTPM
    expand.exe -F:* <Path-To-TrustedTpm.cab> .\TrustedTPM
    ```

3.  Por padrão, o script de configuração instalará certificados para cada fornecedor TPM. Se você quiser importar certificados para o seu fornecedor específico do TPM, exclua as pastas para fornecedores de TPM não confiados para sua organização.

4.  Instale o pacote de certificado confiável executando o script de instalação na pasta expandida.

    ```
    cd .\TrustedTPM
    .\setup.cmd
    ```

Para adicionar novos certificados ou aqueles intencionalmente ignorada durante uma instalação anterior, basta repetir as etapas acima em todos os nós no cluster HGS.
Certificados existentes permanecerão confiáveis, mas novos certificados encontrados no arquivo cab expandido serão adicionados ao TPM confiável armazena.

## <a name="next-step"></a>Próximas etapas

>[!div class="nextstepaction"]
[Configurar DNS de malha](guarded-fabric-configuring-fabric-dns-tpm.md)



