---
title: bootcfg query
description: O tópico de comandos do Windows para a consulta Bootcfg, que consulta e exibe as entradas do carregador de inicialização e da seção do sistema operacional do arquivo boot. ini.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: a4cacfd1-10a6-4a11-b0c5-f8abde72bfc8
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 1ac80c802b1d30dcf7221f94f761233c6b6fc6b6
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80848519"
---
# <a name="bootcfg-query"></a>bootcfg query

>Aplicável a: Windows Server (canal semestral), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Consulta e exibe as entradas da seção [boot loader] e [Operating Systems] do boot. ini.

## <a name="syntax"></a>Sintaxe
```
bootcfg /query [/s <computer> [/u <Domain>\<User> /p <Password>]]
```
### <a name="parameters"></a>Parâmetros

|        Termo         |                                                                                             Definição                                                                                              |
|---------------------|-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
|    /s <computer>    |                                         Especifica o nome ou o endereço IP de um computador remoto (não use barras invertidas). O padrão é o computador local.                                          |
| /u <Domain>\\<User> | Executa o comando com as permissões de conta do usuário especificado por <User>ou <Domain>\\<User>. O padrão é as permissões do usuário conectado no momento no computador que emite o comando. |
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
  Friendly Name:   
  path:            multi(0)disk(0)rdisk(0)partition(1)\WINDOWS
  OS Load Options: /fastdetect /debug /debugport=com1:
  ```
- A parte de configurações do carregador de inicialização da saída da **consulta Bootcfg** exibe cada entrada na seção [boot loader] do boot. ini.
- A parte de entradas de inicialização da saída da **consulta Bootcfg** exibe os detalhes a seguir para cada entrada do sistema operacional na seção [Operating Systems] de boot. ini: ID da entrada de inicialização, nome amigável, caminho e opções de carregamento do so.
  ## <a name="examples"></a><a name=BKMK_examples></a>Disso
  Os exemplos a seguir mostram como você pode usar o comando **Bootcfg/Query** :
  ```
  bootcfg /query
  bootcfg /query /s srvmain /u maindom\hiropln /p p@ssW23
  bootcfg /query /u hiropln /p p@ssW23
  ```
  ## <a name="additional-references"></a>Referências adicionais
  - [Chave da sintaxe de linha de comando](command-line-syntax-key.md)
