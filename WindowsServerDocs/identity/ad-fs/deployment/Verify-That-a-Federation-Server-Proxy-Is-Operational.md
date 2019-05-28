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
ms.openlocfilehash: 26b0ae4f331607d83c6b94a2655ddc9eded8a356
ms.sourcegitcommit: 0b5fd4dc4148b92480db04e4dc22e139dcff8582
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 05/24/2019
ms.locfileid: "66191870"
---
# <a name="verify-that-a-federation-server-proxy-is-operational"></a>Verificar se um proxy do servidor de federação está funcionando


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
[Lista de verificação: Como configurar um proxy do servidor de federação](Checklist--Setting-Up-a-Federation-Server-Proxy.md)  
  

