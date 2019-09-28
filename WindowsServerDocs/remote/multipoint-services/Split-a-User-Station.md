---
title: Dividir uma estação de usuário
description: Saiba como dividir uma exibição nos serviços do MultiPoint para que dois usuários possam usar a mesma estação
ms.custom: na
ms.date: 07/08/2016
ms.prod: windows-server
ms.technology: multipoint-services
ms.reviewer: na
ms.suite: na
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: f0d1fc9c-f5ea-45bc-a8da-623c5d081cdf
author: lizap
manager: dongill
ms.author: elizapo
ms.openlocfilehash: 5067df3f5902570d56ee130264c751d66b5b0d3f
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71394953"
---
# <a name="split-a-user-station"></a>Dividir uma estação de usuário
Qualquer monitor de estação do MultiPoint Services que possua uma resolução maior do que 1024 x 768 pode ser dividido em duas estações usando a tarefa **Dividir estação** na guia **Estações**. A área de trabalho presente no monitor no momento em que a divisão ocorre é movida para a metade esquerda do monitor e uma nova estação é criada na metade direita do mesmo monitor. A nova estação deve ser associada a um teclado, mouse e hub USB para concluir sua criação. Depois que uma estação é dividida, um usuário pode fazer logon na estação esquerda enquanto outro usuário faz logon na estação direita.  
  
Os benefícios do uso de uma estação de tela de divisão podem incluir:  
  
-   Redução do custo e espaço ao acomodar mais alunos em um sistema MultiPoint Services  
  
-   Permitindo que dois alunos colaborem juntos, lado a lado, em um projeto  
  
-   Possibilidade de um professor demonstrar um procedimento em uma estação enquanto um aluno acompanha na outra estação  
   
> [!NOTE]  
> Quando você divide uma estação, a sessão ativa na estação é suspensa. O usuário deve fazer logon na estação novamente para retomar o trabalho após a divisão.  
  
**Para dividir uma estação:**  
  
1.  No Gerenciador do MultiPoint, no modo de estação, clique na guia **estações** .  
  
2.  Na coluna **Estação**, clique no nome da estação que deseja dividir.  
  
3.  Em **Tarefas das estações**, clique em **Dividir estação**.  
  
**Para retornar uma estação de divisão para uma única estação:**  
  
1.  No Gerenciador do MultiPoint, no modo de estação, clique na guia **estações** .  
  
2.  Na coluna **Estação**, clique no nome da estação para a qual você deseja encerrar uma divisão.  
  
3.  Em **tarefas**da estação, clique em **estação de divisão**.  
  
## <a name="see-also"></a>Consulte também  
[Gerenciar estações do usuário](Manage-User-Stations.md)