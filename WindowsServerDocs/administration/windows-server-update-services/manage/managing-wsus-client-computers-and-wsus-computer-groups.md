---
title: Gerenciar os computadores cliente do WSUS e os grupos de computadores do WSUS
description: Tópico Windows Server Update Service (WSUS)-como gerenciar computadores e grupos cliente
ms.custom: na
ms.prod: windows-server
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
ms.openlocfilehash: 454fa385dc9fb91218ad836d4ad34e92e9644dac
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71361632"
---
# <a name="managing-wsus-client-computers-and-wsus-computer-groups"></a>Gerenciar os computadores cliente do WSUS e os grupos de computadores do WSUS

>Aplicável a: Windows Server (canal semestral), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

O nó computadores é o ponto de acesso central no console administrativo do WSUS para gerenciar dispositivos e computadores cliente do WSUS. Nesse nó, você pode encontrar os grupos diferentes que você configurou (mais o grupo padrão, computadores não atribuídos).

## <a name="managing-client-computers"></a>Gerenciando computadores cliente
A seleção de um dos grupos de computador no nó **computadores** em **Opções** faz com que os computadores desse grupo sejam exibidos no painel de detalhes. Se um computador for atribuído a vários grupos, ele será exibido nas listagens de ambos os grupos. Se você selecionar um computador na lista, poderá ver suas propriedades, que incluem detalhes gerais sobre o computador e o status das atualizações para ele, como o status de instalação ou detecção de uma atualização para um determinado computador. Você pode filtrar a lista de computadores em determinado grupo por status. O padrão mostra apenas os computadores para os quais as atualizações são necessárias ou quais tiveram falhas de instalação; no entanto, você pode filtrar a exibição por qualquer status. Clique em **Atualizar** depois de alterar o filtro de status.

Você também pode gerenciar grupos de computadores na página computadores, que inclui a criação de grupos e a atribuição de computadores a eles. Para obter mais informações sobre como gerenciar grupos de computadores, consulte Managing Computer groups na próxima seção deste guia e a seção [1,5. Planeje os grupos de computadores do WSUS](../plan/plan-your-wsus-deployment.md#15-plan-wsus-computer-groups) na etapa 1: Prepare-se para a implantação do WSUS do guia de implantação do WSUS.

> [!NOTE]
> Primeiro, você deve configurar os computadores cliente para contatar o servidor do WSUS antes de gerenciá-los a partir desse servidor. Até que você execute essa tarefa, o servidor do WSUS não reconhecerá seus computadores cliente e eles não serão exibidos na lista da página computadores. Para obter mais informações sobre como configurar computadores cliente, consulte [1,5. Planeje os grupos de computadores do WSUS](../plan/plan-your-wsus-deployment.md#15-plan-wsus-computer-groups) da etapa 1: Prepare-se para a implantação do WSUS e etapa 3: configurar o WSUS, no guia de implantação do WSUS.

## <a name="controlling-when-wsus-client-computers-install-updates"></a>Controlando quando os computadores cliente do WSUS instalam atualizações
Há dois métodos para controlar quando os computadores cliente do WSUS instalam atualizações:

-   Aprovação com prazos: os prazos se impõem estritamente quando uma atualização é instalada

-   Políticas de grupo do WSUS: as políticas de grupo controlam quando o agente de Windows Update examina e instala atualizações

    para obter mais informações, consulte: [etapa 5: definir configurações de política de grupo para atualizações automáticas](../deploy/4-configure-group-policy-settings-for-automatic-updates.md), no guia de implantação do WSUS.

## <a name="managing-computer-groups"></a>Gerenciando grupos de computadores
O WSUS permite direcionar atualizações a grupos de computadores clientes, assim é possível garantir que computadores específicos sempre obterão as atualizações certas na hora certa. Por exemplo, se todos os computadores de um departamento (por exemplo, a equipe de Contabilidade) tiver uma configuração específica, você poderá configurar um grupo para essa equipe, decidir quais atualizações são necessárias aos computadores e em qual horário deverão ser instaladas, e usar os relatórios do WSUS para avaliar as atualizações para a equipe.

Os computadores sempre são atribuídos ao grupo **todos os computadores** e permanecem atribuídos ao grupo **computadores não atribuídos** até que você os atribua a outro grupo. Os computadores podem pertencer a mais de um grupo.

Os grupos de computadores podem ser configurados em hierarquias (por exemplo, o grupo Folha de Pagamento e o grupo Contas a Pagar no grupo Contabilidade). As atualizações que são aprovadas para um grupo superior serão automaticamente implantadas em grupos inferiores, bem como no grupo mais alto. Portanto, se você aprovar Atualização1 para o grupo contabilidade, a atualização será implantada em todos os computadores do grupo contabilidade, em todos os computadores do grupo de folha de pagamento e em todos os computadores do grupo contas a pagar.

Como os computadores podem ser atribuídos a vários grupos, é possível que uma única atualização seja aprovada mais de uma vez para o mesmo computador. Entretanto, a atualização será implantada somente uma vez, e quaisquer conflitos serão resolvidos pelo servidor do WSUS. Para continuar com o exemplo acima, se o computadora for atribuído aos grupos de folha de pagamento e contas a pagar e Atualização1 for aprovado para ambos os grupos, ele será implantado apenas uma vez.

É possível atribuir computadores a grupos de computadores usando um destes dois métodos: direcionamento do lado do servidor ou direcionamento do lado do cliente. Com o direcionamento do lado do servidor, você move manualmente um ou mais computadores cliente para um grupo de computadores de cada vez. Com o direcionamento do lado do cliente, você usa a Diretiva de Grupo ou edita as configurações de registro nos computadores clientes para habilitar esses computadores para serem adicionados automaticamente aos grupos de computadores criados anteriormente. Esse processo pode ser inserido e implantado em vários computadores ao mesmo tempo. Você deve especificar o método de direcionamento que será usado no servidor do WSUS selecionando uma das duas opções na seção **computadores** da página **Opções** .

> [!NOTE]
> Se um servidor do WSUS estiver em execução em modo de réplica, não será possível criar grupos de computadores nesse servidor. Todos os grupos de computadores necessários para clientes do servidor de réplica devem ser criados no servidor do WSUS que é a raiz da hierarquia do servidor do WSUS. Para obter mais informações sobre o modo de réplica, consulte [executando o modo de réplica do WSUS](running-wsus-replica-mode.md) e para obter mais informações sobre o direcionamento do lado do cliente e do servidor, consulte a seção [1,5. Planejar grupos de computadores WSUS](../plan/plan-your-wsus-deployment.md#15-plan-wsus-computer-groups) da etapa 1: Prepare-se para a implantação do WSUS no guia de implantação do WSUS.


