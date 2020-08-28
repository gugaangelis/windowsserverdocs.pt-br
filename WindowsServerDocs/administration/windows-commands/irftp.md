---
title: irftp
description: Artigo de referência para o comando irftp, que envia arquivos por um link de infravermelho.
ms.topic: reference
ms.assetid: e15c60a7-546d-4e9f-9871-43aaa1b569d6
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 4c9d135dd0bb16f10e99fd7916a0965c0542f862
ms.sourcegitcommit: 96d46c702e7a9c3a321bbbb5284f73911c7baa3c
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/27/2020
ms.locfileid: "89035424"
---
# <a name="irftp"></a>irftp

> Aplica-se a: Windows Server (canal semestral), Windows Server 2019, Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Envia arquivos por um link de infravermelho.

> [!IMPORTANT]
> Verifique se os dispositivos destinados à comunicação por meio de um link de infravermelho têm a funcionalidade de infravermelho habilitada e estão funcionando corretamente. Além disso, certifique-se de que um link de infravermelho seja estabelecido entre os dispositivos.

## <a name="syntax"></a>Sintaxe

```
irftp [<drive>:\] [[<path>] <filename>] [/h][/s]
```

### <a name="parameters"></a>Parâmetros

| Parâmetro | Descrição |
| --------- | ----------- |
| `<drive>:\` | Especifica a unidade que contém os arquivos que você deseja enviar por um link de infravermelho. |
| `[path]<filename>` | Especifica o local e o nome do arquivo ou conjunto de arquivos que você deseja enviar por um link de infravermelho. Se você especificar um conjunto de arquivos, deverá especificar o caminho completo de cada arquivo. |
| /h | Especifica o modo oculto. Quando o modo oculto é usado, os arquivos são enviados sem exibir a caixa de diálogo link sem fio. |
| /s | Abre a caixa de diálogo **link sem fio** , para que você possa selecionar o arquivo ou conjunto de arquivos que deseja enviar sem usar a linha de comando para especificar a unidade, o caminho e os nomes de arquivo. A caixa de diálogo **link sem fio** também será aberta se você usar esse comando sem nenhum parâmetro. |

### <a name="examples"></a>Exemplos

Para enviar *c:\example.txt* por meio do link de infravermelho, digite:

```
irftp c:\example.txt
```

## <a name="additional-references"></a>Referências adicionais

- [Chave da sintaxe de linha de comando](command-line-syntax-key.md)
