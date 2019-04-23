---
title: tasklist
description: Saiba como exibir uma lista de processos em execução no computador local ou remoto.
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 8dbe30ee-1484-46be-917b-5ca3ff4fdc9c
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: abb36c8c794836387af5547f3706e910dc06fa42
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59842997"
---
# <a name="tasklist"></a>tasklist

Exibe uma lista de processos em execução atualmente no computador local ou em um computador remoto. **Lista de tarefas** substitui o **tlist** ferramenta.

Para obter exemplos de como usar esse comando, consulte [Exemplos](#BKMK_examples).

## <a name="syntax"></a>Sintaxe

```
tasklist [/s <Computer> [/u [<Domain>\]<UserName> [/p <Password>]]] [{/m <Module> | /svc | /v}] [/fo {table | list | csv}] [/nh] [/fi <Filter> [/fi <Filter> [ ... ]]]
```

## <a name="parameters"></a>Parâmetros

|Parâmetro|Descrição|
|---------|-----------|
|/s \<Computer>|Especifica o nome ou endereço IP de um computador remoto (não use barras invertidas). O padrão é o computador local.|
|/u [\<domínio >\\\]\<nome de usuário >|Executa o comando com as permissões de conta do usuário que é especificado pelo *nome de usuário* ou *domínio*\** nome de usuário. **/u** pode ser especificado somente se **/s** for especificado. O padrão é que as permissões do usuário que está conectado no momento no computador que está emitindo o comando.|
|/p \<Password>|Especifica a senha da conta de usuário que é especificada na **/u** parâmetro.|
|/m \<Module>|Lista todas as tarefas com DLL módulos carregados que corresponde ao nome padrão fornecido. Se o nome do módulo não for especificado, esta opção exibe todos os módulos carregados por cada tarefa.|
|/svc|Lista todas as informações de serviço para cada processo sem truncamento. Válido quando o **/fo** parâmetro for definido como **tabela**.|
|/v|Exibe informações detalhadas de tarefa na saída. Para a saída detalhada completa sem truncamento, use **/v** e **/svc** juntos.|
|/FO {tabela \| lista \| csv}|Especifica o formato a ser usado para a saída. Os valores válidos são **tabela**, **lista**, e **csv**. O formato padrão para a saída é **tabela**.|
|/nh|Suprime os cabeçalhos de coluna na saída. Válido quando o **/fo** parâmetro for definido como **tabela** ou **csv**.|
|/fi \<Filter>|Especifica os tipos de processos a serem incluídos ou excluídos da consulta. Consulte a tabela a seguir para nomes de filtro válido, operadores e valores.|
|/?|Exibe a ajuda no prompt de comando.|

### <a name="filter-names-operators-and-values"></a>Os nomes de filtro, operadores e valores

|Nome do filtro|Operadores válidos|Valores válidos|
|-----------|---------------|------------|
|STATUS|eq, ne|EM EXECUÇÃO | NÃO RESPONDENDO | DESCONHECIDO|
|IMAGENAME|eq, ne|Nome da imagem|
|PID|eq, ne, gt, lt, ge, le|Valor PID|
|SESSÃO|eq, ne, gt, lt, ge, le|Número da sessão|
|SESSIONNAME|eq, ne|Nome da sessão|
|CPUTIME|eq, ne, gt, lt, ge, le|Tempo de CPU no formato *HH ***:*** MM ***:*** SS*, onde *MM* e *SS* estão entre 0 e 59 e *HH* é qualquer número de unsigned|
|MEMUSAGE|eq, ne, gt, lt, ge, le|Uso de memória em KB|
|NOME DE USUÁRIO|eq, ne|Qualquer nome de usuário válido|
|SERVIÇOS|eq, ne|Nome do serviço|
|WINDOWTITLE|eq, ne|Título da janela|
|MÓDULOS|eq, ne|Nome da DLL|

## <a name="remarks"></a>Comentários

Não há suporte para os filtros STATUS e WINDOWTITLE quando um sistema remoto é especificado.

## <a name="BKMK_examples"></a>Exemplos

Para listar todas as tarefas com uma ID de processo maior que 1000 e exibi-los no formato CSV, digite:
```
tasklist /v /fi "PID gt 1000" /fo csv
```
Para listar os processos de sistema que estão sendo executados, digite:
```
tasklist /fi "USERNAME ne NT AUTHORITY\SYSTEM" /fi "STATUS eq running"
```
Para listar informações detalhadas de todos os processos que estão em execução no momento, digite:
```
tasklist /v /fi "STATUS eq running"
```
Para listar todas as informações de serviço para processos no computador remoto "Srvmain" que têm um nome DLL começa com "ntdll", digite:
```
tasklist /s srvmain /svc /fi "MODULES eq ntdll*"
```
Para listar os processos no computador remoto "Srvmain", usando as credenciais da conta do usuário conectado no momento, digite:
```
tasklist /s srvmain 
```
Para listar os processos no computador remoto "Srvmain", usando as credenciais da conta de usuário Hiropln, digite:
```
tasklist /s srvmain /u maindom\hiropln /p p@ssW23
```

#### <a name="additional-references"></a>Referências adicionais

[Chave de sintaxe de linha de comando](command-line-syntax-key.md)