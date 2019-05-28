---
title: Gerenciar os computadores cliente do WSUS e os grupos de computadores do WSUS
description: Tópico do Windows Server Update Service (WSUS) - como gerenciar computadores clientes e grupos
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-wsus
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 4b1ea915-0f9f-4f0e-8913-a1dd460d07ab
author: coreyp-at-msft
ms.author: coreyp
manager: lizapo
ms.date: 10/16/2017
ms.openlocfilehash: 4ede63ab08d204c29555b28ae3a73795291c321c
ms.sourcegitcommit: 8ba2c4de3bafa487a46c13c40e4a488bf95b6c33
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 05/25/2019
ms.locfileid: "66222481"
---
# <a name="managing-wsus-client-computers-and-wsus-computer-groups"></a>Gerenciar os computadores cliente do WSUS e os grupos de computadores do WSUS

>Aplica-se a: Windows Server (canal semestral), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

O nó de computadores é um ponto de acesso central no console administrativo do WSUS para gerenciar dispositivos e computadores cliente WSUS. Sob esse nó, você pode encontrar os diferentes grupos que você configurou (mais o grupo padrão, computadores não atribuídos).

## <a name="managing-client-computers"></a>Gerenciamento de computadores cliente
Selecionando um dos grupos de computadores a **computadores** nó sob **opções** faz com que os computadores nesse grupo a ser exibido no painel de detalhes. Se um computador for atribuído a vários grupos, ele aparecerá nas listagens de ambos os grupos. Se você selecionar um computador na lista, você pode ver suas propriedades, que incluem detalhes gerais sobre o computador e o status das atualizações para ele, como a instalação ou status de detecção de uma atualização de um computador específico. Você pode filtrar a lista de computadores em um grupo de computadores determinado pelo status. O padrão mostra somente os computadores para os quais atualizações são necessárias ou que tiveram falhas de instalação; No entanto, você pode filtrar a exibição por qualquer status. Clique em **Refresh** depois de alterar o filtro de status.

Você também pode gerenciar grupos de computadores em que a página de computadores, que inclui a criação de grupos e atribuir computadores a eles. Para obter mais informações sobre como gerenciar grupos de computadores, consulte Gerenciando grupos de computadores na próxima seção deste guia e seção [1.5. Planejar grupos de computadores do WSUS](../plan/plan-your-wsus-deployment.md#15-plan-wsus-computer-groups) na etapa 1: Prepare para a implantação do WSUS do guia de implantação do WSUS.

> [!NOTE]
> Primeiro você deve configurar computadores clientes para contatar o servidor do WSUS antes que você possa gerenciá-los desse servidor. Até que você executa essa tarefa, o servidor do WSUS não reconhecerá os computadores cliente e elas não serão exibidas na lista na página de computadores. Para obter mais informações sobre como configurar computadores cliente, consulte [1.5. Planejar grupos de computadores do WSUS](../plan/plan-your-wsus-deployment.md#15-plan-wsus-computer-groups) da etapa 1: Preparar para implantação do WSUS e a etapa 3: Configure o WSUS, no guia de implantação do WSUS.

## <a name="controlling-when-wsus-client-computers-install-updates"></a>Controlando quando os computadores cliente WSUS instalam as atualizações
Há dois métodos para controlar quando os computadores cliente WSUS instalam atualizações:

-   Aprovação com datas limite: Prazos estritamente impõem quando uma atualização está instalada

-   Políticas de grupo do WSUS: Quando o Windows Update Agent verifica e instala atualizações de controle de diretivas de grupo

    Para obter mais informações, consulte: [Etapa 5: Definir configurações de política de grupo para atualizações automáticas](../deploy/4-configure-group-policy-settings-for-automatic-updates.md), no guia de implantação do WSUS.

## <a name="managing-computer-groups"></a>Gerenciando grupos de computadores
O WSUS permite direcionar atualizações a grupos de computadores clientes, assim é possível garantir que computadores específicos sempre obterão as atualizações certas na hora certa. Por exemplo, se todos os computadores de um departamento (por exemplo, a equipe de Contabilidade) tiver uma configuração específica, você poderá configurar um grupo para essa equipe, decidir quais atualizações são necessárias aos computadores e em qual horário deverão ser instaladas, e usar os relatórios do WSUS para avaliar as atualizações para a equipe.

Computadores sempre são atribuídos para o **todos os computadores** agrupar e permanecem atribuídos à **computadores não atribuídos** até você atribuí-los para outro grupo de grupo. Os computadores podem pertencer a mais de um grupo.

Os grupos de computadores podem ser configurados em hierarquias (por exemplo, o grupo Folha de Pagamento e o grupo Contas a Pagar no grupo Contabilidade). Atualizações aprovados para um grupo superior serão implantadas automaticamente nos grupos inferiores, bem como ao próprio grupo de maior. Portanto, se você aprovar Update1 para o grupo Contabilidade, a atualização será implantada para todos os computadores no grupo Contabilidade, todos os computadores no grupo de folha de pagamento e todos os computadores no grupo de contas a pagar.

Como os computadores podem ser atribuídos a vários grupos, é possível que uma única atualização seja aprovada mais de uma vez para o mesmo computador. Entretanto, a atualização será implantada somente uma vez, e quaisquer conflitos serão resolvidos pelo servidor do WSUS. Para continuar com o exemplo acima, se o ComputadorA for atribuído a folha de pagamento e os grupos de contas a pagar, e Update1 for aprovada para ambos os grupos, ela será implantada somente uma vez.

É possível atribuir computadores a grupos de computadores usando um destes dois métodos: direcionamento do lado do servidor ou direcionamento do lado do cliente. Com o direcionamento do lado do servidor, você mover manualmente um ou mais computadores cliente para um grupo de computadores por vez. Com o direcionamento do lado do cliente, você usa a Diretiva de Grupo ou edita as configurações de registro nos computadores clientes para habilitar esses computadores para serem adicionados automaticamente aos grupos de computadores criados anteriormente. Esse processo pode ser inserido no script e implantado em vários computadores ao mesmo tempo. Você deve especificar o método de direcionamento que será usado no servidor do WSUS, selecionando uma das duas opções na **computadores** seção o **opções** página.

> [!NOTE]
> Se um servidor do WSUS estiver em execução em modo de réplica, não será possível criar grupos de computadores nesse servidor. Todos os grupos de computador necessários para clientes do servidor de réplica devem ser criados no servidor do WSUS que é a raiz da hierarquia de servidor do WSUS. Para obter mais informações sobre o modo de réplica, consulte [modo de réplica do WSUS em execução](running-wsus-replica-mode.md) e para obter mais informações sobre o direcionamento do lado do servidor e do lado do cliente, consulte a seção [1.5. Planejar grupos de computadores do WSUS](../plan/plan-your-wsus-deployment.md#15-plan-wsus-computer-groups) da etapa 1: Prepare para a implantação do WSUS no guia de implantação do WSUS.


