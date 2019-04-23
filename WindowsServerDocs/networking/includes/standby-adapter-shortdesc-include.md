---
author: shortpatti
ms.author: pashort
ms.date: 10/02/2018
ms.prod: windows-server-threshold
ms:topic: include
ms.openlocfilehash: d1bfc74c4daa751e3081b26c87dd0d2c88f5f095
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59879847"
---
As opções para o adaptador em espera serão **nenhum (todos os adaptadores do Active Directory)** ou a seleção de um adaptador de rede específico na equipe NIC que atua como um adaptador de modo de espera. Quando você configura uma NIC como um adaptador de modo de espera, todos os outros membros da equipe não selecionadas são ativo, e nenhum tráfego de rede é enviado ou processado pelo adaptador, até que uma NIC ativa falhar. Depois que uma NIC ativa falhar, a NIC em espera se torna ativa e o tráfego de rede de processos. Quando todos os membros da equipe obterem restaurados para o serviço, o membro da equipe em espera retorna para o status em espera.  