---
title: Configurar políticas de solicitação de conexão
description: Este tópico fornece informações sobre como configurar políticas de solicitação de conexão no servidor de políticas de rede no Windows Server 2016.
manager: brianlic
ms.prod: windows-server
ms.technology: networking
ms.topic: article
ms.assetid: f62c6a67-4dda-47f8-8bdf-9b76c37953e6
ms.author: pashort
author: shortpatti
ms.openlocfilehash: d62beb3106141d4683c957020bc96e4a7dfb306f
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71405481"
---
# <a name="configure-connection-request-policies"></a>Configurar políticas de solicitação de conexão

>Aplica-se a: Windows Server (Canal Semestral), Windows Server 2016

Você pode usar este tópico para criar e configurar políticas de solicitação de conexão que determinam se o NPS local processa solicitações de conexão ou as encaminha para o servidor RADIUS remoto para processamento.

As políticas de solicitação de conexão são conjuntos de condições e configurações que permitem que os administradores de rede designem quais servidores de serviço RADIUS (RADIUS) executam a autenticação e a autorização de solicitações de conexão que o servidor que executa o servidor de diretivas de rede \(NPS @ no__t-1 recebe de clientes RADIUS.

A política de solicitação de conexão padrão usa o NPS como um servidor RADIUS e processa todas as solicitações de autenticação localmente.

Para configurar um servidor que executa o NPS para atuar como um proxy RADIUS e encaminhar solicitações de conexão para outros servidores NPS ou RADIUS, você deve configurar um grupo de servidores RADIUS remotos, além de adicionar uma nova política de solicitação de conexão que especifica as condições e configurações que as solicitações de conexão devem corresponder.

Você pode criar um novo grupo de servidores remotos RADIUS enquanto estiver criando uma nova política de solicitação de conexão com o assistente de nova política de solicitação de conexão.

Se você não quiser que o NPS atue como um servidor RADIUS e processe as solicitações de conexão localmente, você poderá excluir a política de solicitação de conexão padrão.

Se você quiser que o NPS atue como um servidor RADIUS, processando solicitações de conexão localmente e como um proxy RADIUS, encaminhando algumas solicitações de conexão para um grupo de servidores remotos RADIUS, adicione uma nova política usando o procedimento a seguir e verifique se o padrão a política de solicitação de conexão é a última política processada colocando-a por último na lista de políticas.

## <a name="add-a-connection-request-policy"></a>Adicionar uma política de solicitação de conexão

Ser membro do grupo **Admins. do Domínio**, ou equivalente, é o mínimo necessário para concluir este procedimento.

### <a name="to-add-a-new-connection-request-policy"></a>Para adicionar uma nova política de solicitação de conexão 

1. Em Gerenciador do Servidor, clique em **ferramentas**e, em seguida, clique em **servidor de políticas de rede** para abrir o console do NPS. 
2. Na árvore de console, clique duas vezes em **políticas**.
3. Clique com o botão direito do mouse em **políticas de solicitação de conexão**e clique em **nova política de solicitação de conexão**.
4. Use o assistente para nova política de solicitação de conexão para configurar a política de solicitação de conexão e, se não configurada anteriormente, um grupo de servidores RADIUS remoto.


Para obter mais informações sobre como gerenciar o NPS, consulte [gerenciar o servidor de políticas de rede](nps-manage-top.md).

Para obter mais informações sobre o NPS, consulte [servidor de diretivas de rede (NPS)](nps-top.md).

