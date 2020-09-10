---
title: logman update counter
description: Artigo de referência para o comando do contador de atualização do logman, que atualiza as propriedades de um coletor de dados do contador existente.
ms.topic: reference
ms.assetid: 607df6d5-876c-428d-a0b3-f59cb244e2ce
ms.author: lizross
author: eross-msft
manager: mtillman
ms.date: 10/16/2017
ms.openlocfilehash: c164fdaf8e9a22b6072555a893fb6c41c69f177b
ms.sourcegitcommit: db2d46842c68813d043738d6523f13d8454fc972
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/10/2020
ms.locfileid: "89640368"
---
# <a name="logman-update-counter"></a>logman update counter

> Aplica-se a: Windows Server (canal semestral), Windows Server 2019, Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Atualiza as propriedades de um coletor de dados de contador existente.

## <a name="syntax"></a>Sintaxe

```
logman update counter <[-n] <name>> [options]
```

### <a name="parameters"></a>Parâmetros


| Parâmetro | Descrição |
| --------- | ----------- |
| -s `<computer name>` | Execute o comando no computador remoto especificado. |
| -configuração `<value>` | Especifica o arquivo de configurações que contém as opções de comando. |
| [-n] `<name>` | O nome do objeto de destino. |
| -f `<bin|bincirc>` | Especifica o formato de log para o coletor de dados. |
| -[-] u `<user [password]>` | Especifica o usuário a ser executado como. Inserir um `*` para a senha produz um prompt para a senha. A senha não é exibida quando você a digita no prompt de senha. |
| -m `<[start] [stop] [[start] [stop] [...]]>` | Altera para início ou parada manual em vez de uma hora de início ou de término agendada. |
| -RF `<[[hh:]mm:]ss>` | Executa o coletor de dados durante o período de tempo especificado. |
| -b `<M/d/yyyy h:mm:ss[AM|PM]>` | Inicia a coleta de dados no horário especificado. |
| -e `<M/d/yyyy h:mm:ss[AM|PM]>` | Encerra a coleta de dados no tempo especificado. |
| -si `<[[hh:]mm:]ss>` | Especifica o intervalo de amostragem para coletores de dados de contador de desempenho. |
| -o `<path|dsn!log>` | Especifica o arquivo de log de saída ou o DSN e o nome do conjunto de logs em um banco de dados SQL. |
| -[-] r | Repete o coletor de dados diariamente nas horas de início e término especificadas. |
| -[-] um | Anexa um arquivo de log existente. |
| -[-] Omo | Substitui um arquivo de log existente. |
| -[-] v `<nnnnnn|mmddhhmm>` | Anexa informações de controle de versão do arquivo ao final do nome do arquivo de log. |
| -[-] RC `<task>` | Executa o comando especificado cada vez que o log é fechado. |
| -[-] máx. `<value>` | Tamanho máximo do arquivo de log em MB ou número máximo de registros para logs SQL. |
| -[-] CNF `<[[hh:]mm:]ss>` | Quando o tempo for especificado, crie um novo arquivo quando o tempo especificado tiver decorrido. Quando a hora não for especificada, crie um novo arquivo quando o tamanho máximo for excedido. |
| -y | Responde sim a todas as perguntas sem avisar. |
| -CF `<filename>` | Especifica o arquivo que lista os contadores de desempenho a serem coletados. O arquivo deve conter um nome de contador de desempenho por linha. |
| -c `<path [path [ ]]>` | Especifica os contadores de desempenho a serem coletados. |
| -SC `<value>` | Especifica o número máximo de amostras a serem coletadas com um coletor de dados de contador de desempenho. |
| /? | Exibe a ajuda contextual. |

#### <a name="remarks"></a>Comentários

- Onde [-] está listado, adicionar um hífen extra (-) nega a opção.

### <a name="examples"></a>Exemplos

Para criar um contador chamado *perf_log* usando o contador% tempo do processador da categoria de contador processador (_Total), digite:

```
logman create counter perf_log -c \Processor(_Total)\% Processor time
```

Para atualizar um contador existente chamado *perf_log*, alterando o intervalo de exemplo para 10, o formato de log para CSV e adicionando o controle de versão ao nome do arquivo de log no formato mmddhhmm, digite:

```
logman update counter perf_log -si 10 -f csv -v mmddhhmm
```

## <a name="additional-references"></a>Referências adicionais

- [Chave da sintaxe de linha de comando](command-line-syntax-key.md)

- [comando de criação de contador de logman](logman-create-counter.md)

- [comando logman](logman.md)
