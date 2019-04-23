---
title: TLS (Schannel SSP)
ms.custom: na
ms.prod: windows-server-threshold
ms.topic: article
ms.assetid: ebd3c40c-b4c0-4f6d-a00c-f90eda4691df
manager: alanth
author: justinha
ms.technology: security-authentication
ms.date: 05/16/2018
ms.openlocfilehash: 030fd81e0c6ba0423f1fa73e680006766cf2b180
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59890767"
---
# <a name="tls-schannel-ssp-changes-in-windows-10-and-windows-server-2016"></a>Alterações TLS (Schannel SSP) no Windows 10 e Windows Server 2016

>Aplica-se a: Windows Server (canal semestral), Windows Server 2016 e Windows 10

## <a name="cipher-suite-changes"></a>Alterações do Cipher Suite

Windows 10, versão 1511 e Windows Server 2016 adicionam suporte para a configuração da ordem do conjunto de codificação usando o gerenciamento de dispositivo móvel (MDM).

Para alterações de ordem de prioridade do cipher suite, consulte [conjuntos de codificação no Schannel](https://msdn.microsoft.com/library/windows/desktop/aa374757.aspx).

Adicionado suporte para os seguintes conjuntos de codificação:

- TLS_ECDHE_RSA_WITH_AES_128_GCM_SHA256 (RFC 5289) no Windows 10, versão 1507 e Windows Server 2016
- TLS_ECDHE_RSA_WITH_AES_256_GCM_SHA384 (RFC 5289) no Windows 10, versão 1507 e Windows Server 2016

DisabledByDefault alterar para os seguintes conjuntos de codificação:

- TLS_DHE_DSS_WITH_AES_256_CBC_SHA256 (RFC 5246) no Windows 10, versão 1703
- TLS_DHE_DSS_WITH_AES_128_CBC_SHA256 (RFC 5246) no Windows 10, versão 1703
- TLS_DHE_DSS_WITH_AES_256_CBC_SHA (RFC 5246) no Windows 10, versão 1703
- TLS_DHE_DSS_WITH_AES_128_CBC_SHA (RFC 5246) no Windows 10, versão 1703
- TLS_DHE_DSS_WITH_3DES_EDE_CBC_SHA (RFC 5246) no Windows 10, versão 1703
- TLS_RSA_WITH_RC4_128_SHA no Windows 10, versão 1709
- TLS_RSA_WITH_RC4_128_MD5 no Windows 10, versão 1709

Começando com o Windows 10, versão 1507 e Windows Server 2016, os certificados SHA 512 têm suporte por padrão.

### <a name="rsa-key-changes"></a>Alterações de chave RSA

Windows 10, versão 1507 e Windows Server 2016 adicionam opções de configuração do registro para tamanhos de chave de RSA do cliente.

Para obter mais informações, consulte [KeyExchangeAlgorithm - tamanhos de chave RSA de cliente](tls-registry-settings.md#keyexchangealgorithm---client-rsa-key-sizes).

### <a name="diffie-hellman-key-changes"></a>Alterações de chave Diffie-Hellman

Windows 10, versão 1507 e Windows Server 2016 adicionam opções de configuração do registro para os tamanhos de chave Diffie-Hellman.

Para obter mais informações, consulte [KeyExchangeAlgorithm - tamanhos de chave Diffie-Hellman](tls-registry-settings.md#keyexchangealgorithm---diffie-hellman-key-sizes).

### <a name="schusestrongcrypto-option-changes"></a>SCH_USE_STRONG_CRYPTO option changes

Com o Windows 10, versão 1507 e Windows Server 2016, [SCH_USE_STRONG_CRYPTO](https://msdn.microsoft.com/library/windows/desktop/aa379810.aspx) opção agora desabilita NULL, MD5, DES e exportar as codificações.

## <a name="elliptical-curve-changes"></a>Alterações de curva elípticos

Windows 10, versão 1507 e Windows Server 2016 adicionam configuração de diretiva de grupo para curvas elípticos em configuração do computador > modelos administrativos > rede > definições de configuração de SSL. A lista de ordem de curva ECC especifica a ordem na qual as curvas elípticos são preferenciais, bem como permite curvas com suporte que não estão habilitadas. 
 
Adicionado suporte para curvas elípticos seguintes:

- BrainpoolP256r1 (RFC 7027) no Windows 10, versão 1507 e Windows Server 2016
- BrainpoolP384r1 (RFC 7027) no Windows 10, versão 1507 e Windows Server 2016 
- BrainpoolP512r1 (RFC 7027) no Windows 10, versão 1507 e Windows Server 2016
- Curve25519 (RFC draft-ietf-tls-curve25519) no Windows 10, versão 1607 e Windows Server 2016

## <a name="dispatch-level-support-for-sealmessage--unsealmessage"></a>Suporte de nível de expedição para SealMessage & UnsealMessage

Windows 10, versão 1507 e Windows Server 2016 adicionam suporte para SealMessage/UnsealMessage no nível de expedição.

## <a name="dtls-12"></a>DTLS 1.2

Windows 10, versão 1607 e Windows Server 2016 adicionam suporte para o DTLS 1.2 (RFC 6347).

## <a name="httpsys-thread-pool"></a>HTTP. Pool de threads SYS 

Windows 10, versão 1607 e Windows Server 2016 adicionam configuração de registro do tamanho do pool de threads usado para manipular os handshakes TLS para HTTP. SYS.

Caminho do registro: 

HKLM\SYSTEM\CurrentControlSet\Control\LSA

Para especificar um tamanho de pool máxima de threads por núcleo da CPU, crie uma **MaxAsyncWorkerThreadsPerCpu** entrada. Essa entrada não existe no Registro por padrão. Depois de criar a entrada, altere o valor DWORD para o tamanho desejado. Se não configurado, o máximo é 2 threads por núcleo da CPU.

## <a name="next-protocol-negotiation-npn-support"></a>Suporte de negociação de protocolo (NPN) Avançar

Começando com o Windows 10 versão 1703, negociação de protocolo próximo (NPN) foi removido e não é mais suportada.

## <a name="pre-shared-key-psk"></a>Chave pré-compartilhada (PSK)

Windows 10, versão 1607 e Windows Server 2016 adicionam suporte para o algoritmo de troca de chaves de PSK (RFC 4279).

Adicionado suporte para os seguintes conjuntos de codificação PSK:

- TLS_PSK_WITH_AES_128_CBC_SHA256 (RFC 5487) no Windows 10, versão 1607 e Windows Server 2016
- TLS_PSK_WITH_AES_256_CBC_SHA384(RFC 5487) no Windows 10, versão 1607 e Windows Server 2016
- TLS_PSK_WITH_NULL_SHA256 (RFC 5487) no Windows 10, versão 1607 e Windows Server 2016
- TLS_PSK_WITH_NULL_SHA384 (RFC 5487) no Windows 10, versão 1607 e Windows Server 2016
- TLS_PSK_WITH_AES_128_GCM_SHA256 (RFC 5487) no Windows 10, versão 1607 e Windows Server 2016
- TLS_PSK_WITH_AES_256_GCM_SHA384 (RFC 5487) no Windows 10, versão 1607 e Windows Server 2016

## <a name="session-resumption-without-server-side-state-server-side-performance-improvements"></a>Retomada da sessão sem melhorias de desempenho do lado do servidor de estado do servidor

Windows 10, versão 1507 e Windows Server 2016 fornecem continuações de sessão de 30% a mais por segundo com tíquetes de sessão em comparação comparadas o Windows Server 2012.

## <a name="session-hash-and-extended-master-secret-extension"></a>Hash de sessão e extensão de segredo mestre estendido

Windows 10, versão 1507 e Windows Server 2016 adicionar suporte para RFC 7627: Hash de sessão TLS (segurança) de camada de transporte e estendido de extensão do segredo mestre.

Devido a essa alteração, o Windows 10 e Windows Server 2016 requer 3ª parte [provedor de CNG SSL](https://msdn.microsoft.com/library/windows/desktop/ff468652.aspx) atualizações para dar suporte a NCRYPT_SSL_INTERFACE_VERSION_3 e para descrever essa nova interface.


## <a name="ssl-support"></a>Suporte a SSL

Começando com o Windows 10, versão 1607 e Windows Server 2016, o cliente do TLS e o servidor SSL 3.0 está desabilitado por padrão. Isso significa que, a menos que o aplicativo ou serviço solicita especificamente o SSL 3.0 por meio de SSPI, o cliente nunca oferecem ou aceite o SSL 3.0 e o servidor nunca selecionará o SSL 3.0.

Começando com o Windows 10 versão 1607 e Windows Server 2016, o SSL 2.0 foi removido e não é mais suportada.

## <a name="changes-to-windows-tls-adherence-to-tls-12-requirements-for-connections-with-non-compliant-tls-clients"></a>Alterações a adesão de TLS do Windows aos requisitos de TLS 1.2 para conexões com clientes TLS fora de conformidade

No protocolo TLS 1.2, o cliente usa o ["signature_algorithms" extensão](https://tools.ietf.org/html/rfc5246#section-7.4.1.4.1) para indicar ao servidor quais pares de algoritmo de assinatura/hash podem ser usados em assinaturas digitais (ou seja, certificados de servidor e troca de chaves de servidor). O RFC do TLS 1.2 também requer que a mensagem de certificado do servidor honrar "signature_algorithms" extensão:

"Se o cliente forneceu uma extensão"signature_algorithms", em seguida, todos os certificados fornecidos pelo servidor devem ser assinados por um par de algoritmo de assinatura/hash aparece na extensão."

Na prática, alguns clientes TLS de terceiros não está em conformidade com a RFC do TLS 1.2 e fail para incluir todos os a assinatura e que estão dispostos a aceitar a extensão "signature_algorithms" de pares de algoritmo de hash ou omitir a extensão completamente (o último indica ao o servidor que o cliente oferece suporte apenas a SHA1 com RSA, DSA ou ECDSA).

Um servidor TLS geralmente só tiver um certificado configurado por um ponto de extremidade, o que significa que o servidor sempre não é possível fornecer um certificado que atenda aos requisitos do cliente.

Antes do Windows 10 e Windows Server 2016, a pilha de TLS do Windows estritamente está em conformidade com os TLS 1.2 requisitos de RFC, resultando em falhas de conexão com clientes TLS fora de conformidade do RFC e problemas de interoperabilidade. No Windows 10 e Windows Server 2016, as restrições são relaxadas e o servidor pode enviar um certificado que não é compatível com RFC do TLS 1.2, se isso for a única opção do servidor. O cliente pode, em seguida, continuar ou finalizar o handshake.

Ao validar certificados de cliente e servidor, a pilha de TLS do Windows está estritamente em conformidade com a RFC do TLS 1.2 e permite apenas a assinatura negociada e os algoritmos de hash em certificados de cliente e servidor.


