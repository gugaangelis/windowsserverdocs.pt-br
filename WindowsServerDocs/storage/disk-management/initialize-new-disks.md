---
title: Initialize novos discos
description: Como inicializar novos discos com o gerenciamento de disco, fazendo com que eles prontos para uso. Também inclui links para solução de problemas.
ms.date: 10/24/2018
ms.prod: windows-server-threshold
ms.technology: storage
ms.topic: article
author: JasonGerend
manager: brianlic
ms.author: jgerend
ms.openlocfilehash: 61c8eb2321ee2a282345aba01c6d04a128f0448c
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59819387"
---
# <a name="initialize-new-disks"></a>Initialize novos discos

> **Aplica-se a:** Windows 10, Windows 8.1, Windows 7, Windows Server (canal semestral), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Se você adicionar um novo disco a seu computador e ele não apareça no Explorador de arquivos, você talvez precise [adicionar uma letra de unidade](change-a-drive-letter.md), ou inicializá-lo antes de usá-lo. Você só pode inicializar uma unidade que ainda não está formatada. Inicializar um disco apaga tudo nele e o prepara para uso pelo Windows, após o qual você pode formatá-la e, em seguida, armazenar arquivos nele.

> [!WARNING]
> Se o disco já tiver arquivos que você se preocupa, não inicializá-lo – você perderá todos os arquivos. Em vez disso, recomendamos que o disco de solução de problemas para ver se você pode ler os arquivos - consulte [é de status de um disco não inicializado ou o disco está ausente inteiramente](troubleshooting-disk-management.md#disk-not-initialized).

## <a name="to-initialize-new-disks"></a>Para inicializar novos discos

Aqui está como inicializar um novo disco usando o gerenciamento de disco. Se você preferir usar o PowerShell, use o [initialize-disk](https://docs.microsoft.com/powershell/module/storage/initialize-disk) cmdlet em vez disso.

1. Abra o gerenciamento de disco com permissões de administrador. <br>Para fazer isso, na caixa de pesquisa na barra de tarefas, digite **gerenciamento de disco**, selecione e mantenha (ou clique com botão direito) **gerenciamento de disco**, em seguida, selecione **executar como administrador**  >  **Sim**. Se você não é possível abri-lo como um administrador, digite **gerenciamento do computador** em vez disso e, em seguida, vá para **armazenamento** > **gerenciamento de disco**.
1. No gerenciamento de disco, clique no disco que você deseja inicializar e, em seguida, clique em **inicializar disco** (mostrados aqui). Se o disco estiver listado como *Offline*, primeiro clique duas vezes e selecione **Online**.<br>Observe que algumas unidades USB não tem a opção a ser inicializada, eles apenas obterem formatados e um [letra da unidade](change-a-drive-letter.md).

    ![Gerenciamento de disco mostrando um disco formatado com o menu de atalho inicializar disco exibido](media\uninitialized-disk.PNG)
2. No **inicializar disco** caixa de diálogo (mostrados aqui), a verificação para certificar-se de que o disco correto está selecionado e, em seguida, clique em **Okey** para aceitar o estilo de partição padrão. Se você precisar alterar a partição estilo (GPT ou MBR), consulte [sobre o estilo de partição - GPT e MBR](#about-partition-styles-GPT-and-MBR).<br>O status do disco se altera brevemente para **Inicializando** e, em seguida, para o **Online** status. Se a inicialização falhar por algum motivo, consulte [é de status de um disco não inicializado ou o disco está ausente inteiramente](troubleshooting-disk-management.md#disk-not-initialized).

    ![A caixa de diálogo Inicializar disco com o estilo de partição GPT selecionado](media\initialize-disk.PNG)

## <a name="about-partition-styles---gpt-and-mbr"></a>Sobre os estilos de partição - GPT e MBR

Discos podem ser divididos em várias partes chamadas partições. Cada partição – mesmo se você tiver apenas um - deve ter um estilo de partição - GPT ou MBR. Windows usa o estilo de partição para compreender como acessar os dados no disco.

Tão fascinante como isso provavelmente não é o resultado final é que hoje em dia, você normalmente não precisa se preocupar sobre o estilo de partição - Windows usa automaticamente o tipo de disco apropriado.

A maioria dos PCs usam o tipo de disco de tabela de partição GUID (GPT) para discos rígidos e SSDs. GPT é mais robusta e permite para volumes maior que 2 TB. O tipo de disco mais antigo do registro mestre de inicialização (MBR) é usado por computadores de 32 bits, PCs mais antigos e unidades removíveis, como cartões de memória.

Para converter um disco de MBR para GPT ou vice-versa, você primeiro precisa excluir todos os volumes do disco, apagando tudo no disco. Para obter mais informações, consulte [converter um disco MBR em um disco GPT](change-an-mbr-disk-into-a-gpt-disk.md), ou [converter um disco GPT em um disco MBR](change-a-gpt-disk-into-an-mbr-disk.md).