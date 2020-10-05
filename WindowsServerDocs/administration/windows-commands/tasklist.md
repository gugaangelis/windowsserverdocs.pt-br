---
title: tasklist
description: Artigo de referência para o comando tasklist, que exibe uma lista dos processos em execução no computador local ou remoto.
ms.topic: reference
ms.assetid: 8dbe30ee-1484-46be-917b-5ca3ff4fdc9c
ms.author: lizross
author: eross-msft
manager: mtillman
ms.date: 10/16/2017
ms.openlocfilehash: af89112d98cb7799a2fa910d562ac8c2a35bba19
ms.sourcegitcommit: 720455aad2bac78cf64997d196a13f35ea0acb73
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 10/05/2020
ms.locfileid: "91718053"
---
# <a name="tasklist"></a>tasklist

Exibe uma lista de processos em execução atualmente no computador local ou em um computador remoto. O **TaskList** substitui a ferramenta **tlist** .

> [!NOTE]
> Esse comando substitui a ferramenta **tlist** .

## <a name="syntax"></a>Sintaxe

```
tasklist [/s <computer> [/u [<domain>\]<username> [/p <password>]]] [{/m <module> | /svc | /v}] [/fo {table | list | csv}] [/nh] [/fi <filter> [/fi <filter> [ ... ]]]
```

### <a name="parameters"></a>Parâmetros

| Parâmetro | Descrição |
|--|--|
| /s `<computer>` | Especifica o nome ou o endereço IP de um computador remoto (não use barras invertidas). O padrão é o computador local. |
| /u `<domain>\<username>` | Executa o comando com as permissões de conta do usuário que é especificado pelo `<username>` ou pelo `<domain>\<username>` . O parâmetro **/u** pode ser especificado somente se **/s** também for especificado. O padrão é as permissões do usuário que está conectado no momento no computador que está emitindo o comando. |
| /p `<password>` | Especifica a senha da conta de usuário que é especificada no parâmetro **/u** . |
| opção `<module>` | Lista todas as tarefas com módulos DLL carregados que correspondem ao nome de padrão fornecido. Se o nome do módulo não for especificado, essa opção exibirá todos os módulos carregados por cada tarefa. |
| svc | Lista todas as informações de serviço para cada processo sem truncamento. Válido quando o parâmetro **/fo** é definido como **Table**. |
| /v | Exibe informações detalhadas da tarefa na saída. Para obter a saída detalhada completa sem truncamento, use **/v** e **/svc** juntos. |
| /Fo `{table | list | csv}` | Especifica o formato a ser usado para a saída. Os valores válidos são **tabela**, **lista**e **CSV**. O formato padrão de saída é **tabela**. |
| /NH | Suprime cabeçalhos de coluna na saída. Válido quando o parâmetro **/fo** é definido como **Table** ou **CSV**. |
| /Fi `<filter>` | Especifica os tipos de processos a serem incluídos ou excluídos da consulta. Você pode usar mais de um filtro ou usar o caractere curinga ( `\` ) para especificar todas as tarefas ou nomes de imagem. Os filtros válidos são listados na seção **nomes de filtro, operadores e valores** deste artigo.  |
| /? | Exibe a ajuda no prompt de comando. |

#### <a name="filter-names-operators-and-values"></a>Filtrar nomes, operadores e valores

| Nome do filtro | Operadores válidos | Valor (es) válido (s) |
|--|--|--|
| STATUS | eq, ne | `RUNNING | NOT RESPONDING | UNKNOWN`. Não há suporte para esse filtro se você especificar um sistema remoto. |
| IMAGENAME | eq, ne | Nome da imagem |
| PID | eq, ne, gt, lt, ge, le | Valor PID |
| SESSION | eq, ne, gt, lt, ge, le | Número da sessão |
| SESSIONNAME | eq, ne | Nome da sessão |
| CPUtime | eq, ne, gt, lt, ge, le | O tempo de CPU no formato *hh: mm: SS*, em que *mm* e *SS* estão entre 0 e 59 e *hh* é qualquer número não assinado |
| MEMUSAGE | eq, ne, gt, lt, ge, le | Uso de memória em KB |
| USERNAME | eq, ne | Qualquer nome de usuário válido ( `<user>` ou `<domain\user>` ) |
| SERVIÇOS | eq, ne | Nome do serviço |
| WINDOWTITLE | eq, ne | Título da janela. Não há suporte para esse filtro se você especificar um sistema remoto. |
| MÓDULOS | eq, ne | Nome da DLL |

## <a name="examples"></a>Exemplos

Para listar todas as tarefas com uma *ID de processo maior que 1000*e *exibi-las no formato CSV*, digite:

```
tasklist /v /fi "PID gt 1000" /fo csv
```

Para listar os processos do sistema em execução no momento, digite:

```
tasklist /fi "USERNAME ne NT AUTHORITY\SYSTEM" /fi "STATUS eq running"
```

Para listar informações detalhadas de todos os processos que estão em execução no momento, digite:

```
tasklist /v /fi "STATUS eq running"
```

Para listar todas as informações de serviço para processos no computador remoto *srvmain*, que tem um nome de dll *começando com ntdll*, digite:

```
tasklist /s srvmain /svc /fi "MODULES eq ntdll*"
```

Para listar os processos no computador remoto *srvmain*, usando as credenciais da sua conta de usuário conectada no momento, digite:

```
tasklist /s srvmain
```

Para listar os processos no computador remoto *srvmain*, usando as credenciais da *conta de usuário Hiropln*, digite:

```
tasklist /s srvmain /u maindom\hiropln /p p@ssW23
```

## <a name="additional-references"></a>Referências adicionais

- [Chave da sintaxe de linha de comando](command-line-syntax-key.md)
