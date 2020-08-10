---
title: Visão geral do Gerenciamento de disco
description: Gerenciamento de disco é um utilitário do sistema no Windows que permite a você executar tarefas avançadas de armazenamento, como inicializar uma nova unidade, estender volumes, reduzir partições e alterar letras da unidade.
ms.date: 06/07/2019
ms.topic: article
author: JasonGerend
manager: brianlic
ms.author: jgerend
ms.openlocfilehash: 333b8f5676fbb83a16a8e56249701135eabd7d4a
ms.sourcegitcommit: dfa48f77b751dbc34409aced628eb2f17c912f08
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/07/2020
ms.locfileid: "87935941"
---
# <a name="overview-of-disk-management"></a>Visão geral do Gerenciamento de disco

> **Aplica-se a:** Windows 10, Windows 8.1, Windows 7, Windows Server (Canal Semestral), Windows Server 2019, Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Gerenciamento de disco é um utilitário do sistema no Windows que permite executar tarefas avançadas de armazenamento. Aqui estão algumas boas utilidades do Gerenciamento de disco:

- Configurar uma nova unidade, consulte [Inicializar uma nova unidade](initialize-new-disks.md).
- Estender um volume para o espaço que não ainda faz parte de um volume na mesma unidade, consulte [Estender um volume básico](extend-a-basic-volume.md).
- Reduzir uma partição, geralmente, para que você possa estender uma partição vizinha, consulte [Reduzir um volume básico](shrink-a-basic-volume.md).
- Alterar uma letra da unidade ou atribuir uma nova letra da unidade, consulte [Alterar a letra da unidade](change-a-drive-letter.md).

![Gerenciamento de disco mostrando uma unidade típica com três partições: uma partição de sistema de 499 MB, uma unidade C maior para o Windows e outra partição de 499 MB para recuperação](media/disk-management.png)

> [!TIP]
>  Se você receber um erro ou se algo não funcionar ao executar estes procedimentos, dê uma olhada no tópico [Solução de problemas de gerenciamento de disco](troubleshooting-disk-management.md). Se nada disso ajudar, não se desespere! Há uma tonelada de informações no site [Microsoft Community](https://answers.microsoft.com/en-us/windows): tente pesquisar a seção [Arquivos, pastas e armazenamento](https://answers.microsoft.com/en-us/windows/forum/windows_10-files?sort=lastreplydate&dir=desc&tab=All&status=all&mod=&modAge=&advFil=&postedAfter=&postedBefore=&threadType=all&isFilterExpanded=true&tm=1514405359639) e, se você ainda precisar de ajuda, publique uma pergunta lá e a Microsoft ou outros membros da comunidade tentarão ajudar. Se você tiver comentários sobre como melhorar esses tópicos, queremos muito ouvir sua opinião! Apenas responda à solicitação *Esta página foi útil?* e deixe comentários lá ou no thread de comentários públicos na parte inferior deste tópico.

Seguem algumas tarefas comuns que você talvez queira executar, mas que usam outras ferramentas no Windows:

- Para liberar espaço em disco, consulte [Liberar espaço da unidade no Windows 10](https://support.microsoft.com/help/12425/windows-10-free-up-drive-space).
- Para desfragmentar as unidades, consulte [Desfragmentar o PC com Windows 10](https://support.microsoft.com/help/4026701/windows-defragment-your-windows-10-pc).
- Para selecionar vários discos rígidos e reuni-los em um pool, como em um RAID, consulte [Espaços de armazenamento](https://support.microsoft.com/help/12438/windows-10-storage-spaces).

## <a name="about-those-extra-recovery-partitions"></a>Sobre essas partições de recuperação adicionais

Caso esteja curioso (lemos os seus comentários!), o Windows normalmente inclui três partições na unidade principal (normalmente a unidade C:\):

![Disco 0 mostrando três partições - uma partição de sistema EFI, a partição do Windows e uma partição de recuperação](media/windows-partitions.png)

- **Partição de sistema EFI** - Usada por PCs modernos para iniciar (boot) o PC e o sistema operacional.
- **Unidade do sistema operacional Windows (C:)** - Onde o Windows está instalado e geralmente o local em que você armazena o restante dos aplicativos e arquivos.
- **Partição de recuperação** - Onde as ferramentas especiais são armazenadas para ajudar a recuperar o Windows, caso ocorram problemas de inicialização ou outros problemas graves.

Embora o gerenciamento de disco possa mostrar a partição de sistema EFI e a partição de recuperação como 100% livre, isso não é verdade. Essas partições ficam geralmente bem cheias de arquivos realmente importantes que o PC precisa para operar corretamente. É melhor deixá-las como estão para realizar os trabalhos delas, iniciando seu PC e ajudando você a se recuperar de problemas.

## <a name="additional-references"></a>Referências adicionais

- [Gerenciar discos](manage-disks.md)
- [Gerenciar volumes básicos](manage-basic-volumes.md)
- [Solução de problemas do Gerenciamento de disco](troubleshooting-disk-management.md)
- [Opções de recuperação no Windows 10](https://support.microsoft.com/help/12415/windows-10-recovery-options)
- [Localizar arquivos perdidos após a atualização para o Windows 10](https://support.microsoft.com/help/12386/windows-10-find-lost-files-after-update)
- [Fazer backup e restaurar os arquivos](https://support.microsoft.com/help/17143/windows-10-back-up-your-files)
- [Criar uma unidade de recuperação](https://support.microsoft.com/help/4026852/windows-create-a-recovery-drive)
- [Criar um ponto de restauração do sistema](https://support.microsoft.com/help/4027538/windows-create-a-system-restore-point)
- [Localizar minha chave de recuperação do BitLocker](https://support.microsoft.com/help/4026181/windows-find-my-bitlocker-recovery-key)
