---
title: Criar uma tarefa de gerenciamento de arquivo personalizada
description: Este artigo descreve como criar uma tarefa de gerenciamento de arquivo personalizado e tarefas personalizadas.
ms.date: 7/7/2017
ms.prod: windows-server
ms.technology: storage
ms.topic: article
author: JasonGerend
manager: brianlic
ms.author: jgerend
ms.openlocfilehash: 894c4e3c0b9fa0fde7b749e6effce531c3f999bb
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71403174"
---
# <a name="create-a-custom-file-management-task"></a>Criar uma tarefa de gerenciamento de arquivo personalizada

> Aplica-se a: Windows Server (canal semestral), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012, Windows Server 2008 R2

Expiração nem sempre é uma ação desejada para ser executada nos arquivos. As tarefas de gerenciamento de arquivo também permitem que você execute comandos personalizados.

> [!Note]
> Este procedimento pressupõe que você está familiarizado com as tarefas de gerenciamento de arquivo e capas, portanto, apenas a guia **Ação** onde as configurações personalizadas são configuradas.

## <a name="to-create-a-custom-task"></a>Para criar uma tarefa personalizada

1.  Clique no nó **Tarefas de Gerenciamento de Arquivos**.

2.  Clique com botão direito em **Tarefas de gerenciamento de arquivo** e clique em **Criar tarefas de gerenciamento de arquivo** (ou clique em **Criar tarefas de gerenciamento de arquivo** no painel **Ações**). A caixa de diálogo **Criar Tarefa de Gerenciamento de Arquivos** é aberta.

3.  Na guia **Ação**, insira as seguintes informações:

    -   **Tipo**. Selecione **Personalizar** no menu suspenso.
    -   **Executável**. Digite ou procure por um comando para executar quando a tarefa de gerenciamento de arquivo processa os arquivos. Esse executável deve ser definido para ser gravável apenas pelos Administradores e Sistema. Se nenhum outro usuário tiver acesso ao executável, ele não rodará corretamente.
    -   **Configurações de comandos**. Para configurar os argumentos passados para o executável quando um trabalho de gerenciamento de arquivo processa arquivos, edite a caixa de texto identificada como **Argumentos**. Para inserir variáveis adicionais no texto, coloque o cursor no local na caixa de texto onde você deseja inserir a variável, selecione a variável que deseja inserir e clique em **Inserir variável**. O texto entre colchetes insere informações variáveis que o executável pode receber. Por exemplo, a variável de caminho de arquivo \[Source @ no__t-1 insere o nome do arquivo que deve ser processado pelo executável. Opcionalmente, clique no botão **Diretório de trabalho** para especificar o local do executável personalizado.
    -   **Comando de segurança**. Configure as configurações de segurança deste executável. Por padrão, o comando é executado como Serviço Local, que é a conta mais restritiva disponível.

4.  Clique em **OK**.

## <a name="see-also"></a>Consulte também

-   [Gerenciamento de classificação](classification-management.md)
-   [Tarefas de gerenciamento de arquivos](file-management-tasks.md)