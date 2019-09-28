---
title: Criar uma triagem de arquivo
description: Este artigo descreve como criar uma triagem de arquivo
ms.date: 7/7/2017
ms.prod: windows-server
ms.technology: storage
ms.topic: article
author: JasonGerend
manager: brianlic
ms.author: jgerend
ms.openlocfilehash: e049162e7aff449774928d6a1d25cc1116f9aee9
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71403161"
---
# <a name="create-a-file-screen"></a>Criar uma triagem de arquivo

> Aplica-se a: Windows Server (canal semestral), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012, Windows Server 2008 R2

Ao criar uma nova triagem de arquivo, você pode optar por salvar um modelo de triagem de arquivo com base nas propriedades de triagem de arquivo personalizado que você definir. A vantagem disso é que um link é mantido entre triagens de arquivo e o modelo é usado para criá-los, para que no futuro, as alterações no modelo podem ser aplicadas a todas as triagens de arquivo derivados dele. Esse é um recurso que simplifica a implementação das alterações de políticas de armazenamento, fornecendo um ponto central no qual todas as atualizações podem ser feitas.

## <a name="to-create-a-file-screen-with-custom-properties"></a>Para criar uma triagem de arquivo com propriedades personalizadas

1.  Em **Gerenciamento de triagem de arquivo**, clique no nó **Triagens de arquivo**.

2.  Clique com botão direito em **Triagens de arquivo**e clique em **Criar triagem de arquivo** (ou selecione **Criar triagem de arquivo** no painel **Ações**). A caixa de diálogo **Crie triagem de arquivo** é aberta.

3.  Em **Demarcados de triagem de arquivo**, digite o nome do ou navegue até a pasta onde a triagem de arquivo será aplicada. A triagem de arquivo será aplicada para a pasta selecionada e todas as suas subpastas.

4.  Em **Como você deseja configurar propriedades de triagem de arquivo**, clique em **Definir as propriedades de triagem de arquivo personalizado** e clique em **Propriedades personalizadas**. A caixa de diálogo **Propriedades de triagem de arquivo** é aberta.

5.  Se você quiser copiar as propriedades de um modelo existente para usar como base para uma triagem de arquivo, selecione um modelo na lista suspensa **Copiar propriedades do modelo**. Em seguida, clique em **Copiar**.

    Na caixa de diálogo **Propriedades de triagem de arquivo**, modifique ou defina os seguintes valores na guia **Configurações**:

6.  Em **Tipo de triagem**, clique na opção **Triagem ativa** ou **Triagem passiva**. (Triagem ativa impede que os usuários salvem arquivos que são membros de grupos de arquivos bloqueados e gera notificações quando os usuários tentam salvar arquivos não autorizados. A triagem passiva envia notificações configuradas, mas ela não impede que os usuários salvando arquivos.)

7.  Em **Grupos de arquivos**, selecione cada grupo de arquivos que você deseja incluir em sua triagem de arquivo. (Para marcar a caixa de seleção do grupo de arquivos, clique duas vezes no rótulo do grupo de arquivos.)

    Se você quiser exibir os tipos de arquivo que um grupo de arquivos inclui e exclui, clique no rótulo do grupo de arquivos e clique em **Editar**. Para criar um novo grupo de arquivos, clique em **criar**.

8.  Além disso, você pode configurar **Gerenciador de recursos do servidor de arquivos** para gerar uma ou mais notificações definindo opções nas guias **Email**, **Log de eventos**, **Comando** e **Relatório**. Para obter mais informações sobre opções de notificação de triagem de arquivo, consulte [Criar um modelo de triagem de arquivo](create-file-screen-template.md).

9.  Após selecionar todas as propriedades da triagem de arquivo que deseja usar, clique em **OK** para fechar a caixa de diálogo **Propriedades de triagem de arquivo**.

10. Na caixa de diálogo **Criar triagem de arquivo**, clique em **Criar** para salvar a triagem de arquivo. Essa ação abre a caixa de diálogo **Salvar propriedades personalizadas como modelo**.

11. Selecione o tipo de triagem de arquivo personalizada que deseja criar:

    -   Para salvar um modelo com base nessas propriedades personalizadas (recomendado), clique em **Salvar as propriedades personalizadas como um modelo** e insira um nome para o modelo. Essa opção aplicará o modelo para a nova triagem de arquivo, e você pode usar o modelo para criar triagens de arquivos adicionais no futuro. Isso permitirá que você atualize posteriormente as triagens de arquivo automaticamente, atualizando o modelo.
    -   Se você não quiser salvar um modelo quando você salvar a triagem de arquivo, clique em **Salvar a triagem de arquivo personalizado sem criar um modelo**.

12. Clique em **OK**.

## <a name="see-also"></a>Consulte também

-   [Gerenciamento de triagem de arquivos](file-screening-management.md)
-   [Definir grupos de arquivos para triagem](define-file-groups-for-screening.md)
-   [Criar um modelo de triagem de arquivo](create-file-screen-template.md)
-   [Editar propriedades de modelo de triagem de arquivo](edit-file-screen-template-properties.md)


