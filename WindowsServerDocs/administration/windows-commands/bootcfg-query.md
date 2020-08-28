---
title: bootcfg query
description: Artigo de referência para o comando de consulta Bootcfg, que consulta e exibe as entradas do carregador de inicialização e da seção do sistema operacional de Boot.ini.
ms.topic: reference
ms.assetid: a4cacfd1-10a6-4a11-b0c5-f8abde72bfc8
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 9e6b5a34a0af467c12549f4921d8d5c99e79aaa9
ms.sourcegitcommit: 96d46c702e7a9c3a321bbbb5284f73911c7baa3c
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/27/2020
ms.locfileid: "89034434"
---
# <a name="bootcfg-query"></a>bootcfg query

> Aplica-se a: Windows Server (canal semestral), Windows Server 2019, Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Consulta e exibe as entradas da seção [boot loader] e [Operating Systems] de Boot.ini.

## <a name="syntax"></a>Sintaxe

```
bootcfg /query [/s <computer> [/u <domain>\<user> /p <password>]]
```

### <a name="parameters"></a>Parâmetros

| Parâmetro | Descrição |
| --------- | ----------- |
| `/s <computer>` | Especifica o nome ou o endereço IP de um computador remoto (não use barras invertidas). O padrão é o computador local. |
| `/u <domain>\<user>`  | Executa o comando com as permissões de conta do usuário especificado por `<user>` ou `<domain>\<user>` . O padrão é as permissões do usuário conectado no momento no computador que emite o comando. |
| `/p <password>` | Especifica a senha da conta de usuário que é especificada no parâmetro **/u** . |
| /? | Exibe a ajuda no prompt de comando. |

#### <a name="sample-output"></a>Saída de exemplo

Exemplo de saída para o comando **Bootcfg/Query** :

```
Boot Loader Settings
----------
timeout: 30
default: multi(0)disk(0)rdisk(0)partition(1)\WINDOWS
Boot Entries
------
Boot entry ID:   1
Friendly Name:
path: multi(0)disk(0)rdisk(0)partition(1)\WINDOWS
OS Load Options: /fastdetect /debug /debugport=com1:
```

- A área **configurações do carregador de inicialização** mostra cada entrada na seção [carregador de inicialização] do Boot.ini.

- A área **entradas de inicialização** mostra mais detalhes para cada entrada do sistema operacional na seção [Operating Systems] do Boot.ini

## <a name="examples"></a>Exemplos

Para usar o comando **Bootcfg/Query** :

```
bootcfg /query
bootcfg /query /s srvmain /u maindom\hiropln /p p@ssW23
bootcfg /query /u hiropln /p p@ssW23
```

## <a name="additional-references"></a>Referências adicionais

- [Chave da sintaxe de linha de comando](command-line-syntax-key.md)

- [comando Bootcfg](bootcfg.md)
