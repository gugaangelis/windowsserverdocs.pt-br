---
title: bootcfg debug
description: Artigo de referência para o comando Bootcfg Debug, que adiciona ou altera as configurações de depuração para uma entrada de sistema operacional especificada.
ms.topic: article
ms.assetid: 28afa5fb-a236-46e2-b1a4-a3c43a49c437
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 8b3cbde64da67196a1067791e5dad3c2b02756d4
ms.sourcegitcommit: 53d526bfeddb89d28af44210a23ba417f6ce0ecf
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/06/2020
ms.locfileid: "87880690"
---
# <a name="bootcfg-debug"></a>bootcfg debug

> Aplica-se a: Windows Server (canal semestral), Windows Server 2019, Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Adiciona ou altera as configurações de depuração para uma entrada de sistema operacional especificada.

>[!NOTE]
> Se você estiver tentando depurar a porta 1394, use o comando [Bootcfg dbg1394](bootcfg-dbg1394.md) em vez disso.

## <a name="syntax"></a>Sintaxe

```
bootcfg /debug {on | off | edit}[/s <computer> [/u <domain>\<user> /p <password>]] [/port {COM1 | COM2 | COM3 | COM4}] [/baud {9600 | 19200 | 38400 | 57600 | 115200}] [/id <osentrylinenum>]
```

### <a name="parameters"></a>Parâmetros

| Parâmetro | Descrição |
| --------- | ----------- |
| `{on | off | edit}` | Especifica o valor para a depuração de porta, incluindo:<ul><li>**no.** Habilita o suporte à depuração remota adicionando a opção/debug ao especificado `<osentrylinenum>` .</li><li>**desconto.** Desabilita o suporte à depuração remota removendo a opção/debug do especificado <osentrylinenum> .</li><li>**editar.** Permite alterações nas configurações de porta e taxa de transmissão alterando os valores associados à opção/debug para o especificado <osentrylinenum> .</li></ul> |
| `/s <computer>` | Especifica o nome ou o endereço IP de um computador remoto (não use barras invertidas). O padrão é o computador local. |
| `/u <domain>\<user>`  | Executa o comando com as permissões de conta do usuário especificado por `<user>` ou `<domain>\<user>` . O padrão é as permissões do usuário conectado no momento no computador que emite o comando. |
| `/p <password>` | Especifica a senha da conta de usuário que é especificada no parâmetro **/u** . |
| `/port {COM1 | COM2 | COM3 | COM4}` |  Especifica a porta COM a ser usada para depuração. Não use esse parâmetro se a depuração estiver desabilitada. |
| `/baud {9600 | 19200 | 38400 | 57600 | 115200}` | Especifica a taxa de transmissão a ser usada para depuração. Não use esse parâmetro se a depuração estiver desabilitada. |
| `/id <osentrylinenum>` | Especifica o número da linha de entrada do sistema operacional na seção [Operating Systems] do arquivo de Boot.ini ao qual as opções de carregamento do sistema operacional são adicionadas. A primeira linha após o cabeçalho da seção [Operating Systems] é 1. |
| /? | Exibe a ajuda no prompt de comando. |

## <a name="examples"></a>Exemplos

Para usar o comando **Bootcfg/Debug** :

```
bootcfg /debug on /port com1 /id 2
bootcfg /debug edit /port com2 /baud 19200 /id 2
bootcfg /s srvmain /u maindom\hiropln /p p@ssW23 /debug off /id 2
```

## <a name="additional-references"></a>Referências adicionais

- [Chave da sintaxe de linha de comando](command-line-syntax-key.md)

- [comando Bootcfg](bootcfg.md)
