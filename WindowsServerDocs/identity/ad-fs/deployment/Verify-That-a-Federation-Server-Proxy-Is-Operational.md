---
ms.assetid: d555041a-709e-42c7-920a-8dfd2c7e0488
title: Verificar se um proxy do servidor de federação está funcionando
description: ''
author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.author: billmath
ms.openlocfilehash: 4900d8621b94a514a07bba55b2f7f3df5dd36353
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59814617"
---
# <a name="verify-that-a-federation-server-proxy-is-operational"></a>Verificar se um proxy do servidor de federação está funcionando

>Aplica-se a: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Você pode usar o procedimento a seguir para verificar se o proxy do servidor de Federação pode se comunicar com o serviço de federação nos serviços de Federação do Active Directory \(do AD FS\). Execute este procedimento depois de executar o **o Assistente de configuração do AD FS Federation Server Proxy** para configurar o computador para executar na função de proxy do servidor de Federação. Para obter mais informações sobre como executar este assistente, consulte [configurar um computador para a função de Proxy do servidor de Federação](Configure-a-Computer-for-the-Federation-Server-Proxy-Role.md).  
  
> [!IMPORTANT]  
> O resultado desse teste é a geração bem-sucedida de um evento específico no Visualizador de Eventos no computador proxy de servidor de federação.  
  
A associação a **Administradores**, ou equivalente, no computador local é o requisito mínimo para concluir esse procedimento.  Examine os detalhes sobre como usar as contas apropriadas e associações de grupos em [domínio grupos padrão Local e](https://go.microsoft.com/fwlink/?LinkId=83477).   
  
### <a name="to-verify-that-a-federation-server-proxy-is-operational"></a>Para verificar se um proxy do servidor de Federação está funcionando  
  
1.  Faça logon no proxy do servidor de federação como administrador.  
  
2.  Sobre o **inicie** tela, digite**Visualizador de eventos**, e, em seguida, pressione ENTER.  
  
3.  No painel de detalhes, clique duas vezes\-clique em **Applications and Services Logs**, double\-clique em **AD FS Eventing**e, em seguida, clique em **Admin**.  
  
4.  Na coluna **ID do Evento**, procure o ID de evento 198.  
  
    Se o proxy do servidor de Federação está configurado corretamente, você verá um novo evento no log de aplicativo do Visualizador de eventos, com a ID de evento 198. Esse evento verifica se o serviço de proxy do servidor de Federação foi iniciado com êxito e agora está online.  
  
## <a name="additional-references"></a>Referências adicionais  
[Lista de verificação: Como configurar um Proxy do servidor de Federação](Checklist--Setting-Up-a-Federation-Server-Proxy.md)  
  

