---
title: 'Guia de laboratório de teste: demonstrar o DirectAccess com autenticação OTP e SecurID RSA'
description: Este tópico faz parte do guia de laboratório de teste - demonstrar o DirectAccess com autenticação OTP e SecurID de RSA para o Windows Server 2016
manager: brianlic
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology:
- networking-da
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 10c7a49c-5671-4bec-b562-13fdd67f4629
ms.author: pashort
author: shortpatti
ms.openlocfilehash: a6e4b03ceb331899ac622bffe44061a7d92f5a09
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59866227"
---
# <a name="test-lab-guide-demonstrate-directaccess-with-otp-authentication-and-rsa-securid"></a>Guia do laboratório de teste: Demonstrar o DirectAccess com autenticação OTP e SecurID RSA

>Aplica-se a: Windows Server (canal semestral), Windows Server 2016

Acesso remoto é uma função de servidor em que o sistema operacional Windows Server 2016, Windows Server 2012 R2 e Windows Server 2012 que permite que usuários remotos acessem com segurança os recursos da rede interna usando DirectAccess ou redes virtuais privadas (VPNs) com o serviço de roteamento e o serviço de acesso remoto (RRAS). Este guia contém instruções passo a passo para estender o [guia do laboratório de teste: Demonstrar a instalação de servidor único do DirectAccess com misto de IPv4 e IPv6](https://go.microsoft.com/fwlink/p/?LinkId=237004) para demonstrar uma configuração de senha única (OTP) de acesso remoto.  
  
> [!WARNING]  
> O design deste guia de laboratório de teste inclui servidores de infraestrutura, como um controlador de domínio e uma autoridade de certificação (CA) que executam o Windows Server 2012 R2 ou Windows Server 2012. Usando este guia de laboratório de teste para configurar os servidores de infraestrutura que estão executando outros sistemas operacionais não foi testado e instruções para configurar outros sistemas operacionais não estão incluídas neste guia.  
  
## <a name="about-this-guide"></a>Sobre este guia  
Acesso remoto no Windows Server 2016, Windows Server 2012 R2 e Windows Server 2012 adiciona suporte para autenticação de cliente com a OTP. Para os fins deste laboratório de teste somente RSA SecurID é usado para demonstrar a funcionalidade OTP com o acesso remoto. Outros RADIUS com base em OTP soluções também têm suporte, mas estão fora do escopo deste laboratório de teste. Este guia contém instruções para a configuração e demonstração do Acesso Remoto usando seis servidores e dois computadores clientes. O acesso remoto concluído com um laboratório de teste OTP simula uma intranet, Internet e uma rede doméstica e demonstra a funcionalidade de acesso remoto em diferentes cenários de conexão de Internet.  
  
> [!IMPORTANT]  
> Este laboratório é uma verificação de conceito usando o número mínimo de computadores. A configuração detalhada neste guia é apenas para fins de teste de laboratório, e não deve ser usada em um ambiente de produção.  
  


