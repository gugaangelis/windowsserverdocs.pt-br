---
title: Suspender e manter a sessão do usuário ativa
description: Saiba como suspender um usuário de uma sessão do MultiPoint sem desconectá-los
ms.custom: na
ms.prod: windows-server-threshold
ms.technology: multipoint-services
ms.reviewer: na
ms.suite: na
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 5263bce3-fe92-4398-8393-2e3a4e05d530
author: lizap
manager: dongill
ms.author: elizapo
ms.date: 08/04/2016
ms.openlocfilehash: cc4310e6f7609464cf037b750bec6e5e805e0b26
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59815217"
---
# <a name="suspend-and-leave-user-session-active"></a>Suspender e manter a sessão do usuário ativa
Você pode desconectar ou suspender os usuários do sistema MultiPoint Services quando você não deseja encerrar as sessões dos usuários. Um usuário também pode desconectar a sessão em vez de você desconectá-la para eles. Mesmo que a sessão de um usuário seja suspensa, a sessão permanece ativa na memória do computador do sistema MultiPoint Services até que o computador seja desligado ou reiniciado. Nesse momento, todas as sessões suspensas são encerradas e qualquer trabalho não salvo é perdido.  
  
1.  Abra o MultiPoint Manager no modo de estação e, em seguida, clique no **estações** guia.  
  
2.  Na coluna **Computador**, clique no nome do computador cujas sessões você deseja suspender.  
  
3.  Em **Tarefas das estações**, clique em **Suspender todas as estações**.  
  
Depois que uma sessão do usuário tiver sido suspensa, o usuário poderá fazer logon na mesma estação ou em outra e continuar trabalhando na sessão original.  
  
## <a name="see-also"></a>Consulte também  
[Gerenciar áreas de trabalho do usuário](manage-user-desktops-using-multipoint-dashboard.md)  
[Fazer logoff ou desconectar as sessões do usuário](Log-off-or-Disconnect-User-Sessions.md)