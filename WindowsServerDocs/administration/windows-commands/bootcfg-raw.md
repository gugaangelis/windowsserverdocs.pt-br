---
title: bootcfg raw
description: Artigo de referência para o comando Bootcfg RAW, que adiciona opções de carregamento do sistema operacional, especificadas como uma cadeia de caracteres, a uma entrada do sistema operacional na seção do sistema operacional do arquivo de Boot.ini.
ms.topic: reference
ms.assetid: e3458749-b0a0-460f-a022-3ff199a71f27
ms.author: lizross
author: eross-msft
manager: mtillman
ms.date: 10/16/2017
ms.openlocfilehash: b86945a126c73742982ea01442101c6d1250226d
ms.sourcegitcommit: db2d46842c68813d043738d6523f13d8454fc972
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/10/2020
ms.locfileid: "89630141"
---
# <a name="bootcfg-raw"></a>bootcfg raw

> Aplica-se a: Windows Server (canal semestral), Windows Server 2019, Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Adiciona opções de carregamento do sistema operacional especificadas como uma cadeia de caracteres a uma entrada do sistema operacional na seção [Operating Systems] do arquivo de Boot.ini. Esse comando substitui todas as opções de entrada do sistema operacional existente.

## <a name="syntax"></a>Sintaxe

```
bootcfg /raw [/s <computer> [/u <domain>\<user> /p <password>]] <osloadoptionsstring> [/id <osentrylinenum>] [/a]
```

### <a name="parameters"></a>Parâmetros

| Parâmetro | Descrição |
| --------- | ----------- |
| `/s <computer>` | Especifica o nome ou o endereço IP de um computador remoto (não use barras invertidas). O padrão é o computador local. |
| `/u <domain>\<user>`  | Executa o comando com as permissões de conta do usuário especificado por `<user>` ou `<domain>\<user>` . O padrão é as permissões do usuário conectado no momento no computador que emite o comando. |
| `/p <password>` | Especifica a senha da conta de usuário que é especificada no parâmetro **/u** . |
| `<osloadoptionsstring>` | Especifica as opções de carregamento do sistema operacional a serem adicionadas à entrada do sistema operacional. Essas opções de carregamento substituem qualquer opção de carregamento existente associada à entrada do sistema operacional. Não há nenhuma validação em relação ao `<osloadoptions>` parâmetro.
| `/id <osentrylinenum>` | Especifica o número da linha de entrada do sistema operacional na seção [Operating Systems] do arquivo de Boot.ini ao qual as opções de carregamento do sistema operacional são adicionadas. A primeira linha após o cabeçalho da seção [Operating Systems] é 1. |
| /a | Especifica quais opções do sistema operacional devem ser acrescentadas a qualquer opção do sistema operacional existente. |
| /? | Exibe a ajuda no prompt de comando. |

## <a name="examples"></a>Exemplos

Esse texto deve conter opções de carregamento de sistema operacional válidas, como **/debug**, **/fastdetect**, **/NODEBUG**, **/baudrate**, **/crashdebug**e **/SOS**.

Para adicionar **/debug/fastdetect** ao final da primeira entrada do sistema operacional, substituindo as opções de entrada do sistema operacional anterior:

```
bootcfg /raw /debug /fastdetect /id 1
```

Para usar o comando **Bootcfg/RAW** :

```
bootcfg /raw /debug /sos /id 2
bootcfg /raw /s srvmain /u maindom\hiropln /p p@ssW23 /crashdebug  /id 2
```

## <a name="additional-references"></a>Referências adicionais

- [Chave da sintaxe de linha de comando](command-line-syntax-key.md)

- [comando Bootcfg](bootcfg.md)
