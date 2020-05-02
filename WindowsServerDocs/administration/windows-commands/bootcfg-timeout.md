---
title: bootcfg timeout
description: Tópico de referência para o comando Bootcfg timeout, que altera o valor de tempo limite do sistema operacional.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: aa858eac-2bb7-4a27-a9bc-3e4a6eb8b2c6
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: e10ccab69ca58a6cef260546e965f90eecfc8beb
ms.sourcegitcommit: ab64dc83fca28039416c26226815502d0193500c
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 05/01/2020
ms.locfileid: "82708940"
---
# <a name="bootcfg-timeout"></a>bootcfg timeout

> Aplica-se a: Windows Server (canal semestral), Windows Server 2019, Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Altera o valor de tempo limite do sistema operacional.

## <a name="syntax"></a>Sintaxe

```
bootcfg /timeout <timeoutvalue> [/s <computer> [/u <domain>\<user> /p <password>]]
```

### <a name="parameters"></a>Parâmetros

| Parâmetro | Descrição |
| --------- | ----------- |
| `/timeout <timeoutvalue>` | Especifica o valor de tempo limite na seção [carregador de inicialização]. O `<timeoutvalue>` é o número de segundos que o usuário tem para selecionar um sistema operacional da tela do carregador de inicialização antes de o NTLDR carregar o padrão. O intervalo válido para `<timeoutvalue>` o é 0-999. Se o valor for 0, o NTLDR iniciará imediatamente o sistema operacional padrão sem exibir a tela do carregador de inicialização. |
| `/s <computer>` | Especifica o nome ou o endereço IP de um computador remoto (não use barras invertidas). O padrão é o computador local. |
| `/u <domain>\<user>`  | Executa o comando com as permissões de conta do usuário especificado por `<user>` ou `<domain>\<user>`. O padrão é as permissões do usuário conectado no momento no computador que emite o comando. |
| `/p <password>` | Especifica a senha da conta de usuário que é especificada no parâmetro **/u** . |
| /? | Exibe a ajuda no prompt de comando. |

## <a name="examples"></a>Exemplos

Para usar o comando **Bootcfg/Timeout** :

```
bootcfg /timeout 30
bootcfg /s srvmain /u maindom\hiropln /p p@ssW23 /timeout 50
```

## <a name="additional-references"></a>Referências adicionais

- [Chave da sintaxe de linha de comando](command-line-syntax-key.md)

- [comando Bootcfg](bootcfg.md)
