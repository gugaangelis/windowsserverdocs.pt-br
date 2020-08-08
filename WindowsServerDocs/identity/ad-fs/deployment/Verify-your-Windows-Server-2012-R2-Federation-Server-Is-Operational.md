---
ms.assetid: 1115d276-00f6-4c23-9278-eedcc31295d8
title: Verifique se o servidor de Federação do Windows Server 2012 R2 está funcionando
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.openlocfilehash: 82cad5a13478986208f5f19d1905290fb0097655
ms.sourcegitcommit: dfa48f77b751dbc34409aced628eb2f17c912f08
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/07/2020
ms.locfileid: "87940860"
---
# <a name="verify-your-windows-server-2012-r2-federation-server-is-operational"></a>Verifique se o servidor de Federação do Windows Server 2012 R2 está funcionando



Você pode usar os procedimentos a seguir para verificar se um servidor de federação está funcionando; ou seja, se qualquer cliente na mesma rede pode acessar um novo servidor de federação.

Uma associação em **Usuários**, **Operadores de Backup**, **Usuários Avançados**, **Administradores** ou equivalente no computador local é o mínimo necessário para concluir este procedimento.  Examine os detalhes sobre como usar as contas apropriadas e as associações de grupo em [grupos padrão e de domínio](https://go.microsoft.com/fwlink/?LinkId=83477).

### <a name="procedure-1-to-verify-that-a-federation-server-is-operational"></a>Procedimento 1: para verificar se um servidor de Federação está operacional

1.  Para verificar se Serviços de Informações da Internet \( IIS \) está configurado corretamente no servidor de Federação, faça logon em um computador cliente que esteja localizado na mesma floresta que o servidor de Federação.

2.  Abra uma janela do navegador, na barra de endereços, digite o nome do host DNS do servidor de Federação e, em seguida, acrescente \/ \/ o ADFS FS \/ federationserverservice. asmx a ele para o novo servidor de Federação, por exemplo:

    **https: \/ \/ FS1.fabrikam.com \/ ADFS \/ FS \/ federationserverservice. asmx**

3.  Pressione ENTER e execute o procedimento a seguir no computador servidor de federação. Se você vir a mensagem **há um problema com o certificado de segurança deste site**, clique em **continuar neste site**.

    A saída esperada é uma exibição de XML com o documento de descrição de serviço. Se essa página aparecer, o IIS no servidor de federação está funcionando e servindo as páginas com êxito.

A associação em **Administradores**, ou equivalente, no computador local é o mínimo necessário para concluir este procedimento.  Examine os detalhes sobre como usar as contas apropriadas e as associações de grupo em [grupos padrão e de domínio](https://go.microsoft.com/fwlink/?LinkId=83477).

### <a name="procedure-2-to-verify-that-a-federation-server-is-operational"></a>Procedimento 2: para verificar se um servidor de Federação está operacional

1.  Inicie sessão no novo servidor de federação como um administrador.

2.  Na tela **Iniciar** , digite**Visualizador de eventos**e pressione Enter.

3.  No painel de detalhes, clique duas vezes \- em **logs de aplicativos e serviços**, clique duas vezes \- em **AD FS eventos**e, em seguida, clique em **admin**.

4.  Na coluna **ID do evento** , procure a ID do evento 100. Se o servidor de Federação estiver configurado corretamente, você verá um novo evento — no log do aplicativo de Visualizador de Eventos — com a ID de evento 100. Esse evento verifica se o servidor de Federação foi capaz de se comunicar com êxito com o Serviço de Federação.

## <a name="see-also"></a>Consulte Também

[Implantação do AD FS](../../ad-fs/AD-FS-Deployment.md)

[Guia de Implantação do AD FS do Windows Server 2012 R2](../../ad-fs/deployment/Windows-Server-2012-R2-AD-FS-Deployment-Guide.md)

[Como implantar um farm de servidores de federação](../../ad-fs/deployment/Deploying-a-Federation-Server-Farm.md)



