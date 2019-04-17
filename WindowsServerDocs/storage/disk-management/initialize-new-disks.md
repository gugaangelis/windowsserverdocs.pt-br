---
title: Initialize novos discos
description: Este artigo descreve como inicializar novos discos
ms.date: 10/12/2017
ms.prod: windows-server-threshold
ms.technology: storage
ms.topic: article
author: JasonGerend
manager: brianlic
ms.author: jgerend
ms.openlocfilehash: 587553746d45eceab654efd4d120038088d32991
ms.sourcegitcommit: 583355400f6b0d880dc0ac6bc06f0efb50d674f7
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 10/17/2017
---
# <a name="initialize-new-disks"></a>Initialize novos discos

> **Aplicável a:** Windows 10, Windows 8.1, Windows Server (canal semestral), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

> [!NOTE]
> No mínimo, você deve ser um membro do grupo **Operadores de backup** ou **Administradores** para concluir estas etapas.

## <a name="to-initialize-new-disks"></a>Para inicializar novos discos
1.  No Gerenciamento de Disco, clique com o botão direito do mouse no disco que você deseja inicializar e clique em **Inicializar Disco**.

2.  Na caixa de diálogo **Inicializar Disco**, selecione os discos que você deseja inicializar. Você pode optar por usar o Registro mestre de inicialização (MBR) ou o estilo de partição de Tabela de partição de GUID (GPT).

## <a name="additional-considerations"></a>Considerações adicionais

-   Os novos discos aparecem como **Mão inicializados**. Antes de usar um disco, você deve inicializá-lo. Se você iniciar o Gerenciamento de Disco depois de adicionar um disco, o Assistente de inicialização do disco é exibido para que você possa inicializar do disco.

> [!NOTE]
> O novo disco é inicializado como um disco básico.

