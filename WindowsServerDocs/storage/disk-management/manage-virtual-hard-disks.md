---
title: Gerenciar VHD (Discos Rígidos Virtual)
description: Este artigo descreve como gerenciar discos rígidos virtuais
ms.date: 10/12/2017
ms.prod: windows-server
ms.technology: storage
ms.topic: article
author: JasonGerend
manager: brianlic
ms.author: jgerend
ms.openlocfilehash: 6ffa7e9dc769b8d8c892d0af1ceae5246df62d3e
ms.sourcegitcommit: 3a3d62f938322849f81ee9ec01186b3e7ab90fe0
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/23/2020
ms.locfileid: "71385811"
---
# <a name="manage-virtual-hard-disks-vhd"></a>Gerenciar VHD (Discos Rígidos Virtual)

> **Aplica-se a:** Windows 10, Windows 8.1, Windows Server (Canal Semestral), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Este tópico descreve como criar, anexar e desanexar discos rígidos virtuais com o Gerenciamento de Disco. Os VHDs (discos rígidos virtuais) são arquivos de disco rígido virtualizados que, quando montados, aparecem e funcionam de forma praticamente idêntica a um disco rígido físico. Eles são mais comumente usados com máquinas virtuais Hyper-V. 

## <a name="viewing-vhds-in-disk-management"></a>Visualização de VHDs no Gerenciamento de Disco

VHDs são exibidos assim como discos físicos no Gerenciamento de Disco. Quando um VHD é anexado (ou seja, disponibilizado para uso no sistema), ele aparece na cor azul. Se o disco for desanexado (isto é, fica indisponível), o ícone é revertido para cinza.

## <a name="creating-a-vhd"></a>Criar um VHD

> [!NOTE]
> Para concluir estas etapas, no mínimo, você deve ser um membro do grupo **Operadores de backup** ou **Administradores**.

**Para criar um VHD**

1.  No menu **Ação**, selecione **Criar VHD**.

2.  Na caixa de diálogo **Criar e anexar disco rígido virtual**, especifique o local no computador físico onde quer armazenar o arquivo VHD e o tamanho do VHD.

3.  Em **Formato de disco rígido virtual**, selecione **Expansão dinâmica** ou **Tamanho fixo** e, em seguida, clique em **OK**.

## <a name="attaching-and-detaching-a-vhd"></a>Anexar e desanexar um VHD

Para tornar um VHD disponível para uso (um que você acabou de criar ou outro VHD existente): 

1. No menu **Ação**, selecione **Anexar VHD**.

2. Especifique o local do VHD usando um caminho totalmente qualificado.

Para desanexar o VHD, tornando-o indisponível: Clique no disco com o botão direito no disco, selecione **Desanexar VHD** e, em seguida, clique em **OK**. Desanexar um VHD não exclui o VHD ou quaisquer dados armazenados nele.

## <a name="additional-considerations"></a>Considerações adicionais

-   O caminho que especifica o local do VHD deve ser totalmente qualificado e não pode estar no \\diretório do Windows.
-   O tamanho mínimo para um VHD é de 3 megabytes (MB).
-   Um VHD pode ser somente um disco básico.
-   Como um VHD é inicializado quando é criado, a criação de um VHD de tamanho fixo grande pode levar algum tempo.
