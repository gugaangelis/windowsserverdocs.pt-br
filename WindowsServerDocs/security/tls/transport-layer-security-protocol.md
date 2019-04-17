---
title: "Protocolo de segurança de camada de transporte"
description: "Segurança do Windows Server"
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: security-tls-ssl
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: de510bb0-a9f6-4bbe-8f8a-8dd7473bbae8
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/12/2016
ms.openlocfilehash: 94c81a79e5f3fd8fc22eafde3de8bd3d0bdd73f0
ms.sourcegitcommit: 583355400f6b0d880dc0ac6bc06f0efb50d674f7
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 10/17/2017
---
# Protocolo de segurança de camada de transporte

>Aplica-se a: Windows Server (anual por canal), Windows Server 2016

Este tópico para o profissional de TI descreve como o protocolo Transport Layer Security (TLS) funciona e fornece links para as RFCs IETF para TLS 1.2, TLS 1.1 e TLS 1.0.

Os protocolos TLS (e SSL) estão localizados entre a camada de protocolo de aplicativo e a camada de TCP/IP, onde eles podem segura e enviar dados de aplicativo para a camada de transporte. Como funcionam os protocolos entre a camada de aplicativo e a camada de transporte, TLS e SSL podem dar suporte a vários protocolos de camada do aplicativo.

TLS e SSL suponha que um transporte orientado a conexão, normalmente TCP, está em uso. O protocolo permite que os aplicativos cliente e servidor detectar os riscos de segurança a seguir:

-   Violação de mensagem

-   Interceptação de mensagens

-   Falsificação de mensagem

Os protocolos TLS e SSL podem ser divididos em duas camadas. A primeira camada consiste em protocolo do aplicativo e os três protocolos de handshake: o protocolo de handshake, o protocolo de especificações de codificação de alteração e o protocolo de alerta. A segunda camada é o protocolo de registro. A imagem a seguir ilustra as várias camadas e seus elementos.

**Camadas de protocolo TLS e SSL**


O SSP Schannel implementa os protocolos TLS e SSL sem modificação. O protocolo SSL é proprietário, mas a Internet Engineering Task Force produz as especificações de TLS públicas. Para obter informações sobre quais TLS ou SSL versão tem suporte nas versões do Windows, consulte [protocolos TLS/SSL (Schannel SSP)](https://msdn.microsoft.com/en-us/library/windows/desktop/mt808159(v=vs.85).aspx). A tabela a seguir lista as especificações para cada versão TLS. Cada especificação contém informações sobre:

-   O protocolo de registro de TLS

-   Os protocolos de handshake TLS: \-protocolo spec de codificação de alteração \-alertar protocolo

-   Cálculos criptográficos

-   Pacotes de codificação obrigatório

-   Protocolo de dados de aplicativo

[RFC 5246 - a versão do protocolo Transport Layer Security (TLS) 1.2](http://tools.ietf.org/html/rfc5246)

[RFC 4346 - a versão do protocolo Transport Layer Security (TLS) 1.1](http://tools.ietf.org/html/rfc4346)

[RFC 2246 sobrea versão do protocolo TLS 1.0](http://tools.ietf.org/html/rfc2246)

## <a name="BKMK_SessionResumption"></a>Continuidade da sessão TLS
Introduzida no Windows Server 2012 R2, o Schannel SSP implementado a parte do lado do servidor de retomada de sessão TLS. A implementação do lado do cliente do RFC 5077 foi adicionada no Windows 8.

Dispositivos que se conectam TLS aos servidores com frequência precisam se reconectar. Continuidade da sessão TLS reduz o custo de estabelecer conexões TLS como retomada envolve um handshake TLS abreviado. Isso facilita mais tentativas de retomada, permitindo que um grupo de servidores TLS retomar sessões TLS uns dos outros. Essa modificação oferece as seguintes economias para qualquer cliente TLS que dá suporte a RFC 5077, incluindo dispositivos Windows Phone e Windows RT:

-   Uso de recursos reduzida no servidor

-   Reduzido de largura de banda, o que aumenta a eficiência das conexões de cliente

-   Reduzido o tempo gasto para do handshake TLS devido continuações da conexão

Para obter informações sobre continuidade sem estado da sessão TLS, consulte o documento IETF [RFC 5077.](http://www.ietf.org/rfc/rfc5077)

## <a name="BKMK_AppProtocolNego"></a>Negociação do protocolo de aplicativo
 Windows Server 2012 R2 e Windows 8.1 introduziram o suporte que permite que a negociação de protocolo de aplicativo do lado do cliente TLS. Aplicativos podem aproveitar protocolos como parte do desenvolvimento padrão HTTP 2.0 e os usuários podem acessar os serviços online, como Google e Twitter usando aplicativos que estejam executando o protocolo SPDY.

Para obter informações sobre como funciona a negociação do protocolo de aplicativo, consulte [Transport Layer Security (TLS) extensão de negociação de protocolo de camada de aplicativo](http://tools.ietf.org/search/draft-ietf-tls-applayerprotoneg-05).

## <a name="BKMK_SNI"></a>Suporte a TLS extensões de indicação de nome de servidor
O recurso de indicação de nome de servidor (SNI) estende os protocolos SSL e TLS para permitir que a identificação apropriada do servidor quando várias imagens de virtual estão em execução em um único servidor. Em um cenário de hospedagem virtual, vários domínios (cada um com seu próprio certificado potencialmente distinto) são hospedados em um servidor. Nesse caso, o servidor não tem nenhuma maneira de antemão saber qual certificado para enviar para o cliente. SNI permite que o cliente informar o domínio de destino anteriormente no protocolo, e isso permite que o servidor selecionar corretamente o certificado apropriado.

Essa funcionalidade adicional:

-   Permite que você hospedar vários sites SSL em um único protocolo de Internet e a combinação de porta

-   Reduz o uso da memória quando vários sites SSL são hospedados em um servidor web único

-   Permite que os usuários mais para se conectar a sites SSL simultaneamente



