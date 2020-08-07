---
title: Configurar uma estação de tela dividida
description: Descreve como configurar os serviços do MultiPoint para que dois usuários possam compartilhar um único sistema
ms.date: 07/22/2016
ms.topic: article
ms.assetid: 35d1d434-79b2-4e0a-835f-d94ed8ee6b21
author: evaseydl
manager: scottman
ms.author: evas
ms.openlocfilehash: 274151466c5c71499a2c570aa3eb3f8e6a877954
ms.sourcegitcommit: dfa48f77b751dbc34409aced628eb2f17c912f08
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/07/2020
ms.locfileid: "87953752"
---
# <a name="set-up-a-split-screen-station"></a>Configurar uma estação de tela dividida
Você pode configurar uma estação de tela de divisão para que dois usuários possam usar o sistema simultaneamente.

Qualquer monitor que tenha uma resolução de, no mínimo, 1200 x720, quando conectado a uma estação que ofereça suporte ao recurso de tela de divisão, pode ser dividido em duas estações. Depois que uma estação é dividida, a área de trabalho exibida pelo monitor muda para a metade esquerda da tela e uma nova estação é exibida na metade direita da tela. Para concluir a criação da nova estação, será necessário mapear um teclado, um mouse e um hub USB para a estação. Depois que uma estação é dividida, um usuário pode fazer logon na estação esquerda enquanto outro usuário faz logon na estação direita.

As estações de tela de divisão têm vários benefícios:

-   Você pode reduzir o custo e o espaço acomodando mais usuários em um sistema de serviços do MultiPoint.

-   Dois usuários podem colaborar juntos, lado a lado, em um projeto.

-   Um usuário do painel do MultiPoint pode demonstrar um procedimento em uma estação enquanto o aluno segue na outra estação.

A ilustração a seguir mostra um sistema de serviços do MultiPoint com uma estação de tela dividida (à direita).

![Estações com tela dividida](./media/WMS_diagram3.gif)

## <a name="requirements-for-a-split-screen-station"></a>Requisitos para uma estação de tela dividida
Para criar uma estação de tela de divisão, o monitor e a estação devem atender a estes requisitos:

-   O monitor deve ter uma resolução de 1200 x720 ou superior.

-   Se você estiver usando um cliente USB over-Ethernet zero, verifique com seu fornecedor de hardware para descobrir se há suporte para estações de tela de divisão. Muitos dispositivos cliente USB over-Ethernet têm limitações que impedem sua configuração como estações de tela de divisão.

## <a name="setting-up-a-split-screen-station"></a>Configurando uma estação de tela de divisão
Use os procedimentos a seguir para adicionar um segundo Hub para uma estação de tela de divisão e, em seguida, dividir a estação nos serviços do MultiPoint. O procedimento final explica como retornar uma estação de tela de divisão para uma única estação.

> [!NOTE]
> Quando você divide uma estação, a sessão ativa na estação é suspensa. O usuário deve fazer logon na estação novamente para retomar o trabalho após a divisão ocorrer.

**Para adicionar um segundo Hub com teclado e mouse:**

1.  Conecte um hub USB a uma porta USB aberta no computador, conforme mostrado na ilustração a seguir.

    ![Imagem da conexão do hub USB do MultiPoint Server](./media/WMSUSBHubConnection.gif)

2.  Conecte um teclado e um mouse ao hub USB.

    ![Imagem de conexões de dispositivo de entrada de hub USB](./media/WMSUSBDeviceConnection.gif)

3.  Conecte quaisquer periféricos adicionais, como fones de ouvido ao hub USB.

4.  Se você estiver usando um hub com energia externa, conecte o cabo de alimentação do hub a uma tomada de energia.

**Para dividir uma estação:**

1.  No Gerenciador do MultiPoint, clique na guia **estações** .

2.  Em **estação**, clique no nome da estação que você deseja dividir.

3.  Em **tarefas do item selecionado**, clique em **dividir estação**.

    A tela original é movida para a metade esquerda do monitor e a tela de uma nova estação é criada na metade direita do mesmo monitor.

4.  Crie a nova estação pressionando a letra especificada no teclado recém-adicionado, conforme indicado quando a tela **criar uma estação do MultiPoint Server** aparece na metade direita do monitor.

Depois que uma estação é dividida, um usuário pode fazer logon na estação esquerda enquanto outro usuário faz logon na estação à direita.

**Para retornar uma estação de divisão para uma única estação:**

1.  No Gerenciador do MultiPoint, clique na guia **estações** .

2.  Em **estação**, clique no nome da estação que você deseja redividir.

3.  Em **tarefas do item selecionado**, clique em **estação de divisão**.