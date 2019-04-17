---
title: "Gerenciar volumes básicos"
description: "Este artigo descreve como gerenciar volumes básicos."
ms.date: 10/12/2017
ms.prod: windows-server-threshold
ms.technology: storage
ms.topic: article
author: JasonGerend
manager: brianlic
ms.author: jgerend
ms.openlocfilehash: c75d887a6427673319999522b890d523f4276871
ms.sourcegitcommit: 583355400f6b0d880dc0ac6bc06f0efb50d674f7
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 10/17/2017
---
# <a name="manage-basic-volumes"></a>Gerenciar volumes básicos

> **Aplicável a:** Windows 10, Windows 8.1, Windows Server (canal semestral), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Um disco básico é um disco físico contendo partições primárias, estendidas ou unidades lógicas. Partições e unidades lógicas em discos básicos são conhecidas como volumes básicos. Você pode criar volumes básicos somente em discos básicos.

Você pode adicionar mais espaço às partições primárias e lógicas existentes estendendo-as para um espaço não alocado adjacente e contíguo no mesmo disco. Para estender um volume básico, ele deve ser formatado com o sistema de arquivos NTFS. É possível estender uma unidade lógica no espaço livre contíguo da partição estendida que a contém. Se você estender uma unidade lógica além do espaço livre disponível na partição estendida, esta aumenta para conter a unidade lógica contanto que a partição estendida seja seguida por espaço não alocado contíguo.

## <a name="see-also"></a>Consulte também

-   [Atribuir um caminho de pasta do ponto de montagem a uma unidade](assign-a-mount-point-folder-path-to-a-drive.md)
-   [Estender um volume básico](extend-a-basic-volume.md)
-   [Reduzir um volume básico](shrink-a-basic-volume.md)