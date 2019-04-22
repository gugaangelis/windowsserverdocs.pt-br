---
title: Configurar grupos de servidores RADIUS remotos
description: Este tópico fornece informações sobre como configurar grupos de servidores RADIUS remotos no servidor de políticas de rede no Windows Server 2016.
manager: brianlic
ms.prod: windows-server-threshold
ms.technology: networking
ms.topic: article
ms.assetid: ca125e57-249c-4d97-85d1-2929cbf871f1
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 02088a35196c0bfadeb65e8971a47fdcc741258d
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59818757"
---
# <a name="configure-remote-radius-server-groups"></a>Configurar grupos de servidores RADIUS remotos

>Aplica-se a: Windows Server (canal semestral), Windows Server 2016

Você pode usar este tópico para configurar grupos de servidores remotos RADIUS quando você deseja configurar o NPS para atuar como um servidor proxy e as solicitações de conexão direta para outros NPSs para processamento.

## <a name="add-a-remote-radius-server-group"></a>Adicionar um grupo de servidores remotos RADIUS

Você pode usar este procedimento para adicionar um novo grupo de servidores RADIUS remoto no snap-in do servidor de diretivas de rede (NPS).

Quando você configura o NPS como um proxy RADIUS, você pode criar uma nova diretiva de solicitação de conexão que o NPS usa para determinar qual conexão solicitações para encaminhar para outros servidores RADIUS. Além disso, a diretiva de solicitação de conexão é configurada com a especificação de um servidor remoto RADIUS que contém um ou mais servidores RADIUS, que informa ao NPS para onde enviar as solicitações de conexão que correspondam à diretiva de solicitação de conexão.

>[!NOTE]
>Você também pode configurar um novo grupo de servidores RADIUS remoto durante o processo de criação de uma nova diretiva de solicitação de conexão.

Ser membro do grupo **Admins. do Domínio**, ou equivalente, é o mínimo necessário para concluir este procedimento.

### <a name="to-add-a-remote-radius-server-group"></a>Para adicionar um grupo de servidores remotos RADIUS 

1. No Gerenciador do servidor, clique em **ferramentas**e, em seguida, clique em **Network Policy Server** para abrir o console do NPS.
2. Na árvore de console, clique duas vezes **clientes e servidores RADIUS**, clique com botão direito **grupos de servidores remotos RADIUS**e, em seguida, clique em **New**.
3. O **novo grupo de servidores remotos RADIUS** caixa de diálogo é aberta. Na **nome do grupo**, digite um nome para o grupo de servidores RADIUS remotos.
4. **Em servidores RADIUS**, clique em **Add**. O **Adicionar servidor RADIUS** caixa de diálogo é aberta. Digite o endereço IP do servidor RADIUS que você deseja adicionar ao grupo ou digite o nome de domínio totalmente qualificado \(FQDN\) do servidor RADIUS e, em seguida, clique **verificar**.
5. Na **Adicionar servidor RADIUS**, clique no **autenticação/contabilização** guia. Na **segredo compartilhado** e **Confirmar segredo compartilhado**, digite o segredo compartilhado. Quando você configura o computador local como um cliente RADIUS no servidor RADIUS remoto, você deve usar o mesmo segredo compartilhado.
6. Se você não estiver usando o protocolo EAP (Extensible Authentication) para autenticação, clique em **solicitação deve conter o atributo de assinatura**. Por padrão, o EAP usa o atributo de autenticador de mensagem.
7. Verifique se os números de porta de autenticação e as estatísticas estão corretos para sua implantação.
8. Se você usar um segredo compartilhado diferente para contabilidade, no **contabilização**, desmarque as **usam o mesmo segredo compartilhado para autenticação e estatísticas** caixa de seleção e, em seguida, digite o segredo compartilhado de contabilização em  **Segredo compartilhado** e **Confirmar segredo compartilhado**.
9. Se você não deseja encaminhar o início do servidor de acesso de rede e parar mensagens para o servidor RADIUS remoto, desmarque a **encaminhar a inicialização do servidor de acesso de rede e interromper as notificações para este servidor** caixa de seleção.

Para obter mais informações sobre como gerenciar o NPS, consulte [gerenciar o servidor de políticas de rede](nps-manage-top.md).

Para obter mais informações sobre o NPS, consulte [servidor de diretivas de rede (NPS)](nps-top.md).

