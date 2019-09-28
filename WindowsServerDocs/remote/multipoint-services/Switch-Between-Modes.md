---
title: Alternar entre modos
description: Saiba como alternar entre o modo de estação e de console nos serviços do MultiPoint
ms.custom: na
ms.prod: windows-server
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
ms.openlocfilehash: 181324bff3227f7b071b8ae32f756d365b059a74
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71389237"
---
# <a name="switch-between-modes"></a>Alternar entre modos
O MultiPoint Manager inclui os seguintes modos para ajudá-lo a executar diferentes tipos de gerenciamento do sistema de serviços do MultiPoint:  
  
-   *Modo de estação*: Por padrão, o sistema de serviços do MultiPoint é iniciado no modo de estação. No modo de estação, as estações de Serviços do MultiPoint se comportam como se cada estação fosse um computador separado executando o Windows e vários usuários podem usar o sistema ao mesmo tempo. Você e seus usuários podem compartilhar arquivos e executar o trabalho que você deve fazer.  
  
-   *Modo de console*: Quando o sistema de serviços do MultiPoint estiver no modo de console, você poderá instalar e atualizar software e drivers ou executar outras tarefas de manutenção. Quando o sistema está em modo de console, nenhuma *estação* está disponível para uso por outros usuários do computador. Essas estações não são exibidas no MultiPoint Manager. Todos os monitores conectados diretamente ao servidor são tratados como exibições deste sistema de computador.   
  
> [!NOTE]
> Você pode impor o sistema iniciando no modo de Console ao alterar o padrão nas configurações para o servidor.  
> ## <a name="to-switch-from-station-mode-to-console-mode"></a>Para alternar do modo de estação para o modo de console  
  
1.  Abra o Gerenciador do MultiPoint no modo de estação e clique na guia **página inicial** .  
  
2.  Na coluna **Computador**, clique no computador para o qual você deseja alterar os modos.  
  
3.  Em **tarefas**de nome do computador, clique em **alternar para o modo de console**. O computador é reiniciado e todas as estações ficam indisponíveis.  
  
## <a name="to-switch-from-console-mode-to-station-mode"></a>Para alternar do modo de console para o modo de estação  
  
1.  Abra o Gerenciador do MultiPoint no modo de console e clique na guia **página inicial** .  
  
2.  Na coluna **Computador**, clique no computador para o qual você deseja alterar os modos.  
  
3.  Em **tarefas**de nome do computador, clique em **alternar para o modo de estação**. O computador reinicia e todas as estações disponíveis.  
  
## <a name="see-also"></a>Consulte também  
[Gerenciar tarefas de sistema usando o MultiPoint Manager](Manage-System-Tasks-Using-MultiPoint-Manager.md)