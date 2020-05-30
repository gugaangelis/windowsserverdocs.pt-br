---
title: criar API do logman
description: Tópico de referência para o comando logman Create API, que cria um coletor de dados de rastreamento de API.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 2ecc0a75-2613-464a-8616-c5dc404bb736
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: f479fcdf3db4bb5a61b0cd0724220d27c934872f
ms.sourcegitcommit: 29bc8740e5a8b1ba8f73b10ba4d08afdf07438b0
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 05/30/2020
ms.locfileid: "84222805"
---
# <a name="logman-create-api"></a>criar API do logman

> Aplica-se a: Windows Server (canal semestral), Windows Server 2019, Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Cria um coletor de dados de rastreamento de API.

## <a name="syntax"></a>Sintaxe

```
logman create api <[-n] <name>> [options]
```

### <a name="parameters"></a>Parâmetros

| Parâmetro | Descrição |
| --------- | ----------- |
| -s`<computer name>` | Executa o comando no computador remoto especificado. |
| -configuração`<value>` | Especifica o arquivo de configurações que contém as opções de comando. |
| [-n]`<name>` | O nome do objeto de destino. |
| -f`<bin|bincirc>` | Especifica o formato de log para o coletor de dados. |
| -[-] u`<user [password]>` | Especifica o usuário a ser executado como. Inserir um `*` para a senha produz um prompt para a senha. A senha não é exibida quando você a digita no prompt de senha. |
| -m`<[start] [stop] [[start] [stop] [...]]>` | Alterado para iniciar ou parar manualmente em vez de uma hora de início ou de término agendada. |
| -RF`<[[hh:]mm:]ss>` | Execute o coletor de dados para o período de tempo especificado. |
| -b`<M/d/yyyy h:mm:ss[AM|PM]>` | Comece a coletar dados no horário especificado. |
| -e `<M/d/yyyy h:mm:ss[AM|PM]>` | Terminar a coleta de dados na hora especificada. |
| -si`<[[hh:]mm:]ss>` | Especifica o intervalo de amostragem para coletores de dados de contador de desempenho. |
| -o`<path|dsn!log>` | Especifica o arquivo de log de saída ou o DSN e o nome do conjunto de logs em um banco de dados SQL. |
| -[-] r | Repita o coletor de dados diariamente nas horas de início e término especificadas. |
| -[-] um | Acrescentar um arquivo de log existente. |
| -[-] Omo | Substituir um arquivo de log existente. |
| -[-] v`<nnnnnn|mmddhhmm>` | Anexa informações de controle de versão do arquivo ao final do nome do arquivo de log. |
| -[-] RC`<task>` | Execute o comando especificado cada vez que o log for fechado. |
| -[-] máx.`<value>` | Tamanho máximo do arquivo de log em MB ou número máximo de registros para logs SQL. |
| -[-] CNF`<[[hh:]mm:]ss>` | Quando o tempo for especificado, o criará um novo arquivo quando o tempo especificado tiver decorrido. Quando a hora não for especificada, o criará um novo arquivo quando o tamanho máximo for excedido. |
| -y | Responda sim a todas as perguntas sem avisar. |
| -mods`<path [path [...]]>` | Especifica a lista de módulos da qual registrar chamadas de API. |
| -inapis` <module!api [module!api [...]]>` | Especifica a lista de chamadas de API a serem incluídas no registro em log. |
| -exapis`<module!api [module!api [...]]>` | Especifica a lista de chamadas de API a serem excluídas do registro em log. |
| -[-] ano | Somente nomes de API de log (-ano) ou não registram somente os nomes de API (-ano). |
| -[-] recursivo | Registrar (-recursivo) ou não registrar (-recursivo) APIs recursivamente além da primeira camada. |
| -exe`<value>` | Especifica o caminho completo para um executável para rastreamento de API. |
| /? | Exibe a ajuda contextual. |

#### <a name="remarks"></a>Comentários

- Onde [-] está listado, adicionar um hífen extra (-) nega a opção.

### <a name="examples"></a>Exemplos

Para criar um contador de rastreamento de API chamado trace_notepad, para o arquivo executável c:\Windows\Notepad.exe e colocar os resultados no arquivo c:\notepad.ETL, digite:

```
logman create api trace_notepad -exe c:\windows\notepad.exe -o c:\notepad.etl
```

Para criar um contador de rastreamento de API chamado trace_notepad, para o arquivo executável c:\Windows\Notepad.exe, coletando valores produzidos pelo módulo em c:\windows\system32\advapi32.dll, digite:

```
logman create api trace_notepad -exe c:\windows\notepad.exe -mods c:\windows\system32\advapi32.dll
```

Para criar um contador de rastreamento de API chamado trace_notepad, para o arquivo executável c:\Windows\Notepad.exe, excluindo a chamada de API TlsGetValue produzida pelo módulo Kernel32. dll, digite:
```
logman create api trace_notepad -exe c:\windows\notepad.exe -exapis kernel32.dll!TlsGetValue
```

## <a name="additional-references"></a>Referências adicionais

- [Chave da sintaxe de linha de comando](command-line-syntax-key.md)

- [comando de API de atualização do logman](logman-update-api.md)

- [comando logman](logman.md)
