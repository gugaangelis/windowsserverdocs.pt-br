---
title: Protocolo de datagrama Transport Layer Security
description: "Segurança do Windows Server"
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
ms.date: 10/12/2016
ms.openlocfilehash: 31c1cf1f3218c0511a4407b560be30d0c6f86233
ms.sourcegitcommit: 583355400f6b0d880dc0ac6bc06f0efb50d674f7
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 10/17/2017
---
# Protocolo de datagrama Transport Layer Security

>Aplica-se a: Windows Server (anual por canal), Windows Server 2016

Este tópico de referência para o profissional de TI descreve o protocolo de datagrama Transport Layer Security (DTLS), que faz parte do Schannel Security Support Provider (SSP).

## <a name="BKMK_DTLS"></a>
Introduzido no Schannel SSP no Windows Server 2012 e Windows 8, o protocolo DTLS fornece a privacidade de comunicação para os protocolos de datagrama. Para obter informações sobre quais DTLS versão tem suporte nas versões do Windows, consulte [protocolos TLS/SSL (Schannel SSP)](https://msdn.microsoft.com/en-us/library/windows/desktop/mt808159(v=vs.85).aspx). O protocolo permite que aplicativos cliente e servidor para se comunicar de maneira que foi projetada para impedir a interceptação, adulteração ou falsificação de mensagem. O protocolo DTLS é baseado no protocolo Transport Layer Security (TLS), e ele fornece garantias de segurança equivalente, reduzindo a necessidade de usar IPsec ou a criação de um protocolo de segurança de camada de aplicativo personalizado.

Datagramas são comuns em streaming de mídia, como jogo ou protegida videoconferência. Os desenvolvedores podem desenvolver aplicativos para usar o protocolo DTLS dentro do contexto do modelo de Interface de provedor de suporte de segurança (SSPI) de autenticação do Windows para proteger a comunicação entre clientes e servidores. O protocolo DTLS baseia-se sobre o protocolo (UDP). DTLS foi projetado para ser mais semelhante ao TLS quanto possível para minimizar invenção de segurança e para maximizar a quantidade de reutilização de código e infraestrutura.

Os pacotes de codificação que estão disponíveis para configuração padronizadas após os que você pode definir para TLS. RC4 não é permitido. Schannel continua a usar a criptografia de próxima geração (CNG). Isso tira proveito da certificação FIPS 140, que foi introduzido no Windows Vista.

## Consulte também

[IETF RFC 4347 datagrama Transport Layer Security](http://tools.ietf.org/html/rfc4347)


