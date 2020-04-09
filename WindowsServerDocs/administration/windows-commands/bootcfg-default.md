---
title: bootcfg default
description: O tópico de comandos do Windows para Bootcfg Default, que especifica a entrada do sistema operacional para designar como o padrão.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: e21824d7-8278-41d7-a2c5-ce09803d513a
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 517cf444a5517b3d612266b57b428e47ac60d4ef
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80848560"
---
# <a name="bootcfg-default"></a>bootcfg default

>Aplicável a: Windows Server (canal semestral), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Especifica a entrada do sistema operacional a ser designada como padrão.

## <a name="syntax"></a>Sintaxe
```
bootcfg /default [/s <computer> [/u <Domain>\<User> /p <Password>]] [/id <OSEntryLineNum>]
```
### <a name="parameters"></a>Parâmetros

|      Parâmetro       |                                                                                             Descrição                                                                                              |
|----------------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
|    /s <computer>     |                                          Especifica o nome ou o endereço IP de um computador remoto (não use barras invertidas). O padrão é o computador local.                                          |
| /u <Domain>\\<User>  | Executa o comando com as permissões de conta do usuário especificado por <User> ou <Domain>\\<User>. O padrão é as permissões do usuário conectado no momento no computador que emite o comando. |
|    /p <Password>     |                                                        Especifica a senha da conta de usuário que é especificada no parâmetro **/u** .                                                         |
| /ID <OSEntryLineNum> | Especifica o número da linha de entrada do sistema operacional na seção [Operating Systems] do arquivo boot. ini para designar como padrão. A primeira linha após o cabeçalho da seção [Operating Systems] é 1.  |
|          /?          |                                                                                 Exibe a ajuda no prompt de comando.                                                                                 |

## <a name="examples"></a><a name=BKMK_examples></a>Disso
Os exemplos a seguir mostram como você pode usar o comando **Bootcfg/default**:
```
bootcfg /default /id 2
bootcfg /default /s srvmain /u maindom\hiropln /p p@ssW23 /id 2
```
## <a name="additional-references"></a>Referências adicionais
- [Chave da sintaxe de linha de comando](command-line-syntax-key.md)
