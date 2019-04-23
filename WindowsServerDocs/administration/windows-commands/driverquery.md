---
title: driverquery
description: 'Tópico de comandos do Windows para * * *- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 92ca4b84-e4e2-405b-9f31-bf6db9f66839
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 6436ea47e3ec5c7c9ceee9fd50d052dba4a49724
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59861167"
---
# <a name="driverquery"></a>driverquery



Permite que um administrador exibir uma lista de drivers de dispositivo instalados e suas propriedades. Se usado sem parâmetros, **driverquery** é executado no computador local.

Para obter exemplos de como usar esse comando, consulte [Exemplos](#BKMK_examples).

## <a name="syntax"></a>Sintaxe

```
driverquery [/s <System> [/u [<Domain>\]<Username> [/p <Password>]]] [/fo {table | list | csv}] [/nh] [/v | /si]
```

## <a name="parameters"></a>Parâmetros

|Parâmetro|Descrição|
|---------|-----------|
|/s \<System>|Especifica o nome ou endereço IP de um computador remoto. Não use barras invertidas. O padrão é o computador local.|
|/u [\<Domain>\]<Username>|Executa o comando com as credenciais da conta de usuário conforme especificado por *usuário* ou *domínio*\*usuário *. Por padrão, **/s** usa as credenciais do usuário que está conectado no momento no computador que está emitindo o comando. **/u** não pode ser usado, a menos que **/s** for especificado.|
|/p \<Password>|Especifica a senha da conta de usuário que é especificada na **/u** parâmetro. **/p** não pode ser usado, a menos que **/u** for especificado.|
|/FO {tabela | lista | csv}|Especifica o formato para exibir as informações do driver. Os valores válidos são **tabela**, **lista**, e **csv**. O formato padrão para a saída é **tabela**.|
|/nh|Omite a linha de cabeçalho a partir das informações exibidas de driver. Não é válido se o **/fo** parâmetro for definido como **lista**.|
|/v|Exibe a saída detalhada. **/v** não é válido para drivers assinados.|
|/si|Fornece informações sobre drivers assinados.|
|/?|Exibe a ajuda no prompt de comando.|

## <a name="BKMK_examples"></a>Exemplos

Para exibir uma lista de drivers de dispositivo instalados no computador local, digite:
```
driverquery 
```
Para exibir a saída em um formato de valores separados por vírgulas (CSV), digite:
```
driverquery /fo csv 
```
Para ocultar a linha de cabeçalho na saída, digite:
```
driverquery /nh 
```
Para usar o **driverquery** comando em um servidor remoto denominado **server1** usando suas credenciais atuais no computador local, digite:
```
driverquery /s server1
```
Para usar o **driverquery** comando em um servidor remoto denominado **server1** usando as credenciais da **user1** no domínio **maindom**, tipo:
```
driverquery /s server1 /u maindom\user1 /p p@ssw3d
```

#### <a name="additional-references"></a>Referências adicionais

[Chave de sintaxe de linha de comando](command-line-syntax-key.md)