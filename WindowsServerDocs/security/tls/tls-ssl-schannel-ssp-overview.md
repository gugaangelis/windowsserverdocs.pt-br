---
title: "TLS - visão geral SSL (Schannel SSP)"
description: "Segurança do Windows Server"
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: security-tls-ssl
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 1b7b0432-1bef-4912-8c9a-8989d47a4da9
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/12/2016
ms.openlocfilehash: afd0b70264dba1e720f95e40d3d201c2c5bf1c64
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 12/12/2017
---
# <a name="tls---ssl-schannel-ssp-overview"></a>TLS - visão geral SSL (Schannel SSP)

>Aplica-se a: Windows Server (anual por canal), Windows Server 2016

Este tópico para o profissional de TI apresenta a implementação de TLS/SSL no Windows usando o Schannel Security Service Provider (SSP) descrevendo aplicativos práticos, as alterações na implementação da Microsoft e requisitos de software, além de recursos adicionais para o Windows Server 2012 e Windows 8.

**Você quis dizer:**

-   [O pacote de segurança Schannel](https://msdn.microsoft.com/library/ms678421.aspx)

-   [Canal seguro](https://msdn.microsoft.com/library/windows/desktop/aa380123.aspx)

-   [Protocolo de segurança de camada de transporte](https://msdn.microsoft.com/library/windows/desktop/aa380516.aspx)

## <a name="BKMK_OVER"></a>Descrição de \(Schannel\) TLS\SSL
Schannel é um \(SSP\) Security Support Provider que implementa o \(SSL\) Secure Sockets Layer e Transport Layer Security \(TLS\) protocolos de autenticação padrão da Internet.

\(SSPI\) a Interface do provedor de suporte de segurança é uma API usada pelos sistemas do Windows para executar funções relacionadas à security\ incluindo autenticação. A SSPI funciona como uma interface comum para diversas \(SSPs\) provedores de suporte de segurança, incluindo o Schannel SSP.

As versões do protocolo Transport Layer Security \(TLS\) 1.0, 1.1 e 1.2, o protocolo Secure Sockets Layer \(SSL\), o protocolo \(PCT\) versões 2.0 e 3.0, datagrama Transport Layer Security \(DTLS\) versão 1.0 e o transporte de comunicações privadas são baseadas em criptografia de chave pública. O conjunto de protocolos de autenticação do canal de segurança \(Schannel\) fornece esses protocolos. Todos os protocolos Schannel usam um modelo de client\/servidor.

## <a name="BKMK_APP"></a>Aplicativos práticos
Um problema quando você administra a uma rede é proteger dados que estão sendo enviados entre aplicativos em uma rede não confiável. Você pode usar TLS\SSL para autenticar servidores e computadores cliente e, em seguida, use o protocolo para criptografar as mensagens entre as partes autenticadas.

Por exemplo, você pode usar TLS\SSL para:

-   Transações protegidas SSL\ com um site de comércio e\

-   Clientes autenticados acessem um site protegido SSL\

-   Acesso remoto

-   Acesso SQL

-   E\ mail

## <a name="BKMK_SOFT"></a>Requisitos de software
O protocolo TLS\SSL usam um modelo de cliente/servidor e são baseados na autenticação de certificado, que requer uma infraestrutura de chave pública.

## <a name="BKMK_INSTALL"></a>Informações do Gerenciador do servidor
Não há nenhuma etapa de configuração necessárias para implementar TLS, SSL ou Schannel.

