---
title: Visão geral do TLS/SSL (Schannel SSP)
description: Segurança do Windows Server
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
ms.date: 05/16/2018
ms.openlocfilehash: a6571e5e06e07fd62ad4cf39bab322b45c90a9f9
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59848597"
---
# <a name="tlsssl-overview-schannel-ssp"></a>Visão geral do TLS/SSL (Schannel SSP)

>Aplica-se a: Windows Server (canal semestral), Windows Server 2016, Windows 10

Este tópico para profissionais de TI apresenta as implementações de TLS e SSL no Windows usando o provedor de serviço de segurança (SSP) Schannel descrevendo as aplicações práticas, as alterações na implementação da Microsoft e requisitos de software, além de recursos adicionais para o Windows Server 2012 e Windows 8.

## <a name="BKMK_OVER"></a>Descrição
Schannel é um SSP (Provedor de Suporte de Segurança) que implementa os protocolos de autenticação padrão de Internet SSL e TLS.

A Interface SSPI é uma API usada por sistemas Windows para executar funções relacionadas à segurança, incluindo autenticação. A SSPI funciona como uma interface comum para vários SSPs, incluindo o SSP. Schannel

TLS versões 1.0, 1.1 e 1.2, o SSL 2.0 e 3.0, bem como a segurança de camada de transporte do datagrama \(DTLS\) protocol versão 1.0 e o transporte de comunicações privados \(PCT\) protocolo se baseiam no criptografia de chave pública. O pacote de protocolos de autenticação Schannel fornece esses protocolos. Todos os protocolos Schannel usam um modelo de cliente/servidor.

## <a name="BKMK_APP"></a>Aplicativos
Um problema quando você administra uma rede é proteger os dados que estão sendo enviados entre os aplicativos em uma rede não confiável. Você pode usar o TLS e SSL para autenticar servidores e computadores cliente e, em seguida, usar o protocolo para criptografar mensagens entre as partes autenticadas.

Por exemplo, você pode usar o TLS/SSL para:

-   Transações protegidas por SSL com um site de comércio eletrônico
-   Acesso a cliente autenticado para um site protegido por SSL
-   Acesso remoto
-   Acesso ao SQL
-   Email

## <a name="BKMK_SOFT"></a>Requisitos
Protocolos TLS e SSL usam um modelo cliente/servidor e são baseados na autenticação de certificado, que exige uma infraestrutura de chave pública.

## <a name="BKMK_INSTALL"></a>Informações do Gerenciador do servidor
Não há nenhuma etapa de configuração necessárias para implementar o TLS, SSL ou o Schannel.

## <a name="see-also"></a>Consulte também ##

-   [O pacote de segurança Schannel](https://docs.microsoft.com/windows/desktop/com/schannel)
-   [Canal seguro](https://docs.microsoft.com/windows/desktop/SecAuthN/secure-channel)
-   [Protocolo de segurança de camada de transporte](https://docs.microsoft.com/windows/desktop/SecAuthN/transport-layer-security-protocol)
