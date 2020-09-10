---
title: lpr
description: Artigo de referência para o comando LPR, que envia um arquivo para um computador ou dispositivo de compartilhamento de impressora que executa o Serviço LPD (daemon de impressora de linha) em preparação para impressão.
ms.topic: reference
ms.assetid: afc8790b-8b52-45c4-acdf-be0ffa9da534
ms.author: lizross
author: eross-msft
manager: mtillman
ms.date: 10/16/2017
ms.openlocfilehash: 802527251b56b1955544a4131a7bcf1aa9f11153
ms.sourcegitcommit: db2d46842c68813d043738d6523f13d8454fc972
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/10/2020
ms.locfileid: "89640358"
---
# <a name="lpr"></a>lpr

> Aplica-se a: Windows Server (canal semestral), Windows Server 2019, Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Envia um arquivo para um computador ou dispositivo de compartilhamento de impressora que executa o Serviço LPD (daemon de impressora de linha) em preparação para impressão.

## <a name="syntax"></a>Sintaxe

```
lpr [-S <servername>] -P <printername> [-C <bannercontent>] [-J <jobname>] [-o | -o l] [-x] [-d] <filename>
```

### <a name="parameters"></a>Parâmetros

| Parâmetro | Descrição |
| --------- | ----------- |
| -S `<servername>` | Especifica (por nome ou endereço IP) o computador ou o dispositivo de compartilhamento de impressora que hospeda a fila de impressão LPD com um status que você deseja exibir.  Esse parâmetro é necessário e deve estar em letras maiúsculas. |
| -P `<printername> `| Especifica (por nome) a impressora para a fila de impressão com um status que você deseja exibir. Para localizar o nome da impressora, abra a pasta **impressoras** . Esse parâmetro é necessário e deve estar em letras maiúsculas. |
| -C `<bannercontent>` | Especifica o conteúdo a ser impresso na página de faixa do trabalho de impressão. Se você não incluir esse parâmetro, o nome do computador do qual o trabalho de impressão foi enviado aparecerá na página de faixa. Esse parâmetro deve estar em letras maiúsculas. |
| -J `<jobname>` | Especifica o nome do trabalho de impressão que será impresso na página de faixa. Se você não incluir esse parâmetro, o nome do arquivo que está sendo impresso aparecerá na página de faixa. Esse parâmetro deve estar em letras maiúsculas. |
| `[-o | -o l]` | Especifica o tipo de arquivo que você deseja imprimir. O parâmetro **-o** especifica que você deseja imprimir um arquivo de texto. O parâmetro **-o l** especifica que você deseja imprimir um arquivo binário (por exemplo, um arquivo PostScript). |
| -d | Especifica que o arquivo de dados deve ser enviado antes do arquivo de controle. Use esse parâmetro se a sua impressora exigir que o arquivo de dados seja enviado primeiro. Para obter mais informações, consulte a documentação da sua impressora. |
| -X | Especifica que o comando **LPR** deve ser compatível com o sistema operacional Sun Microsystems (chamado de SunOS) para versões até e incluindo 4.1.4_u1. |
| `<filename>` | Especifica (por nome) o arquivo a ser impresso. Este parâmetro é necessário. |
| /? | Exibe a ajuda no prompt de comando. |

### <a name="examples"></a>Exemplos

Para imprimir o arquivo de texto de *Document.txt* na fila de impressora *Laserprinter1* em um host LPD em *10.0.0.45*, digite:

```
lpr -S 10.0.0.45 -P Laserprinter1 -o Document.txt
```

Para imprimir o arquivo Adobe PostScript *PostScript_file. PS* na fila de impressora *Laserprinter1* em um host LPD em *10.0.0.45*, digite:

```
lpr -S 10.0.0.45 -P Laserprinter1 -o l PostScript_file.ps
```

## <a name="additional-references"></a>Referências adicionais

- [Chave da sintaxe de linha de comando](command-line-syntax-key.md)

- [Referência aos comandos de impressão](print-command-reference.md)
