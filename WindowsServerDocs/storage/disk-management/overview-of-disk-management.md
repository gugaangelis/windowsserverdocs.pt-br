---
title: Visão geral do Gerenciamento de Disco
description: Gerenciamento de disco é um utilitário do sistema no Windows que permite que você execute tarefas de armazenamento avançado, como inicializar uma nova unidade, extensão dos volumes, reduzindo as partições e alterar as letras de unidade.
ms.date: 06/07/2019
ms.prod: windows-server-threshold
ms.technology: storage
ms.topic: article
author: JasonGerend
manager: brianlic
ms.author: jgerend
ms.openlocfilehash: a3885ae6b09ad431fd1ea5e4c593e02c7bb274d9
ms.sourcegitcommit: 6ef4986391607bb28593852d06cc6645e548a4b3
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/07/2019
ms.locfileid: "66812552"
---
# <a name="overview-of-disk-management"></a>Visão geral do Gerenciamento de Disco

> **Aplica-se a:** Windows 10, Windows 8.1, Windows 7, Windows Server (canal semestral), Windows Server 2019, Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Gerenciamento de disco é um utilitário do sistema no Windows que permite que você execute tarefas de armazenamento avançado. Aqui estão algumas coisas que o gerenciamento de disco é bom para:

- Para configurar uma nova unidade, consulte [inicializar uma nova unidade](initialize-new-disks.md).
- Para estender um volume para o espaço que não ainda faz parte de um volume na mesma unidade, consulte [estender um volume básico](extend-a-basic-volume.md).
- Para reduzir uma partição, geralmente, de modo que você pode estender uma partição de vizinha, consulte [encolher um volume básico](shrink-a-basic-volume.md).
- Para alterar uma letra de unidade ou atribuir uma nova letra de unidade, consulte [alterar uma letra de unidade](change-a-drive-letter.md).

![Gerenciamento de disco mostrando uma unidade típica com três partições - uma partição de sistema 499 MB, uma maior unidade C para Windows e outra partição 499 MB para a recuperação](media/disk-management.png)

> [!TIP]
>  Se você receber um erro ou se algo não funciona ao executar estes procedimentos, dê uma espiada na [solução de problemas de gerenciamento de disco](troubleshooting-disk-management.md) tópico. Se isso não ajudar - não entre em pânico! Há uma tonelada de informações o [comunidade da Microsoft](https://answers.microsoft.com/en-us/windows) do site - tente pesquisar o [arquivos, pastas e armazenamento](https://answers.microsoft.com/en-us/windows/forum/windows_10-files?sort=lastreplydate&dir=desc&tab=All&status=all&mod=&modAge=&advFil=&postedAfter=&postedBefore=&threadType=all&isFilterExpanded=true&tm=1514405359639) seção e, se você ainda precisar de Ajuda, poste uma pergunta lá e a Microsoft ou outros membros do comunidade tentará ajudar. Se você tiver comentários sobre como melhorar esses tópicos, adoraríamos ouvir sua opinião! Responder apenas a *esta página é útil?* prompt e deixar comentários lá ou no thread de comentários público na parte inferior deste tópico.

Aqui estão algumas tarefas comuns que você talvez queira fazer, mas que usam outras ferramentas no Windows:

- Para liberar espaço em disco, consulte [liberar espaço de unidade no Windows 10](https://support.microsoft.com/help/12425/windows-10-free-up-drive-space).
- Para desfragmentar unidades, consulte [desfragmente seu PC com Windows 10](https://support.microsoft.com/help/4026701/windows-defragment-your-windows-10-pc).
- Para levar a vários discos rígidos e pool-los juntos, como em um RAID, consulte [espaços de armazenamento](https://support.microsoft.com/help/12438/windows-10-storage-spaces).

## <a name="about-those-extra-recovery-partitions"></a>Sobre essas partições de recuperação adicionais

Se você estiver curioso (estamos leu seus comentários!), o Windows normalmente incluem três partições na unidade principal (normalmente C:\ unidade):

![Disco 0 mostrando três partições - uma partição do sistema EFI, a partição do Windows e uma partição de recuperação](media/windows-partitions.png)

- **Partição do sistema EFI** -isso é usado por PCs modernos para iniciar (Inicializar) seu PC e seu sistema operacional.
- **Unidade do sistema operacional Windows (c)** -isso é onde o Windows está instalado e, geralmente onde você colocou o restante dos seus aplicativos e arquivos.
- **Partição de recuperação** -isso é onde as ferramentas especiais são armazenadas para ajudar a recuperar o Windows, caso tenha problemas ao iniciar ou de outros problemas graves.

Embora o gerenciamento de disco pode mostrar a partição do sistema EFI e a partição de recuperação como 100% livre, ele reside. Essas partições são geralmente muito cheia com arquivos realmente importantes, que seu PC precisa para operar corretamente. É melhor deixá-las sozinho para realizar seus trabalhos Iniciando seu PC e ajudando você a se recuperar de problemas.

## <a name="see-also"></a>Consulte também

- [Gerenciar discos](manage-disks.md)
- [Gerenciar volumes básicos](manage-basic-volumes.md)
- [Solução de problemas do Gerenciamento de disco](troubleshooting-disk-management.md)
- [Opções de recuperação no Windows 10](https://support.microsoft.com/help/12415/windows-10-recovery-options)
- [Localizar arquivos perdidos após a atualização para o Windows 10](https://support.microsoft.com/help/12386/windows-10-find-lost-files-after-update)
- [Fazer backup e restaurar os arquivos](https://support.microsoft.com/help/17143/windows-10-back-up-your-files)
- [Criar uma unidade de recuperação](https://support.microsoft.com/help/4026852/windows-create-a-recovery-drive)
- [Criar um ponto de restauração do sistema](https://support.microsoft.com/help/4027538/windows-create-a-system-restore-point)
- [Localizar minha chave de recuperação do BitLocker](https://support.microsoft.com/help/4026181/windows-find-my-bitlocker-recovery-key)
