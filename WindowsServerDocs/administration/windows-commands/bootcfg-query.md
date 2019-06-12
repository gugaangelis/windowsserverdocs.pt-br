---
title: bootcfg query
description: Tópico de comandos do Windows para **bootcfg consulta** -consultas e exibe o carregador de inicialização] e [sistemas operacionais] seção entradas do Boot. ini.
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: a4cacfd1-10a6-4a11-b0c5-f8abde72bfc8
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: e79acc100a9ec9955f2692a3c6ee812d0310b687
ms.sourcegitcommit: eaf071249b6eb6b1a758b38579a2d87710abfb54
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 05/31/2019
ms.locfileid: "66434736"
---
# <a name="bootcfg-query"></a>bootcfg query

>Aplica-se a: Windows Server (canal semestral), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Consulta e exibe o carregador de inicialização] e [sistemas operacionais] seção entradas do Boot. ini.

## <a name="syntax"></a>Sintaxe
```
bootcfg /query [/s <computer> [/u <Domain>\<User> /p <Password>]]
```
## <a name="parameters"></a>Parâmetros

|        Termo         |                                                                                             Definição                                                                                              |
|---------------------|-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
|    /s <computer>    |                                         Especifica o nome ou endereço IP de um computador remoto (não use barras invertidas). O padrão é o computador local.                                          |
| /u <Domain>\\<User> | Executa o comando com as permissões de conta do usuário especificado por <User>ou <Domain> \\ <User>. O padrão é que as permissões do usuário no computador que está emitindo o comando de logon atual. |
|    /p <Password>    |                                                        Especifica a senha da conta de usuário que é especificada na **/u** parâmetro.                                                        |
|         /?          |                                                                                Exibe a ajuda no prompt de comando.                                                                                 |

##### <a name="remarks"></a>Comentários
- A seguir está um exemplo dos **bootcfg /query** saída:
  ```
  Boot Loader Settings
  ----------
  timeout: 30
  default: multi(0)disk(0)rdisk(0)partition(1)\WINDOWS
  Boot Entries
  ------
  Boot entry ID:   1
  Friendly Name:   ""
  path:            multi(0)disk(0)rdisk(0)partition(1)\WINDOWS
  OS Load Options: /fastdetect /debug /debugport=com1:
  ```
- A parte de configurações do carregador de inicialização do **bootcfg consulta** saída exibe cada entrada na seção [carregador de inicialização do Boot. ini.
- A parte de entradas de inicialização do **bootcfg consulta** saída exibe os seguintes detalhes para cada entrada do sistema operacional na seção [operating systems] de Boot. ini: ID da entrada de inicialização, nome amigável, caminho e opções de carregamento do sistema operacional.
  ## <a name="BKMK_examples"></a>Exemplos
  Os exemplos a seguir mostram como você pode usar o **bootcfg /query** comando:
  ```
  bootcfg /query
  bootcfg /query /s srvmain /u maindom\hiropln /p p@ssW23
  bootcfg /query /u hiropln /p p@ssW23
  ```
  #### <a name="additional-references"></a>Referências adicionais
  [Chave da sintaxe de linha de comando](command-line-syntax-key.md)
