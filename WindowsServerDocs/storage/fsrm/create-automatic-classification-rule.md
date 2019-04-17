---
title: "Criar uma regra de classificação automática"
description: "Este artigo descreve como criar uma regra de classificação para uma propriedade."
ms.date: 7/7/2017
ms.prod: windows-server-threshold
ms.technology: storage
ms.topic: article
author: JasonGerend
manager: brianlic
ms.author: jgerend
ms.openlocfilehash: c472949228184c6202681d257412c046bbc90d37
ms.sourcegitcommit: 583355400f6b0d880dc0ac6bc06f0efb50d674f7
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 10/17/2017
---
# <a name="create-an-automatic-classification-rule"></a>Criar uma regra de classificação automática

> Aplicável a: Windows Server (canal semestral), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012, Windows Server 2008 R2

O procedimento a seguir orienta você pelo processo de criação de uma regra de classificação. Cada regra define o valor para uma única propriedade. Por padrão, uma regra é executada apenas uma vez e ignora os arquivos que já têm um valor de propriedade atribuído. No entanto, você pode configurar uma regra para avaliar arquivos, independentemente se um valor já é atribuído à propriedade.

## <a name="to-create-a-classification-rule"></a>Para uma Regra de Classificação

1.  Em **Gerenciamento de Classificação**, clique no nó **Regras de Classificação**.

2.  Clique com botão direito em **Regras de classificação** e clique em **Criar uma nova regra** (ou em **Criar uma nova regra** no painel **Ações**). Isso abre a caixa de diálogo **Definições de regra de classificação**.

3.  Na guia **Configurações de regra**, insira a seguinte informação:

    -   **Nome da regra**. Digite um nome para a regra.
    -   **Habilitada**. Essa regra apenas será aplicada se a caixa Habilitada estiver marcada. Para desabilitar a regra, desmarque essa caixa.
    -   **Descrição**. Digite uma descrição opcional para essa regra.
    -   **Escopo**. Clique em **Adicionar** para selecionar um local em que esta regra será aplicada. Você pode adicionar vários locais, ou remover um local clicando em **Remover**. A regra de classificação será aplicada a todas as pastas e suas subpastas nessa lista.

4.  Na guia **Classificação**, insira a seguinte informação:

    -   **Mecanismo de classificação**. Escolha um método para atribuir o valor da propriedade.
    -   **Nome da propriedade**. Selecione a propriedade a que esta regra se aplicará.
    -   **Valor de propriedade**. Selecione o valor de propriedade a que esta regra se aplicará.

5.  Opcionalmente, clique no botão **Avançado** para selecionar mais opções. Na guia **Tipo de avaliação**, a caixa de seleção para **Reavaliar arquivos** é desmarcada por padrão. As opções que podem ser selecionadas aqui são as seguintes:

    -   **Reavaliar arquivos** desmarcada: uma regra é aplicada a um arquivo se e somente se, a propriedade especificada pela regra não foi definida como qualquer valor no arquivo.
    -   **Reavaliar arquivos** verificados e a opção **Substituir o valor existente** selecionada: a regra será aplicada aos arquivos sempre que o processo de classificação automática é executado. Por exemplo, se um arquivo tem uma propriedade booliana que é definida como **Sim**, uma regra usando o classificador de pasta para definir todos os arquivos **não** com essa opção conjunto deixará a propriedade definida como **No**.
    -   **Reavaliar arquivos** verificados e a opção **Agregar os valores** selecionada: a regra será aplicada aos arquivos sempre que o processo de classificação automática é executado. No entanto, quando a regra decidir para qual valor para definir o arquivo de propriedade, ela agregará esse valor com aquela já no arquivo. Por exemplo, se um arquivo tem uma propriedade booliana que é definida como **Sim**, uma regra usando o classificador de pasta para definir todos os arquivos **não** com essa opção conjunto deixará a propriedade definida como **Sim**.

    Na guia **parâmetros adicionais de classificação**, você pode especificar parâmetros adicionais que são reconhecidos pelo método de classificação selecionado digitando o nome e o valor e clicando no botão **Inserir**.

6.  Clique em **OK** ou **Cancelar** para fechar a caixa de diálogo **Avançado**.

7.  Clique em **OK**.

## <a name="see-also"></a>Veja também

-   [Criar uma propriedade de classificação](create-classification-property.md)
-   [Gerenciamento de classificação](classification-management.md)