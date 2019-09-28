---
title: bootcfg query
description: O tópico de comandos do Windows para **Bootcfg Query** -consulta e exibe as entradas da seção [boot loader] e [Operating Systems] do boot. ini.
ms.custom: na
ms.prod: windows-server
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
ms.openlocfilehash: ae82357cfe178343872448c2ebd46c49a797b5a9
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71379915"
---
# <a name="bootcfg-query"></a>bootcfg query

>Aplica-se a: Windows Server (canal semestral), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Consulta e exibe as entradas da seção [boot loader] e [Operating Systems] do boot. ini.

## <a name="syntax"></a>Sintaxe
```
bootcfg /query [/s <computer> [/u <Domain>\<User> /p <Password>]]
```
## <a name="parameters"></a>Parâmetros

|        Termo         |                                                                                             Definição                                                                                              |
|---------------------|-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
|    /s <computer>    |                                         Especifica o nome ou o endereço IP de um computador remoto (não use barras invertidas). O padrão é o computador local.                                          |
| /u <Domain> @ no__t-1 @ no__t-2 | Executa o comando com as permissões de conta do usuário especificado por <User>or <Domain> @ no__t-2 @ no__t-3. O padrão é as permissões do usuário conectado no momento no computador que emite o comando. |
|    /p <Password>    |                                                        Especifica a senha da conta de usuário que é especificada no parâmetro **/u** .                                                        |
|         /?          |                                                                                Exibe a ajuda no prompt de comando.                                                                                 |

##### <a name="remarks"></a>Comentários
- Veja a seguir um exemplo de saída de **Bootcfg/Query** :
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
- A parte de configurações do carregador de inicialização da saída da **consulta Bootcfg** exibe cada entrada na seção [boot loader] do boot. ini.
- A parte de entradas de inicialização da saída da **consulta Bootcfg** exibe os detalhes a seguir para cada entrada do sistema operacional na seção [Operating Systems] do boot. ini: ID de entrada de inicialização, nome amigável, caminho e opções de carregamento de so.
  ## <a name="BKMK_examples"></a>Disso
  Os exemplos a seguir mostram como você pode usar o comando **Bootcfg/Query** :
  ```
  bootcfg /query
  bootcfg /query /s srvmain /u maindom\hiropln /p p@ssW23
  bootcfg /query /u hiropln /p p@ssW23
  ```
  #### <a name="additional-references"></a>Referências adicionais
  [Chave da sintaxe de linha de comando](command-line-syntax-key.md)
