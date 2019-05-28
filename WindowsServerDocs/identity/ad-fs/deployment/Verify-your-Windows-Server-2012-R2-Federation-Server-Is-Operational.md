---
ms.assetid: 1115d276-00f6-4c23-9278-eedcc31295d8
title: Verifique se que seu servidor de Federação do Windows Server 2012 R2 está operacional
description: ''
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: 7cab415cc599f388c2bb5966d45998874ce56987
ms.sourcegitcommit: 0b5fd4dc4148b92480db04e4dc22e139dcff8582
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 05/24/2019
ms.locfileid: "66191844"
---
# <a name="verify-your-windows-server-2012-r2-federation-server-is-operational"></a>Verifique se que seu servidor de Federação do Windows Server 2012 R2 está operacional



Você pode usar os procedimentos a seguir para verificar se um servidor de federação está funcionando; ou seja, se qualquer cliente na mesma rede pode acessar um novo servidor de federação.  
  
Uma associação em **Usuários**, **Operadores de Backup**, **Usuários Avançados**, **Administradores** ou equivalente no computador local é o mínimo necessário para concluir este procedimento.  Examine os detalhes sobre como usar as contas apropriadas e associações de grupos em [domínio grupos padrão Local e](https://go.microsoft.com/fwlink/?LinkId=83477).   
  
### <a name="procedure-1-to-verify-that-a-federation-server-is-operational"></a>Procedimento 1: Para verificar se um servidor de federação está operacional  
  
1.  Para verificar os serviços de informações da Internet \(IIS\) está configurado corretamente no servidor de federação, faça logon em um computador cliente que está localizado na mesma floresta que o servidor de Federação.  
  
2.  Abra uma janela do navegador, na barra de endereços digite o nome do host DNS do servidor de Federação e, em seguida, acrescente \/adfs\/fs\/FederationServerService a ele para o novo servidor de federação, por exemplo:  
  
    **https:\/\/fs1.fabrikam.com\/adfs\/fs\/federationserverservice.asmx**  
  
3.  Pressione ENTER e execute o procedimento a seguir no computador servidor de federação. Se você receber a mensagem **Há um problema no certificado de segurança do site**, clique em **Continuar neste site**.  
  
    A saída esperada é uma exibição de XML com o documento de descrição de serviço. Se essa página aparecer, o IIS no servidor de federação está funcionando e servindo as páginas com êxito.  
  
A associação a **Administradores**, ou equivalente, no computador local é o requisito mínimo para concluir esse procedimento.  Examine os detalhes sobre como usar as contas apropriadas e associações de grupos em [domínio grupos padrão Local e](https://go.microsoft.com/fwlink/?LinkId=83477).   
  
### <a name="procedure-2-to-verify-that-a-federation-server-is-operational"></a>Procedimento 2: Para verificar se um servidor de federação está operacional  
  
1.  Faça logon no novo servidor de federação como administrador.  
  
2.  Sobre o **inicie** tela, digite**Visualizador de eventos**, e, em seguida, pressione ENTER.  
  
3.  No painel de detalhes, clique duas vezes\-clique em **Applications and Services Logs**, double\-clique em **AD FS Eventing**e, em seguida, clique em **Admin**.  
  
4.  No **ID do evento** coluna, procure a ID de evento 100. Se o servidor de Federação está configurado corretamente, você verá um novo evento — no log do aplicativo do Visualizador de eventos — com a ID de evento 100. Esse evento verifica que o servidor de Federação conseguiu se comunicar com êxito com o serviço de Federação.  
  
## <a name="see-also"></a>Consulte também 

[Implantação do AD FS](../../ad-fs/AD-FS-Deployment.md)  

[Guia de implantação do Windows Server 2012 R2 AD FS](../../ad-fs/deployment/Windows-Server-2012-R2-AD-FS-Deployment-Guide.md)  
 
[Como implantar um farm de servidores de federação](../../ad-fs/deployment/Deploying-a-Federation-Server-Farm.md)  
   
  

