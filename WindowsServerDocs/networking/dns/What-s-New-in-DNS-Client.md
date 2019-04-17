---
title: O que há de novo no cliente DNS no Windows Server
description: Este tópico fornece uma visão geral dos novos recursos do cliente DNS no Windows Server e Windows 10
manager: brianlic
ms.prod: windows-server-threshold
ms.technology: networking-dns
ms.topic: article
ms.assetid: 6edaba84-4595-4fd8-95d7-64d4d975a38a
ms.author: pashort
author: shortpatti
ms.openlocfilehash: defe88fe2a1e4f5393be4d5d5938d9f5bfbfb5d6
ms.sourcegitcommit: 19d9da87d87c9eefbca7a3443d2b1df486b0b010
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/28/2018
---
# <a name="whats-new-in-dns-client-in-windows-server-2016"></a>O que há de novo no cliente DNS no Windows Server 2016

>Aplica-se a: Windows Server (anual por canal), Windows Server 2016

Este tópico descreve a funcionalidade de cliente do sistema de nome de domínio (DNS) que é novo ou alterado no Windows 10 e Windows Server 2016 e versões posteriores desses sistemas operacionais.
  
## <a name="updates-to-dns-client"></a>Atualizações para o cliente DNS

**Associação de serviço do cliente DNS**: no Windows 10, o serviço de cliente DNS oferece suporte aprimorado para computadores com mais de uma interface de rede. Para computadores de hospedagem múltipla, resolução DNS é otimizada das seguintes maneiras:  
  
-   Quando um servidor DNS que está configurado em uma interface específica é usado para resolver uma consulta DNS, o serviço cliente DNS se vinculará a essa interface antes de enviar a consulta DNS.  
  
    Associando a uma interface específica, o pode do cliente DNS claramente especificar a interface onde ocorre a resolução de nomes, permitindo aplicativos para otimizar comunicações com o cliente DNS sobre isso interface de rede.  
  
-   Se o servidor DNS que é usado é indicado por uma configuração de política de grupo do nome da política de tabela NRPT (resolução), o serviço de cliente DNS não associar a uma interface específica.  
  
> [!NOTE]  
> Alterações no serviço de cliente DNS no Windows 10 também estão presentes nos computadores que executam o Windows Server 2016 e versões posteriores.  
  
## <a name="see-also"></a>Consulte também  
  
-   [O que há de novo no servidor DNS no Windows Server 2016](What-s-New-in-DNS-Server.md)  
  

