---
title: Protocolo DTLS
description: Segurança do Windows Server
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: security-tls-ssl
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 57b8873a-ad9c-4f2c-93e0-a2af352c6965
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 05/16/2018
ms.openlocfilehash: 6f8d7d10ccaace0f75bb470647e3f4571940d9c0
ms.sourcegitcommit: afb0602767de64a76aaf9ce6a60d2f0e78efb78b
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/20/2019
ms.locfileid: "67284320"
---
# <a name="datagram-transport-layer-security-protocol"></a>Protocolo DTLS

Windows Server (canal semestral), Windows Server 2016, Windows 10

Este tópico de referência para profissionais de TI descreve o protocolo DTLS Datagram Transport Layer Security (), que faz parte do Schannel segurança suporte Provider (SSP).

## <a name="BKMK_DTLS"></a>
Introduzido no SSP Schannel no Windows Server 2012 e Windows 8, o protocolo DTLS proporciona privacidade de comunicação para protocolos de datagrama. Para obter informações sobre quais DTLS versão é suportada em versões do Windows, consulte [protocolos em TLS/SSL (Schannel SSP)](https://msdn.microsoft.com/library/windows/desktop/mt808159(v=vs.85).aspx). O protocolo permite que aplicativos cliente e servidor se comuniquem de uma maneira concebida para impedir espionagem, sabotagem ou falsificação de mensagem. O protocolo DTLS baseia-se no protocolo TLS e fornece garantias de segurança equivalentes, reduzindo a necessidade de usar o IPsec ou criar um protocolo personalizado de segurança de camada de aplicativos.

Datagramas são comuns em streaming de mídia, como jogo ou videoconferência segura. Os desenvolvedores podem desenvolver aplicativos para usar o protocolo DTLS dentro do contexto do modelo de Interface de provedor de suporte de segurança (SSPI) de autenticação do Windows para proteger a comunicação entre clientes e servidores. O protocolo DTLS baseia-se sobre o protocolo UDP (User Datagram). DTLS foi projetado para ser o mais semelhantes ao TLS possível para minimizar novas invenções de segurança e maximizar a quantidade de reutilização de código e infraestrutura.

Os conjuntos de codificação que estão disponíveis para configuração em conformidade com as que possíveis para o TLS. RC4 não é permitido. Schannel continua a usar a geração de CNG (Cryptography Next). Ele se beneficia de certificação FIPS 140, que foi introduzida no Windows Vista.

## <a name="see-also"></a>Consulte também

[Segurança da camada de transporte do datagrama IETF RFC 4347](http://tools.ietf.org/html/rfc4347)


