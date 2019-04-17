---
title: "Gerenciar discos rígidos virtuais (VHD)"
description: "Este artigo descreve como gerenciar Discos rígidos virtuais"
ms.date: 10/12/2017
ms.prod: windows-server-threshold
ms.technology: storage
ms.topic: article
author: JasonGerend
manager: brianlic
ms.author: jgerend
ms.openlocfilehash: 2e371710752d59ebc7a1f8aa2dad3d9189872c47
ms.sourcegitcommit: 583355400f6b0d880dc0ac6bc06f0efb50d674f7
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 10/17/2017
---
# <a name="manage-virtual-hard-disks-vhd"></a>Gerenciar discos rígidos virtuais (VHD)

> **Aplicável a:** Windows 10, Windows 8.1, Windows Server (canal semestral), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Este tópico descreve como criar, anexar e desanexar discos rígidos virtuais com o Gerenciamento de Disco. Os discos rígidos virtuais (VHDs) são arquivos de disco rígido virtualizado que, quando montados, aparecem e funcionam de forma quase idêntica a um disco rígido físico. Eles são mais comumente usados com máquinas virtuais Hyper-V. 

## <a name="viewing-vhds-in-disk-management"></a>Exibir VHDs no Gerenciamento de Disco

Os VHDs aparecem como discos físicos no Gerenciamento de Disco. Quando um VHD é conectado (ou seja, disponibilizado para uso no sistema), ele aparece na cor azul. Se o disco for desconectado (isto é, fica indisponível), o ícone é revertido para cinza.

## <a name="creating-a-vhd"></a>Criando um VHD

> [!NOTE]
> No mínimo, você deve ser um membro do grupo **Operadores de backup** ou **Administradores** para concluir estas etapas.

**Para criar um VHD**

1.  No menu **Ação**, selecione **Criar VHD**.

2.  Na caixa de diálogo **Criar e anexar disco rígido virtual**, especifique o local no computador físico onde você deseja que o arquivo VHD seja armazenado e o tamanho do VHD.

3.  Em **Formato de disco rígido virtual**, selecione **Expandindo dinamicamente** ou **Tamanho fixo** e clique em **OK**.

## <a name="attaching-and-detaching-a-vhd"></a>Anexar e desanexar um VHD

Para disponibilizar um VHD para uso (um que você acabou de criar ou outro VHD existente): 

1. No menu **Ação**, selecione **Anexar VHD**.

2. Especifique o local do VHD usando um caminho totalmente qualificado.

Para desanexar o VHD, deixando-o indisponível: clique com botão direito do mouse no disco, selecione **Desanexar o VHD**e clique em **OK**. A desconexão de um VHD não exclui o VHD ou quaisquer dados armazenados nele.

## <a name="additional-considerations"></a>Considerações adicionais

-   O caminho que especifica o local do VHD deve ser totalmente qualificado e não pode estar no diretório \\Windows.
-   O tamanho mínimo para um VHD é de 3 megabytes (MB).
-   Um VHD pode ser somente um disco básico.
-   Como um VHD é inicializado quando é criado, a criação de um VHD de tamanho fixo grande pode demorar alguns minutos.
