---
title: logman update cfg
description: Artigo de referência para o comando logman update cfg, que atualiza as propriedades de um coletor de dados de configuração existente.
ms.topic: reference
ms.assetid: 9da4e8b4-3be5-42d3-b0b4-c429630c35c4
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: b4a952819f4fd2ffe9be6a188fd07db933117667
ms.sourcegitcommit: 96d46c702e7a9c3a321bbbb5284f73911c7baa3c
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/27/2020
ms.locfileid: "89036524"
---
# <a name="logman-update-cfg"></a>logman update cfg

> Aplica-se a: Windows Server (canal semestral), Windows Server 2019, Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Atualiza as propriedades de um coletor de dados de configuração existente.

## <a name="syntax"></a>Sintaxe

```
logman update cfg <[-n] <name>> [options]
```

### <a name="parameters"></a>Parâmetros


| Parâmetro | Descrição |
| --------- | ----------- |
| -s `<computer name>` | Executa o comando no computador remoto especificado. |
| -configuração `<value>` | Especifica o arquivo de configurações que contém as opções de comando. |
| [-n] `<name>` | O nome do objeto de destino. |
| -[-] u `<user [password]>` | Especifica o usuário a ser executado como. Inserir um \* para a senha produz um prompt para a senha. A senha não é exibida quando você a digita no prompt de senha. |
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
| -[-] Ni | Habilita a consulta de interface de rede (-Ni) ou Disable (-Ni). |
| -Reg `<path [path [...]]>` | Especifica o (s) valor (es) do registro a serem coletados. |
| -gerenciamento `<query [query [...]]>` | Especifica o objeto (s) WMI a ser coletado usando a linguagem de consulta SQL. |
| -FTC `<path [path [...]]>` | Especifica o caminho completo para os arquivos a serem coletados. |
| /? | Exibe a ajuda contextual. |

#### <a name="remarks"></a>Comentários

- Onde [-] está listado, adicionar um hífen extra (-) nega a opção.

### <a name="examples"></a>Exemplos

Para atualizar um coletor de dados de configuração chamado *cfg_log*, para coletar a chave do registro `HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows NT\Currentverion\` , digite:

```
logman update cfg cfg_log -reg HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows NT\Currentverion\
```

## <a name="additional-references"></a>Referências adicionais

- [Chave da sintaxe de linha de comando](command-line-syntax-key.md)

- [comando Create cfg do logman](logman-create-cfg.md)

- [comando logman](logman.md)
