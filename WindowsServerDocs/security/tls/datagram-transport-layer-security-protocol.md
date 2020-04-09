---
title: Protocolo DTLS
description: Segurança do Windows Server
ms.prod: windows-server
ms.technology: security-tls-ssl
ms.topic: article
ms.assetid: 57b8873a-ad9c-4f2c-93e0-a2af352c6965
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 05/16/2018
ms.openlocfilehash: d0c066b063cbfc8def54c2e0d02cbb0eaf7f1d40
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80852919"
---
# <a name="datagram-transport-layer-security-protocol"></a>Protocolo DTLS

Windows Server (Canal Semestral), Windows Server 2016, Windows 10

Este tópico de referência para o profissional de ti descreve o protocolo DTLS (datatransport Layer Security), que faz parte do SSP (provedor de suporte de segurança Schannel).

## <a name="BKMK_DTLS"></a>
Introduzido no Schannel SSP no Windows Server 2012 e no Windows 8, o protocolo DTLS fornece privacidade de comunicação para protocolos de datagrama. Para obter informações sobre qual versão do DTLS tem suporte em versões do Windows, consulte [protocolos em TLS/SSL (Schannel SSP)](https://msdn.microsoft.com/library/windows/desktop/mt808159(v=vs.85).aspx). O protocolo permite que aplicativos cliente e servidor se comuniquem de uma maneira concebida para impedir espionagem, sabotagem ou falsificação de mensagem. O protocolo DTLS baseia-se no protocolo TLS e fornece garantias de segurança equivalentes, reduzindo a necessidade de usar o IPsec ou criar um protocolo personalizado de segurança de camada de aplicativos.

Os datagramas são comuns em mídia de streaming, como jogos ou videoconferências de vídeo protegido. Os desenvolvedores podem desenvolver aplicativos para usar o protocolo DTLS dentro do contexto do modelo SSPI (interface do provedor de suporte de segurança) de autenticação do Windows para proteger a comunicação entre clientes e servidores. O protocolo DTLS é criado sobre o UDP (User Datagram Protocol). O DTLS foi projetado para ser tão semelhante ao TLS quanto possível para minimizar a nova invenção de segurança e maximizar a quantidade de reutilização de código e de infraestrutura.

Os conjuntos de codificação que estão disponíveis para configuração são padronizados após aqueles que você pode configurar para TLS. RC4 não é permitido. O Schannel continua a usar a CNG (Cryptography Next Generation). Isso aproveita a certificação FIPS 140, que foi introduzida no Windows Vista.

## <a name="see-also"></a>Consulte também

[Segurança da camada de transporte de datagrama IETF RFC 4347](http://tools.ietf.org/html/rfc4347)


                                        