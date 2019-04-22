---
title: 'Guia de laboratório de teste: demonstrar implantação multissite do DirectAccess'
description: 'Este tópico é parte do guia de laboratório de teste: demonstrar uma implantação de multissite de DirectAccess para Windows Server 2016'
manager: brianlic
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology:
- networking-da
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 3c98106c-67cc-406a-810e-f2e09f7e2c5e
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 47b6848789a7e61bdb3cc12e6339777ed1a4f1b3
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59811977"
---
# <a name="test-lab-guide-demonstrate-a-directaccess-multisite-deployment"></a>Guia do laboratório de teste: Demonstrar uma implantação multissite do DirectAccess

>Aplica-se a: Windows Server (canal semestral), Windows Server 2016

Acesso remoto é uma função de servidor nos sistemas operacionais Windows Server 2016, Windows Server 2012 R2 e Windows Server 2012 que permite que usuários remotos acessem com segurança os recursos da rede interna usando DirectAccess ou RRAS VPN. Este guia contém instruções passo a passo para estender o [guia do laboratório de teste: Demonstrar a instalação de servidor único do DirectAccess com misto de IPv4 e IPv6](https://go.microsoft.com/fwlink/p/?LinkId=237004) para demonstrar o acesso remoto em um cenário multissite.  
  
Implantar acesso remoto em um cenário multissite permite que você configure os servidores de acesso remoto em locais dispersos geograficamente. Anteriormente, eram necessário que os usuários remotos sempre se conectam à rede corporativa por meio de um servidor DirectAccess específico. Com o Windows Server 2016, Windows Server 2012 R2 ou Windows Server 2012 e Windows 10 ou Windows 8, você pode configurar pontos de entrada para cada localização geográfica em sua implantação. Cada ponto de entrada pode ser um único servidor de acesso remoto ou um cluster de servidores de acesso remoto. Os usuários remotos têm a opção para se conectar a qualquer um dos pontos de entrada de acesso remoto da organização. Por exemplo, se um usuário remoto normalmente se conecta ao ponto de entrada de acesso remoto localizado na Ásia, mas, em seguida, entra em uma viagem de negócios para a Europa, o computador cliente se conecta automaticamente ao ponto de entrada de acesso remoto mais próximo.  
  
## <a name="about-this-guide"></a>Sobre este guia  
Este guia contém instruções para configurar e demonstrar o acesso remoto usando três computadores cliente e nove servidores. O laboratório de teste multissite de acesso remoto concluído simula uma intranet, Internet e uma rede doméstica e demonstra a funcionalidade de acesso remoto em diferentes cenários de conexão de Internet.  
  
> [!IMPORTANT]  
> Este laboratório é uma verificação de conceito usando o número mínimo de computadores. A configuração detalhada neste guia é apenas para fins de teste de laboratório, e não deve ser usada em um ambiente de produção.  
  


