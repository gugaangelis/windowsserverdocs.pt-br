---
title: Visão geral de TLS/SSL (SSP do Schannel)
description: Segurança do Windows Server
ms.prod: windows-server
ms.technology: security-tls-ssl
ms.topic: article
ms.assetid: 1b7b0432-1bef-4912-8c9a-8989d47a4da9
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 05/16/2018
ms.openlocfilehash: 0d963116fc9f22482398b38482f0c3c49f4be505
ms.sourcegitcommit: 771db070a3a924c8265944e21bf9bd85350dd93c
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/27/2020
ms.locfileid: "85475513"
---
# <a name="tlsssl-overview-schannel-ssp"></a>Visão geral de TLS/SSL (SSP do Schannel)

>Aplica-se a: Windows Server (Canal Semestral), Windows Server 2016, Windows 10

Este tópico para o profissional de ti apresenta as implementações de TLS e SSL no Windows usando o SSP (provedor de serviços de segurança do Schannel) descrevendo aplicativos práticos, alterações na implementação da Microsoft e requisitos de software, além de recursos adicionais para o Windows Server 2012 e o Windows 8.

## <a name="description"></a><a name="BKMK_OVER"></a>Descrição
Schannel é um SSP (Provedor de Suporte de Segurança) que implementa os protocolos de autenticação padrão de Internet SSL e TLS.

A Interface SSPI é uma API usada por sistemas Windows para executar funções relacionadas à segurança, incluindo autenticação. O SSPI funciona como uma interface comum para vários SSPs, incluindo o SSP do Schannel.

As versões 1,0, 1,1 e 1,2 do TLS, versões do SSL 2,0 e 3,0, bem como a versão do protocolo de DTLS de segurança da camada de transporte de datatransport, \( \) e o protocolo PCT de transporte de comunicações privada \( \) são baseados na criptografia de chave pública. O pacote de protocolos de autenticação Schannel fornece esses protocolos. Todos os protocolos Schannel usam um modelo de cliente/servidor.

## <a name="applications"></a><a name="BKMK_APP"></a>Aplicativos
Um problema ao administrar uma rede é proteger os dados que estão sendo enviados entre aplicativos em uma rede não confiável. Você pode usar TLS e SSL para autenticar servidores e computadores cliente e, em seguida, usar o protocolo para criptografar mensagens entre as partes autenticadas.

Por exemplo, você pode usar o TLS/SSL para:

-   Transações protegidas por SSL com um site de comércio eletrônico
-   Acesso a cliente autenticado para um site protegido por SSL
-   Acesso remoto
-   Acesso ao SQL
-   Email

## <a name="requirements"></a><a name="BKMK_SOFT"></a>Requisitos
Os protocolos TLS e SSL usam um modelo de cliente/servidor e são baseados na autenticação de certificado, o que requer uma infraestrutura de chave pública.

## <a name="server-manager-information"></a><a name="BKMK_INSTALL"></a>Informações sobre o Gerenciador do Servidor
Não há nenhuma etapa de configuração necessária para implementar TLS, SSL ou Schannel.

## <a name="additional-references"></a>Referências adicionais ##

-   [O pacote de segurança Schannel](https://docs.microsoft.com/windows/desktop/com/schannel)
-   [Canal Seguro](https://docs.microsoft.com/windows/desktop/SecAuthN/secure-channel)
-   [Protocolo de segurança da camada de transporte](https://docs.microsoft.com/windows/desktop/SecAuthN/transport-layer-security-protocol)
