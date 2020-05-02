---
title: bootcfg raw
description: Tópico de referência para o comando de Bootcfg RAW, que adiciona opções de carregamento do sistema operacional, especificadas como uma cadeia de caracteres, a uma entrada do sistema operacional na seção do sistema operacional do arquivo boot. ini.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: e3458749-b0a0-460f-a022-3ff199a71f27
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: b9127b278a41bb8f1f36f7b45c544bf09f5c4ff7
ms.sourcegitcommit: ab64dc83fca28039416c26226815502d0193500c
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 05/01/2020
ms.locfileid: "82708951"
---
# <a name="bootcfg-raw"></a>bootcfg raw

> Aplica-se a: Windows Server (canal semestral), Windows Server 2019, Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Adiciona opções de carregamento do sistema operacional especificadas como uma cadeia de caracteres a uma entrada do sistema operacional na seção [Operating Systems] do arquivo boot. ini. Esse comando substitui todas as opções de entrada do sistema operacional existente.

## <a name="syntax"></a>Sintaxe

```
bootcfg /raw [/s <computer> [/u <domain>\<user> /p <password>]] <osloadoptionsstring> [/id <osentrylinenum>] [/a]
```

### <a name="parameters"></a>Parâmetros

| Parâmetro | Descrição |
| --------- | ----------- |
| `/s <computer>` | Especifica o nome ou o endereço IP de um computador remoto (não use barras invertidas). O padrão é o computador local. |
| `/u <domain>\<user>`  | Executa o comando com as permissões de conta do usuário especificado por `<user>` ou `<domain>\<user>`. O padrão é as permissões do usuário conectado no momento no computador que emite o comando. |
| `/p <password>` | Especifica a senha da conta de usuário que é especificada no parâmetro **/u** . |
| `<osloadoptionsstring>` | Especifica as opções de carregamento do sistema operacional a serem adicionadas à entrada do sistema operacional. Essas opções de carregamento substituem qualquer opção de carregamento existente associada à entrada do sistema operacional. Não há nenhuma validação em relação `<osloadoptions>` ao parâmetro.
| `/id <osentrylinenum>` | Especifica o número da linha de entrada do sistema operacional na seção [Operating Systems] do arquivo boot. ini ao qual as opções de carregamento do sistema operacional são adicionadas. A primeira linha após o cabeçalho da seção [Operating Systems] é 1. |
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
