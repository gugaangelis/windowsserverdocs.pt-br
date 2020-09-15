---
title: TLS (Schannel SSP)
ms.topic: article
ms.assetid: ebd3c40c-b4c0-4f6d-a00c-f90eda4691df
author: justinha
ms.author: Justinha
ms.date: 05/16/2018
ms.openlocfilehash: 389a5a009320f7a19f5cbf942fe7c86f08f573ac
ms.sourcegitcommit: 7cacfc38982c6006bee4eb756bcda353c4d3dd75
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/14/2020
ms.locfileid: "90078523"
---
# <a name="tls-schannel-ssp-changes-in-windows-10-and-windows-server-2016"></a>Alterações de TLS (Schannel SSP) no Windows 10 e no Windows Server 2016

>Aplicável a: Windows Server (canal semestral), Windows Server 2016 e Windows 10

## <a name="cipher-suite-changes"></a>Alterações do conjunto de codificação

Windows 10, versão 1511 e Windows Server 2016 Adicione suporte para configuração da ordem do conjunto de codificação usando o MDM (gerenciamento de dispositivo móvel).

Para alterações de ordem de prioridade do pacote de codificação, consulte [pacotes de codificação no Schannel](/windows/win32/secauthn/cipher-suites-in-schannel).

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

A partir do Windows 10, versão 1507 e Windows Server 2016, os certificados SHA 512 têm suporte por padrão.

### <a name="rsa-key-changes"></a>Alterações de chave RSA

Windows 10, versão 1507 e Windows Server 2016 adicionar opções de configuração do registro para os tamanhos de chave RSA do cliente.

Para obter mais informações, consulte [KeyExchangeAlgorithm-Client RSA Key sizes](tls-registry-settings.md#keyexchangealgorithm---client-rsa-key-sizes).

### <a name="diffie-hellman-key-changes"></a>Alterações de chave de Diffie-Hellman

Windows 10, versão 1507 e Windows Server 2016 adicionar opções de configuração do registro para tamanhos de chave de Diffie-Hellman.

Para obter mais informações, consulte [tamanhos de chave KeyExchangeAlgorithm-Diffie-Hellman](tls-registry-settings.md#keyexchangealgorithm---diffie-hellman-key-sizes).

### <a name="sch_use_strong_crypto-option-changes"></a>SCH_USE_STRONG_CRYPTO alterações de opção

Com o Windows 10, versão 1507 e Windows Server 2016, [SCH_USE_STRONG_CRYPTO](/windows/win32/api/schannel/ns-schannel-schannel_cred) opção agora desabilita as codificações nulas, MD5, des e exportar.

## <a name="elliptical-curve-changes"></a>Alterações de curva elíptica

Windows 10, versão 1507 e Windows Server 2016 adicione Política de Grupo configuração para curvas elípticas em configuração do computador > Modelos Administrativos > rede > definições de configuração SSL.
A lista ordem de curva de ECC especifica a ordem na qual as curvas elípticas são preferenciais e habilita as curvas com suporte que não estão habilitadas.

Adicionado suporte para as seguintes curvas elípticas:

- BrainpoolP256r1 (RFC 7027) no Windows 10, versão 1507 e Windows Server 2016
- BrainpoolP384r1 (RFC 7027) no Windows 10, versão 1507 e Windows Server 2016
- BrainpoolP512r1 (RFC 7027) no Windows 10, versão 1507 e Windows Server 2016
- Curve25519 (RFC draft-ietf-TLS-Curve25519) no Windows 10, versão 1607 e Windows Server 2016

## <a name="dispatch-level-support-for-sealmessage--unsealmessage"></a>Suporte de nível de expedição para SealMessage & UnsealMessage

O Windows 10, versão 1507 e Windows Server 2016 adicionam suporte para SealMessage/UnsealMessage no nível de expedição.

## <a name="dtls-12"></a>DTLS 1,2

Windows 10, versão 1607 e Windows Server 2016 adicionar suporte para DTLS 1,2 (RFC 6347).

## <a name="httpsys-thread-pool"></a>Pool de threads HTTP.SYS

Windows 10, versão 1607 e Windows Server 2016 adicionar configuração de registro do tamanho do pool de threads usado para lidar com Handshakes de TLS para HTTP.SYS.

Caminho de Registro:

HKLM\SYSTEM\CurrentControlSet\Control\LSA

Para especificar um tamanho máximo de pool de threads por núcleo de CPU, crie uma entrada **MaxAsyncWorkerThreadsPerCpu** .
Essa entrada não existe no Registro por padrão.
Depois de criar a entrada, altere o valor DWORD para o tamanho desejado.
Se não estiver configurado, o máximo será 2 threads por núcleo de CPU.

## <a name="next-protocol-negotiation-npn-support"></a>Suporte de NPN (Next Protocol negotiation)

A partir do Windows 10 versão 1703, a próxima negociação de protocolo (NPN) foi removida e não é mais suportada.

## <a name="pre-shared-key-psk"></a>PSK (chave pré-compartilhada)

Windows 10, versão 1607 e Windows Server 2016 Adicione suporte para algoritmo de troca de chave PSK (RFC 4279).

Adicionado suporte para os seguintes conjuntos de codificação PSK:

- TLS_PSK_WITH_AES_128_CBC_SHA256 (RFC 5487) no Windows 10, versão 1607 e Windows Server 2016
- TLS_PSK_WITH_AES_256_CBC_SHA384 (RFC 5487) no Windows 10, versão 1607 e Windows Server 2016
- TLS_PSK_WITH_NULL_SHA256 (RFC 5487) no Windows 10, versão 1607 e Windows Server 2016
- TLS_PSK_WITH_NULL_SHA384 (RFC 5487) no Windows 10, versão 1607 e Windows Server 2016
- TLS_PSK_WITH_AES_128_GCM_SHA256 (RFC 5487) no Windows 10, versão 1607 e Windows Server 2016
- TLS_PSK_WITH_AES_256_GCM_SHA384 (RFC 5487) no Windows 10, versão 1607 e Windows Server 2016

## <a name="session-resumption-without-server-side-state-server-side-performance-improvements"></a>Retomada da sessão sem melhorias de desempenho no servidor de estado do servidor

O Windows 10, versão 1507 e Windows Server 2016 fornecem 30% mais de retomadas de sessão por segundo com tíquetes de sessão em comparação com o Windows Server 2012.

## <a name="session-hash-and-extended-master-secret-extension"></a>Hash de sessão e extensão de segredo mestre estendido

O Windows 10, versão 1507 e Windows Server 2016 adicionam suporte para RFC 7627: hash de sessão TLS (Transport Layer Security) e extensão de segredo mestre estendido.

Devido a essa alteração, o Windows 10 e o Windows Server 2016 exigem atualizações de [provedor CNG SSL](/windows/win32/seccng/cng-ssl-provider-functions) de terceiros para dar suporte a NCRYPT_SSL_INTERFACE_VERSION_3 e para descrever essa nova interface.


## <a name="ssl-support"></a>Suporte a SSL

A partir do Windows 10, versão 1607 e Windows Server 2016, o cliente TLS e o servidor SSL 3,0 estão desabilitados por padrão.
Isso significa que, a menos que o aplicativo ou serviço solicite especificamente o SSL 3,0 por meio do SSPI, o cliente nunca oferecerá ou aceitará o SSL 3,0 e o servidor nunca selecionará SSL 3,0.

A partir do Windows 10 versão 1607 e do Windows Server 2016, o SSL 2,0 foi removido e não tem mais suporte.

## <a name="changes-to-windows-tls-adherence-to-tls-12-requirements-for-connections-with-non-compliant-tls-clients"></a>Alterações na adesão do Windows TLS aos requisitos de TLS 1,2 para conexões com clientes TLS não compatíveis

No TLS 1,2, o cliente usa a [extensão "signature_algorithms"](https://tools.ietf.org/html/rfc5246#section-7.4.1.4.1) para indicar ao servidor quais pares de algoritmos de assinatura/hash podem ser usados em assinaturas digitais (ou seja, certificados de servidor e troca de chaves de servidor).
A RFC 1,2 do TLS também exige que a mensagem de certificado do servidor obedeça "signature_algorithms" extensão:

"Se o cliente forneceu uma extensão" signature_algorithms ", todos os certificados fornecidos pelo servidor devem ser assinados por um par de algoritmos de hash/assinatura que aparece nessa extensão."

Na prática, alguns clientes TLS de terceiros não estão em conformidade com a RFC 1,2 do TLS e não incluem todos os pares de algoritmos de assinatura e de hash que eles estão dispostos a aceitar na extensão "signature_algorithms" ou omitem a extensão por completo (o segundo indica ao servidor que o cliente oferece suporte apenas a SHA1 com RSA, DSA ou ECDSA).

Um servidor TLS geralmente tem apenas um certificado configurado por ponto de extremidade, o que significa que o servidor nem sempre pode fornecer um certificado que atenda aos requisitos do cliente.

Antes do Windows 10 e do Windows Server 2016, a pilha do Windows TLS está estritamente em conformidade com os requisitos de TLS 1,2 RFC, resultando em falhas de conexão com clientes TLS não compatíveis com RFC e problemas de interoperabilidade.
No Windows 10 e no Windows Server 2016, as restrições são relaxadas e o servidor pode enviar um certificado que não esteja em conformidade com o TLS 1,2 RFC, se essa for a única opção do servidor.
O cliente pode continuar ou encerrar o handshake.

Ao validar certificados do servidor e do cliente, a pilha do Windows TLS é estritamente compatível com a RFC 1,2 do TLS e permite apenas a assinatura negociada e os algoritmos de hash no servidor e nos certificados do cliente.