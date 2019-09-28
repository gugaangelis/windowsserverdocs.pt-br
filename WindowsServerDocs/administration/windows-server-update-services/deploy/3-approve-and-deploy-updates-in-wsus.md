---
title: Etapa 3-aprovar e implantar atualizações no WSUS
description: Tópico do Windows Server Update Service (WSUS) – aprovar e implantar atualizações no WSUS é a etapa três em um processo de quatro etapas para implantar o WSUS
ms.prod: windows-server
ms.reviewer: na
ms.technology: manage-wsus
ms.topic: article
ms.assetid: 8d728ff9-170f-47e6-aefe-52be93315a75
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 68ff4c893302167815e3e8368d8b03f97d9be131
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71361687"
---
# <a name="step-3-approve-and-deploy-updates-in-wsus"></a>Etapa 3: Aprovar e implantar atualizações no WSUS

>Aplica-se a: Windows Server (canal semestral), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Os computadores de um grupo de computadores contatam automaticamente o servidor do WSUS nas 24 horas seguintes para obter atualizações. Você pode usar o recurso de relatórios do WSUS para determinar se essas atualizações foram implantadas nos computadores de teste. Quando os testes são concluídos com êxito, você pode aprovar as atualizações para os grupos de computadores aplicáveis em sua organização. A lista de verificação a seguir descreve as etapas para aprovar e implantar atualizações usando o console de gerenciamento do WSUS.

|Tarefa|Descrição|
|----|--------|
|,1. Aprovar e implantar atualizações do WSUS @ no__t-0|Aprove e implante atualizações do WSUS usando o Console de Gerenciamento do WSUS.|
|,2. Configurar regras de aprovação automática @ no__t-0|Configure o WSUS para aprovar automaticamente a instalação de atualizações para grupos selecionados e como aprovar revisões de atualizações existentes.|
|,3. Examinar as atualizações instaladas com os relatórios do WSUS @ no__t-0|Examine as atualizações que foram instaladas, os computadores que receberam essas atualizações e outros detalhes usando o recurso de Relatórios do WSUS.|

## <a name="BKM_3.1."></a>3,1. Aprovar e implantar atualizações do WSUS
Use o procedimento a seguir para aprovar e implantar atualizações.

#### <a name="to-approve-and-deploy-wsus-updates"></a>Para aprovar e implantar atualizações do WSUS

1.  No Console de Administração do WSUS, clique em **Atualizações**. No painel direito, um resumo do status das atualizações é exibido para **Todas as Atualizações**, **Atualizações Críticas**, **Atualizações de Segurança** e **Atualizações do WSUS**.

2.  Na seção **Todas as Atualizações** , clique em **Atualizações necessárias aos computadores**.

3.  Na lista de atualizações, escolha aquelas que deseja aprovar para serem instaladas em seu grupo de computadores de teste. As informações sobre uma atualização escolhida estão disponíveis no painel inferior do painel **Atualizações**. Para selecionar várias atualizações contíguas, mantenha pressionada a tecla **Shift** enquanto clica nos nomes de atualização. Para escolher várias atualizações não contíguas, mantenha pressionada a tecla **CTRL** ao clicar nos nomes das atualizações.

4.  Clique com o botão direito do mouse na seleção e clique em **Aprovar**.

5.  Na caixa de diálogo **Aprovar Atualizações**, escolha seu grupo de teste e clique na seta para baixo.

6.  Clique **Aprovado para Instalação**e clique em **OK**.

7.  Será exibida a janela **Progresso da Aprovação** , que mostra o progresso das tarefas que afetam a aprovação de atualizações. Quando o processo de aprovação for concluído, clique em **Fechar**.

## <a name="BKM_3.2.a."></a>3,2. Configurar regras de aprovação automática
As Aprovações Automáticas permitem que você especifique como aprovar automaticamente a instalação de atualizações para grupos selecionados e como aprovar revisões de atualizações existentes.

#### <a name="to-configure-automatic-approvals"></a>Para configurar as Aprovações Automáticas

1.  No Console de Administração do WSUS, em **Update Services**, expanda o servidor WSUS e, em seguida, clique em **Opções**. A janela **Opções** é aberta.

2.  Em **Opções**, clique em **Aprovações Automáticas**. A caixa de diálogo Aprovações Automáticas é aberta.

3.  Em **Regras de Atualização**, clique em **Nova Regra**. A caixa de diálogo **Adicionar regra** é aberta.

4.  Em **Adicionar regra**, na **etapa 1: selecione Propriedades**, selecione qualquer opção única ou combinação de opções do seguinte:

    -   **Quando uma atualização está em uma classificação específica**

    -   **Quando uma atualização está em um produto específico**

    -   **Definir um prazo final para a aprovação**

5.  Na **etapa 2: Edite as propriedades**, clique em cada uma das opções listadas e, em seguida, selecione as opções apropriadas para cada uma.

6.  Em **Step 3: Especifique um nome @ no__t-0, digite um nome para a regra e clique em **OK**.

7.  Clique em **OK** para fechar a caixa de diálogo Aprovações Automáticas.

## <a name="BKM_3.3."></a>3,3. Examinar as atualizações instaladas com os Relatórios do WSUS
Após 24 horas da aprovação das atualizações, você poderá usar o recurso de Relatórios do WSUS para determinar se as atualizações foram implantadas nos computadores de grupos de teste. Para verificar o status de uma atualização, é possível usar o recurso de Relatórios do WSUS da seguinte maneira.

#### <a name="to-review-updates"></a>Para examinar as atualizações

1.  No painel de navegação do Console de Administração do WSUS, clique em **Relatórios**.

2.  Na página **Relatórios** , clique no relatório **Resumo do Status da Atualização** . A janela **Relatório de Atualizações** será exibida.

3.  Se quiser filtrar a lista de atualizações, escolha os critérios que quer usar, por exemplo, **Incluir atualizações nestas classificações**e clique em **Executar Relatório**.

4.  Você verá o painel **Relatório de Atualizações**. É possível verificar o status de uma atualização individual escolhendo-a na seção esquerda do painel. A última seção do painel de relatórios mostra o resumo do status da atualização.

5.  Você pode salvar ou imprimir esse relatório clicando no ícone apropriado na barra de ferramentas.

6.  Depois de testar as atualizações, você poderá aprovar as atualizações para serem instaladas nos grupos de computadores aplicáveis em sua organização.
