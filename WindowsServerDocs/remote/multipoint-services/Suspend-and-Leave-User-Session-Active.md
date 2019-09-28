---
title: Suspender e manter a sessão do usuário ativa
description: Saiba como suspender um usuário de uma sessão do MultiPoint sem desconectá-los
ms.custom: na
ms.prod: windows-server
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
ms.openlocfilehash: 0ef9d98584df568438cc3c905a7c86cd58f53343
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71394932"
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