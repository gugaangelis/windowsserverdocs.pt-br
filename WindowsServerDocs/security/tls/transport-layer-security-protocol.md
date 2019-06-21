---
title: Protocolo TLS
description: Segurança do Windows Server
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: security-tls-ssl
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: de510bb0-a9f6-4bbe-8f8a-8dd7473bbae8
author: justinha
ms.author: justinha
manager: brianlic-msft
ms.date: 05/16/2018
ms.openlocfilehash: 0a3b241fe0d2a61361d551b7f515507ad55d71cd
ms.sourcegitcommit: afb0602767de64a76aaf9ce6a60d2f0e78efb78b
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/20/2019
ms.locfileid: "67284235"
---
# <a name="transport-layer-security-protocol"></a>Protocolo TLS

>Aplica-se a: Windows Server (canal semestral), Windows Server 2016, Windows 10

Este tópico para profissionais de TI descreve como o protocolo de segurança de camada de transporte (TLS) funciona e fornece links para a IETF RFCs para TLS 1.0, TLS 1.1 e TLS 1.2.

Os protocolos TLS (e SSL) estão localizados entre a camada de protocolo de aplicativo e a camada de TCP/IP, onde podem proteger e enviar dados de aplicativo para a camada de transporte. Como funcionam os protocolos entre a camada de aplicativo e a camada de transporte, TLS e SSL podem dar suporte a vários protocolos de camada de aplicativo.

TLS e SSL supõem que um transporte orientado a conexão, normalmente, o TCP, está em uso. O protocolo permite que aplicativos cliente e servidor detectar os seguintes riscos de segurança:

-   Violação da mensagem

-   Interceptação de mensagens

-   Falsificação de mensagem

Os protocolos TLS e SSL podem ser divididos em duas camadas. A primeira camada consiste em três protocolos de handshake e o protocolo de aplicativo: o protocolo de handshake, o protocolo de especificações de codificação de alteração e o protocolo de alerta. A segunda camada é o protocolo de registro. A imagem a seguir ilustra várias camadas e seus elementos.

**Camadas de protocolo TLS e SSL**


O SSP Schannel implementa os protocolos TLS e SSL sem modificação. O protocolo SSL é proprietário, mas a Internet Engineering Task Force produz as especificações de TLS públicas. Para obter informações sobre quais TLS ou SSL versão é suportada em versões do Windows, consulte [protocolos em TLS/SSL (Schannel SSP)](https://msdn.microsoft.com/library/windows/desktop/mt808159(v=vs.85).aspx). A tabela a seguir lista as especificações para cada versão do TLS. Cada especificação contém informações sobre:

-   O protocolo de registro de TLS

-   Os protocolos de handshake TLS: \- Alterar o protocolo de cipher spec \- protocolo de alerta

-   Cálculos criptográficos

-   Conjuntos de criptografia obrigatória

-   Protocolo de dados de aplicativo

[RFC 5246 - Transport Layer Security (TLS) Protocol versão 1.2](http://tools.ietf.org/html/rfc5246)

[RFC 4346 - Transport Layer Security (TLS) Protocol versão 1.1](http://tools.ietf.org/html/rfc4346)

[RFC 2246 - a versão do protocolo TLS 1.0](http://tools.ietf.org/html/rfc2246)

## <a name="BKMK_SessionResumption"></a>Retomada da sessão TLS
Introduzido no Windows Server 2012 R2, o SSP Schannel implementado a parte do lado do servidor de retomada da sessão TLS. A implementação do lado do cliente do RFC 5077 foi adicionada no Windows 8.

Dispositivos que conectam o TLS aos servidores frequentemente precisam se reconectar. Retomada da sessão TLS reduz o custo de estabelecer conexões de TLS, porque a continuidade envolve um handshake de TLS abreviado. Isso facilita mais tentativas de retomada, permitindo que um grupo de servidores TLS para retomar as sessões TLS uns dos outros. Essa modificação fornece as seguintes economias para qualquer cliente TLS que dá suporte ao RFC 5077, incluindo dispositivos Windows Phone e Windows RT:

-   Uso reduzido de recursos do servidor

-   Redução de largura de banda, o que melhora a eficiência das conexões de clientes

-   Menos tempo gasto para o handshake TLS devido às retomadas da conexão

Para obter informações sobre a retomada da sessão TLS sem monitoração de estado, consulte o documento IETF [RFC 5077.](http://www.ietf.org/rfc/rfc5077)

## <a name="BKMK_AppProtocolNego"></a>Negociação de protocolo de aplicativo
 Windows Server 2012 R2 e Windows 8.1 introduziram o suporte que permite que a negociação de protocolo de aplicativo do lado do cliente TLS. Aplicativos podem aproveitar os protocolos como parte do desenvolvimento padrão HTTP 2.0 e os usuários podem acessar serviços online, como Google e Twitter por meio de aplicativos em execução o protocolo SPDY.

Para obter informações sobre como funciona a negociação de protocolo de aplicativo, consulte [extensão de negociação de protocolo de camada de aplicativo segurança de camada de transporte (TLS)](http://tools.ietf.org/search/draft-ietf-tls-applayerprotoneg-05).

## <a name="BKMK_SNI"></a>Suporte do TLS para extensões de indicação de nome de servidor
O recurso de indicação de nome de servidor (SNI) estende os protocolos SSL e TLS para permitir identificação adequada do servidor quando diversas imagens virtuais estão em execução em um único servidor. Em um cenário de hospedagem virtual, vários domínios (cada um com seu próprio certificado potencialmente distinto) são hospedados em um servidor. Nesse caso, o servidor não tem como saber previamente qual certificado para enviar para o cliente. O SNI permite que o cliente informar o domínio de destino antecipadamente no protocolo, e isso permite que o servidor selecione corretamente o certificado apropriado.

Esta funcionalidade adicional:

-   Permite que você hospede vários sites SSL em uma combinação de porta e protocolo de Internet único

-   Reduz o uso de memória quando diversos sites SSL são hospedados em um único servidor Web

-   Permite que mais usuários para se conectar a sites SSL simultaneamente



