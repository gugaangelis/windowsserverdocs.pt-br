---
title: logman create counter
description: Artigo de referência para o comando do contador Create do logman, que cria um coletor de dados de contador.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 1e214c32-b704-43c1-b548-e1cf43b583c3
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 80ea79eb8c286a8dbe7c020b4c23ae1483d2cb9b
ms.sourcegitcommit: 2afed2461574a3f53f84fc9ec28d86df3b335685
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/02/2020
ms.locfileid: "85927202"
---
# <a name="logman-create-counter"></a>logman create counter

> Aplica-se a: Windows Server (canal semestral), Windows Server 2019, Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Cria um coletor de dados de contador.

## <a name="syntax"></a>Sintaxe

```
logman create counter <[-n] <name>> [options]
```

### <a name="parameters"></a>Parâmetros

| Parâmetro | Descrição |
| --------- | ----------- |
| -s`<computer name>` | Execute o comando no computador remoto especificado. |
| -configuração`<value>` | Especifica o arquivo de configurações que contém as opções de comando. |
| [-n]`<name>` | O nome do objeto de destino. |
| -f`<bin|bincirc>` | Especifica o formato de log para o coletor de dados. |
| -[-] u`<user [password]>` | Especifica o usuário a ser executado como. Inserir um `*` para a senha produz um prompt para a senha. A senha não é exibida quando você a digita no prompt de senha. |
| -m`<[start] [stop] [[start] [stop] [...]]>` | Altera para início ou parada manual em vez de uma hora de início ou de término agendada. |
| -RF`<[[hh:]mm:]ss>` | Executa o coletor de dados durante o período de tempo especificado. |
| -b`<M/d/yyyy h:mm:ss[AM|PM]>` | Inicia a coleta de dados no horário especificado. |
| -e `<M/d/yyyy h:mm:ss[AM|PM]>` | Encerra a coleta de dados no tempo especificado. |
| -si`<[[hh:]mm:]ss>` | Especifica o intervalo de amostragem para coletores de dados de contador de desempenho. |
| -o`<path|dsn!log>` | Especifica o arquivo de log de saída ou o DSN e o nome do conjunto de logs em um banco de dados SQL. |
| -[-] r | Repete o coletor de dados diariamente nas horas de início e término especificadas. |
| -[-] um | Anexa um arquivo de log existente. |
| -[-] Omo | Substitui um arquivo de log existente. |
| -[-] v`<nnnnnn|mmddhhmm>` | Anexa informações de controle de versão do arquivo ao final do nome do arquivo de log. |
| -[-] RC`<task>` | Executa o comando especificado cada vez que o log é fechado. |
| -[-] máx.`<value>` | Tamanho máximo do arquivo de log em MB ou número máximo de registros para logs SQL. |
| -[-] CNF`<[[hh:]mm:]ss>` | Quando o tempo for especificado, crie um novo arquivo quando o tempo especificado tiver decorrido. Quando a hora não for especificada, crie um novo arquivo quando o tamanho máximo for excedido. |
| -y | Responde sim a todas as perguntas sem avisar. |
| -CF`<filename>` | Especifica o arquivo que lista os contadores de desempenho a serem coletados. O arquivo deve conter um nome de contador de desempenho por linha. |
| -c`<path [path [ ]]>` | Especifica os contadores de desempenho a serem coletados. |
| -SC`<value>` | Especifica o número máximo de amostras a serem coletadas com um coletor de dados de contador de desempenho. |
| /? | Exibe a ajuda contextual. |

#### <a name="remarks"></a>Comentários

- Onde [-] está listado, adicionar um hífen extra (-) nega a opção.

### <a name="examples"></a>Exemplos

Para criar um contador chamado *perf_log* usando o contador% tempo do processador da categoria de contador processador (_Total), digite:

```
logman create counter perf_log -c \Processor(_Total)\% Processor time
```

Para criar um contador chamado *perf_log* usando o contador% tempo do processador da categoria de contador processador (_Total), criando um arquivo de log com um tamanho máximo de 10 MB e coletando dados por 1 minuto e 0 segundo, digite:

```
logman create counter perf_log -c \Processor(_Total)\% Processor time -max 10 -rf 01:00
```

## <a name="additional-references"></a>Referências adicionais

- [Chave da sintaxe de linha de comando](command-line-syntax-key.md)

- [comando do contador de atualização do logman](logman-update-counter.md)

- [comando logman](logman.md)
