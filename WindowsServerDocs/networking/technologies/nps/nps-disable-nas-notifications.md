---
title: Desabilitar o encaminhamento de notificações no NPS
description: Este tópico fornece instruções sobre como configurar autenticações simultâneas do servidor de política de rede no Windows Server 2016.
manager: brianlic
ms.prod: windows-server-threshold
ms.technology: networking
ms.topic: article
ms.assetid: a09bfb03-95fc-4534-bf3c-97078ef6b07e
ms.author: pashort
author: shortpatti
ms.openlocfilehash: c25f3b5a94624a35099e84ede3296f7ab860da7c
ms.sourcegitcommit: 19d9da87d87c9eefbca7a3443d2b1df486b0b010
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/28/2018
---
# <a name="disable-nas-notification-forwarding-in-nps"></a>Desabilitar o encaminhamento de notificações no NPS

>Aplica-se a: Windows Server (anual por canal), Windows Server 2016

Você pode usar este procedimento para desativar o encaminhamento de iniciar e parar de mensagens de servidores de acesso à rede (NASs) aos membros de um grupo de servidores remotos RADIUS configurados no NPS.

Quando você tiver grupos de servidores remotos RADIUS configurados e, em NPS **diretivas de solicitação de Conexão**, você limpar o **encaminhar solicitações de contabilidade para esse grupo de servidores remotos RADIUS** caixa de seleção, esses grupos ainda são enviados NAS iniciar e interromper notificações de mensagens. 

Isso cria o tráfego de rede desnecessário. Para eliminar esse tráfego, desative notificação NAS encaminhando para servidores individuais em cada grupo de servidores remotos RADIUS.

Para concluir este procedimento, você deve ser um membro do **administradores** grupo.

### <a name="to-disable-nas-notification-forwarding"></a>Para desativar o encaminhamento de notificação NAS

1. No Gerenciador do servidor, clique em **ferramentas**e clique em **NPS**. Abre o console do NPS.

2. No console do NPS, clique duas vezes em **clientes e servidores RADIUS**, clique em **grupos de servidores remotos RADIUS**e clique duas vezes o grupo de servidores remotos RADIUS que você deseja configurar. O grupo de servidores remotos RADIUS **propriedades** caixa de diálogo é aberta.

3. Clique duas vezes o membro do grupo que você deseja configurar e, em seguida, clique no **autenticação/contabilidade** guia.

4. Em **Accounting**, desmarque o **encaminhar inicial de servidor de acesso de rede e interromper notificações para esse servidor** caixa de seleção e, em seguida, clique em **Okey**.

5. Repita as etapas 3 e 4 para todos os membros do grupo que você deseja configurar.

Para obter mais informações sobre o gerenciamento de NPS, consulte [gerenciar o servidor de políticas de rede](nps-manage-top.md).

Para obter mais informações sobre o NPS, consulte [servidor de política de rede (NPS)](nps-top.md).
