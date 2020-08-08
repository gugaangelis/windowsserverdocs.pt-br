---
title: Desabilitar o encaminhamento de notificação do NAS no NPS
description: Este tópico fornece instruções sobre como configurar as autenticações simultâneas do servidor de políticas de rede no Windows Server 2016.
manager: brianlic
ms.topic: article
ms.assetid: a09bfb03-95fc-4534-bf3c-97078ef6b07e
ms.author: lizross
author: eross-msft
ms.openlocfilehash: 72ac7d13005fef609df437ce3fcb4e12f617a400
ms.sourcegitcommit: dfa48f77b751dbc34409aced628eb2f17c912f08
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/07/2020
ms.locfileid: "87946970"
---
# <a name="disable-nas-notification-forwarding-in-nps"></a>Desabilitar o encaminhamento de notificação do NAS no NPS

>Aplica-se a: Windows Server (Canal Semestral), Windows Server 2016

Você pode usar este procedimento para desabilitar o encaminhamento de mensagens de início e de parada de servidores de acesso à rede (NASs) para membros de um grupo de servidores RADIUS remoto configurado no NPS.

Quando você tiver grupos de servidores RADIUS remotos configurados e, em **políticas de solicitação de conexão**NPS, desmarque a caixa de seleção **encaminhar solicitações de contabilidade para este grupo de servidores remotos RADIUS** , esses grupos ainda serão enviados nas mensagens de notificação de início e parada do nas.

Isso cria tráfego de rede desnecessário. Para eliminar esse tráfego, desabilite o encaminhamento de notificação do NAS para servidores individuais em cada grupo de servidores RADIUS remotos.

Para concluir este procedimento, é preciso ser um membro do grupo **Administradores**.

### <a name="to-disable-nas-notification-forwarding"></a>Para desabilitar o encaminhamento de notificação do NAS

1. No Gerenciador do Servidor, clique em **Ferramentas**e, em seguida, clique em **Servidor de Políticas de Rede**. O console do NPS é aberto.

2. No console do NPS, clique duas vezes em **clientes e servidores RADIUS**, clique em **grupos de servidores RADIUS remotos**e clique duas vezes no grupo de servidores remotos RADIUS que você deseja configurar. A caixa de diálogo **Propriedades** do grupo de servidores remotos RADIUS é aberta.

3. Clique duas vezes no membro do grupo que você deseja configurar e, em seguida, clique na guia **Autenticação/Contabilização** .

4. Em **contabilidade**, desmarque a caixa de seleção **encaminhar o servidor de acesso à rede iniciar e parar notificações neste servidor** e clique em **OK**.

5. Repita as etapas 3 e 4 para todos os membros do grupo que você deseja configurar.

Para obter mais informações sobre como gerenciar o NPS, consulte [gerenciar o servidor de políticas de rede](nps-manage-top.md).

Para obter mais informações sobre o NPS, consulte [servidor de diretivas de rede (NPS)](nps-top.md).
