---
title: systeminfo
description: 'Tópico de comandos do Windows para * * * *- '
ms.custom: na
ms.prod: windows-server
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
ms.openlocfilehash: 32a84a33c5339e9949648a4e40d71daf25c055d8
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71370688"
---
# <a name="systeminfo"></a>systeminfo



Exibe informações de configuração detalhadas sobre um computador e seu sistema operacional, incluindo configuração do sistema operacional, informações de segurança, ID do produto e propriedades de hardware (como RAM, espaço em disco e placas de rede).

Para obter exemplos de como usar esse comando, consulte [Exemplos](#BKMK_examples).

## <a name="syntax"></a>Sintaxe

```
Systeminfo [/s <Computer> [/u <Domain>\<UserName> [/p <Password>]]] [/fo {TABLE | LIST | CSV}] [/nh]
```

## <a name="parameters"></a>Parâmetros

|Parâmetro|Descrição|
|---------|-----------|
|/s \<Computer >|Especifica o nome ou o endereço IP de um computador remoto (não use barras invertidas). O padrão é o computador local.|
|/u \<Domain > \<UserName >|Executa o comando com as permissões de conta da conta de usuário especificada. Se **/u** não for especificado, esse comando usará as permissões do usuário que está conectado no momento ao computador que está emitindo o comando.|
|/p \<senha >|Especifica a senha da conta de usuário que é especificada no parâmetro **/u** .|
|/Fo \<Format >|Especifica o formato de saída com um dos seguintes valores:</br>TABELA Exibe a saída em uma tabela.</br>LISTA Exibe a saída em uma lista.</br>CSV Exibe a saída em formato de valores separados por vírgula.|
|/NH|Suprime cabeçalhos de coluna na saída. Válido quando o parâmetro **/fo** é definido como Table ou CSV.|
|/?|Exibe a ajuda no prompt de comando.|

## <a name="BKMK_examples"></a>Disso

Para exibir as informações de configuração de um computador chamado srvmain, digite:

**SystemInfo/s srvmain**

Para exibir remotamente as informações de configuração de um computador chamado Srvmain2 que está localizado no domínio Maindom, digite:

**SystemInfo/s srvmain2/u maindom\hiropln**

Para exibir remotamente as informações de configuração (em formato de lista) para um computador chamado Srvmain2 que está localizado no domínio Maindom, digite:

**SystemInfo/s srvmain2/u maindom\hiropln/p p@ssW23/fo List**

#### <a name="additional-references"></a>Referências adicionais

[Chave da sintaxe de linha de comando](command-line-syntax-key.md)