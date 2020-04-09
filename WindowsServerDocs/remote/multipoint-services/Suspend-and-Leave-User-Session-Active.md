---
title: Suspender e manter a sessão do usuário ativa
description: Saiba como suspender um usuário de uma sessão do MultiPoint sem desconectá-los
ms.prod: windows-server
ms.technology: multipoint-services
ms.topic: article
ms.assetid: 5263bce3-fe92-4398-8393-2e3a4e05d530
author: lizap
manager: dongill
ms.author: elizapo
ms.date: 08/04/2016
ms.openlocfilehash: 2d2771fd60f4d8c11c602a4c5d55f3b5ae2d8b11
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80861619"
---
# <a name="suspend-and-leave-user-session-active"></a>Suspender e manter a sessão do usuário ativa
Você pode desconectar ou suspender usuários do sistema de serviços do MultiPoint quando não quiser encerrar as sessões dos usuários. Um usuário também pode desconectar a sessão em vez de você desconectá-la para eles. Enquanto uma sessão de usuário é suspensa, a sessão permanece ativa na memória do computador do sistema de serviços do MultiPoint até que o computador seja desligado ou reiniciado. Nesse momento, todas as sessões suspensas são encerradas e qualquer trabalho não salvo é perdido.  
  
1.  Abra o Gerenciador do MultiPoint no modo de estação e clique na guia **estações** .  
  
2.  Na coluna **Computador**, clique no nome do computador cujas sessões você deseja suspender.  
  
3.  Em **Tarefas das estações**, clique em **Suspender todas as estações**.  
  
Depois que uma sessão do usuário tiver sido suspensa, o usuário poderá fazer logon na mesma estação ou em outra e continuar trabalhando na sessão original.  
  
## <a name="see-also"></a>Consulte também  
[Gerenciar áreas de trabalho de usuários](manage-user-desktops-using-multipoint-dashboard.md)  
[Fazer logoff ou desconectar as sessões do usuário](Log-off-or-Disconnect-User-Sessions.md)