---
title: logman update trace
description: Artigo de referência para o comando de rastreamento de atualização do logman, que atualiza as propriedades de um coletor de dados de rastreamento de eventos existente.
ms.topic: reference
ms.assetid: b7111f7f-4162-4d1a-8e53-d766db0ede1f
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 17a63116408458edaf11c2ff44ccf2c1a978cea0
ms.sourcegitcommit: 96d46c702e7a9c3a321bbbb5284f73911c7baa3c
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/27/2020
ms.locfileid: "89036344"
---
# <a name="logman-update-trace"></a>logman update trace

> Aplica-se a: Windows Server (canal semestral), Windows Server 2019, Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Atualiza as propriedades de um coletor de dados de rastreamento de eventos existente.

## <a name="syntax"></a>Sintaxe

```
logman update trace <[-n] <name>> [options]
```

### <a name="parameters"></a>Parâmetros

| Parâmetro | Descrição |
| --------- | ----------- |
| -s `<computer name>` | Executa o comando no computador remoto especificado. |
| -configuração `<value>` | Especifica o arquivo de configurações que contém as opções de comando. |
| -ETS | Envia comandos para sessões de rastreamento de eventos diretamente sem salvar ou agendar. |
| [-n] `<name>` | O nome do objeto de destino. |
| -f `<bin|bincirc>` | Especifica o formato de log para o coletor de dados. |
| -[-] u `<user [password]>` | Especifica o usuário a ser executado como. Inserir um `*` para a senha produz um prompt para a senha. A senha não é exibida quando você a digita no prompt de senha. |
| -m `<[start] [stop] [[start] [stop] [...]]>` | Altera para início ou parada manual em vez de uma hora de início ou de término agendada. |
| -RF `<[[hh:]mm:]ss>` | Executa o coletor de dados durante o período de tempo especificado. |
| -b `<M/d/yyyy h:mm:ss[AM|PM]>` | Inicia a coleta de dados no horário especificado. |
| -e `<M/d/yyyy h:mm:ss[AM|PM]>` | Encerra a coleta de dados no tempo especificado. |
| -o `<path|dsn!log>` | Especifica o arquivo de log de saída ou o DSN e o nome do conjunto de logs em um banco de dados SQL. |
| -[-] r | Repete o coletor de dados diariamente nas horas de início e término especificadas. |
| -[-] um | Anexa um arquivo de log existente. |
| -[-] Omo | Substitui um arquivo de log existente. |
| -[-] v `<nnnnnn|mmddhhmm>` | Anexa informações de controle de versão do arquivo ao final do nome do arquivo de log. |
| -[-] RC `<task>` | Executa o comando especificado cada vez que o log é fechado. |
| -[-] máx. `<value>` | Tamanho máximo do arquivo de log em MB ou número máximo de registros para logs SQL. |
| -[-] CNF `<[[hh:]mm:]ss>` | Quando o tempo for especificado, o criará um novo arquivo quando o tempo especificado tiver decorrido. Quando a hora não for especificada, o criará um novo arquivo quando o tamanho máximo for excedido. |
| -y | Responde sim a todas as perguntas sem avisar. |
| -CT `<perf|system|cycle>` | Especifica o tipo de relógio da sessão de rastreamento de eventos. |
| -ln `<logger_name>` | Especifica o nome do agente para sessões de rastreamento de eventos. |
| -ft `<[[hh:]mm:]ss>` | Especifica o timer de liberação da sessão de rastreamento de eventos. |
| -[-] p `<provider [flags [level]]>` | Especifica um único provedor de rastreamento de eventos a ser habilitado. |
| -PF `<filename>` | Especifica um arquivo que lista vários provedores de rastreamento de eventos a serem habilitados. O arquivo deve ser um arquivo de texto que contém um provedor por linha. |
| -[-] RT | Executa a sessão de rastreamento de eventos em modo em tempo real. |
| -[-] UL | Executa a sessão de rastreamento de eventos no usuário. |
| -BS `<value>` | Especifica o tamanho do buffer da sessão de rastreamento de eventos em KB. |
| -NB `<min max>` | Especifica o número de buffers de sessão de rastreamento de eventos. |
| -modo `<globalsequence|localsequence|pagedmemory>` | Especifica o modo de agente de sessão de rastreamento de eventos, incluindo:<ul><li>**Globalsequence** -especifica que o rastreador de eventos adiciona um número de sequência a cada evento que ele recebe, independentemente de qual sessão de rastreamento recebeu o evento.</li><li>**Localsequence** -especifica que o rastreador de eventos adiciona números de sequência para eventos recebidos em uma sessão de rastreamento específica. Quando essa opção é usada, números de sequência duplicados podem existir em todas as sessões, mas serão exclusivos em cada sessão de rastreamento.</li><li>**Pagedmemory** -especifica que o rastreador de eventos usa memória paginável em vez do pool de memória não paginável padrão para suas alocações de buffer internas.</li></ul> |
| /? | Exibe a ajuda contextual. |

#### <a name="remarks"></a>Comentários

- Onde [-] está listado, adicionar um hífen extra (-) nega a opção.

### <a name="examples"></a>Exemplos

Para atualizar um coletor de dados de rastreamento de eventos existente chamado *trace_log*, alterando o tamanho máximo do log para 10 MB, atualizando o formato do arquivo de log para CSV e acrescentando o controle de versão do arquivo no formato mmddhhmm, digite:

```
logman update trace trace_log -max 10 -f csv -v mmddhhmm
```

## <a name="additional-references"></a>Referências adicionais

- [Chave da sintaxe de linha de comando](command-line-syntax-key.md)

- [comando de criação de rastreamento do logman](logman-create-trace.md)

- [comando logman](logman.md)
