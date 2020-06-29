---
title: Editar propriedades de modelo de cota
description: Este artigo descreve como editar as propriedades de modelo de cota para estender alterações para cotas criadas com o modelo de cota original
ms.date: 7/7/2017
ms.prod: windows-server
ms.technology: storage
ms.topic: article
author: JasonGerend
manager: brianlic
ms.author: jgerend
ms.openlocfilehash: f4793616caf9ba4a8f9db09cd1902432323c081b
ms.sourcegitcommit: 771db070a3a924c8265944e21bf9bd85350dd93c
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/27/2020
ms.locfileid: "85474033"
---
# <a name="edit-quota-template-properties"></a>Editar propriedades de modelo de cota

> Aplicável a: Windows Server (canal semestral), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012, Windows Server 2008 R2

Quando você faz alterações a um modelo de cota, você tem a opção de ampliar essas alterações para cotas que foram criadas com o modelo de cota original. Você pode optar por modificar somente as cotas que ainda correspondem ao modelo original ou todas as cotas que foram derivadas do modelo original, independentemente de quaisquer modificações que foram feitas às cotas desde que eles foram criadas. Esse recurso simplifica o processo de atualização das propriedades de suas cotas fornecendo um ponto central onde você pode realizar todas as alterações.

> [!Note]
> Se optar por aplicar as alterações em todas as cotas derivadas do modelo original, você substituirá quaisquer propriedades de cota personalizadas que você criou.

## <a name="to-edit-quota-template-properties"></a>Para editar propriedades de modelo de cota

1.  Em **Modelos de cota**, selecione o modelo que você deseja modificar.

2.  Clique com botão direito no modelo de cota e clique em **Editar propriedades de modelo** (ou no painel **Ações** em **Modelos de cota selecionados**, selecione **Editar propriedades de modelo**). Essa ação abre a caixa de diálogo **Propriedades de modelo de cota**.

3.  Execute todas as alterações necessárias. As configurações e opções de notificação são idênticas às que você pode definir quando cria um modelo de cota. Opcionalmente, você pode copiar as propriedades de um modelo diferente e modificá-las para esta.

4.  Quando tiver terminado de editar as propriedades de modelo, clique em **OK**. Isso abrirá a caixa de diálogo **Atualizar cotas derivadas de modelo**.

5.  Selecione o tipo de atualização que deseja aplicar:

    -   Se você tiver cotas que foram modificadas desde que elas foram criados com o modelo original e não desejar alterá-las, selecione **Aplicar modelo somente às cotas derivadas que correspondem ao modelo original**. Essa opção atualizará somente as cotas que não foram editadas desde que elas foram criados com o modelo original.
    -   Se você quiser modificar todas as suas cotas que foram criadas a partir do modelo original, selecione **Aplicar modelo em todas as cotas derivadas**.
    -   Se quiser manter as cotas existentes inalteradas, selecione **Não aplicar modelo ao cotas derivadas**.

6.  Clique em **OK**.

## <a name="additional-references"></a>Referências adicionais

-   [Gerenciamento de cota](quota-management.md)
-   [Criar um modelo de cota](create-quota-template.md)


