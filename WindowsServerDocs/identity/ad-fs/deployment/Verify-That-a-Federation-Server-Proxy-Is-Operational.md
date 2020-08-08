---
ms.assetid: d555041a-709e-42c7-920a-8dfd2c7e0488
title: Verificar se um proxy do servidor de federação está funcionando
author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.author: billmath
ms.openlocfilehash: 4550934c56b4406ea7aff9b90509a205014dd95d
ms.sourcegitcommit: dfa48f77b751dbc34409aced628eb2f17c912f08
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/07/2020
ms.locfileid: "87940955"
---
# <a name="verify-that-a-federation-server-proxy-is-operational"></a>Verificar se um proxy do servidor de federação está funcionando


Você pode usar o procedimento a seguir para verificar se o proxy do servidor de Federação pode se comunicar com o Serviço de Federação no Serviços de Federação do Active Directory (AD FS) \( AD FS \) . Execute esse procedimento depois de executar o **AD FS assistente de configuração de proxy do servidor de Federação** para configurar o computador a ser executado na função proxy do servidor de Federação. Para obter mais informações sobre como executar esse assistente, consulte [configurar um computador para a função proxy do servidor de Federação](Configure-a-Computer-for-the-Federation-Server-Proxy-Role.md).

> [!IMPORTANT]
> O resultado desse teste é a geração bem-sucedida de um evento específico no Visualizador de Eventos no computador proxy de servidor de federação.

A associação em **Administradores**, ou equivalente, no computador local é o mínimo necessário para concluir este procedimento.  Examine os detalhes sobre como usar as contas apropriadas e as associações de grupo em [grupos padrão e de domínio](https://go.microsoft.com/fwlink/?LinkId=83477).

### <a name="to-verify-that-a-federation-server-proxy-is-operational"></a>Para verificar se um proxy do servidor de Federação está operacional

1.  Faça logon no proxy do servidor de Federação como administrador.

2.  Na tela **Iniciar** , digite**Visualizador de eventos**e pressione Enter.

3.  No painel de detalhes, clique duas vezes \- em **logs de aplicativos e serviços**, clique duas vezes \- em **AD FS eventos**e, em seguida, clique em **admin**.

4.  Na coluna **ID do Evento**, procure o ID de evento 198.

    Se o proxy do servidor de Federação estiver configurado corretamente, você verá um novo evento no log do aplicativo de Visualizador de Eventos, com a ID de evento 198. Este evento verifica se o serviço proxy do servidor de Federação foi iniciado com êxito e agora está online.

## <a name="additional-references"></a>Referências adicionais
[Lista de verificação: Como configurar um proxy do servidor de federação](Checklist--Setting-Up-a-Federation-Server-Proxy.md)


