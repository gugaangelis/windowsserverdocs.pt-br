---
title: Dividir uma estação de usuário
description: Saiba como dividir uma exibição nos serviços do MultiPoint para que dois usuários possam usar a mesma estação
ms.date: 07/08/2016
ms.prod: windows-server
ms.technology: multipoint-services
ms.topic: article
ms.assetid: f0d1fc9c-f5ea-45bc-a8da-623c5d081cdf
author: lizap
manager: dongill
ms.author: elizapo
ms.openlocfilehash: db828da06045bb884db138458f875af1d4d5b99d
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80853889"
---
# <a name="split-a-user-station"></a>Dividir uma estação de usuário
Qualquer monitor de estação de serviços do MultiPoint que tenha uma resolução maior que 1024x768 pode ser dividido em duas estações usando a tarefa **dividir estação** na guia **estações** . A área de trabalho que está presente no monitor no momento em que a divisão ocorre é movida para a metade esquerda do monitor e uma nova estação é criada na metade direita do mesmo monitor. A nova estação deve ser associada a um teclado, mouse e hub USB para concluir sua criação. Depois que uma estação é dividida, um usuário pode fazer logon na estação esquerda enquanto outro usuário faz logon na estação direita.  
  
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
  
3.  Em **Tarefas das estações**, clique em **Reverter divisão da estação**.  
  
## <a name="see-also"></a>Consulte também  
[Gerenciar estações do usuário](Manage-User-Stations.md)