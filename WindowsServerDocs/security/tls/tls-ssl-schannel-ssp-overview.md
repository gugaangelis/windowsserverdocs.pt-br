---
title: Visão geral de TLS/SSL (SSP do Schannel)
description: Segurança do Windows Server
ms.custom: na
ms.prod: windows-server
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
ms.openlocfilehash: 51e3886339b7864951b322d210e1a05bef4dc1e1
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71403367"
---
# <a name="tlsssl-overview-schannel-ssp"></a>Visão geral de TLS/SSL (SSP do Schannel)

>Aplicável a: Windows Server (Canal Semestral), Windows Server 2016, Windows 10

Este tópico para o profissional de ti apresenta as implementações de TLS e SSL no Windows usando o SSP (provedor de serviços de segurança do Schannel) descrevendo aplicativos práticos, alterações na implementação da Microsoft e requisitos de software, além de recursos adicionais para o Windows Server 2012 e o Windows 8.

## <a name="BKMK_OVER"></a>Descrição
Schannel é um SSP (Provedor de Suporte de Segurança) que implementa os protocolos de autenticação padrão de Internet SSL e TLS.

A Interface SSPI é uma API usada por sistemas Windows para executar funções relacionadas à segurança, incluindo autenticação. O SSPI funciona como uma interface comum para vários SSPs, incluindo o SSP do Schannel.

As versões 1,0, 1,1 e 1,2 do TLS, as versões 2,0 e 3,0, bem como a segurança da camada de transporte de datagrama \(DTLS\) protocolo versão 1,0, e o transporte de comunicações privada \(protocolo PCT\) são baseados na criptografia de chave pública. O pacote de protocolos de autenticação Schannel fornece esses protocolos. Todos os protocolos Schannel usam um modelo de cliente/servidor.

## <a name="BKMK_APP"></a>Aplicativos
Um problema quando você administra uma rede é proteger os dados que estão sendo enviados entre os aplicativos em uma rede não confiável. Você pode usar TLS e SSL para autenticar servidores e computadores cliente e, em seguida, usar o protocolo para criptografar mensagens entre as partes autenticadas.

Por exemplo, você pode usar o TLS/SSL para:

-   Transações protegidas por SSL com um site de comércio eletrônico
-   Acesso a cliente autenticado para um site protegido por SSL
-   Acesso remoto
-   Acesso ao SQL
-   Email

## <a name="BKMK_SOFT"></a>Requirement
Os protocolos TLS e SSL usam um modelo de cliente/servidor e são baseados na autenticação de certificado, o que requer uma infraestrutura de chave pública.

## <a name="BKMK_INSTALL"></a>Informações de Gerenciador do Servidor
Não há nenhuma etapa de configuração necessária para implementar TLS, SSL ou Schannel.

## <a name="see-also"></a>Consulte também ##

-   [O pacote de segurança Schannel](https://docs.microsoft.com/windows/desktop/com/schannel)
-   [Canal seguro](https://docs.microsoft.com/windows/desktop/SecAuthN/secure-channel)
-   [Protocolo de segurança da camada de transporte](https://docs.microsoft.com/windows/desktop/SecAuthN/transport-layer-security-protocol)
