---
title: O que há de novo no cliente DNS no Windows Server
description: Este tópico fornece uma visão geral dos novos recursos no cliente DNS no Windows Server e Windows 10
manager: brianlic
ms.prod: windows-server-threshold
ms.technology: networking-dns
ms.topic: article
ms.assetid: 6edaba84-4595-4fd8-95d7-64d4d975a38a
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 34b1a64465e217fbd7e6b3ae55e89832a7a4e48c
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59860557"
---
# <a name="whats-new-in-dns-client-in-windows-server-2016"></a>Novidades no cliente DNS no Windows Server 2016

>Aplica-se a: Windows Server (canal semestral), Windows Server 2016

Este tópico descreve a funcionalidade de cliente do sistema de nome de domínio (DNS) que é nova ou alterada no Windows 10 e Windows Server 2016 e versões posteriores desses sistemas operacionais.
  
## <a name="updates-to-dns-client"></a>Atualizações para o cliente DNS

**Associação de serviço cliente DNS**: No Windows 10, o serviço cliente DNS oferece suporte avançado para computadores com mais de uma interface de rede. Para computadores com hospedagem múltipla, resolução de DNS é otimizada das seguintes maneiras:  
  
-   Quando um servidor DNS que está configurado em uma interface específica é usado para resolver uma consulta DNS, o serviço cliente DNS será associado a essa interface antes de enviar a consulta DNS.  
  
    Associando a uma interface específica, o cliente do DNS pode claramente especificar a interface onde ocorre a resolução de nomes, habilitação de aplicativos para otimizar as comunicações com o cliente DNS sobre este adaptador de rede.  
  
-   Se o servidor DNS que é usado é designado por uma configuração de diretiva de grupo do nome de resolução de política NRPT (tabela), o serviço cliente DNS não associar a uma interface específica.  
  
> [!NOTE]  
> As alterações para o serviço cliente DNS no Windows 10 também estão presentes em computadores que executam o Windows Server 2016 e versões posteriores.  
  
## <a name="see-also"></a>Consulte também  
  
-   [O que há de novo no servidor DNS no Windows Server 2016](What-s-New-in-DNS-Server.md)  
  

