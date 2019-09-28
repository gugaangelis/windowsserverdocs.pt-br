---
title: Criar uma cota
description: Este artigo descreve como criar uma cota baseada em um modelo
ms.date: 7/7/2017
ms.prod: windows-server
ms.technology: storage
ms.topic: article
author: JasonGerend
manager: brianlic
ms.author: jgerend
ms.openlocfilehash: 6b199d8cc520021fcbbe32397997cc483f9c68cb
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71403132"
---
# <a name="create-a-quota"></a>Criar uma cota

> Aplica-se a: Windows Server (canal semestral), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012, Windows Server 2008 R2

Cotas podem ser criadas de um modelo ou com as propriedades personalizadas. O procedimento a seguir descreve como criar uma cota com base em um modelo (recomendado). Se você precisar criar uma cota com propriedades personalizadas, você pode salvar essas propriedades como um modelo para reutilizar mais tarde.

Quando você cria uma cota, você pode escolher um caminho de cota, que é um volume ou uma pasta ao qual o limite de armazenamento se aplica. Em um determinado caminho de cota, você pode usar um modelo para criar um dos seguintes tipos de cota:

-   Uma cota única que limita o espaço para uma pasta ou todo o volume.
-   Uma cota aplicada automaticamente, que atribui o modelo de cota para uma pasta ou volume. Cotas com base nesse modelo são geradas automaticamente e aplicadas a todas as subpastas. Para obter mais informações sobre a criação de cotas com aplicação automática, consulte [Criar uma cota de aplicação automática](create-auto-apply-quota.md).


> [!Note]
> Ao criar cotas exclusivamente a partir de modelos, você pode gerenciar suas cotas de forma centralizada, atualizando os modelos em vez das cotas individuais. Em seguida, você pode aplicar alterações em todas as cotas com base no modelo modificado. Esse recurso simplifica a implementação das alterações de políticas de armazenamento, fornecendo um ponto central no qual todas as atualizações podem ser feitas.

## <a name="to-create-a-quota-that-is-based-on-a-template"></a>Para criar uma cota com base em um modelo

1.  Em **Gerenciamento de Cota**, clique no nó **Modelos de Cota**.

2.  No painel Resultados, selecione o modelo no qual você baseará sua nova cota.

3.  Clique com o botão direito no modelo e clique em **Criar cota de modelo** (ou selecione **Criar cota de modelo** no painel **Ações**). Isso abre a caixa de diálogo **Criar cota** com as propriedades de resumo do modelo de cota exibidas.

4.  Em **Demarcador de cota**, digite ou procure a pasta a qual a cota será aplicada.

5.  Clique na opção **Criar cota no demarcador**. Observe que as propriedades de cota serão aplicadas à pasta inteira.

     > [!Note]
     > Para criar uma aplicação de cota automaticamente, clique na opção **Aplicar modelo automaticamente e criar cotas em subpastas existentes e novas**. Para obter mais informações sobre cotas com aplicação automática, consulte [Criar uma cota de aplicação automática](create-auto-apply-quota.md).

6.  Em **Derivar propriedades deste modelo de cota**, o modelo usado na etapa 2 para criar sua nova cota é pré-selecionado (ou você pode selecionar outro modelo na lista). Observe que as propriedades do modelo são exibidas em **Resumo das propriedades da cota**.

7.  Clique em **Criar**.

## <a name="see-also"></a>Consulte também

-   [Gerenciamento de cota](quota-management.md)
-   [Criar uma cota de aplicação automática](create-auto-apply-quota.md)
-   [Criar um modelo de cota](create-quota-template.md)


