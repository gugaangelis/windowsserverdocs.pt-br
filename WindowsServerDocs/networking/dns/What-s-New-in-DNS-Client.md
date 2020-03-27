---
title: O que há de novo no cliente DNS no Windows Server
description: Este tópico fornece uma visão geral dos novos recursos no cliente DNS no Windows Server e no Windows 10
manager: brianlic
ms.prod: windows-server
ms.technology: networking-dns
ms.topic: article
ms.assetid: 6edaba84-4595-4fd8-95d7-64d4d975a38a
ms.author: lizross
author: eross-msft
ms.openlocfilehash: a02aea149a658be2696195a2a17429b506dec1fa
ms.sourcegitcommit: da7b9bce1eba369bcd156639276f6899714e279f
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/26/2020
ms.locfileid: "80317980"
---
# <a name="whats-new-in-dns-client-in-windows-server-2016"></a>Novidades no cliente DNS no Windows Server 2016

>Aplicável a: Windows Server (canal semestral), Windows Server 2016

Este tópico descreve a funcionalidade de cliente DNS (sistema de nomes de domínio) que é nova ou alterada no Windows 10 e no Windows Server 2016 e versões posteriores desses sistemas operacionais.
  
## <a name="updates-to-dns-client"></a>Atualizações para o cliente DNS

**Associação de serviço do cliente DNS**: no Windows 10, o serviço cliente DNS oferece suporte aprimorado para computadores com mais de uma interface de rede. Para computadores com hospedagem múltipla, a resolução DNS é otimizada das seguintes maneiras:  
  
-   Quando um servidor DNS configurado em uma interface específica é usado para resolver uma consulta DNS, o serviço cliente DNS se associará a essa interface antes de enviar a consulta DNS.  
  
    Ligando-se a uma interface específica, o cliente DNS pode especificar claramente a interface na qual ocorre a resolução de nomes, permitindo que os aplicativos otimizem as comunicações com o cliente DNS nesse adaptador de rede.  
  
-   Se o servidor DNS usado for designado por uma configuração de Política de Grupo do Tabela de Políticas de Resolução de Nomes (NRPT), o serviço cliente DNS não se associará a uma interface específica.  
  
> [!NOTE]  
> As alterações no serviço cliente DNS no Windows 10 também estão presentes em computadores que executam o Windows Server 2016 e versões posteriores.  
  
## <a name="see-also"></a>Consulte também  
  
-   [O que há de novo no servidor DNS no Windows Server 2016](What-s-New-in-DNS-Server.md)  
  

