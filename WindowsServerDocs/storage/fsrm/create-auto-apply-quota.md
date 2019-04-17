---
title: "Crie uma cota de aplicação automática"
description: "Este artigo descreve como criar cotas de aplicação automática com base em um modelo de cota"
ms.date: 7/7/2017
ms.prod: windows-server-threshold
ms.technology: storage
ms.topic: article
author: JasonGerend
manager: brianlic
ms.author: jgerend
ms.openlocfilehash: e2837df448434252470d783a6c06f0690ba09021
ms.sourcegitcommit: 583355400f6b0d880dc0ac6bc06f0efb50d674f7
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 10/17/2017
---
# <a name="create-an-auto-apply-quota"></a>Crie uma cota de aplicação automática

> Aplicável a: Windows Server (canal semestral), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012, Windows Server 2008 R2

Usando as cotas de aplicação automática, você pode atribuir um modelo de cota a um volume ou pasta pai. Em seguida, o Gerenciador de recursos do servidor de arquivos gera automaticamente cotas que se baseiam no modelo. Cotas são geradas para cada uma das subpastas existentes e subpastas que você criar no futuro.

Por exemplo, você pode definir uma cota de aplicação automática de subpastas que são criadas por demanda, para usuários de perfil móvel ou para novos usuários. Sempre que uma subpasta é criada, uma nova entrada de cota é gerada automaticamente, usando o modelo da pasta pai. Essas entradas de cota automaticamente geradas podem ser vistas como cotas individuais no nó **Cotas**. Cada entrada de cota pode ser mantida separadamente.

## <a name="to-create-an-auto-apply-quota"></a>Para criar uma cota de aplicação automática

1.  Em **Gerenciamento de Cota**, clique no nó **Cotas**.

2.  Clique com o botão direito do mouse em **Cotas** e em **Criar Cota** (ou selecione **Criar Cota** no painel **Ações**). Essa ação abre a caixa de diálogo **Criar cota**.

3.  Em **Demarcador de cota**, digite o nome do ou navegue até a pasta pai onde o perfil de cota será aplicado. A cota de aplicação automática será aplicada a cada uma das subpastas (atuais e futuras) nesta pasta.

4.  Clique em **Aplicar modelo e criar cotas em subpastas novas e existentes automaticamente**.

5.  Em **Derivar propriedades deste modelo de cota**, selecione o modelo de cota que deseja aplicar na lista suspensa. Observe que as propriedades de cada modelo são exibidas em **Resumo das propriedades da cota**.

6.  Clique em **Criar**.

> [!Note]
> Você pode verificar todas as cotas geradas automaticamente, selecionando o nó **Cotas** e, em seguida, selecionando **Atualizar**. Uma cota individual para cada subpasta e o perfil de cota com aplicação automática no volume pai ou pasta são listados.

## <a name="see-also"></a>Veja também

-   [Gerenciamento de cota](quota-management.md)
-   [Editar propriedades de cota de aplicação automática](edit-auto-apply-quota-properties.md)