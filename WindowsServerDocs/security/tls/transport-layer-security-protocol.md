---
title: Protocolo TLS
description: Segurança do Windows Server
ms.custom: na
ms.prod: windows-server
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
ms.openlocfilehash: aca2db3ae5bf424dd0f855d24c1ef771039c8b14
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71403381"
---
# <a name="transport-layer-security-protocol"></a>Protocolo TLS

>Aplica-se a: Windows Server (canal semestral), Windows Server 2016, Windows 10

Este tópico para o profissional de ti descreve como funciona o protocolo TLS e fornece links para as RFCs da IETF para TLS 1,0, TLS 1,1 e TLS 1,2.

Os protocolos TLS (e SSL) estão localizados entre a camada de protocolo do aplicativo e a camada TCP/IP, na qual eles podem proteger e enviar dados de aplicativo para a camada de transporte. Como os protocolos funcionam entre a camada de aplicativo e a camada de transporte, o TLS e o SSL podem dar suporte a vários protocolos de camada de aplicativo.

O TLS e o SSL pressupõem que um transporte orientado a conexão, geralmente TCP, esteja em uso. O protocolo permite que aplicativos cliente e servidor detectem os seguintes riscos de segurança:

-   Violação de mensagem

-   Interceptação de mensagem

-   Falsificação de mensagem

Os protocolos TLS e SSL podem ser divididos em duas camadas. A primeira camada consiste no protocolo de aplicativo e nos três protocolos de handshake: o protocolo de handshake, o protocolo de especificação de codificação de alteração e o protocolo de alerta. A segunda camada é o protocolo de registro. A imagem a seguir ilustra as várias camadas e seus elementos.

**Camadas de protocolo TLS e SSL**


O SSP do Schannel implementa os protocolos TLS e SSL sem modificação. O protocolo SSL é proprietário, mas a força de tarefas de engenharia da Internet produz as especificações do TLS público. Para obter informações sobre qual versão TLS ou SSL tem suporte em versões do Windows, consulte [protocolos em TLS/SSL (Schannel SSP)](https://msdn.microsoft.com/library/windows/desktop/mt808159(v=vs.85).aspx). A tabela a seguir lista as especificações para cada versão de TLS. Cada especificação contém informações sobre:

-   O protocolo de registro TLS

-   Os protocolos de handshaking TLS: \- alterar protocolo de especificação de codificação \- protocolo de alerta

-   Computações criptográficas

-   Conjuntos de codificação obrigatórios

-   Protocolo de dados de aplicativo

[RFC 5246-a versão 1,2 do protocolo TLS da camada de transporte](http://tools.ietf.org/html/rfc5246)

[RFC 4346-a versão 1,1 do protocolo TLS da camada de transporte](http://tools.ietf.org/html/rfc4346)

[RFC 2246-o protocolo TLS versão 1,0](http://tools.ietf.org/html/rfc2246)

## <a name="BKMK_SessionResumption"></a>Retomada da sessão TLS
Introduzido no Windows Server 2012 R2, o SSP do Schannel implementou a parte do lado do servidor da retomada da sessão TLS. A implementação do lado do cliente do RFC 5077 foi adicionada ao Windows 8.

Dispositivos que conectam o TLS aos servidores frequentemente precisam se reconectar. A retomada da sessão TLS reduz o custo de estabelecer conexões TLS, pois a retomada envolve um handshake TLS abreviado. Isso facilita mais tentativas de retomadas, permitindo que um grupo de servidores TLS retome as sessões de TLS uns dos outros. Essa modificação fornece a economia a seguir para qualquer cliente TLS que ofereça suporte a RFC 5077, incluindo dispositivos Windows Phone e Windows RT:

-   Uso reduzido de recursos do servidor

-   Redução de largura de banda, o que melhora a eficiência das conexões de clientes

-   Menor tempo gasto para o handshake de TLS devido a retomadas da conexão

Para obter informações sobre a retomada de sessão TLS sem estado, consulte o documento IETF [RFC 5077.](http://www.ietf.org/rfc/rfc5077)

## <a name="BKMK_AppProtocolNego"></a>Negociação de protocolo de aplicativo
 O Windows Server 2012 R2 e o Windows 8.1 introduziu suporte que permite a negociação do protocolo de aplicativo TLS do lado do cliente. Os aplicativos podem aproveitar os protocolos como parte do desenvolvimento padrão HTTP 2,0, e os usuários podem acessar serviços online como Google e Twitter usando aplicativos que executam o protocolo SPDY.

Para obter informações sobre como funciona a negociação do protocolo de aplicativo, consulte [extensão de negociação do protocolo TLS](http://tools.ietf.org/search/draft-ietf-tls-applayerprotoneg-05).

## <a name="BKMK_SNI"></a>Suporte a TLS para extensões de Indicação de Nome de Servidor
O recurso de Indicação de Nome de Servidor (SNI) estende os protocolos SSL e TLS para permitir a identificação adequada do servidor quando várias imagens virtuais são executadas em um único servidor. Em um cenário de hospedagem virtual, vários domínios (cada um com seu próprio certificado potencialmente distinto) são hospedados em um servidor. Nesse caso, o servidor não tem como saber antecipadamente qual certificado enviar ao cliente. O SNI permite que o cliente informe o domínio de destino anteriormente no protocolo, e isso permite que o servidor selecione corretamente o certificado apropriado.

Esta funcionalidade adicional:

-   Permite hospedar vários sites SSL em uma única combinação de protocolo e porta da Internet

-   Reduz o uso de memória quando diversos sites SSL são hospedados em um único servidor Web

-   Permite que mais usuários se conectem a sites SSL simultaneamente



