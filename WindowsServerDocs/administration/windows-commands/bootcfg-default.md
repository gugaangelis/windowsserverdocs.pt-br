---
title: bootcfg default
description: Artigo de referência do comando Bootcfg padrão, que especifica a entrada do sistema operacional a ser designada como padrão.
ms.topic: reference
ms.assetid: e21824d7-8278-41d7-a2c5-ce09803d513a
ms.author: lizross
author: eross-msft
manager: mtillman
ms.date: 10/16/2017
ms.openlocfilehash: f96a7d09f580614763d7dc6803012969daa144c9
ms.sourcegitcommit: db2d46842c68813d043738d6523f13d8454fc972
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/10/2020
ms.locfileid: "89630229"
---
# <a name="bootcfg-default"></a>bootcfg default

> Aplica-se a: Windows Server (canal semestral), Windows Server 2019, Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Especifica a entrada do sistema operacional a ser designada como padrão.

## <a name="syntax"></a>Sintaxe

```
bootcfg /default [/s <computer> [/u <domain>\<user> /p <password>]] [/id <osentrylinenum>]
```

### <a name="parameters"></a>Parâmetros

| Parâmetro | Descrição |
| --------- | ----------- |
| `/s <computer>` | Especifica o nome ou o endereço IP de um computador remoto (não use barras invertidas). O padrão é o computador local. |
| `/u <domain>\<user>`  | Executa o comando com as permissões de conta do usuário especificado por `<user>` ou `<domain>\<user>` . O padrão é as permissões do usuário conectado no momento no computador que emite o comando. |
| `/p <password>` | Especifica a senha da conta de usuário que é especificada no parâmetro **/u** . |
| `/id <osentrylinenum>` | Especifica o número da linha de entrada do sistema operacional na seção [Operating Systems] do arquivo de Boot.ini ao qual as opções de carregamento do sistema operacional são adicionadas. A primeira linha após o cabeçalho da seção [Operating Systems] é 1. |
| /? | Exibe a ajuda no prompt de comando. |

## <a name="examples"></a>Exemplos

Para usar o comando **Bootcfg/default** :

```
bootcfg /default /id 2
bootcfg /default /s srvmain /u maindom\hiropln /p p@ssW23 /id 2
```

## <a name="additional-references"></a>Referências adicionais

- [Chave da sintaxe de linha de comando](command-line-syntax-key.md)

- [comando Bootcfg](bootcfg.md)
