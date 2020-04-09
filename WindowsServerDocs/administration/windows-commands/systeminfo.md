---
title: systeminfo
description: O tópico de comandos do Windows para SystemInfo, que exibe informações detalhadas de configuração sobre um computador e seu sistema operacional, incluindo configuração do sistema operacional, informações de segurança, ID do produto e propriedades de hardware (como RAM, espaço em disco e placas de rede).
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 39954968-3c2e-4d3e-9d89-c9c43347461e
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 41c2a499bc10f5b44f250958471b90f4b88dfede
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80833569"
---
# <a name="systeminfo"></a>systeminfo

Exibe informações de configuração detalhadas sobre um computador e seu sistema operacional, incluindo configuração do sistema operacional, informações de segurança, ID do produto e propriedades de hardware (como RAM, espaço em disco e placas de rede).

Para obter exemplos de como usar esse comando, consulte [Exemplos](#BKMK_examples).

## <a name="syntax"></a>Sintaxe

```
Systeminfo [/s <Computer> [/u <Domain>\<UserName> [/p <Password>]]] [/fo {TABLE | LIST | CSV}] [/nh]
```

### <a name="parameters"></a>Parâmetros

|Parâmetro|Descrição|
|---------|-----------|
|/s \<computador >|Especifica o nome ou o endereço IP de um computador remoto (não use barras invertidas). O padrão é o computador local.|
|/u \<> de domínio\<nome de usuário >|Executa o comando com as permissões de conta da conta de usuário especificada. Se **/u** não for especificado, esse comando usará as permissões do usuário que está conectado no momento ao computador que está emitindo o comando.|
|/p \<senha >|Especifica a senha da conta de usuário que é especificada no parâmetro **/u** .|
|/Fo \<formato >|Especifica o formato de saída com um dos seguintes valores:</br>TABELA: exibe a saída em uma tabela.</br>LISTA: exibe a saída em uma lista.</br>CSV: exibe a saída no formato de valores separados por vírgula.|
|/NH|Suprime cabeçalhos de coluna na saída. Válido quando o parâmetro **/fo** é definido como Table ou CSV.|
|/?|Exibe a ajuda no prompt de comando.|

## <a name="examples"></a><a name=BKMK_examples></a>Disso

Para exibir as informações de configuração de um computador chamado srvmain, digite:

**SystemInfo/s srvmain**

Para exibir remotamente as informações de configuração de um computador chamado Srvmain2 que está localizado no domínio Maindom, digite:

**SystemInfo/s srvmain2/u maindom\hiropln**

Para exibir remotamente as informações de configuração (em formato de lista) para um computador chamado Srvmain2 que está localizado no domínio Maindom, digite:

**SystemInfo/s srvmain2/u maindom\hiropln/p p@ssW23/fo List**

## <a name="additional-references"></a>Referências adicionais

- [Chave da sintaxe de linha de comando](command-line-syntax-key.md)