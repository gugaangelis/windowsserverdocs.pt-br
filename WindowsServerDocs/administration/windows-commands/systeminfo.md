---
title: systeminfo
description: Tópico de referência para SystemInfo, que exibe informações detalhadas de configuração sobre um computador e seu sistema operacional, incluindo configuração do sistema operacional, informações de segurança, ID do produto e propriedades de hardware (como RAM, espaço em disco e placas de rede).
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 39954968-3c2e-4d3e-9d89-c9c43347461e
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 6e95a61e4312e37288bc587264cf35240920abd3
ms.sourcegitcommit: ab64dc83fca28039416c26226815502d0193500c
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 05/01/2020
ms.locfileid: "82721578"
---
# <a name="systeminfo"></a>systeminfo

Exibe informações de configuração detalhadas sobre um computador e seu sistema operacional, incluindo configuração do sistema operacional, informações de segurança, ID do produto e propriedades de hardware (como RAM, espaço em disco e placas de rede).



## <a name="syntax"></a>Sintaxe

```
Systeminfo [/s <Computer> [/u <Domain>\<UserName> [/p <Password>]]] [/fo {TABLE | LIST | CSV}] [/nh]
```

### <a name="parameters"></a>Parâmetros

|Parâmetro|Descrição|
|---------|-----------|
|/s \<> do computador|Especifica o nome ou o endereço IP de um computador remoto (não use barras invertidas). O padrão é o computador local.|
|/u \<domínio>\<nome de usuário>|Executa o comando com as permissões de conta da conta de usuário especificada. Se **/u** não for especificado, esse comando usará as permissões do usuário que está conectado no momento ao computador que está emitindo o comando.|
|/p \<senha>|Especifica a senha da conta de usuário que é especificada no parâmetro **/u** .|
|/Fo \<formato>|Especifica o formato de saída com um dos seguintes valores:</br>TABELA: exibe a saída em uma tabela.</br>LISTA: exibe a saída em uma lista.</br>CSV: exibe a saída no formato de valores separados por vírgula.|
|/NH|Suprime cabeçalhos de coluna na saída. Válido quando o parâmetro **/fo** é definido como Table ou CSV.|
|/?|Exibe a ajuda no prompt de comando.|

## <a name="examples"></a>Exemplos

Para exibir as informações de configuração de um computador chamado srvmain, digite:

**SystemInfo/s srvmain**

Para exibir remotamente as informações de configuração de um computador chamado Srvmain2 que está localizado no domínio Maindom, digite:

**SystemInfo/s srvmain2/u maindom\hiropln**

Para exibir remotamente as informações de configuração (em formato de lista) para um computador chamado Srvmain2 que está localizado no domínio Maindom, digite:

**SystemInfo/s srvmain2/u maindom\hiropln/p p@ssW23 /fo List**

## <a name="additional-references"></a>Referências adicionais

- [Chave da sintaxe de linha de comando](command-line-syntax-key.md)