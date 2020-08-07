---
title: tasklist
description: Saiba como exibir uma lista dos processos em execução no computador local ou remoto.
ms.topic: article
ms.assetid: 8dbe30ee-1484-46be-917b-5ca3ff4fdc9c
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: a4f87c4cc2dc80c67e2004c929fa23aea8791fb9
ms.sourcegitcommit: 53d526bfeddb89d28af44210a23ba417f6ce0ecf
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/06/2020
ms.locfileid: "87881817"
---
# <a name="tasklist"></a>tasklist

Exibe uma lista de processos em execução atualmente no computador local ou em um computador remoto. O **TaskList** substitui a ferramenta **tlist** .



## <a name="syntax"></a>Sintaxe

```
tasklist [/s <Computer> [/u [<Domain>\]<UserName> [/p <Password>]]] [{/m <Module> | /svc | /v}] [/fo {table | list | csv}] [/nh] [/fi <Filter> [/fi <Filter> [ ... ]]]
```

### <a name="parameters"></a>Parâmetros

|          Parâmetro           |                                                                                                                                            Descrição                                                                                                                                             |
|------------------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
|        /s\<Computer>        |                                                                                         Especifica o nome ou o endereço IP de um computador remoto (não use barras invertidas). O padrão é o computador local.                                                                                         |
| t\<Domain>\\\]\<UserName> | Executa o comando com as permissões de conta do usuário que é especificado por *username* ou *Domain* \* username<em>. \* \* /u</em> \* pode ser especificado somente se **/s** for especificado. O padrão é as permissões do usuário que está conectado no momento no computador que está emitindo o comando. |
|        /p\<Password>        |                                                                                                       Especifica a senha da conta de usuário que é especificada no parâmetro **/u** .                                                                                                        |
|         opção\<Module>         |                                                               Lista todas as tarefas com módulos DLL carregados que correspondem ao nome de padrão fornecido. Se o nome do módulo não for especificado, essa opção exibirá todos os módulos carregados por cada tarefa.                                                                |
|             /svc             |                                                                                    Lista todas as informações de serviço para cada processo sem truncamento. Válido quando o parâmetro **/fo** é definido como **Table**.                                                                                    |
|              /v              |                                                                                 Exibe informações detalhadas da tarefa na saída. Para obter a saída detalhada completa sem truncamento, use **/v** e **/svc** juntos.                                                                                 |
|  /Fo {CSV da lista de tabelas \| \| }  |                                                                             Especifica o formato a ser usado para a saída. Os valores válidos são **tabela**, **lista**e **CSV**. O formato padrão de saída é **tabela**.                                                                             |
|             /NH              |                                                                                             Suprime cabeçalhos de coluna na saída. Válido quando o parâmetro **/fo** é definido como **Table** ou **CSV**.                                                                                              |
|        /Fi\<Filter>         |                                                                          Especifica os tipos de processos a serem incluídos ou excluídos da consulta. Consulte a tabela a seguir para obter nomes de filtro, operadores e valores válidos.                                                                          |
|              /?              |                                                                                                                                Exibe a ajuda no prompt de comando.                                                                                                                                |

### <a name="filter-names-operators-and-values"></a>Filtrar nomes, operadores e valores

| Nome do filtro |    Operadores válidos     |                                                                 Valores válidos                                                                 |
|-------------|------------------------|----------------------------------------------------------------------------------------------------------------------------------------------|
|   STATUS    |         eq, ne         |                                                                   RUNNING                                                                    |
|  IMAGENAME  |         eq, ne         |                                                                  Nome da imagem                                                                  |
|     PID     | eq, ne, gt, lt, ge, le |                                                                  Valor PID                                                                   |
|   SESSION   | eq, ne, gt, lt, ge, le |                                                                Número da sessão                                                                |
| SESSIONNAME |         eq, ne         |                                                                 Nome da sessão                                                                 |
|   CPUTIME   | eq, ne, gt, lt, ge, le | O tempo de CPU no formato <em>hh</em>**:**<em>mm</em>**:**<em>SS</em>, em que *mm* e *SS* estão entre 0 e 59 e *hh* é qualquer número não assinado |
|  MEMUSAGE   | eq, ne, gt, lt, ge, le |                                                              Uso de memória em KB                                                              |
|  USERNAME   |         eq, ne         |                                                             Um nome de usuário válido                                                              |
|  SERVIÇOS   |         eq, ne         |                                                                 Nome do serviço                                                                 |
| WINDOWTITLE |         eq, ne         |                                                                 Título da janela                                                                 |
|   MÓDULOS   |         eq, ne         |                                                                   Nome da DLL                                                                   |

## <a name="remarks"></a>Comentários

Não há suporte para os filtros de STATUS e WINDOWTITLE quando um sistema remoto é especificado.

## <a name="examples"></a><a name="BKMK_examples"></a>Exemplos

Para listar todas as tarefas com uma ID de processo maior que 1000 e exibi-las no formato CSV, digite:
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
Para listar todas as informações de serviço para processos no computador remoto "srvmain" que têm um nome de DLL começando com "ntdll", digite:
```
tasklist /s srvmain /svc /fi "MODULES eq ntdll*"
```
Para listar os processos no computador remoto "srvmain", usando as credenciais da sua conta de usuário conectada no momento, digite:
```
tasklist /s srvmain
```
Para listar os processos no computador remoto "srvmain", usando as credenciais da conta de usuário Hiropln, digite:
```
tasklist /s srvmain /u maindom\hiropln /p p@ssW23
```

## <a name="additional-references"></a>Referências adicionais

- [Chave da sintaxe de linha de comando](command-line-syntax-key.md)