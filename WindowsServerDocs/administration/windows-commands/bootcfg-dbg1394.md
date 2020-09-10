---
title: bootcfg dbg1394
description: Artigo de referência para o comando Bootcfg dbg1394, que configura a depuração de porta 1394 para uma entrada de sistema operacional especificada
ms.topic: reference
ms.assetid: 35724697-90dd-4dbe-85b0-337fbd369dcc
ms.author: lizross
author: eross-msft
manager: mtillman
ms.date: 10/16/2017
ms.openlocfilehash: 56d91256a74bc247749956bc8948245636b085d1
ms.sourcegitcommit: db2d46842c68813d043738d6523f13d8454fc972
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/10/2020
ms.locfileid: "89630270"
---
# <a name="bootcfg-dbg1394"></a>bootcfg dbg1394

> Aplica-se a: Windows Server (canal semestral), Windows Server 2019, Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Configura a depuração de porta 1394 para uma entrada de sistema operacional especificada.

## <a name="syntax"></a>Sintaxe

```
bootcfg /dbg1394 {on | off}[/s <computer> [/u <domain>\<user> /p <password>]] [/ch <channel>] /id <osentrylinenum>
```

### <a name="parameters"></a>Parâmetros

| Parâmetro | Descrição |
| --------- | ----------- |
| `{on | off}` | Especifica o valor para a depuração de porta 1394, incluindo:<ul><li>**no.** Habilita o suporte à depuração remota adicionando a opção/dbg1394 ao especificado `<osentrylinenum>` .</li><li>**desconto.** Desabilita o suporte à depuração remota removendo a opção/dbg1394 do especificado <osentrylinenum> .</li></ul> |
| `/s <computer>` | Especifica o nome ou o endereço IP de um computador remoto (não use barras invertidas). O padrão é o computador local. |
| `/u <domain>\<user>`  | Executa o comando com as permissões de conta do usuário especificado por `<user>` ou `<domain>\<user>` . O padrão é as permissões do usuário conectado no momento no computador que emite o comando. |
| `/p <password>` | Especifica a senha da conta de usuário que é especificada no parâmetro **/u** . |
| `/ch <channel>` | Especifica o canal a ser usado para depuração. Os valores válidos incluem inteiros, entre 1 e 64. Não use esse parâmetro se a depuração de porta 1394 estiver desabilitada. |
| `/id <osentrylinenum>` | Especifica o número da linha de entrada do sistema operacional na seção [Operating Systems] do arquivo de Boot.ini ao qual as opções de carregamento do sistema operacional são adicionadas. A primeira linha após o cabeçalho da seção [Operating Systems] é 1. |
| /? | Exibe a ajuda no prompt de comando. |

## <a name="examples"></a>Exemplos

Para usar o comando **Bootcfg/dbg1394**:

```
bootcfg /dbg1394 /id 2
bootcfg /dbg1394 on /ch 1 /id 3
bootcfg /dbg1394 edit /ch 8 /id 2
bootcfg /s srvmain /u maindom\hiropln /p p@ssW23 /dbg1394 off /id 2
```

## <a name="additional-references"></a>Referências adicionais

- [Chave da sintaxe de linha de comando](command-line-syntax-key.md)

- [comando Bootcfg](bootcfg.md)
