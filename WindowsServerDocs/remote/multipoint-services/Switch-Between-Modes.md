---
title: Alternar entre modos
description: Saiba como alternar entre o modo de estação e console no MultiPoint Services
ms.custom: na
ms.prod: windows-server-threshold
ms.technology: multipoint-services
ms.reviewer: na
ms.suite: na
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 5f1b2324-c1b0-4b61-ab51-39af15e7792a
author: lizap
manager: dongill
ms.author: elizapo
ms.date: 08/04/2016
ms.openlocfilehash: a9c00d65ad1c59e91f4011ab932bf9f921957c59
ms.sourcegitcommit: eaf071249b6eb6b1a758b38579a2d87710abfb54
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 05/31/2019
ms.locfileid: "66446153"
---
# <a name="switch-between-modes"></a>Alternar entre modos
MultiPoint Manager inclui os seguintes modos para ajudá-lo a executar diferentes tipos de gerenciamento do sistema MultiPoint Services:  
  
-   *Modo de estação*: Por padrão, o sistema MultiPoint Services é iniciado no modo de estação. No modo de estação, as estações de Serviços do MultiPoint se comportam como se cada estação fosse um computador separado executando o Windows e vários usuários podem usar o sistema ao mesmo tempo. Você e seus usuários podem compartilhar arquivos e executar o trabalho que você deve fazer.  
  
-   *Modo de console*: Quando o sistema MultiPoint Services está em modo de console, você pode instalar e atualizar drivers e software ou executar outras tarefas de manutenção. Quando o sistema está em modo de console, nenhuma *estação* está disponível para uso por outros usuários do computador. Essas estações não são exibidas no Gerenciador do MultiPoint. Todos os monitores conectados diretamente ao servidor são tratados como exibições desse sistema de computador.   
  
> [!NOTE]
> Você pode impor o sistema iniciando no modo de Console ao alterar o padrão nas configurações para o servidor.  
> ## <a name="to-switch-from-station-mode-to-console-mode"></a>Para alternar do modo de estação para o modo de console  
  
1.  Abra o MultiPoint Manager no modo de estação e, em seguida, clique no **Home** guia.  
  
2.  Na coluna **Computador**, clique no computador para o qual você deseja alterar os modos.  
  
3.  Sob *nome do computador* **tarefas**, clique em **alternar para modo de console**. O computador é reiniciado e todas as estações ficam indisponíveis.  
  
## <a name="to-switch-from-console-mode-to-station-mode"></a>Para alternar do modo de console para o modo de estação  
  
1.  Abra o MultiPoint Manager no modo de console e, em seguida, clique no **Home** guia.  
  
2.  Na coluna **Computador**, clique no computador para o qual você deseja alterar os modos.  
  
3.  Sob *nome do computador* **tarefas**, clique em **alternar para modo de estação**. O computador reinicia e todas as estações disponíveis.  
  
## <a name="see-also"></a>Consulte também  
[Gerenciar tarefas de sistema usando o MultiPoint Manager](Manage-System-Tasks-Using-MultiPoint-Manager.md)