---
title: bitsadmin addfileset
description: Artigo de referência para o comando Bitsadmin addfileset, que adiciona um ou mais arquivos ao trabalho especificado.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 75466994-262f-4724-b14d-f813c5397675
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 861186dfc7ba1a230e1df05c98378d27bfff26b1
ms.sourcegitcommit: 2afed2461574a3f53f84fc9ec28d86df3b335685
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/02/2020
ms.locfileid: "85927084"
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
| textfile | Um arquivo de texto, cada linha que contém um nome de arquivo remoto e local. **Observação:** Os nomes devem ser delimitados por espaço. As linhas que começam com um `#` caractere são tratadas como um comentário. |

## <a name="examples"></a>Exemplos

```
bitsadmin /addfileset files.txt
```

## <a name="additional-references"></a>Referências adicionais

- [Chave da sintaxe de linha de comando](command-line-syntax-key.md)

- [comando Bitsadmin](bitsadmin.md)
