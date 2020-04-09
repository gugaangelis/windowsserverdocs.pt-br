---
title: recover
description: Tópico de comandos do Windows para * * * *-
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: cf9be2e3-90c8-4773-a201-dc503b91948e
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 172471c5c56823e29dd0882072920db3d77639da
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80836719"
---
# <a name="recover"></a>recover



Recupera informações legíveis de um disco defeituoso ou defeituoso.

Para obter exemplos de como usar esse comando, consulte [Exemplos](#BKMK_examples).

## <a name="syntax"></a>Sintaxe

```
recover [<Drive>:][<Path>]<FileName>
```

### <a name="parameters"></a>Parâmetros

|           Parâmetro           |                                          Descrição                                          |
|-------------------------------|-----------------------------------------------------------------------------------------------|
| [\<drive >:] [<Path>]<FileName> | Especifica o local e o nome do arquivo que você deseja recuperar. O *nome do arquivo* é obrigatório. |
|              /?               |                             Exibe a ajuda no prompt de comando.                              |

## <a name="remarks"></a>Comentários

-   O comando de **recuperação** lê um arquivo, setor por setor e recupera dados de setores bons. Os dados em setores inválidos são perdidos.
-   Os setores inválidos relatados por **chkdsk** foram marcados como ruins quando o disco estava preparado para operação. Eles não apresentam nenhum perigo e a **recuperação** não os afeta.
-   Como todos os dados em setores inválidos são perdidos quando você recupera um arquivo, você deve recuperar apenas um arquivo de cada vez.
-   Você não pode usar caracteres curinga **&#42;** (e **?** ) com o comando de **recuperação** . Você deve especificar um arquivo (e o local do arquivo se ele não estiver no diretório atual).

## <a name="examples"></a><a name=BKMK_examples></a>Disso

Para recuperar o arquivo Story. txt no diretório \Fiction na unidade D, digite:
```
recover d:\fiction\story.txt 
```

## <a name="additional-references"></a>Referências adicionais

- [Chave da sintaxe de linha de comando](command-line-syntax-key.md)