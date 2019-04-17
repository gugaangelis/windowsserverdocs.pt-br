---
title: Aumentar autenticações simultâneas processadas pelo NPS
description: Este tópico fornece instruções sobre como configurar autenticações simultâneas do servidor de política de rede no Windows Server 2016.
manager: brianlic
ms.prod: windows-server-threshold
ms.technology: networking
ms.topic: article
ms.assetid: 2d9cdada-0625-41c8-8248-a32259b03e47
ms.author: pashort
author: shortpatti
ms.openlocfilehash: aa70c1a26e2c22d26545e1b46a6151d71a2b4095
ms.sourcegitcommit: 19d9da87d87c9eefbca7a3443d2b1df486b0b010
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/28/2018
---
# <a name="increase-concurrent-authentications-processed-by-nps"></a>Aumentar autenticações simultâneas processadas pelo NPS

>Aplica-se a: Windows Server (anual por canal), Windows Server 2016

Você pode usar este tópico para obter instruções sobre como configurar autenticações simultâneas do servidor de políticas de rede.

Se você instalou o servidor de política de rede \(NPS\) em um computador que não seja um controlador de domínio e o servidor NPS está recebendo um grande número de solicitações de autenticação por segundo, você pode melhorar o desempenho de NPS, aumentando o número de autenticações simultâneas permitido entre o servidor NPS e o controlador de domínio.

Para fazer isso, você deve editar a seguinte chave do registro: 

`HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Services\Netlogon\Parameters`

Adicione um novo valor denominado **MaxConcurrentApi** e atribua a ele um valor de 2 a 5. 

>[!CAUTION]
>Se você atribuir um valor para **MaxConcurrentApi** que é muito alta, seu servidor NPS pode colocar uma carga excessiva no controlador de domínio.

Para obter mais informações sobre o gerenciamento de NPS, consulte [gerenciar o servidor de políticas de rede](nps-manage-top.md).

Para obter mais informações sobre o NPS, consulte [servidor de política de rede (NPS)](nps-top.md).