---
title: systeminfo
description: 'Tópico de comandos do Windows para * * *- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 39954968-3c2e-4d3e-9d89-c9c43347461e
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 3d08d3f86bdbd176aa4de157f58a58c9ea418470
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59841497"
---
# <a name="systeminfo"></a>systeminfo



Exibe configuração informações detalhadas sobre um computador e seu sistema operacional, incluindo a configuração do sistema operacional, informações de segurança, ID de produto e as propriedades de hardware (como RAM, espaço em disco e placas de rede).

Para obter exemplos de como usar esse comando, consulte [Exemplos](#BKMK_examples).

## <a name="syntax"></a>Sintaxe

```
Systeminfo [/s <Computer> [/u <Domain>\<UserName> [/p <Password>]]] [/fo {TABLE | LIST | CSV}] [/nh]
```

## <a name="parameters"></a>Parâmetros

|Parâmetro|Descrição|
|---------|-----------|
|/s \<Computer>|Especifica o nome ou endereço IP de um computador remoto (não use barras invertidas). O padrão é o computador local.|
|/u \<Domain>\<UserName>|Executa o comando com as permissões de conta da conta de usuário especificado. Se **/u** não for especificado, esse comando usa as permissões do usuário que está conectado no momento no computador que está emitindo o comando.|
|/p \<Password>|Especifica a senha da conta de usuário que é especificada na **/u** parâmetro.|
|/FO \<formato >|Especifica o formato de saída com um dos seguintes valores:</br>TABELA: Exibe a saída em uma tabela.</br>LISTA: Exibe a saída em uma lista.</br>CSV: Exibe a saída no formato de valores separados por vírgulas.|
|/nh|Suprime os cabeçalhos de coluna na saída. Válido quando o **/fo** parâmetro for definido como TABLE ou CSV.|
|/?|Exibe a ajuda no prompt de comando.|

## <a name="BKMK_examples"></a>Exemplos

Para exibir informações de configuração para um computador chamado Srvmain, digite:

**systeminfo /s srvmain**

Para exibir remotamente as informações de configuração para um computador chamado Srvmain2, que está localizado no domínio Maindom, digite:

**systeminfo /s srvmain2 /u maindom\hiropln**

Para exibir remotamente as informações de configuração (no formato de lista) para um computador chamado Srvmain2, que está localizado no domínio Maindom, digite:

**systeminfo /s srvmain2 /u maindom\hiropln /p p@ssW23 /fo list**

#### <a name="additional-references"></a>Referências adicionais

[Chave de sintaxe de linha de comando](command-line-syntax-key.md)