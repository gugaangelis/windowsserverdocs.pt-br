---
title: Configurar grupos de servidores RADIUS remotos
description: Este tópico fornece informações sobre como configurar grupos de servidores remotos RADIUS no servidor de políticas de rede no Windows Server 2016.
manager: brianlic
ms.topic: article
ms.assetid: ca125e57-249c-4d97-85d1-2929cbf871f1
ms.author: lizross
author: eross-msft
ms.openlocfilehash: 0c3977681220ecbd15c02b7e6174f20b3f536bf8
ms.sourcegitcommit: dfa48f77b751dbc34409aced628eb2f17c912f08
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/07/2020
ms.locfileid: "87969373"
---
# <a name="configure-remote-radius-server-groups"></a>Configurar grupos de servidores RADIUS remotos

>Aplica-se a: Windows Server (Canal Semestral), Windows Server 2016

Você pode usar este tópico para configurar grupos de servidores remotos RADIUS quando desejar configurar o NPS para atuar como um servidor proxy e encaminhar solicitações de conexão para outros NPSs para processamento.

## <a name="add-a-remote-radius-server-group"></a>Adicionar um grupo de servidores RADIUS remoto

Você pode usar este procedimento para adicionar um novo grupo de servidores remotos RADIUS no snap-in servidor de diretivas de rede (NPS).

Ao configurar o NPS como um proxy RADIUS, você cria uma nova política de solicitação de conexão que o NPS usa para determinar quais solicitações de conexão encaminhar para outros servidores RADIUS. Além disso, a política de solicitação de conexão é configurada especificando um grupo de servidores RADIUS remoto que contém um ou mais servidores RADIUS, o que informa ao NPS para onde enviar as solicitações de conexão que correspondem à política de solicitação de conexão.

>[!NOTE]
>Você também pode configurar um novo grupo de servidores remotos RADIUS durante o processo de criação de uma nova política de solicitação de conexão.

A associação no **Admins. do Domínio** ou equivalente é o requisito mínimo exigido para concluir este procedimento.

### <a name="to-add-a-remote-radius-server-group"></a>Para adicionar um grupo de servidores RADIUS remotos

1. Em Gerenciador do Servidor, clique em **ferramentas**e, em seguida, clique em **servidor de políticas de rede** para abrir o console do NPS.
2. Na árvore de console, clique duas vezes em **clientes e servidores RADIUS**, clique com o botão direito do mouse em **grupos de servidores RADIUS remotos**e clique em **novo**.
3. A caixa de diálogo **novo grupo de servidores remotos RADIUS** é aberta. Em **nome do grupo**, digite um nome para o grupo de servidores remotos RADIUS.
4. **Em servidores RADIUS**, clique em **Adicionar**. A caixa de diálogo **adicionar servidores RADIUS** é aberta. Digite o endereço IP do servidor RADIUS que você deseja adicionar ao grupo ou digite o FQDN do nome de domínio totalmente qualificado \( \) do servidor RADIUS e, em seguida, clique em **verificar**.
5. Em **adicionar servidores RADIUS**, clique na guia **Autenticação/Contabilização** . Em **segredo compartilhado** e **confirmar segredo compartilhado**, digite o segredo compartilhado. Você deve usar o mesmo segredo compartilhado ao configurar o computador local como um cliente RADIUS no servidor RADIUS remoto.
6. Se você não estiver usando o protocolo EAP para autenticação, clique em **solicitar deve conter o atributo autenticador de mensagem**. O EAP usa o atributo de autenticador de mensagem por padrão.
7. Verifique se os números de porta de autenticação e contabilização estão corretos para sua implantação.
8. Se você usar um segredo compartilhado diferente para contabilidade, em **contabilidade**, desmarque a caixa de seleção **usar o mesmo segredo compartilhado para autenticação e contabilização** e, em seguida, digite o segredo compartilhado de contabilidade em **segredo compartilhado** e **confirme segredo compartilhado**.
9. Se você não quiser encaminhar mensagens de início e parada do servidor de acesso à rede para o servidor RADIUS remoto, desmarque a caixa de seleção **encaminhar notificações do servidor de acesso à rede para este servidor** .

Para obter mais informações sobre como gerenciar o NPS, consulte [gerenciar o servidor de políticas de rede](nps-manage-top.md).

Para obter mais informações sobre o NPS, consulte [servidor de diretivas de rede (NPS)](nps-top.md).

