---
title: Aumentar autenticações simultâneas processadas pelo NPS
description: Este tópico fornece instruções sobre como configurar autenticações simultâneas do servidor de políticas de rede no Windows Server 2016.
manager: brianlic
ms.prod: windows-server-threshold
ms.technology: networking
ms.topic: article
ms.assetid: 2d9cdada-0625-41c8-8248-a32259b03e47
ms.author: pashort
author: shortpatti
ms.openlocfilehash: fd930e34a4adf6c55812385b691df3e3575a4280
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59818257"
---
# <a name="increase-concurrent-authentications-processed-by-nps"></a>Aumentar autenticações simultâneas processadas pelo NPS

>Aplica-se a: Windows Server (canal semestral), Windows Server 2016

Você pode usar este tópico para obter instruções sobre como configurar autenticações simultâneas do servidor de políticas de rede.

Se você instalou o servidor de políticas de rede \(NPS\) em um computador que não seja um domínio controlador e o NPS está recebendo um grande número de solicitações de autenticação por segundo, você pode melhorar o desempenho do NPS, aumentando o número de autenticações simultâneas permitidas entre o NPS e o controlador de domínio.

Para fazer isso, você deve editar a seguinte chave do registro: 

`HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Services\Netlogon\Parameters`

Adicionar um novo valor denominado **MaxConcurrentApi** e atribuir a ela um valor de 2 a 5. 

>[!CAUTION]
>Se você atribuir um valor para **MaxConcurrentApi** que é muito alto, o NPS pode colocar uma carga excessiva no controlador de domínio.

Para obter mais informações sobre como gerenciar o NPS, consulte [gerenciar o servidor de políticas de rede](nps-manage-top.md).

Para obter mais informações sobre o NPS, consulte [servidor de diretivas de rede (NPS)](nps-top.md).