---
title: Alterar a letra da unidade
description: Como alterar ou atribuir uma letra da unidade no Windows usando o Gerenciamento de disco.
ms.date: 06/08/2020
ms.prod: windows-server
ms.technology: storage
ms.topic: article
author: JasonGerend
manager: brianlic
ms.author: jgerend
ms.openlocfilehash: 424fa2c81954e022d7e1b297a60bd3415fdafa39
ms.sourcegitcommit: a538474d2c0a9520567f4e6ad0933f8660273098
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/08/2020
ms.locfileid: "84505768"
---
# <a name="change-a-drive-letter"></a>Alterar a letra da unidade

> **Aplica-se a:** Windows 10, Windows 8.1, Windows 7, Windows Server 2019, Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Se você não gostar a letra da unidade atribuída a uma unidade, ou se tiver uma unidade que ainda não tem uma letra da unidade, é possível usar o Gerenciamento de disco para alterá-la. Para montar a unidade em uma pasta vazia, assim ela aparece como apenas outra pasta, confira [Montar uma unidade em uma pasta](assign-a-mount-point-folder-path-to-a-drive.md).

> [!IMPORTANT]
> Se você alterar a letra da unidade de uma unidade em que o Windows ou aplicativos são instalados, os aplicativos podem ter problemas para executar ou localizar essa unidade. Por esse motivo, sugerimos que você não altere a letra da unidade de uma unidade em que o Windows ou aplicativos estão instalados.

Veja como alterar a letra da unidade:

1. Abra o Gerenciamento de Disco com permissões de administrador.
    Para isso, selecione e mantenha o cursor (ou clique com o botão direito do mouse) no botão Iniciar e selecione **Gerenciamento de Disco**.
1. No Gerenciamento de Disco, selecione e mantenha o cursor (ou clique com o botão direito do mouse) no volume que você deseja alterar ou adicionar uma letra da unidade e escolha **Alterar letra da unidade e caminhos**.

    ![Gerenciamento de disco exibindo uma unidade](media/change-drive-letter.png)
    > [!TIP]
    > Se você não visualizar a opção **Alterar letra de unidade e caminho** ou se ela estiver esmaecida, é possível o volume não esteja pronto para receber uma letra de unidade, o que pode ser o caso se a unidade não estiver alocada e precisa ser [Inicializada](initialize-new-disks.md). Ou, talvez a unidade não foi projetada para ser acessada, que é o caso de partições de sistema EFI e partições de recuperação. Se você confirmou que tem um volume formatado com uma letra da unidade que seja possível acessar e mesmo assim não consegue alterá-la, infelizmente este tópico provavelmente não poderá ajudá-lo, portanto, sugerimos [Entrar em contato com a Microsoft](https://support.microsoft.com/contactus/) ou o fabricante do seu PC para obter mais ajuda.

1. Para alterar a letra da unidade, selecione **Alterar**. Para adicionar uma letra da unidade se a unidade ainda não tiver uma, selecione **Adicionar**.

    ![A caixa de diálogo Alterar letra de unidade e caminho](media/change-drive-letter2.png)
1. Escolha a nova letra da unidade, selecione **OK** e, em seguida, selecione **Sim** quando solicitado sobre como os programas que utilizam a letra da unidade podem não funcionar corretamente.

    ![A caixa de diálogo Alterar letra de unidade ou caminho mostrando a alteração da letra da unidade](media/change-drive-letter3.png)
