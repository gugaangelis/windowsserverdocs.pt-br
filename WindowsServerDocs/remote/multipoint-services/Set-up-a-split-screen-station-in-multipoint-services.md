---
title: Configurar uma estação de tela dividida
description: Descreve como configurar o MultiPoint Services para que dois usuários possam compartilhar um único sistema
ms.custom: na
ms.date: 07/22/2016
ms.prod: windows-server-threshold
ms.technology: multipoint-services
ms.reviewer: na
ms.suite: na
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 35d1d434-79b2-4e0a-835f-d94ed8ee6b21
author: evaseydl
manager: scottman
ms.author: evas
ms.openlocfilehash: b2d01df6175eee99fd1374cd5af270cbcc764ca8
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59849937"
---
# <a name="set-up-a-split-screen-station"></a>Configurar uma estação de tela dividida
Você pode configurar uma estação de tela dividida para que dois usuários simultaneamente podem usar o sistema.

Qualquer monitor que tem uma resolução de mínimo 1200 x 720, quando conectado a uma estação que suporta o recurso o split screen, pode ser dividido em duas estações. Depois que uma estação é dividida, a área de trabalho que o monitor teve exibido move para a metade esquerda da tela, e uma nova estação é exibida na metade direita da tela. Para concluir a criação da nova estação, você precisará mapear um teclado, mouse e hub USB para a estação. Depois que uma estação é dividida, um usuário pode fazer logon na estação esquerda enquanto outro usuário faz logon na estação direita.  
  
Estações com tela dividida tem vários benefícios:  
  
-   Você pode reduzir o custo e espaço ao acomodar mais usuários em um sistema MultiPoint Services.  
  
-   Dois usuários podem colaborar juntos, lado a lado, em um projeto.  
  
-   Um usuário do MultiPoint Dashboard pode demonstrar um procedimento em uma estação enquanto um aluno acompanha na outra estação.  
  
A ilustração a seguir mostra um sistema MultiPoint Services com uma estação de tela de divisão (à direita).  
  
![Estações de tela dividida](./media/WMS_diagram3.gif)  
   
## <a name="requirements-for-a-split-screen-station"></a>Requisitos para uma estação de tela dividida  
Para criar uma estação de tela dividida, o monitor e a estação devem satisfazer estes requisitos:  
  
-   O monitor deve ter uma resolução de 1200 x720 ou superior.  
  
-   Se você estiver usando um cliente de USB over Ethernet zero, verifique com seu fornecedor de hardware para descobrir se o split screen estações têm suporte. Muitos dispositivos de cliente de USB over Ethernet zero têm limitações que impedem sua configuração como estações com tela dividida.  
  
## <a name="setting-up-a-split-screen-station"></a>Como configurar uma estação de tela dividida  
Use os procedimentos a seguir para adicionar um segundo hub para uma tela dividida estação e, em seguida, dividir a existência da estação do MultiPoint Services. O último procedimento explica como retornar uma estação de tela dividida para uma única estação.  
  
> [!NOTE]  
> Quando você divide uma estação, a sessão ativa na estação é suspensa. O usuário deve fazer logon estação novamente para retomar o trabalho após a divisão ocorre.  
  
**Para adicionar um segundo hub com o teclado e mouse:**  
  
1.  Conecte-se um hub USB a uma porta USB aberta no computador, conforme mostrado na ilustração a seguir.  
  
    ![Imagem do MultiPoint server conexão de hub USB](./media/WMSUSBHubConnection.gif)  
  
2.  Conecte um teclado e mouse ao hub USB.  
  
    ![Imagem de conexões de dispositivo de entrada de hub USB](./media/WMSUSBDeviceConnection.gif)  
  
3.  Conecte-se todos os periféricos adicionais, como fones de ouvido ao hub USB.  
  
4.  Se você estiver usando um hub alimentado externamente, conecte o cabo de alimentação do hub a uma tomada elétrica.  
  
**Para dividir uma estação:**  
  
1.  No Gerenciador do MultiPoint, clique o **estações** guia.  
  
2.  Sob **estação**, clique no nome da estação que deseja dividir.  
  
3.  Sob **tarefas do Item selecionado**, clique em **dividir estação**.  
  
    A tela original se move para a metade esquerda do monitor e tela de uma nova estação é criada na metade direita do mesmo monitor.  
  
4.  Criar nova estação pressionando a letra especificada no teclado recém-adicionada, conforme indicado quando o **criar uma estação do MultiPoint Server** tela é exibida na metade direita do monitor.  
  
Depois que uma estação é dividida, um usuário pode fazer logon na estação esquerda enquanto outro usuário faz logon na estação direita.  
  
**Para retornar uma estação dividida para uma única estação:**  
  
1.  No Gerenciador do MultiPoint, clique o **estações** guia.  
  
2.  Sob **estação**, clique no nome da estação que deseja dividido.  
  
3.  Sob **tarefas do Item selecionado**, clique em **reverter divisão da estação**.