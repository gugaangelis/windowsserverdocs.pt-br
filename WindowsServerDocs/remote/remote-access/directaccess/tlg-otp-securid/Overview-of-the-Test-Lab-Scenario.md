---
title: Visão geral da autenticação de OTP do cenário de laboratório de teste e RSA SecurID
description: Este tópico faz parte do guia de laboratório de teste – demonstre o DirectAccess com autenticação OTP e RSA SecurID para Windows Server 2016
manager: brianlic
ms.topic: article
ms.assetid: ce584811-b209-48fe-ab2b-4c399bd0bd79
ms.author: lizross
author: eross-msft
ms.openlocfilehash: a295d3eb549512b301a29b81e3e1c0e1167763b3
ms.sourcegitcommit: dfa48f77b751dbc34409aced628eb2f17c912f08
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/07/2020
ms.locfileid: "87955103"
---
# <a name="overview-of-the-test-lab-scenario-otp-authentication-and-rsa-securid"></a>Visão geral da autenticação de OTP do cenário de laboratório de teste e RSA SecurID

>Aplica-se a: Windows Server (Canal Semestral), Windows Server 2016

O acesso remoto é uma função de servidor nos sistemas operacionais Windows Server 2016, Windows Server 2012 R2 e Windows Server 2012 que permite que os usuários remotos acessem com segurança recursos de rede internos usando o DirectAccess ou redes virtuais privadas (VPNs) com o RRAS (serviço de roteamento e acesso remoto). Este guia contém instruções passo a passo para estender o guia de [laboratório de teste: demonstre a configuração do servidor único do DirectAccess com IPv4 e IPv6 mistos](https://go.microsoft.com/fwlink/p/?LinkId=237004) para demonstrar uma configuração de OTP (senha de uso único) de acesso remoto.

> [!WARNING]
> O design deste guia de laboratório de teste inclui servidores de infraestrutura, como um controlador de domínio e uma CA (autoridade de certificação) que executam o Windows Server 2016, o Windows Server 2012 R2 ou o Windows Server 2012. O uso deste guia de laboratório de teste para configurar servidores de infraestrutura que executam outros sistemas operacionais não foi testado e as instruções para configurar outros sistemas operacionais não estão incluídas neste guia.

## <a name="about-this-guide"></a>Sobre este guia
O acesso remoto no Windows Server 2016, Windows Server 2012 R2 e Windows Server 2012 adiciona suporte para autenticação de cliente com OTP. Para os fins deste laboratório de teste, somente RSA SecurID é usado para demonstrar a funcionalidade de OTP com acesso remoto. Outras soluções de OTP baseadas em RADIUS também têm suporte, mas estão fora do escopo deste laboratório de teste. Este guia contém instruções para a configuração e demonstração do Acesso Remoto usando seis servidores e dois computadores clientes. O laboratório de teste de acesso remoto concluído com OTP simula uma intranet, a Internet e uma rede doméstica e demonstra a funcionalidade de acesso remoto em diferentes cenários de conexão com a Internet.

> [!IMPORTANT]
> Este laboratório é uma verificação de conceito usando o número mínimo de computadores. A configuração detalhada neste guia é apenas para fins de teste de laboratório, e não deve ser usada em um ambiente de produção.



