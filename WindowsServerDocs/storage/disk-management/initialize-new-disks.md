---
title: Inicializar novos discos
description: Como inicializar novos discos com o Gerenciamento de Disco, preparando-os para uso. Também inclui links para solução de problemas.
ms.date: 12/20/2019
ms.prod: windows-server
ms.technology: storage
ms.topic: article
author: JasonGerend
manager: brianlic
ms.author: jgerend
ms.openlocfilehash: 68e51cff5b70ed0b11488e44cebba057e7432d99
ms.sourcegitcommit: d5e27c1f2f168a71ae272bebf8f50e1b3ccbcca3
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/23/2020
ms.locfileid: "86965268"
---
# <a name="initialize-new-disks"></a>Inicializar novos discos

> **Aplica-se a:** Windows 10, Windows 8.1, Windows 7, Windows Server (Canal Semestral), Windows Server 2019, Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Se você adicionar um novo disco ao computador e ele não aparecer no Explorador de Arquivos, talvez seja necessário [adicionar uma letra de unidade](change-a-drive-letter.md), ou inicializá-lo antes do uso. Somente é possível inicializar uma unidade que ainda não está formatada. Inicializar um disco apaga tudo contido nele e o prepara para uso pelo Windows, sendo posteriormente possível formatá-lo e, em seguida, armazenar arquivos nele.

> [!WARNING]
> Se o disco já contiver arquivos que sejam importantes para você, não o inicialize, pois perderá todos os arquivos. Em vez disso, recomendamos você solucione problemas no disco para ver se é possível ler os arquivos. Consulte [O status de um disco é Não inicializado ou o disco está ausente inteiramente](troubleshooting-disk-management.md#disks-that-are-missing-or-not-initialized-plus-general-troubleshooting-steps).

## <a name="to-initialize-new-disks"></a>Para inicializar novos discos

Aqui está como inicializar um novo disco usando o Gerenciamento de Disco. Se você preferir usar o PowerShell, use o cmdlet [initialize-disk](/powershell/module/storage/initialize-disk).

1. Abra o Gerenciamento de Disco com permissões de administrador.
 
    Para fazer isso, na caixa de pesquisa na barra de tarefas, digite **Gerenciamento de disco**, selecione e mantenha o cursor (ou clique com botão direito) em **Gerenciamento de disco** e, em seguida, selecione **Executar como administrador** > **Sim**. Se não for possível abrir como um administrador, digite **Gerenciamento do computador** e, em seguida, vá para **Armazenamento** > **Gerenciamento de disco**.
1. No Gerenciamento de Disco, clique com o botão direito do mouse no disco que você quer inicializar e clique em **Inicializar disco** (mostrado aqui). Se o disco estiver listado como *Offline*, primeiro clique duas vezes nele e selecione **Online**.

     Observe que algumas unidades USB não tem a opção de ser inicializada, elas apenas são formatadas e recebem uma [letra da unidade](change-a-drive-letter.md).

    ![Gerenciamento de Disco mostrando um disco não formatado com o menu de atalho Inicializar disco exibido](media/uninitialized-disk.PNG)
2. Na caixa de diálogo **Inicializar disco** (mostrada aqui), verifique para certificar-se de que o disco correto está selecionado e, em seguida, clique em **OK** para aceitar o estilo de partição padrão. Se for necessário alterar o estilo de partição (GPT ou MBR), consulte [Sobre os estilos de partição - GPT e MBR](#about-partition-styles---gpt-and-mbr).

     O status do disco altera brevemente para o status **Inicializando** e, em seguida, para **Online**. Se a inicialização falhar por algum motivo, consulte [O status de um disco é Não inicializado ou o disco está ausente inteiramente](troubleshooting-disk-management.md#disks-that-are-missing-or-not-initialized-plus-general-troubleshooting-steps).

    ![A caixa de diálogo Inicializar disco com o estilo de partição GPT selecionado](media/initialize-disk.PNG)

3. Selecione e mantenha o cursor (ou clique com o botão direito do mouse) no espaço não alocado na unidade e, em seguida, selecione **Novo Volume Simples**.
4. Selecione **Avançar**, especifique o tamanho do volume (talvez você queira ficar com o padrão, que usa a unidade inteira) e, em seguida, selecione **Avançar**.
5. Especifique a letra da unidade que você deseja atribuir ao volume e, em seguida, selecione **Avançar**.
6. Especifique o sistema de arquivos que você deseja usar (geralmente NTFS), selecione **Avançar** e **Concluir**.

## <a name="about-partition-styles---gpt-and-mbr"></a>Sobre os estilos de partição - GPT e MBR

Discos podem ser divididos em várias partes chamadas partições. Cada partição, mesmo se você tiver apenas uma, deve ter um estilo de partição - GPT ou MBR. O Windows usa o estilo de partição para entender como acessar os dados no disco.

Tão fascinante como isso possa parecer, o resultado final é que hoje em dia, você normalmente não precisa se preocupar com o estilo de partição, pois o Windows usa automaticamente o tipo de disco apropriado.

A maioria dos PCs usa o tipo de disco GPT (Tabela de Partição GUID) para discos rígidos e SSDs. GPT é mais robusto e permite usar volumes maiores que 2 TB. O tipo de disco MBR (Registro Mestre de Inicialização), mais antigo, é usado por PCs de 32 bits, PCs mais antigos e unidades removíveis, como cartões de memória.

Para converter um disco de MBR em GPT, ou vice-versa, primeiro é necessário excluir todos os volumes do disco, apagando tudo que está armazenado no disco. Para obter mais informações, consulte [Converter um disco MBR em um disco GPT](change-an-mbr-disk-into-a-gpt-disk.md) ou [Converter um disco GPT em um disco MBR](change-a-gpt-disk-into-an-mbr-disk.md).
