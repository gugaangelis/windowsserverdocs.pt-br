---
title: bitsadmin addfileset
description: Tópico de referência para o comando Bitsadmin addfileset, que adiciona um ou mais arquivos ao trabalho especificado.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 75466994-262f-4724-b14d-f813c5397675
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: d610c1330818cf820923b6d4f2e3555dc477444b
ms.sourcegitcommit: ab64dc83fca28039416c26226815502d0193500c
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 05/01/2020
ms.locfileid: "82718475"
---
# <a name="bitsadmin-addfileset"></a>bitsadmin addfileset

Adiciona um ou mais arquivos ao trabalho especificado.

## <a name="syntax"></a>Sintaxe

```
bitsadmin /addfileset <job> <textfile>
```

### <a name="parameters"></a>Parâmetros

| Parâmetro | Descrição |
| --------- | ----------- |
| trabalho | O nome de exibição ou o GUID do trabalho. |
| textfile | Um arquivo de texto, cada linha que contém um nome de arquivo remoto e local. **Observação:** Os nomes devem ser delimitados por espaço. As linhas que começam `#` com um caractere são tratadas como um comentário. |

## <a name="examples"></a>Exemplos

```
bitsadmin /addfileset files.txt
```

## <a name="additional-references"></a>Referências adicionais

- [Chave da sintaxe de linha de comando](command-line-syntax-key.md)

- [comando Bitsadmin](bitsadmin.md)
