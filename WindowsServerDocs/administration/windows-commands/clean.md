---
title: clean
description: 'Tópico de comandos do Windows para * * *- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 9bbe6fd3-e07e-487b-9035-910957a1d326
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 6e7d7613784f3e599005b25259f70466db60e626
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59877427"
---
# <a name="clean"></a>clean

>Aplica-se a: Windows Server (canal semestral), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

O comando de Diskpart limpo remove toda e qualquer partição ou volume de formatação do disco com foco.
## <a name="syntax"></a>Sintaxe
```
clean [all]
```
## <a name="parameters"></a>Parâmetros
|Parâmetro|Descrição|
|-------|--------|
|all|Especifica que todos os setores do disco é definido como zero, o que exclui completamente todos os dados contidos no disco.|
## <a name="remarks"></a>Comentários
-   No mestre de inicialização discos MBR (registro), somente o particionamento MBR informações e setores ocultos são substituídos.
-   Em discos de tabela de partição GUID (gpt), o particionamento de informações, incluindo o MBR protetor, gpt será substituído. Não há nenhuma informação de setores ocultos.
-   Um disco deve ser selecionado para essa operação seja bem-sucedida. Use o **Selecionar disco** comando para selecionar um disco e mudar o foco a ele.
## <a name="BKMK_examples"></a>Exemplos
Para remover toda a formatação do disco selecionado, digite:
```
clean
```

[Clear-Disk](https://technet.microsoft.com/library/hh848661.aspx)
