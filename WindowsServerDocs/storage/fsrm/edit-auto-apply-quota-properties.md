---
title: Editar propriedades de cota de aplicação automática
description: Este artigo descreve como editar aplicação automática a propriedades de cota
ms.date: 7/7/2017
ms.topic: article
author: JasonGerend
manager: brianlic
ms.author: jgerend
ms.openlocfilehash: 558f6b094e97a6196177e728c238f5bb7a38e7a1
ms.sourcegitcommit: dfa48f77b751dbc34409aced628eb2f17c912f08
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/07/2020
ms.locfileid: "87957443"
---
# <a name="edit-auto-apply-quota-properties"></a>Editar propriedades de cota de aplicação automática

> Aplicável a: Windows Server (canal semestral), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012, Windows Server 2008 R2

Quando você faz alterações em uma cota aplicada automaticamente, você tem a opção de ampliar essas alterações para cotas existentes no demarcador de cota com aplicação automática. Você pode optar por modificar somente as cotas que ainda correspondem à cota de aplicação automática original ou todas as cotas no demarcador de cota com aplicação automática, independentemente de quaisquer modificações que foram feitas às cotas desde que elas foram criadas. Esse recurso simplifica o processo de atualização das propriedades de cotas derivadas de uma cota com aplicação automática fornecendo um ponto central onde você pode realizar todas as alterações.

> [!Note]
> Se optar por aplicar as alterações em todas as cotas no demarcador de cota com aplicação automática, você substituirá quaisquer propriedades de cota personalizadas que você criou.

## <a name="to-edit-an-auto-apply-quota"></a>Para editar uma cota de aplicação automática

1.  Em **Cotas**, selecione a cota de aplicação automática que você deseja modificar. Você pode filtrar as cotas para mostrar apenas as cotas de aplicação automática.

2.  Clique com botão direito na entrada da cota e clique em **Editar propriedades de cota** (ou no painel **Ações** em **Cotas selecionadas**, selecione **Editar propriedades de cota**). Essa ação abre a caixa de diálogo **Editar cota de aplicação automática**.

3.  Em **Derivar propriedades deste modelo de cota**, selecione o modelo de cota que deseja aplicar. Você pode examinar as propriedades de cada modelo de cota na caixa de listagem de resumo.

4.  Clique em **OK**. Isso abrirá a caixa de diálogo **Atualizar cotas derivadas de cota com aplicação automática**.

5.  Selecione o tipo de atualização que deseja aplicar:

    -   Se você tiver cotas que foram modificadas desde que foram automaticamente criadas, e não quiser mudá-las, selecione **Aplicar cota de aplicação automática apenas em cotas derivadas que correspondem a cota de aplicação automática original**. Essa opção atualizará somente as cotas no demarcador de cota de aplicação automática que não foram editadas desde que foram geradas automaticamente.
    -   Se você quiser modificar todas as cotas existentes no demarcador de cota de aplicação automática, selecione **Aplicar cota de aplicação automática para todas cotas derivadas**.
    -   Se quiser manter as cotas existentes inalteradas, mas tornara cota de aplicação automática modificada efetiva para novas subpastas no demarcador de cota de aplicação automática, selecione **Não aplicar cota de aplicação automática a cotas derivadas**.

6.  Clique em **OK**.

## <a name="additional-references"></a>Referências adicionais

-   [Gerenciamento de cota](quota-management.md)
-   [Criar uma cota de aplicação automática](create-auto-apply-quota.md)


