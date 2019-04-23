---
title: Desabilitar o encaminhamento de notificação no NPS
description: Este tópico fornece instruções sobre como configurar autenticações simultâneas do servidor de políticas de rede no Windows Server 2016.
manager: brianlic
ms.prod: windows-server-threshold
ms.technology: networking
ms.topic: article
ms.assetid: a09bfb03-95fc-4534-bf3c-97078ef6b07e
ms.author: pashort
author: shortpatti
ms.openlocfilehash: bc4c6afdcb02eb2bbab1f0373a5b3a28236269bf
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59882257"
---
# <a name="disable-nas-notification-forwarding-in-nps"></a>Desabilitar o encaminhamento de notificação no NPS

>Aplica-se a: Windows Server (canal semestral), Windows Server 2016

Você pode usar este procedimento para desabilitar o encaminhamento de iniciar e parar mensagens de servidores de acesso de rede (NASs) a membros de um grupo de servidores remotos RADIUS configurado no NPS.

Quando você tiver grupos de servidores remotos RADIUS configurados e, no NPS **diretivas de solicitação de Conexão**, você desmarcar a **encaminhar solicitações de estatísticas para esse grupo de servidores remotos RADIUS** caixa de seleção, esses grupos são ainda enviadas NAS iniciar e parar as mensagens de notificação. 

Isso cria o tráfego de rede desnecessário. Para eliminar esse tráfego, desabilite a notificação de NAS encaminhamento para servidores individuais em cada grupo de servidores RADIUS remotos.

Para concluir este procedimento, é preciso ser um membro do grupo **Administradores**.

### <a name="to-disable-nas-notification-forwarding"></a>Para desabilitar o encaminhamento de notificação de NAS

1. No Gerenciador do servidor, clique em **ferramentas**e, em seguida, clique em **Network Policy Server**. Abre o console do NPS.

2. No console do NPS, clique duas vezes **clientes e servidores RADIUS**, clique em **grupos de servidores remotos RADIUS**e, em seguida, clique duas vezes o grupo de servidores remotos RADIUS que você deseja configurar. O grupo de servidores remotos RADIUS **propriedades** caixa de diálogo é aberta.

3. Clique duas vezes o membro do grupo que você deseja configurar e, em seguida, clique o **autenticação/contabilização** guia.

4. Na **contabilização**, desmarque as **encaminhar a inicialização do servidor de acesso de rede e interromper as notificações para este servidor** caixa de seleção e, em seguida, clique em **Okey**.

5. Repita as etapas 3 e 4 para todos os membros do grupo que você deseja configurar.

Para obter mais informações sobre como gerenciar o NPS, consulte [gerenciar o servidor de políticas de rede](nps-manage-top.md).

Para obter mais informações sobre o NPS, consulte [servidor de diretivas de rede (NPS)](nps-top.md).
