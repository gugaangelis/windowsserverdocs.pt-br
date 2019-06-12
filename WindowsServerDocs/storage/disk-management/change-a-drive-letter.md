---
title: Alterar uma letra de unidade
description: Como alterar ou atribuir uma letra de unidade no Windows usando o gerenciamento de disco.
ms.date: 10/24/2018
ms.prod: windows-server-threshold
ms.technology: storage
ms.topic: article
author: JasonGerend
manager: brianlic
ms.author: jgerend
ms.openlocfilehash: b972ab05c192dca9a9a0a2bda4f083d2906acadb
ms.sourcegitcommit: 6ef4986391607bb28593852d06cc6645e548a4b3
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/07/2019
ms.locfileid: "66812476"
---
# <a name="change-a-drive-letter"></a>Alterar uma letra de unidade

> **Aplica-se a:** Windows 10, Windows 8.1, Windows 7, Windows Server (canal semestral), Windows Server 2019, Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Se você não gostar a letra da unidade atribuída a uma unidade, ou se você tem uma unidade que ainda não tem uma letra de unidade, você pode usar o gerenciamento de disco para alterá-lo.

> [!IMPORTANT]
> Se você alterar a letra da unidade de uma unidade em que o Windows ou aplicativos são instalados, aplicativos podem ter problemas em execução ou localizar essa unidade. Por esse motivo, sugerimos que você não altere a letra da unidade de uma unidade em que o Windows ou aplicativos estão instalados.

Aqui está como alterar a letra da unidade (em vez disso, para montar a unidade em um vazio pasta para que ele apareça como qualquer outra pasta, consulte [atribua um caminho de pasta do ponto de montagem para uma unidade](assign-a-mount-point-folder-path-to-a-drive.md)).

1. Abra o gerenciamento de disco com permissões de administrador. 
    Para fazer isso, na caixa de pesquisa na barra de tarefas, digite **gerenciamento de disco**, selecione e mantenha (ou clique com botão direito) **gerenciamento de disco**, em seguida, selecione **executar como administrador**  >  **Sim**. Se você não é possível abri-lo como um administrador, digite **gerenciamento do computador** em vez disso e, em seguida, vá para **armazenamento** > **gerenciamento de disco**.
1. No gerenciamento de disco, clique com botão direito a unidade para o qual você deseja alterar ou adicionar uma letra de unidade e, em seguida, selecione **Alterar letra de unidade e caminhos**.

    ![Exibindo uma unidade de gerenciamento de disco](media/change-drive-letter.png)
    > [!TIP]
    > Se você não vir as **Alterar letra de unidade e caminhos** opção ou ele está esmaecido, é possível o volume não está pronto para receber uma letra de unidade, que pode ser o caso se a unidade não está alocada e precisa ser [inicializado](initialize-new-disks.md). Ou, talvez ele não foi projetado para ser acessado, que é o caso de partições do sistema EFI e partições de recuperação. Se você confirmou que você tem um volume formatado com uma letra de unidade que você pode acessar e se você ainda não é possível alterá-lo, infelizmente neste tópico provavelmente não pode ajudá-lo, portanto, sugerimos [entrando em contato com o Microsoft](https://support.microsoft.com/contactus/) ou fabricante do seu PC para obter mais ajuda.

1. Para alterar a letra da unidade, selecione **alterar**. Para adicionar uma letra de unidade se a unidade ainda não tiver um, selecione **adicionar**.

    ![A caixa de diálogo Alterar letra de unidade e caminhos](media/change-drive-letter2.png)
1. Selecione a nova letra de unidade, selecione **Okey**e, em seguida, selecione **Sim** quando solicitado sobre como os programas que utilizam a letra da unidade podem não funcionar corretamente.

    ![A caixa de diálogo Alterar letra de unidade ou caminho mostrando a alterar a letra da unidade](media/change-drive-letter3.png)