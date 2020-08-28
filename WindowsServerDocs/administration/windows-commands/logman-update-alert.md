---
title: logman update alert
description: Artigo de referência para o comando de alerta de atualização do logman, que atualiza as propriedades de um coletor de dados de alerta existente.
ms.topic: reference
ms.assetid: ede94a76-931c-40ed-9fda-6766bed8ff72
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: ba0e94579e7b61992bd81f91ed0906d2472cb226
ms.sourcegitcommit: 96d46c702e7a9c3a321bbbb5284f73911c7baa3c
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/27/2020
ms.locfileid: "89035344"
---
# <a name="logman-update-alert"></a>logman update alert

> Aplica-se a: Windows Server (canal semestral), Windows Server 2019, Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Atualiza as propriedades de um coletor de dados de alerta existente.

## <a name="syntax"></a>Sintaxe

```
logman update alert <[-n] <name>> [options]
```

### <a name="parameters"></a>Parâmetros

| Parâmetro | Descrição |
| --------- | ----------- |
| -s `<computer name>` | Execute o comando no computador remoto especificado. |
| -configuração `<value>` | Especifica o arquivo de configurações que contém as opções de comando. |
| [-n] `<name>` | O nome do objeto de destino. |
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
| -[-] CNF `<[[hh:]mm:]ss>` | Quando o tempo for especificado, o criará um novo arquivo quando o tempo especificado tiver decorrido. Quando a hora não for especificada, o criará um novo arquivo quando o tamanho máximo for excedido. |
| -y | Responde sim a todas as perguntas sem avisar. |
| -CF `<filename>` | Especifica o arquivo que lista os contadores de desempenho a serem coletados. O arquivo deve conter um nome de contador de desempenho por linha. |
| -[-] El | Habilita ou desabilita o relatório do log de eventos. |
| -th `<threshold [threshold [...]]>` | Especifique os contadores e seus valores limites para um alerta. |
| -[-]rdcs `<name>` | Especifica o conjunto de coletores de dados a ser iniciado quando um alerta é disparado. |
| -[-] TN `<task>` | Especifica a tarefa a ser executada quando um alerta é disparado. |
| -[-] Tino `<argument>` | Especifica os argumentos da tarefa a serem usados com a tarefa especificada usando-TN. |
| /? | Exibe a ajuda contextual. |

#### <a name="remarks"></a>Comentários

- Onde [-] está listado, adicionar um hífen extra (-) nega a opção.

### <a name="examples"></a>Exemplos

Para atualizar o alerta existente chamado *new_alert*, definindo o valor de limite para o contador% tempo do processador no grupo de contadores processador (_Total) como 40%, digite:

```
logman update alert new_alert -th \Processor(_Total)\% Processor time>40
```

## <a name="additional-references"></a>Referências adicionais

- [Chave da sintaxe de linha de comando](command-line-syntax-key.md)

- [comando de criação de alerta do logman](logman-create-alert.md)

- [comando logman](logman.md)
