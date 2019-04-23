---
title: Fazer logoff ou desconectar sessões do usuário
description: Saiba como fazer logoff de um usuário manualmente
ms.custom: na
ms.prod: windows-server-threshold
ms.technology: multipoint-services
ms.reviewer: na
ms.suite: na
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 3e9bbcdc-e33b-481e-8b46-787a4f6d58bc
author: lizap
manager: dongill
ms.author: elizapo
ms.date: 08/04/2016
ms.openlocfilehash: 518e9dc9ba9603d988a7e21e08caa29db9f04bde
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59854937"
---
# <a name="log-off-or-disconnect-user-sessions"></a>Fazer logoff ou desconectar sessões do usuário
Usuários do multiPoint Services podem fazer logon e logoff de suas sessões da área de trabalho como fariam com qualquer sessão do Windows. Os usuários também podem desconectar ou suspender a sessão para que a estação do MultiPoint Services não está sendo usada, mas a sessão permanece ativa na memória do computador do sistema MultiPoint Services.  
  
Além disso, os usuários administrativos podem encerrar uma sessão do usuário se o usuário tiver se ausentado da sua sessão do MultiPoint Services ou esquecido de fazer logoff do sistema.  
  
## <a name="logging-off-or-disconnecting-a-session"></a>Logoff ou desconexão de uma sessão  
A tabela a seguir descreve as diferentes opções que você ou qualquer usuário pode usar para fazer logoff, suspender ou encerrar uma sessão.  
  
|||  
|-|-|  
|**Ação**|**Effect**|  
|Clique em **inicie**, clique em configurações, clique no nome de usuário (canto superior direito) e, em seguida, clique em **sair**.|A sessão é encerrada e a estação fica disponível para logon por qualquer usuário.|  
|Clique em **Iniciar**, clique em **Configurações**, clique em Energia e, em seguida, clique em **Desconectar**.|A sessão é desconectada e fica preservada na memória do computador. A estação fica disponível para o logon pelo mesmo usuário ou por um usuário diferente.|  
|Clique em **inicie**, clique em configurações, clique no nome de usuário (canto superior direito) e, em seguida, clique em **bloqueio**|A estação é bloqueada e fica preservada na memória do computador.|  
  
## <a name="suspending-or-ending-a-users-session"></a>Suspensão ou encerramento da sessão de um usuário  
A tabela a seguir descreve as diferentes opções que você, como usuário administrativo, pode usar para desconectar ou encerrar uma sessão de usuário.  
  
|||  
|-|-|  
|**Ação**|**Effect**|  
|**Suspenda:** No Gerenciador do MultiPoint, use o **estações** tab para suspender a sessão do usuário. Para obter mais informações, consulte o tópico [Suspender e manter a sessão do usuário ativa](Suspend-and-Leave-User-Session-Active.md).|A sessão do usuário é encerrada e fica preservada na memória do computador. A estação fica disponível para o logon pelo mesmo usuário ou por um usuário diferente. O usuário pode fazer logon na mesma estação ou em outra e continuar trabalhando.|  
|**Fim:** No Gerenciador do MultiPoint, use o **estações** guia para encerrar a sessão do usuário. Você também pode encerrar todas as sessões do usuário na guia **Estações**. Para obter mais informações, consulte o tópico [Encerrar uma sessão de usuário](End-a-User-Session.md).|A sessão do usuário é encerrada e a estação fica disponível para logon por qualquer usuário. A sessão do usuário não é mais exibida na guia **Estações** e não fica na memória do computador.|  
  
## <a name="see-also"></a>Consulte também  
[Suspender e manter a sessão de usuário](Suspend-and-Leave-User-Session-Active.md)  
[Encerrar uma sessão de usuário](End-a-User-Session.md)  
[Gerenciar áreas de trabalho do usuário](manage-user-desktops-using-multipoint-dashboard.md)  
[Fazer logoff das sessões de usuário](Log-Off-User-Sessions.md)    