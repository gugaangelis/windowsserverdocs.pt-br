---
title: Guia de laboratório de teste – demonstrar o DirectAccess com autenticação de OTP e RSA SecurID
description: Este tópico faz parte do guia de laboratório de teste – demonstre o DirectAccess com autenticação OTP e RSA SecurID para Windows Server 2016
manager: brianlic
ms.prod: windows-server
ms.technology: networking-da
ms.topic: article
ms.assetid: 10c7a49c-5671-4bec-b562-13fdd67f4629
ms.author: lizross
author: eross-msft
ms.openlocfilehash: ee307804b5a03db5638775cb41d6e0a29f8a4a5d
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80814469"
---
# <a name="test-lab-guide-demonstrate-directaccess-with-otp-authentication-and-rsa-securid"></a>Guia de Teste de Laboratório: Demonstrar o DirectAccess com autenticação OTP e SecurID RSA

>Aplicável ao: Windows Server (canal semestral), Windows Server 2016

O acesso remoto é uma função de servidor no sistema operacional Windows Server 2016, Windows Server 2012 R2 e Windows Server 2012 que permite que os usuários remotos acessem com segurança recursos de rede internos usando o DirectAccess ou redes virtuais privadas (VPNs) com o roteamento e o serviço de acesso remoto (RRAS). Este guia contém instruções passo a passo para estender o guia de [laboratório de teste: demonstre a configuração do servidor único do DirectAccess com IPv4 e IPv6 mistos](https://go.microsoft.com/fwlink/p/?LinkId=237004) para demonstrar uma configuração de OTP (senha de uso único) de acesso remoto.  
  
> [!WARNING]  
> O design deste guia de laboratório de teste inclui servidores de infraestrutura, como um controlador de domínio e uma CA (autoridade de certificação) que executam o Windows Server 2012 R2 ou o Windows Server 2012. O uso deste guia de laboratório de teste para configurar servidores de infraestrutura que executam outros sistemas operacionais não foi testado e as instruções para configurar outros sistemas operacionais não estão incluídas neste guia.  
  
## <a name="about-this-guide"></a>Sobre este guia  
O acesso remoto no Windows Server 2016, Windows Server 2012 R2 e Windows Server 2012 adiciona suporte para autenticação de cliente com OTP. Para os fins deste laboratório de teste, somente RSA SecurID é usado para demonstrar a funcionalidade de OTP com acesso remoto. Outras soluções de OTP baseadas em RADIUS também têm suporte, mas estão fora do escopo deste laboratório de teste. Este guia contém instruções para a configuração e demonstração do Acesso Remoto usando seis servidores e dois computadores clientes. O laboratório de teste de acesso remoto concluído com OTP simula uma intranet, a Internet e uma rede doméstica e demonstra a funcionalidade de acesso remoto em diferentes cenários de conexão com a Internet.  
  
> [!IMPORTANT]  
> Este laboratório é uma verificação de conceito usando o número mínimo de computadores. A configuração detalhada neste guia é apenas para fins de teste de laboratório, e não deve ser usada em um ambiente de produção.  
  


