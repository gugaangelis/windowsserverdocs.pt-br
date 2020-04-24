---
title: Alterar a letra da unidade
description: Como alterar ou atribuir uma letra da unidade no Windows usando o Gerenciamento de disco.
ms.date: 10/24/2018
ms.prod: windows-server
ms.technology: storage
ms.topic: article
author: JasonGerend
manager: brianlic
ms.author: jgerend
ms.openlocfilehash: 3e18092a71e12cadb86052204738fafc8a149ff4
ms.sourcegitcommit: 3a3d62f938322849f81ee9ec01186b3e7ab90fe0
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/23/2020
ms.locfileid: "71386025"
---
# <a name="change-a-drive-letter"></a>Alterar a letra da unidade

> **Aplica-se a:** Windows 10, Windows 8.1, Windows 7, Windows Server (Canal Semestral), Windows Server 2019, Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Se você não gostar a letra da unidade atribuída a uma unidade, ou se tiver uma unidade que ainda não tem uma letra da unidade, é possível usar o Gerenciamento de disco para alterá-la.

> [!IMPORTANT]
> Se você alterar a letra da unidade de uma unidade em que o Windows ou aplicativos são instalados, os aplicativos podem ter problemas para executar ou localizar essa unidade. Por esse motivo, sugerimos que você não altere a letra da unidade de uma unidade em que o Windows ou aplicativos estão instalados.

Aqui está como alterar a letra da unidade (para, em vez disso, montar a unidade em uma pasta vazia, de modo que ela apareça assim como qualquer outra pasta, consulte [Atribuir um caminho de pasta do ponto de montagem para uma unidade](assign-a-mount-point-folder-path-to-a-drive.md)).

1. Abra o Gerenciamento de disco com permissões de administrador. 
    Para fazer isso, na caixa de pesquisa na barra de tarefas, digite **Gerenciamento de disco**, selecione e mantenha o cursor (ou clique com botão direito) em **Gerenciamento de disco** e, em seguida, selecione **Executar como administrador** > **Sim**. Se não for possível abrir como um administrador, digite **Gerenciamento do computador** e, em seguida, vá para **Armazenamento** > **Gerenciamento de Disco**.
1. No Gerenciamento de disco, clique com botão direito na unidade que deseja alterar ou adicionar uma letra de unidade e, em seguida, selecione **Alterar letra de unidade e caminho**.

    ![Gerenciamento de disco exibindo uma unidade](media/change-drive-letter.png)
    > [!TIP]
    > Se você não visualizar a opção **Alterar letra de unidade e caminho** ou se ela estiver esmaecida, é possível o volume não esteja pronto para receber uma letra de unidade, o que pode ser o caso se a unidade não estiver alocada e precisa ser [Inicializada](initialize-new-disks.md). Ou, talvez a unidade não foi projetada para ser acessada, que é o caso de partições de sistema EFI e partições de recuperação. Se você confirmou que tem um volume formatado com uma letra da unidade que seja possível acessar e mesmo assim não consegue alterá-la, infelizmente este tópico provavelmente não poderá ajudá-lo, portanto, sugerimos [Entrar em contato com a Microsoft](https://support.microsoft.com/contactus/) ou o fabricante do seu PC para obter mais ajuda.

1. Para alterar a letra da unidade, selecione **Alterar**. Para adicionar uma letra da unidade se a unidade ainda não tiver uma, selecione **Adicionar**.

    ![A caixa de diálogo Alterar letra de unidade e caminho](media/change-drive-letter2.png)
1. Escolha a nova letra da unidade, selecione **OK** e, em seguida, selecione **Sim** quando solicitado sobre como os programas que utilizam a letra da unidade podem não funcionar corretamente.

    ![A caixa de diálogo Alterar letra de unidade ou caminho mostrando a alteração da letra da unidade](media/change-drive-letter3.png)