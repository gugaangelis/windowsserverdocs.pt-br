---
title: lodctr
description: Artigo de referência do comando Lodctr, que permite registrar ou salvar o nome do contador de desempenho e as configurações do registro em um arquivo e designar serviços confiáveis.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 5a849abd-6b31-4833-bc8a-306c05eca29a
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: e8b1cae87818d3f77474e4193b03836bf1c84990
ms.sourcegitcommit: 2afed2461574a3f53f84fc9ec28d86df3b335685
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/02/2020
ms.locfileid: "85931653"
---
# <a name="lodctr"></a>lodctr

> Aplica-se a: Windows Server (canal semestral), Windows Server 2019, Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Permite registrar ou salvar o nome do contador de desempenho e as configurações do registro em um arquivo e designar serviços confiáveis.

## <a name="syntax"></a>Sintaxe

```
lodctr <filename> [/s:<filename>] [/r:<filename>] [/t:<servicename>]
```

### <a name="parameters"></a>Parâmetros

| Parâmetro | Descrição |
| --------- | ----------- |
| `<filename>` | Especifica o nome do arquivo de inicialização que registra as configurações do nome do contador de desempenho e o texto explicativo. |
| /s`<filename>` | Especifica o nome do arquivo no qual as configurações do registro do contador de desempenho e o texto explicativo são salvos. |
| /r | Restaura as configurações do registro do contador e o texto explicativo das configurações atuais do registro e dos arquivos de desempenho em cache relacionados ao registro. |
| /r`<filename>` | Especifica o nome do arquivo que restaura as configurações do registro do contador de desempenho e o texto explicativo.<p>**AVISO:** Se você usar esse comando, substituirá todas as configurações do registro do contador de desempenho e o texto explicativo, substituindo-as pela configuração definida no arquivo especificado. |
| /t:`<servicename>` | Indica que o serviço `<servicename>` é confiável. |
| /? | Exibe a ajuda no prompt de comando. |

#### <a name="remarks"></a>Comentários

- Se as informações fornecidas contiverem espaços, use aspas ao contrário do texto (por exemplo, "nome do arquivo 1").

### <a name="examples"></a>Exemplos

Para salvar as configurações atuais do registro de desempenho e o texto explicativo em *backup1.txtde desempenho *do arquivo, digite:

```
lodctr /s:perf backup1.txt
```

## <a name="additional-references"></a>Referências adicionais

- [Chave da sintaxe de linha de comando](command-line-syntax-key.md)
