---
title: Gerenciar estações de usuário
description: Saiba como gerenciar estações do usuário no MultiPoint Services
ms.custom: na
ms.prod: windows-server-threshold
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
ms.openlocfilehash: 8c5351f3e8ec9890ef72905b646c37e9b049745e
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59823717"
---
# <a name="manage-user-stations"></a>Gerenciar estações de usuário
Esta seção aborda o gerenciamento das *estações* que compõem o sistema MultiPoint Services. Gerenciamento de um sistema MultiPoint Services inclui tanto gerenciar componentes de hardware e software do Gerenciador do MultiPoint. Em um sistema MultiPoint Services, uma área de trabalho é a interface de usuário do software apresentada no monitor para cada estação de usuário.  
  
## <a name="station-status"></a>Status da estação  
Você pode exibir os seguintes tipos de status para cada área de trabalho na guia **Estações**. O status inclui:  
  
-   Usuários que estão conectados  
  
-   Sessões do usuário suspensas, mas ainda ativas no computador  
  
-   Quais estações estão sendo usadas e por qual usuário  
  
Para obter mais informações sobre como exibir o status da área de trabalho, consulte o tópico [Exibir status de conexão de usuário](View-User-Connection-Status.md).  

>[!TIP] 
> Você pode atribuir nomes amigáveis para cada estação, o que pode ajudar a identificar as estações mais facilmente. Use **Identify station (Identificar estação)**, que mostra o nome da estação na tela atribuída.
  
## <a name="different-ways-to-log-standard-users-off-of-the-multipoint-services-system"></a>Diferentes maneiras de fazer logoff de usuários padrão do sistema MultiPoint Services  
Como *usuário administrativo*, você pode fazer logoff do Windows a qualquer momento, que não afetará os usuários ativos no seu sistema MultiPoint Services. *Usuários padrão* podem *desconectar* sua sessão ou *fazer logoff* do sistema MultiPoint Services também. Caso a proteção de disco esteja habilitada, se um usuário estiver saindo de folga, certifique-se de salvar seu trabalho no computador ou em um dispositivo de armazenamento externo para que, quando o sistema MultiPoint Services for desligado, o usuário ainda possa recuperar seu trabalho salvo em outro dia.  
  
Como usuário administrativo, talvez você precise encerrar a *sessão* de um usuário padrão em vez de esperar o usuário fazer o logoff. Você pode encerrar a sessão de um usuário padrão em uma das duas maneiras:  
  
-   Encerrar a sessão e fazer logoff de usuário. Para obter mais informações sobre encerrar a sessão de um usuário, consulte o tópico [Encerrar a sessão de um usuário](End-a-User-Session.md).  
  
-   Suspenda o usuário para encerrar temporariamente a sessão de usuário, mas mantenha a sessão ativa na memória do computador do sistema MultiPoint Services. O usuário suspenso pode se reconectar à sessão da mesma estação a mesmo ou de uma estação diferente e continuar seu trabalho. Para obter mais informações sobre a suspensão da sessão de um usuário, consulte o tópico [Suspender e manter a sessão de usuário ativa](Suspend-and-Leave-User-Session-Active.md).  
  
## <a name="set-a-station-to-automatically-log-on"></a>Definir uma estação para fazer logon automaticamente  
Como usuário administrativo, você pode configurar uma ou mais estações para fazer logon automaticamente quando o computador que estiver executando o MultiPoint Services for iniciado. Para obter mais informações sobre como fazer logon automaticamente, consulte o tópico [Configurar uma estação para executar logon automático](Set-up-a-Station-for-Automatic-Logon.md).  
  
## <a name="split-a-station"></a>Dividir uma estação  
Qualquer monitor da estação com uma resolução maior do que 1024x768 pode ser dividido em duas estações. Para obter mais informações sobre divisão de uma estação, consulte o tópico [Dividir uma estação de usuário](Split-a-User-Station.md).  
  
## <a name="see-also"></a>Consulte também  
[Exibir Status de Conexão do usuário](View-User-Connection-Status.md)  
[Fazer logoff ou desconectar as sessões do usuário](Log-off-or-Disconnect-User-Sessions.md)  
[Suspender e manter a sessão de usuário](Suspend-and-Leave-User-Session-Active.md)  
[Configurar uma estação para Logon automático](Set-up-a-Station-for-Automatic-Logon.md)  
[Encerrar uma sessão de usuário](End-a-User-Session.md)  
[Dividir uma estação de usuário](Split-a-User-Station.md)