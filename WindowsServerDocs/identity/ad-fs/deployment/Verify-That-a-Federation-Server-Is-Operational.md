---
ms.assetid: ad61c586-ba8a-4534-8824-b45994d60c6b
title: Verificar se um servidor de federação está funcionando
description: ''
author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.author: billmath
ms.openlocfilehash: 2034b4c35061879a64004486395d0887c59087b2
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59877707"
---
# <a name="verify-that-a-federation-server-is-operational"></a>Verificar se um servidor de federação está funcionando

>Aplica-se a: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Você pode usar os procedimentos a seguir para verificar se um servidor de federação está funcionando; ou seja, se qualquer cliente na mesma rede pode acessar um novo servidor de federação.  
  
Uma associação em **Usuários**, **Operadores de Backup**, **Usuários Avançados**, **Administradores** ou equivalente no computador local é o mínimo necessário para concluir este procedimento.  Examine os detalhes sobre como usar as contas apropriadas e associações de grupos em [domínio grupos padrão Local e](https://go.microsoft.com/fwlink/?LinkId=83477).   
  
### <a name="procedure-1-to-verify-that-a-federation-server-is-operational"></a>Procedimento 1: Para verificar se um servidor de federação está operacional  
  
1.  Para verificar os serviços de informações da Internet \(IIS\) está configurado corretamente no servidor de federação, faça logon em um computador cliente que está localizado na mesma floresta que o servidor de Federação.  
  
2.  Abra uma janela do navegador, na barra de endereço digite federação DNS do servidor nome do host e, em seguida, acrescente /adfs/fs/federationserverservice.asmx nela para o novo servidor de federação, por exemplo:  
  
    **https://fs1.fabrikam.com/adfs/fs/federationserverservice.asmx**  
  
3.  Pressione ENTER e execute o procedimento a seguir no computador servidor de federação. Se você receber a mensagem **Há um problema no certificado de segurança do site**, clique em **Continuar neste site**.  
  
    A saída esperada é uma exibição de XML com o documento de descrição de serviço. Se essa página aparecer, o IIS no servidor de federação está funcionando e servindo as páginas com êxito.  
  
A associação a **Administradores**, ou equivalente, no computador local é o requisito mínimo para concluir esse procedimento.  Examine os detalhes sobre como usar as contas apropriadas e associações de grupos em [domínio grupos padrão Local e](https://go.microsoft.com/fwlink/?LinkId=83477).   
  
### <a name="procedure-2-to-verify-that-a-federation-server-is-operational"></a>Procedimento 2: Para verificar se um servidor de federação está operacional  
  
1.  Faça logon no novo servidor de federação como administrador.  
  
2.  Sobre o **inicie** tela, digite **Visualizador de eventos**, e, em seguida, pressione ENTER.  
  
3.  No painel de detalhes, clique duas vezes\-clique em **Applications and Services Logs**, double\-clique em **AD FS Eventing**e, em seguida, clique em **Admin**.  
  
4.  No **ID do evento** coluna, procure a ID de evento 100. Se o servidor de Federação está configurado corretamente, você verá um novo evento — no log do aplicativo do Visualizador de eventos — com a ID de evento 100. Esse evento verifica que o servidor de Federação conseguiu se comunicar com êxito com o serviço de Federação.  
  
## <a name="additional-references"></a>Referências adicionais  
[Lista de verificação: Configurando um servidor de Federação](Checklist--Setting-Up-a-Federation-Server.md)  
  

