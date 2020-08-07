---
title: bitsadmin setnotifycmdline
description: Artigo de referência para o comando Bitsadmin setnotifycmdline, que define o comando de linha de comando que será executado quando o trabalho terminar de transferir dados ou quando um trabalho entrar em um estado.
ms.topic: article
ms.assetid: 415ae6ef-8549-48b2-9693-2368a6e24075
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: a81b4521d8c765d85e6b4a92d0429b128f43198e
ms.sourcegitcommit: 53d526bfeddb89d28af44210a23ba417f6ce0ecf
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/06/2020
ms.locfileid: "87893005"
---
# <a name="bitsadmin-setnotifycmdline"></a>bitsadmin setnotifycmdline

Define o comando de linha de comando que é executado depois que o trabalho termina de transferir dados ou depois que um trabalho entra em um estado especificado.

> [!NOTE]
> Esse comando não tem suporte no BITS 1,2 e versões anteriores.

## <a name="syntax"></a>Sintaxe

```
bitsadmin /setnotifycmdline <job> <program_name> [program_parameters]
```

### <a name="parameters"></a>Parâmetros

| Parâmetro | Descrição |
| --------- | ----------- |
| trabalho | O nome de exibição ou o GUID do trabalho. |
| program_name | Nome do comando a ser executado quando o trabalho for concluído. Você pode definir esse valor como nulo, mas, se fizer isso, *program_parameters* também deverá ser definido como nulo. |
| program_parameters | Parâmetros que você deseja passar para *program_name*. Você pode definir esse valor como nulo. Se *program_parameters* não for definido como NULL, o primeiro parâmetro em *program_parameters* deverá corresponder ao *program_name*. |

## <a name="examples"></a>Exemplos

Para executar Notepad.exe na conclusão do trabalho chamado *myDownloadJob*:

```
bitsadmin /setnotifycmdline myDownloadJob c:\winnt\system32\notepad.exe NULL
```

Para mostrar o texto do EULA em Notepad.exe, na conclusão do trabalho chamado myDownloadJob:

```
bitsadmin /setnotifycmdline myDownloadJob c:\winnt\system32\notepad.exe notepad c:\eula.txt
```

## <a name="additional-references"></a>Referências adicionais

- [Chave da sintaxe de linha de comando](command-line-syntax-key.md)

- [comando Bitsadmin](bitsadmin.md)
