---
ms.assetid: d555041a-709e-42c7-920a-8dfd2c7e0488
title: "Verificar se um Proxy de servidor de Federação está funcionando"
description: 
author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.author: billmath
ms.openlocfilehash: 4900d8621b94a514a07bba55b2f7f3df5dd36353
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 12/12/2017
---
# <a name="verify-that-a-federation-server-proxy-is-operational"></a>Verificar se um Proxy de servidor de Federação está funcionando

>Aplica-se a: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Você pode usar o procedimento a seguir para verificar se o proxy do servidor de Federação pode se comunicar com o serviço de Federação em \(AD FS\) serviços de Federação do Active Directory. Executar este procedimento depois de executar o **Assistente de configuração do AD FS federação servidor Proxy** para configurar o computador para ser executado na função de proxy do servidor de Federação. Para obter mais informações sobre como executar esse assistente, consulte [configurar um computador para a função de Proxy do servidor de Federação](Configure-a-Computer-for-the-Federation-Server-Proxy-Role.md).  
  
> [!IMPORTANT]  
> O resultado desse teste é a geração de um evento específico no Visualizador de eventos no computador de proxy do servidor de Federação bem-sucedida.  
  
A associação ao grupo **administradores**, ou equivalente, no computador local é o requisito mínimo para concluir este procedimento.  Examinar detalhes sobre como usar as contas apropriadas e agrupar associações em [Local e os grupos de domínio padrão ](https://go.microsoft.com/fwlink/?LinkId=83477).   
  
### <a name="to-verify-that-a-federation-server-proxy-is-operational"></a>Para verificar se um proxy de servidor de Federação está funcionando  
  
1.  Faça logon no proxy do servidor de federação como administrador.  
  
2.  Sobre o **iniciar** de tela, digite**Visualizador de eventos**, e pressione ENTER.  
  
3.  No painel de detalhes, clique double\ **Logs de aplicativos e serviços**, clique double\ **AD FS eventos**e, em seguida, clique em **Admin**.  
  
4.  No **ID de evento** coluna, procure 198 de ID de evento.  
  
    Se o proxy do servidor de Federação está configurado corretamente, você verá um novo evento no log do aplicativo do Visualizador de eventos, com o evento 198 ID. Esse evento verifica se o serviço de proxy do servidor de Federação foi iniciado com êxito e agora está online.  
  
## <a name="additional-references"></a>Referências adicionais  
[Lista de verificação: Configurando um Proxy de servidor de Federação](Checklist--Setting-Up-a-Federation-Server-Proxy.md)  
  

