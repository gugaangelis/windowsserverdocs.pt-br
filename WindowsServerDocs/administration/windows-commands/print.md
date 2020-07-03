---
title: print
description: Artigo de referência para o comando Print, que envia um arquivo de texto para uma impressora.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: aa2325d5-a993-4ed3-b996-255165452db8
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: a8e8cf07ca19af1cf0a0445b9feed06916be10c0
ms.sourcegitcommit: 2afed2461574a3f53f84fc9ec28d86df3b335685
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/02/2020
ms.locfileid: "85926036"
---
# <a name="print"></a>print

Envia um arquivo de texto para uma impressora. Um arquivo pode ser impresso em segundo plano se você o enviar para uma impressora conectada a uma porta serial ou paralela no computador local.

> [!NOTE]
> Você pode executar muitas tarefas de configuração no prompt de comando usando o [comando mode](mode.md), incluindo a configuração de uma impressora conectada a uma porta paralela ou serial, a exibição do status da impressora ou a preparação de uma impressora para a alternância da página de código.

## <a name="syntax"></a>Sintaxe

```
print [/d:<printername>] [<drive>:][<path>]<filename>[ ...]
```

### <a name="parameters"></a>Parâmetros

| Parâmetro | Descrição |
|--|--|
| /d`<printername>` | Especifica a impressora na qual você deseja imprimir o trabalho. Para imprimir em uma impressora conectada localmente, especifique a porta no computador em que a impressora está conectada. Os valores válidos para as portas paralelas são **LPT1**, **LPT2**e **LPT3**. Os valores válidos para as portas seriais são **COM1**, **COM2**, **com3**e **COM4**. Você também pode especificar uma impressora de rede usando seu nome de fila ( `\\server_name\printer_name` ). Se você não especificar uma impressora, o trabalho de impressão será enviado para **LPT1** por padrão. |
| `<drive>`: | Especifica a unidade lógica ou física na qual o arquivo que você deseja imprimir está localizado. Esse parâmetro não será necessário se o arquivo que você deseja imprimir estiver localizado na unidade atual. |
| `<path>` | Especifica o local do arquivo que você deseja imprimir. Esse parâmetro não será necessário se o arquivo que você deseja imprimir estiver localizado no diretório atual. |
| `<filename>[ ...]` | Obrigatórios. Especifica o arquivo que você deseja imprimir. Você pode incluir vários arquivos em um comando. |
| /? | Exibe a ajuda no prompt de comando. |

### <a name="examples"></a>Exemplos

Para enviar o arquivo de **report.txt** , localizado no diretório atual, para uma impressora conectada ao **LPT2** no computador local, digite:

```
print /d:lpt2 report.txt
```

Para enviar o arquivo de **report.txt** , localizado no diretório **c:\Accounting** , para a fila de impressão **printer1** no servidor **/d: \\ copyroom** , digite:

```
print /d:\\copyroom\printer1 c:\accounting\report.txt
```

## <a name="additional-references"></a>Referências adicionais

- [Chave da sintaxe de linha de comando](command-line-syntax-key.md)

- [Referência aos comandos de impressão](print-command-reference.md)

- [Comando de modo](mode.md)