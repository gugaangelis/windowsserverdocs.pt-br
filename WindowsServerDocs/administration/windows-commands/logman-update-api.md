---
title: logman update api
description: Artigo de referência para o comando da API de atualização do logman, que atualiza as propriedades de um coletor de dados de rastreamento de API existente.
ms.topic: reference
ms.assetid: 6f322e52-0f9f-42b1-bd64-8b8f8fe086fc
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 9b95c69b4e4b334155c1a22327b1acb8f756c7cd
ms.sourcegitcommit: 96d46c702e7a9c3a321bbbb5284f73911c7baa3c
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/27/2020
ms.locfileid: "89035334"
---
# <a name="logman-update-api"></a>logman update api

> Aplica-se a: Windows Server (canal semestral), Windows Server 2019, Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Atualiza as propriedades de um coletor de dados de rastreamento de API existente.

## <a name="syntax"></a>Sintaxe

```
logman update api <[-n] <name>> [options]
```

### <a name="parameters"></a>Parâmetros

| Parâmetro | Descrição |
| --------- | ----------- |
| -s `<computer name>` | Executa o comando no computador remoto especificado. |
| -configuração `<value>` | Especifica o arquivo de configurações que contém as opções de comando. |
| [-n] `<name>` | O nome do objeto de destino. |
| -f `<bin|bincirc>` | Especifica o formato de log para o coletor de dados. |
| -[-] u `<user [password]>` | Especifica o usuário a ser executado como. Inserir um `*` para a senha produz um prompt para a senha. A senha não é exibida quando você a digita no prompt de senha. |
| -m `<[start] [stop] [[start] [stop] [...]]>` | Alterado para iniciar ou parar manualmente em vez de uma hora de início ou de término agendada. |
| -RF `<[[hh:]mm:]ss>` | Execute o coletor de dados para o período de tempo especificado. |
| -b `<M/d/yyyy h:mm:ss[AM|PM]>` | Comece a coletar dados no horário especificado. |
| -e `<M/d/yyyy h:mm:ss[AM|PM]>` | Terminar a coleta de dados na hora especificada. |
| -si `<[[hh:]mm:]ss>` | Especifica o intervalo de amostragem para coletores de dados de contador de desempenho. |
| -o `<path|dsn!log>` | Especifica o arquivo de log de saída ou o DSN e o nome do conjunto de logs em um banco de dados SQL. |
| -[-] r | Repita o coletor de dados diariamente nas horas de início e término especificadas. |
| -[-] um | Acrescentar um arquivo de log existente. |
| -[-] Omo | Substituir um arquivo de log existente. |
| -[-] v `<nnnnnn|mmddhhmm>` | Anexa informações de controle de versão do arquivo ao final do nome do arquivo de log. |
| -[-] RC `<task>` | Execute o comando especificado cada vez que o log for fechado. |
| -[-] máx. `<value>` | Tamanho máximo do arquivo de log em MB ou número máximo de registros para logs SQL. |
| -[-] CNF `<[[hh:]mm:]ss>` | Quando o tempo for especificado, o criará um novo arquivo quando o tempo especificado tiver decorrido. Quando a hora não for especificada, o criará um novo arquivo quando o tamanho máximo for excedido. |
| -y | Responda sim a todas as perguntas sem avisar. |
| -mods `<path [path [...]]>` | Especifica a lista de módulos da qual registrar chamadas de API. |
| -inapis` <module!api [module!api [...]]>` | Especifica a lista de chamadas de API a serem incluídas no registro em log. |
| -exapis `<module!api [module!api [...]]>` | Especifica a lista de chamadas de API a serem excluídas do registro em log. |
| -[-] ano | Somente nomes de API de log (-ano) ou não registram somente os nomes de API (-ano). |
| -[-] recursivo | Registrar (-recursivo) ou não registrar (-recursivo) APIs recursivamente além da primeira camada. |
| -exe `<value>` | Especifica o caminho completo para um executável para rastreamento de API. |
| /? | Exibe a ajuda contextual. |

#### <a name="remarks"></a>Comentários

- Onde [-] está listado, adicionar um hífen extra (-) nega a opção.

### <a name="examples"></a>Exemplos

Para atualizar um contador de rastreamento de API existente chamado *trace_notepad*, para o arquivo executável c:\windows\notepad.exe, excluindo a chamada de API TlsGetValue produzida pelo módulo kernel32.dll, digite:

```
logman update api trace_notepad -exe c:\windows\notepad.exe -exapis kernel32.dll!TlsGetValue
```

## <a name="additional-references"></a>Referências adicionais

- [Chave da sintaxe de linha de comando](command-line-syntax-key.md)

- [comando logman Create API](logman-create-api.md)

- [comando logman](logman.md)
