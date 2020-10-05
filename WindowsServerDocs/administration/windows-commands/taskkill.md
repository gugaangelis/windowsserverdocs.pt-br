---
title: taskkill
description: Artigo de referência para o comando taskkill, que encerra uma ou mais tarefas ou processos.
ms.topic: reference
ms.assetid: 2b71e792-08b6-46d4-95a5-cb6336a79524
ms.author: lizross
author: eross-msft
manager: mtillman
ms.date: 10/16/2017
ms.openlocfilehash: e022bc980e2acd7fb70bf13af52f8096fd6aaa1f
ms.sourcegitcommit: 720455aad2bac78cf64997d196a13f35ea0acb73
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 10/05/2020
ms.locfileid: "91718073"
---
# <a name="taskkill"></a>taskkill

> Aplica-se a: Windows Server (canal semestral), Windows Server 2019, Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Termina uma ou mais tarefas ou processos. Os processos podem ser encerrados pela identificação do processo ou nome da imagem. Você pode usar o comando de [comando tasklist](tasklist.md) para determinar a ID do processo (PID) do processo a ser finalizado.

> [!NOTE]
> Esse comando substitui a ferramenta **Kill** .

## <a name="syntax"></a>Sintaxe

```
taskkill [/s <computer> [/u [<domain>\]<username> [/p [<password>]]]] {[/fi <filter>] [...] [/pid <processID> | /im <imagename>]} [/f] [/t]
```

### <a name="parameters"></a>Parâmetros

| Parâmetro | Descrição |
|--|--|
|  /s `<computer>` | Especifica o nome ou o endereço IP de um computador remoto (não use barras invertidas). O padrão é o computador local. |
| /u `<domain>\<username>` | Executa o comando com as permissões de conta do usuário que é especificado pelo `<username>` ou pelo `<domain>\<username>` . O parâmetro **/u** pode ser especificado somente se **/s** também for especificado. O padrão é as permissões do usuário que está conectado no momento no computador que está emitindo o comando. |
| /p `<password>` | Especifica a senha da conta de usuário que é especificada no parâmetro **/u** . |
| /Fi `<filter>` | Aplica um filtro para selecionar um conjunto de tarefas. Você pode usar mais de um filtro ou usar o caractere curinga ( `*` ) para especificar todas as tarefas ou nomes de imagem. Os filtros válidos são listados na seção **nomes de filtro, operadores e valores** deste artigo. |
| /PID `<processID>` | Especifica a ID do processo a ser encerrada. |
| /im `<imagename>` | Especifica o nome da imagem do processo a ser encerrado. Use o caractere curinga ( `*` ) para especificar todos os nomes de imagem. |
| /f | Especifica que os processos foram finalizados de modo forçado. Esse parâmetro é ignorado para processos remotos; todos os processos remotos são finalizados de modo forçado. |
| /t | Encerra o processo especificado e todos os processos filho iniciados por ele. |

#### <a name="filter-names-operators-and-values"></a>Filtrar nomes, operadores e valores

| Nome do filtro | Operadores válidos | Valor (es) válido (s) |
|--|--|--|
| STATUS | eq, ne | `RUNNING | NOT RESPONDING | UNKNOWN` |
| IMAGENAME | eq, ne | Nome da imagem |
| PID | eq, ne, gt, lt, ge, le | Valor PID |
| SESSION | eq, ne, gt, lt, ge, le | Número da sessão |
| CPUtime | eq, ne, gt, lt, ge, le | O tempo de CPU no formato *hh: mm: SS*, em que *mm* e *SS* estão entre 0 e 59 e *hh* é qualquer número não assinado |
| MEMUSAGE | eq, ne, gt, lt, ge, le | Uso de memória em KB |
| USERNAME | eq, ne | Qualquer nome de usuário válido ( `<user>` ou `<domain\user>` ) |
| SERVIÇOS | eq, ne | Nome do serviço |
| WINDOWTITLE | eq, ne | Título da janela |
| MÓDULOS | eq, ne | Nome da DLL |

## <a name="remarks"></a>Comentários

- Os filtros de **status** e **WindowTitle** não têm suporte quando um sistema remoto é especificado.

- O caractere curinga ( `*` ) é aceito para a `*/im` opção somente quando um filtro é aplicado.

- Encerrar um processo remoto sempre é executado de modo forçado, independentemente se a opção **/f** está especificada.

- Fornecer um nome de computador para o filtro de nome de host causa um desligamento, interrompendo todos os processos.

## <a name="examples"></a>Exemplos

Para finalizar os processos com as IDs de processo *1230*, *1241*e *1253*, digite:

```
taskkill /pid 1230 /pid 1241 /pid 1253
```

Para forçar o encerramento do processo *Notepad.exe* se ele foi iniciado pelo sistema, digite:

```
taskkill /f /fi "USERNAME eq NT AUTHORITY\SYSTEM" /im notepad.exe
```

Para finalizar todos os processos no computador remoto *srvmain* com um nome de imagem começando com *Observação*, ao usar as credenciais para a conta de usuário *Hiropln*, digite:

```
taskkill /s srvmain /u maindom\hiropln /p p@ssW23 /fi "IMAGENAME eq note*" /im *
```

Para encerrar o processo com a ID de processo *2134* e todos os processos filho que ele iniciou, mas somente se esses processos tiverem sido iniciados pela conta de administrador, digite:

```
taskkill /pid 2134 /t /fi "username eq administrator"
```

Para finalizar todos os processos que têm uma ID *de processo maior ou igual a 1000*, independentemente de seus nomes de imagem, digite:

```
taskkill /f /fi "PID ge 1000" /im *
```

## <a name="additional-references"></a>Referências adicionais

- [Chave da sintaxe de linha de comando](command-line-syntax-key.md)

- [comando tasklist](tasklist.md)
