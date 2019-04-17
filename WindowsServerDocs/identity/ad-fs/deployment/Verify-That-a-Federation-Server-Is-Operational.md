---
ms.assetid: ad61c586-ba8a-4534-8824-b45994d60c6b
title: "Verifique se que um servidor de Federação está funcionando"
description: 
author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.author: billmath
ms.openlocfilehash: 2034b4c35061879a64004486395d0887c59087b2
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 12/12/2017
---
# <a name="verify-that-a-federation-server-is-operational"></a>Verifique se que um servidor de Federação está funcionando

>Aplica-se a: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Você pode usar os procedimentos a seguir para verificar se um servidor de Federação está operacional; ou seja, que qualquer cliente na mesma rede pode acessar um servidor de Federação novo.  
  
A associação ao grupo **usuários**, **operadores de Backup**, **usuários avançados**, **administradores** ou equivalente, no computador local é o requisito mínimo para concluir este procedimento.  Examinar detalhes sobre como usar as contas apropriadas e agrupar associações em [Local e os grupos de domínio padrão ](https://go.microsoft.com/fwlink/?LinkId=83477).   
  
### <a name="procedure-1-to-verify-that-a-federation-server-is-operational"></a>Procedimento 1: Para verificar se um servidor de Federação está operacional  
  
1.  Para verificar se o Internet Information Services \(IIS\) está configurado corretamente no servidor de federação, faça logon um computador cliente que está localizado na mesma floresta que o servidor de Federação.  
  
2.  Abra uma janela do navegador, no tipo de barra de endereço do federação DNS do servidor nome de host e, em seguida, acrescente /adfs/fs/federationserverservice.asmx-lo para o servidor de Federação novo, por exemplo:  
  
    **https://FS1.Fabrikam.com/ADFS/FS/FederationServerService.asmx**  
  
3.  Pressione ENTER e, em seguida, conclua o procedimento a seguir no computador do servidor de Federação. Se você vir a mensagem **há um problema com o certificado de segurança deste site**, clique em **continuar para este site**.  
  
    O resultado esperado é uma exibição de XML com o documento de descrição do serviço. Se esta página será exibida, o IIS no servidor de Federação é páginas operacionais e estar pronto com êxito.  
  
A associação ao grupo **administradores**, ou equivalente, no computador local é o requisito mínimo para concluir este procedimento.  Examinar detalhes sobre como usar as contas apropriadas e agrupar associações em [Local e os grupos de domínio padrão ](https://go.microsoft.com/fwlink/?LinkId=83477).   
  
### <a name="procedure-2-to-verify-that-a-federation-server-is-operational"></a>Procedimento 2: Para verificar se um servidor de Federação está operacional  
  
1.  Faça logon no servidor de Federação novo como administrador.  
  
2.  Sobre o **iniciar** de tela, digite **Visualizador de eventos**, e pressione ENTER.  
  
3.  No painel de detalhes, clique double\ **Logs de aplicativos e serviços**, clique double\ **AD FS eventos**e, em seguida, clique em **Admin**.  
  
4.  No **ID de evento** coluna, procure o evento ID 100. Se o servidor de Federação está configurado corretamente, você verá um novo evento — no log do aplicativo do Visualizador de eventos — com o evento ID 100. Esse evento verifica que o servidor de Federação foi capaz de se comunicar com o serviço de federação com êxito.  
  
## <a name="additional-references"></a>Referências adicionais  
[Lista de verificação: Configurar um servidor de Federação](Checklist--Setting-Up-a-Federation-Server.md)  
  

