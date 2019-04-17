---
title: TLS (Schannel SSP)
ms.custom: na
ms.prod: windows-server-threshold
ms.topic: article
ms.assetid: ebd3c40c-b4c0-4f6d-a00c-f90eda4691df
manager: alanth
author: justinha
ms.technology: security-authentication
ms.date: 11/09/2016
ms.openlocfilehash: 6e1a229bfc7063597f8994c71bbaec5637d084a4
ms.sourcegitcommit: 583355400f6b0d880dc0ac6bc06f0efb50d674f7
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 10/17/2017
---
# <a name="tls-schannel-ssp-changes-in-windows-10-and-windows-server-2016"></a>Alterações TLS (Schannel SSP) no Windows 10 e Windows Server 2016

>Aplica-se a: Windows Server 2016 e Windows 10

## <a name="cipher-suite-changes"></a>Alterações de pacote de codificação

Windows 10, versão 1511 e Windows Server 2016 adicionam suporte para a configuração da ordem de pacote de codificação usando o gerenciamento de dispositivos móveis (MDM).

Para alterações de ordem de prioridade de suite de codificação, consulte [pacotes de codificação no Schannel](https://msdn.microsoft.com/library/windows/desktop/aa374757.aspx).

Adicionado o suporte para os pacotes de codificação a seguir:

- TLS_ECDHE_RSA_WITH_AES_128_GCM_SHA256 (RFC 5289) no Windows 10, versão 1507 e Windows Server 2016
- TLS_ECDHE_RSA_WITH_AES_256_GCM_SHA384 (RFC 5289) no Windows 10, versão 1507 e Windows Server 2016

Alterar DisabledByDefault para os pacotes de codificação a seguir:

- TLS_DHE_DSS_WITH_AES_256_CBC_SHA256 (RFC 5246) no Windows 10, versão 1703
- TLS_DHE_DSS_WITH_AES_128_CBC_SHA256 (RFC 5246) no Windows 10, versão 1703
- TLS_DHE_DSS_WITH_AES_256_CBC_SHA (RFC 5246) no Windows 10, versão 1703
- TLS_DHE_DSS_WITH_AES_128_CBC_SHA (RFC 5246) no Windows 10, versão 1703
- TLS_DHE_DSS_WITH_3DES_EDE_CBC_SHA (RFC 5246) no Windows 10, versão 1703
- TLS_RSA_WITH_RC4_128_SHA no Windows 10, versão 1709
- TLS_RSA_WITH_RC4_128_MD5 no Windows 10, versão 1709

A partir do Windows 10, versão 1507 e Windows Server 2016, SHA 512 certificados são mantidos por padrão.

### <a name="rsa-key-changes"></a>Alterações de chaves RSA

Windows 10, versão 1507 e Windows Server 2016 adicionam opções de configuração do registro para tamanhos de chave de RSA de cliente.

Para obter mais informações, consulte [KeyExchangeAlgorithm - tamanhos de chave RSA de cliente](tls-registry-settings.md#keyexchangealgorithm---client-rsa-key-sizes).

### <a name="diffie-hellman-key-changes"></a>Mudanças importantes Diffie-Hellman

Windows 10, versão 1507 e Windows Server 2016 adicionam opções de configuração do registro para tamanhos de chave Diffie-Hellman.

Para obter mais informações, consulte [KeyExchangeAlgorithm - tamanhos de chave Diffie-Hellman](tls-registry-settings.md#keyexchangealgorithm---diffie-hellman-key-sizes).

### <a name="schusestrongcrypto-option-changes"></a>Alterações de opção SCH_USE_STRONG_CRYPTO

Com o Windows 10, versão 1507 e Windows Server 2016, [SCH_USE_STRONG_CRYPTO](https://msdn.microsoft.com/library/windows/desktop/aa379810.aspx) opção agora desabilita NULL, MD5, DES e as codificações de exportação.

## <a name="elliptical-curve-changes"></a>Alterações de curva elíptica

Windows 10, versão 1507 e Windows Server 2016 adicionam uma configuração de política de grupo para Curvas Elípticas em configuração do computador > modelos administrativos > rede > configurações do SSL. A lista de ordem de curva ECC especifica a ordem em que Curvas Elípticas são preferenciais, bem como permite curvas com suporte que não estão habilitadas. 
 
Adicionado o suporte para os seguintes Curvas Elípticas:

- BrainpoolP256r1 (RFC 7027) no Windows 10, versão 1507 e Windows Server 2016
- BrainpoolP384r1 (RFC 7027) no Windows 10, versão 1507 e Windows Server 2016 
- BrainpoolP512r1 (RFC 7027) no Windows 10, versão 1507 e Windows Server 2016
- Curve25519 (RFC rascunho-ietf-tls-curve25519) no Windows 10, versão 1607 e Windows Server 2016

## <a name="dispatch-level-support-for-sealmessage--unsealmessage"></a>Suporte de nível de expedição para SealMessage & UnsealMessage

Windows 10, versão 1507 e Windows Server 2016 adicionam suporte para SealMessage/UnsealMessage no nível de expedição.

## <a name="dtls-12"></a>DTLS 1.2

Windows 10, versão 1607 e Windows Server 2016 adicionam suporte para 1.2 DTLS (RFC 6347).

## <a name="httpsys-thread-pool"></a>HTTP. Pool de threads SYS 

Windows 10, versão 1607 e Windows Server 2016 adicionam a configuração de registro do tamanho do pool de threads usado para manipular handshakes TLS para HTTP. SYS.

Caminho do registro: 

HKLM\System\CurrentControlSet\Control\LSA.

Para especificar um tamanho de pool máximo thread por núcleo de CPU, crie um **MaxAsyncWorkerThreadsPerCpu** entrada. Essa entrada não existir no registro por padrão. Depois que tiver criado a entrada, altere o valor DWORD para o tamanho desejado. Se não estiver configurado, o máximo é 2 threads por núcleo de CPU.

## <a name="next-protocol-negotiation-npn-support"></a>Suporte a negociação do protocolo (NPN) próximo

A partir do Windows 10 versão 1703, negociação de protocolo próximo (NPN) foi removido e não é mais compatível.

## <a name="pre-shared-key-psk"></a>Chave pré-compartilhada (PSK)

Windows 10, versão 1607 e Windows Server 2016 adicionam suporte para o algoritmo de troca de chave PSK (RFC 4279).

Adicionado o suporte para os seguintes pacotes de codificação PSK:

- TLS_PSK_WITH_AES_128_CBC_SHA256 (RFC 5487) no Windows 10, versão 1607 e Windows Server 2016
- TLS_PSK_WITH_AES_256_CBC_SHA384(RFC 5487) no Windows 10, versão 1607 e Windows Server 2016
- TLS_PSK_WITH_NULL_SHA256 (RFC 5487) no Windows 10, versão 1607 e Windows Server 2016
- TLS_PSK_WITH_NULL_SHA384 (RFC 5487) no Windows 10, versão 1607 e Windows Server 2016
- TLS_PSK_WITH_AES_128_GCM_SHA256 (RFC 5487) no Windows 10, versão 1607 e Windows Server 2016
- TLS_PSK_WITH_AES_256_GCM_SHA384 (RFC 5487) no Windows 10, versão 1607 e Windows Server 2016

## <a name="session-resumption-without-server-side-state-server-side-performance-improvements"></a>Continuidade da sessão sem melhorias de desempenho do lado do servidor de estado do lado do servidor

Windows 10, versão 1507 e Windows Server 2016 fornecem continuações de sessão de 30% mais por segundo tíquetes de sessão em comparação ao Windows Server 2012.

## <a name="session-hash-and-extended-master-secret-extension"></a>Hash de sessão e extensão de segredo mestre estendida

Windows 10, versão 1507 e Windows Server 2016 adicionam suporte a RFC 7627: Transport Layer Security (TLS) sessão Hash "e" extensão de segredo mestre estendido.

Devido a essa alteração, o Windows 10 e Windows Server 2016 requer terceiros 3ª [provedor CNG SSL](https://msdn.microsoft.com/library/windows/desktop/ff468652.aspx) atualizações para dar suporte a NCRYPT_SSL_INTERFACE_VERSION_3 e para descrever essa nova interface.


## <a name="ssl-support"></a>Suporte a SSL

A partir do Windows 10, versão 1607 e Windows Server 2016, o cliente TLS e o servidor SSL 3.0 está desabilitada por padrão. Isso significa que, a menos que o aplicativo ou serviço solicita especificamente SSL 3.0 via a SSPI, o cliente nunca será oferecer ou aceitar SSL 3.0 e o servidor nunca selecionará SSL 3.0.

A partir do Windows 10 versão 1607 e Windows Server 2016, o SSL 2.0 foi removido e não é mais compatível.

## <a name="changes-to-windows-tls-adherence-to-tls-12-requirements-for-connections-with-non-compliant-tls-clients"></a>Alterações em observância Windows TLS dos requisitos de TLS 1.2 para conexões com clientes TLS não compatível

No TLS 1.2, o cliente usa o [extensão "signature_algorithms"](https://tools.ietf.org/html/rfc5246#section-7.4.1.4.1) para indicar ao servidor que pares de algoritmo de hash/assinatura podem ser usados em assinaturas digitais (isto é, certificados de servidor e troca de chave de servidor). O TLS 1.2 RFC também exige que a mensagem de certificado de servidor respeitar extensão "signature_algorithms":

"Se o cliente forneceu uma extensão"signature_algorithms", em seguida, todos os certificados fornecidos pelo servidor devem ser assinados por um par de algoritmo de hash/assinatura que aparece nessa extensão."

Na prática, alguns clientes TLS de terceiros não estar em conformidade com o TLS 1.2 RFC e falha para incluir todos os a assinatura e eles estão prestes a aceitar na extensão de "signature_algorithms" de pares de algoritmo de hash ou omitir a extensão completamente (o último indica ao servidor que o cliente só dá suporte a SHA1 com RSA, DSA ou ECDSA).

Um servidor TLS geralmente só tem um certificado configurado por ponto de extremidade, o que significa que o servidor não pode sempre fornecer um certificado que atenda aos requisitos do cliente.

Antes do Windows 10 e Windows Server 2016, a pilha do Windows TLS estritamente respeitando os TLS 1.2 requisitos do RFC, resultando em falhas de conexão com os clientes de fora de conformidade TLS RFC e problemas de interoperabilidade. No Windows 10 e Windows Server 2016, as restrições são relaxadas e o servidor pode enviar um certificado que não é compatível com TLS 1.2 RFC, se essa for a única opção do servidor. O cliente pode, em seguida, continuar ou encerrar o handshake.

Ao validar certificados de cliente e servidor, a pilha do Windows TLS estritamente esteja em conformidade com o TLS 1.2 RFC e só permite a assinatura negociada e algoritmos de hash nos certificados cliente e servidor.


