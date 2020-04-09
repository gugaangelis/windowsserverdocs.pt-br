---
title: bitsadmin addfileset
description: O tópico de comandos do Windows para **Bitsadmin addfileset**, que adiciona um ou mais arquivos ao trabalho especificado.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 75466994-262f-4724-b14d-f813c5397675
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 1c4cff7dc8439fe8e1c54d1f5d231d1b487dc70c
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80850969"
---
# <a name="bitsadmin-addfileset"></a>bitsadmin addfileset

Adiciona um ou mais arquivos ao trabalho especificado.

## <a name="syntax"></a>Sintaxe

```
bitsadmin /addfileset <Job> <TextFile>
```

### <a name="parameters"></a>Parâmetros

| Parâmetro | Descrição |
| --------- | ----------- |
| Trabalho | O nome de exibição ou o GUID do trabalho. |
| TextFile | Um arquivo de texto, cada linha que contém um nome de arquivo remoto e local. **Observação:** Os nomes são delimitados por espaço. As linhas que começam com um `#` caractere são tratadas como um comentário. |

## <a name="examples"></a><a name=BKMK_examples></a>Disso

```
C:\>bitsadmin /addfileset files.txt
```

## <a name="additional-references"></a>Referências adicionais

- [Chave da sintaxe de linha de comando](command-line-syntax-key.md)