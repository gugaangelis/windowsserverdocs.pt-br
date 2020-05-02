---
title: recover
description: Tópico de referência para * * * *-
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: cf9be2e3-90c8-4773-a201-dc503b91948e
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: b316ed26f008a62f88aaeb4a7a7f3030d08f1588
ms.sourcegitcommit: ab64dc83fca28039416c26226815502d0193500c
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 05/01/2020
ms.locfileid: "82722620"
---
# <a name="recover"></a>recover



Recupera informações legíveis de um disco defeituoso ou defeituoso.



## <a name="syntax"></a>Sintaxe

```
recover [<Drive>:][<Path>]<FileName>
```

### <a name="parameters"></a>Parâmetros

|           Parâmetro           |                                          Descrição                                          |
|-------------------------------|-----------------------------------------------------------------------------------------------|
| [\<Unidade>:] [<Path>]<FileName> | Especifica o local e o nome do arquivo que você deseja recuperar. O *nome do arquivo* é obrigatório. |
|              /?               |                             Exibe a ajuda no prompt de comando.                              |

## <a name="remarks"></a>Comentários

-   O comando de **recuperação** lê um arquivo, setor por setor e recupera dados de setores bons. Os dados em setores inválidos são perdidos.
-   Os setores inválidos relatados por **chkdsk** foram marcados como ruins quando o disco estava preparado para operação. Eles não apresentam nenhum perigo e a **recuperação** não os afeta.
-   Como todos os dados em setores inválidos são perdidos quando você recupera um arquivo, você deve recuperar apenas um arquivo de cada vez.
-   Você não pode usar caracteres curinga (**&#42;** e **?**) com o comando de **recuperação** . Você deve especificar um arquivo (e o local do arquivo se ele não estiver no diretório atual).

## <a name="examples"></a>Exemplos

Para recuperar o arquivo Story. txt no diretório \Fiction na unidade D, digite:
```
recover d:\fiction\story.txt 
```

## <a name="additional-references"></a>Referências adicionais

- [Chave da sintaxe de linha de comando](command-line-syntax-key.md)