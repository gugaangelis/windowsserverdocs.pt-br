---
title: bootcfg timeout
description: Artigo de referência para o comando Bootcfg timeout, que altera o valor de tempo limite do sistema operacional.
ms.topic: reference
ms.assetid: aa858eac-2bb7-4a27-a9bc-3e4a6eb8b2c6
ms.author: lizross
author: eross-msft
manager: mtillman
ms.date: 10/16/2017
ms.openlocfilehash: 520889fce38eb48fe56b9b4c0a38277b5c7f8c8c
ms.sourcegitcommit: db2d46842c68813d043738d6523f13d8454fc972
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/10/2020
ms.locfileid: "89630112"
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
| `/timeout <timeoutvalue>` | Especifica o valor de tempo limite na seção [carregador de inicialização]. O `<timeoutvalue>` é o número de segundos que o usuário tem para selecionar um sistema operacional da tela do carregador de inicialização antes de o NTLDR carregar o padrão. O intervalo válido para o `<timeoutvalue>` é 0-999. Se o valor for 0, o NTLDR iniciará imediatamente o sistema operacional padrão sem exibir a tela do carregador de inicialização. |
| `/s <computer>` | Especifica o nome ou o endereço IP de um computador remoto (não use barras invertidas). O padrão é o computador local. |
| `/u <domain>\<user>`  | Executa o comando com as permissões de conta do usuário especificado por `<user>` ou `<domain>\<user>` . O padrão é as permissões do usuário conectado no momento no computador que emite o comando. |
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
