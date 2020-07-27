---
title: Estender um volume básico
description: Você pode adicionar espaço a um volume existente no Windows, estendê-lo para um espaço vazio na unidade, mas apenas se o espaço vazio não tiver um volume nele (não alocado) e chegar imediatamente após o volume que você deseja estender, sem outros volumes intermediários. Este artigo descreve como fazer isso.
ms.date: 12/19/2019
ms.prod: windows-server
ms.technology: storage
ms.topic: article
author: JasonGerend
manager: brianlic
ms.author: jgerend
ms.openlocfilehash: 19b48bd74dad4e20780b41852afbee15f5ec1ade
ms.sourcegitcommit: d5e27c1f2f168a71ae272bebf8f50e1b3ccbcca3
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/23/2020
ms.locfileid: "86966098"
---
# <a name="extend-a-basic-volume"></a>Estender um volume básico

> **Aplica-se a:** Windows 10, Windows 8.1, Windows Server (Canal Semestral), Windows Server 2019, Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Você pode usar o Gerenciamento de Disco para adicionar espaço a um volume existente, estendê-lo para um espaço vazio na unidade, mas apenas se o espaço vazio não tiver um volume nele (não alocado) e chegar imediatamente após o volume que você deseja estender, sem outros volumes intermediários, conforme mostrado na imagem a seguir. O volume a ser estendido também deve ser formatado com os sistemas de arquivos NTFS ou ReFS.

:::image type="content" source="media/extend-volume-space-highlighted.png" alt-text="Gerenciamento de Disco mostrando o espaço livre em que um volume pode se estender.":::

## <a name="to-extend-a-volume-by-using-disk-management"></a>Para estender um volume usando o Gerenciamento de Disco

Veja como estender um volume para o espaço vazio imediatamente após o volume na unidade:

1. Abra o Gerenciamento de Disco com permissões de administrador.

   Para fazer isso facilmente, digite **Gerenciamento do Computador** na caixa de pesquisa na barra de tarefas, selecione e mantenha o cursor (ou clique com o botão direito do mouse) em **Gerenciamento do Computador** e, em seguida, selecione **Executar como administrador** > **Sim**. Depois que o Gerenciamento do Computador for aberto, acesse **Armazenamento** > **Gerenciamento de Disco**.
2. Selecione e mantenha o cursor (ou clique com o botão direito do mouse) no volume que você deseja estender e, em seguida, selecione **Estender Volume**.

   Se a opção **Estender Volume** estiver esmaecida, verifique o seguinte:
    - O Gerenciamento de Disco ou o Gerenciamento do Computador foi aberto com permissões de administrador
    - Há espaço não alocado diretamente após (à direita) do volume, conforme mostrado no gráfico acima. Se houver outro volume entre o espaço não alocado e o volume que você deseja estender, você poderá excluir o volume e todos os arquivos nele (faça backup ou mova todos os arquivos importantes primeiro), usar um aplicativo de particionamento de disco que não seja da Microsoft que pode mover volumes sem destruir dados ou ignorar a extensão do volume e criar um volume separado no espaço não alocado.
    - O volume é formatado com o sistema de arquivos NTFS ou ReFS. Outros sistemas de arquivos não podem ser estendidos. Portanto, você precisaria mover ou fazer backup dos arquivos no volume e, em seguida, formatar o volume com o sistema de arquivos NTFS ou ReFS.
    - Se o disco for maior do que 2 TB, verifique se ele está usando o esquema de particionamento GPT. Para usar mais de 2 TB em um disco, ele deve ser inicializado usando o esquema de particionamento GPT. Para converter em GPT, consulte [Reverter um disco MBR em um disco GPT](change-an-mbr-disk-into-a-gpt-disk.md).
    - Se você ainda não conseguir estender o volume, tente pesquisar no site [Microsoft Community – Arquivos, pastas e armazenamento](https://answers.microsoft.com/en-us/windows/forum/windows_10-files?sort=lastreplydate&dir=desc&tab=All&status=all&mod=&modAge=&advFil=&postedAfter=&postedBefore=&threadType=all&isFilterExpanded=true&tm=1514405359639) e, se não encontrar uma resposta, publique uma pergunta lá e a Microsoft ou outros membros da comunidade tentarão ajudar ou [entre em contato com o Suporte da Microsoft](https://support.microsoft.com/contactus/).

3. Selecione **Avançar** e, em seguida, na página **Selecionar Discos** do assistente (mostrado aqui), especifique o quanto deseja estender o volume. Normalmente, você usará o valor padrão, que utiliza todo o espaço livre disponível, mas poderá usar um valor menor se desejar criar volumes adicionais no espaço livre.

   :::image type="content" source="media/extend-volume-wizard.png" alt-text="O assistente Estender Volume mostrando um volume que está sendo estendido para ocupar todo o espaço disponível":::

4. Selecione **Avançar** e **Concluir** para estender o volume.

## <a name="to-extend-a-volume-by-using-powershell"></a>Para estender um volume usando o PowerShell

1. Selecione e mantenha o cursor (ou clique com o botão direito do mouse) no botão Iniciar e selecione Windows PowerShell (administrador).
2. Digite o seguinte comando para redimensionar o volume para o tamanho máximo, especificando a letra da unidade do volume que você deseja estender na variável *$drive _letter*:

   ```PowerShell
   # Variable specifying the drive you want to extend
   $drive_letter = "C"

   # Script to get the partition sizes and then resize the volume
   $size = (Get-PartitionSupportedSize -DriveLetter $drive_letter)
   Resize-Partition -DriveLetter $drive_letter -Size $size.SizeMax
   ```

## <a name="see-slso"></a>Consulte também

- [Resize-Partition](/powershell/module/storage/resize-partition)
- [Diskpart extend](../../administration/windows-commands/extend.md)
