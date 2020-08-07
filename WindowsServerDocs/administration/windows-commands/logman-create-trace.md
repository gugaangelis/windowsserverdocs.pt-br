---
title: logman create trace
description: Artigo de referência para o comando Create Trace do logman, que cria um coletor de dados de rastreamento de eventos.
ms.topic: article
ms.assetid: 1b4dfecd-6f56-4c51-b622-c2054b4aabd7
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 3e1bb5f4252e5244f2d8a1f1add77ca6db061534
ms.sourcegitcommit: 53d526bfeddb89d28af44210a23ba417f6ce0ecf
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/06/2020
ms.locfileid: "87887399"
---
# <a name="logman-create-trace"></a>logman create trace

> Aplica-se a: Windows Server (canal semestral), Windows Server 2019, Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Criar um coletor de dados de rastreamento de eventos.

## <a name="syntax"></a>Sintaxe

```
logman create trace <[-n] <name>> [options]
```

### <a name="parameters"></a>Parâmetros

| Parâmetro | Descrição |
| --------- | ----------- |
| -s`<computer name>` | Executa o comando no computador remoto especificado. |
| -configuração`<value>` | Especifica o arquivo de configurações que contém as opções de comando. |
| -ETS | Envia comandos para sessões de rastreamento de eventos diretamente sem salvar ou agendar. |
| [-n]`<name>` | O nome do objeto de destino. |
| -f`<bin|bincirc>` | Especifica o formato de log para o coletor de dados. |
| -[-] u`<user [password]>` | Especifica o usuário a ser executado como. Inserir um `*` para a senha produz um prompt para a senha. A senha não é exibida quando você a digita no prompt de senha. |
| -m`<[start] [stop] [[start] [stop] [...]]>` | Altera para início ou parada manual em vez de uma hora de início ou de término agendada. |
| -RF`<[[hh:]mm:]ss>` | Executa o coletor de dados durante o período de tempo especificado. |
| -b`<M/d/yyyy h:mm:ss[AM|PM]>` | Inicia a coleta de dados no horário especificado. |
| -e `<M/d/yyyy h:mm:ss[AM|PM]>` | Encerra a coleta de dados no tempo especificado. |
| -o`<path|dsn!log>` | Especifica o arquivo de log de saída ou o DSN e o nome do conjunto de logs em um banco de dados SQL. |
| -[-] r | Repete o coletor de dados diariamente nas horas de início e término especificadas. |
| -[-] um | Anexa um arquivo de log existente. |
| -[-] Omo | Substitui um arquivo de log existente. |
| -[-] v`<nnnnnn|mmddhhmm>` | Anexa informações de controle de versão do arquivo ao final do nome do arquivo de log. |
| -[-] RC`<task>` | Executa o comando especificado cada vez que o log é fechado. |
| -[-] máx.`<value>` | Tamanho máximo do arquivo de log em MB ou número máximo de registros para logs SQL. |
| -[-] CNF`<[[hh:]mm:]ss>` | Quando o tempo for especificado, o criará um novo arquivo quando o tempo especificado tiver decorrido. Quando a hora não for especificada, o criará um novo arquivo quando o tamanho máximo for excedido. |
| -y | Responde sim a todas as perguntas sem avisar. |
| -CT`<perf|system|cycle>` | Especifica o tipo de relógio da sessão de rastreamento de eventos. |
| -ln`<logger_name>` | Especifica o nome do agente para sessões de rastreamento de eventos. |
| -ft`<[[hh:]mm:]ss>` | Especifica o timer de liberação da sessão de rastreamento de eventos. |
| -[-] p`<provider [flags [level]]>` | Especifica um único provedor de rastreamento de eventos a ser habilitado. |
| -PF`<filename>` | Especifica um arquivo que lista vários provedores de rastreamento de eventos a serem habilitados. O arquivo deve ser um arquivo de texto que contém um provedor por linha. |
| -[-] RT | Executa a sessão de rastreamento de eventos em modo em tempo real. |
| -[-] UL | Executa a sessão de rastreamento de eventos no usuário. |
| -BS`<value>` | Especifica o tamanho do buffer da sessão de rastreamento de eventos em KB. |
| -NB`<min max>` | Especifica o número de buffers de sessão de rastreamento de eventos. |
| -modo`<globalsequence|localsequence|pagedmemory>` | Especifica o modo de agente de sessão de rastreamento de eventos, incluindo:<ul><li>**Globalsequence** -especifica que o rastreador de eventos adiciona um número de sequência a cada evento que ele recebe, independentemente de qual sessão de rastreamento recebeu o evento.</li><li>**Localsequence** -especifica que o rastreador de eventos adiciona números de sequência para eventos recebidos em uma sessão de rastreamento específica. Quando essa opção é usada, números de sequência duplicados podem existir em todas as sessões, mas serão exclusivos em cada sessão de rastreamento.</li><li>**Pagedmemory** -especifica que o rastreador de eventos usa memória paginável em vez do pool de memória não paginável padrão para suas alocações de buffer internas.</li></ul> |
| /? | Exibe a ajuda contextual. |

#### <a name="remarks"></a>Comentários

- Onde [-] está listado, adicionar um hífen extra (-) nega a opção.

### <a name="examples"></a>Exemplos

Para criar um coletor de dados de rastreamento de eventos chamado *trace_log*, usando no máximo 16 e no máximo 256 buffers, com cada buffer com tamanho de 64 KB, colocando os resultados em c:\logfile, digite:

```
logman create trace trace_log -nb 16 256 -bs 64 -o c:\logfile
```

## <a name="additional-references"></a>Referências adicionais

- [Chave da sintaxe de linha de comando](command-line-syntax-key.md)

- [comando de rastreamento de atualização do logman](logman-update-trace.md)

- [comando logman](logman.md)
