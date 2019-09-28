---
title: Gerenciar estações de usuário
description: Saiba como gerenciar estações de usuário nos serviços do MultiPoint
ms.custom: na
ms.prod: windows-server
ms.technology: multipoint-services
ms.reviewer: na
ms.suite: na
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: b418578d-3a4c-49b0-90db-8389b320b2f6
author: lizap
manager: dongill
ms.author: elizapo
ms.date: 08/04/2016
ms.openlocfilehash: 7f46d2a68fc6247bddc1251c32ac55544b6fbf52
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71405062"
---
# <a name="manage-user-stations"></a>Gerenciar estações de usuário
Esta seção aborda o gerenciamento das *estações* que compõem o sistema MultiPoint Services. O gerenciamento de um sistema de serviços do MultiPoint inclui o gerenciamento de componentes de hardware e software do MultiPoint Manager. Em um sistema de serviços do MultiPoint, uma área de trabalho é a interface do usuário do software apresentada no monitor para cada estação de usuário.  
  
## <a name="station-status"></a>Status da estação  
Você pode exibir os seguintes tipos de status para cada área de trabalho na guia **Estações**. O status inclui:  
  
-   Usuários que estão conectados  
  
-   Sessões do usuário suspensas, mas ainda ativas no computador  
  
-   Quais estações estão sendo usadas e por qual usuário  
  
Para obter mais informações sobre como exibir o status da área de trabalho, consulte o tópico [Exibir status de conexão de usuário](View-User-Connection-Status.md).  

>[!TIP] 
> Você pode atribuir nomes amigáveis para cada estação, o que pode ajudar a identificar as estações mais facilmente. Use **Identify station (Identificar estação)** , que mostra o nome da estação na tela atribuída.
  
## <a name="different-ways-to-log-standard-users-off-of-the-multipoint-services-system"></a>Diferentes maneiras de fazer logoff de usuários padrão do sistema MultiPoint Services  
Como *usuário administrativo*, você pode fazer logoff do Windows a qualquer momento, que não afetará os usuários ativos no seu sistema MultiPoint Services. *Usuários padrão* podem *desconectar* sua sessão ou *fazer logoff* do sistema MultiPoint Services também. Caso a proteção de disco esteja habilitada, se um usuário estiver saindo de folga, certifique-se de salvar seu trabalho no computador ou em um dispositivo de armazenamento externo para que, quando o sistema MultiPoint Services for desligado, o usuário ainda possa recuperar seu trabalho salvo em outro dia.  
  
Como um usuário administrativo, talvez seja necessário encerrar a *sessão*de um usuário padrão, em vez de fazer o logoff do usuário. Você pode encerrar a sessão de um usuário padrão de uma das duas maneiras:  
  
-   Encerrar a sessão e fazer logoff de usuário. Para obter mais informações sobre como encerrar a sessão de um usuário, consulte o tópico [encerrar uma sessão de usuário](End-a-User-Session.md) .  
  
-   Suspender o usuário para encerrar temporariamente a sessão do usuário, mas manter a sessão ativa na memória do computador do sistema de serviços do MultiPoint. O usuário suspenso pode se reconectar à sessão da mesma estação a mesmo ou de uma estação diferente e continuar seu trabalho. Para obter mais informações sobre como suspender a sessão de um usuário, consulte o tópico [suspender e deixar a sessão de usuário ativa](Suspend-and-Leave-User-Session-Active.md) .  
  
## <a name="set-a-station-to-automatically-log-on"></a>Definir uma estação para fazer logon automaticamente  
Como usuário administrativo, você pode configurar uma ou mais estações para fazer logon automaticamente quando o computador que estiver executando o MultiPoint Services for iniciado. Para obter mais informações sobre como fazer logon automaticamente, consulte o tópico [Configurar uma estação para executar logon automático](Set-up-a-Station-for-Automatic-Logon.md).  
  
## <a name="split-a-station"></a>Dividir uma estação  
Qualquer monitor da estação com uma resolução maior do que 1024x768 pode ser dividido em duas estações. Para obter mais informações sobre divisão de uma estação, consulte o tópico [Dividir uma estação de usuário](Split-a-User-Station.md).  
  
## <a name="see-also"></a>Consulte também  
[Exibir o status de conexão do usuário](View-User-Connection-Status.md)  
[Fazer logoff ou desconectar as sessões do usuário](Log-off-or-Disconnect-User-Sessions.md)  
[Suspender e deixar a sessão de usuário ativa](Suspend-and-Leave-User-Session-Active.md)  
[Configurar uma estação para logon automático](Set-up-a-Station-for-Automatic-Logon.md)  
[Encerrar uma sessão do usuário](End-a-User-Session.md)  
[Dividir uma estação do usuário](Split-a-User-Station.md)