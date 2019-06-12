---
title: recover
description: 'Tópico de comandos do Windows para * * *- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: cf9be2e3-90c8-4773-a201-dc503b91948e
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: d6b9b5544394bfc69a2dc9f7be26ed8355a3f690
ms.sourcegitcommit: eaf071249b6eb6b1a758b38579a2d87710abfb54
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 05/31/2019
ms.locfileid: "66441964"
---
# <a name="recover"></a>recover



Recupera informações legíveis de um disco com defeito ou danificado.

Para obter exemplos de como usar esse comando, consulte [Exemplos](#BKMK_examples).

## <a name="syntax"></a>Sintaxe

```
recover [<Drive>:][<Path>]<FileName>
```

## <a name="parameters"></a>Parâmetros

|           Parâmetro           |                                          Descrição                                          |
|-------------------------------|-----------------------------------------------------------------------------------------------|
| [\<Drive>:][<Path>]<FileName> | Especifica o local e o nome do arquivo que você deseja recuperar. *Nome do arquivo* é necessária. |
|              /?               |                             Exibe a ajuda no prompt de comando.                              |

## <a name="remarks"></a>Comentários

-   O **recuperar** comando lê um arquivo, setor por setor e recupera os dados dos setores de BOM. Dados em setores inválidos serão perdidos.
-   Setores inválidos relatado pelo **chkdsk** foram marcados como "ruins" quando o disco tiver sido preparado para operação. Eles oferecem nenhum perigo, e **recuperar** não afeta a eles.
-   Como todos os dados em setores inválidos são perdidos quando você recupera um arquivo, você deve recuperar apenas um arquivo por vez.
-   Você não pode usar caracteres curinga ( **&#42;** e **?** ) com o **recuperar** comando. Você deve especificar um arquivo (e o local do arquivo se ele não estiver no diretório atual).

## <a name="BKMK_examples"></a>Exemplos

Para recuperar o arquivo história. txt no diretório \Fiction na unidade D, digite:
```
recover d:\fiction\story.txt 
```

#### <a name="additional-references"></a>Referências adicionais

[Chave da sintaxe de linha de comando](command-line-syntax-key.md)