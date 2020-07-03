---
title: logman import e logman export
description: Artigo de referência para importação de Logman e exportação de logman, que importa um conjunto de coletores de dados de um arquivo XML ou exporta um conjunto de coletores de dados para um arquivo XML.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: c258daba-fb93-47c0-a53b-2fe83ed2c743
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: d5fbe0139a1c320056e4ba80a845b43755fc0026
ms.sourcegitcommit: 2afed2461574a3f53f84fc9ec28d86df3b335685
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/02/2020
ms.locfileid: "85934389"
---
# <a name="logman-import-and-logman-export"></a>logman import e logman export

> Aplica-se a: Windows Server (canal semestral), Windows Server 2019, Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Importa um conjunto de coletores de dados de um arquivo XML ou exporta um conjunto de coletores de dados para um arquivo XML.

## <a name="syntax"></a>Sintaxe

```
logman import <[-n] <name> <-xml <name> [options]
logman export <[-n] <name> <-xml <name> [options]
```

### <a name="parameters"></a>Parâmetros

| Parâmetro | Descrição |
| --------- | ----------- |
| -s`<computer name>` | Execute o comando no computador remoto especificado. |
| -configuração`<value>` | Especifica o arquivo de configurações que contém as opções de comando. |
| [-n]`<name>` | O nome do objeto de destino. |
| -XML`<name>` | Nome do arquivo XML a ser importado ou exportado. |
| -ETS | Envia comandos para sessões de rastreamento de eventos diretamente sem salvar ou agendar. |
| -[-] u`<user [password]>` | Especifica o usuário a ser executado como. Inserir um `*` para a senha produz um prompt para a senha. A senha não é exibida quando você a digita no prompt de senha. |
| -y | Responde sim a todas as perguntas sem avisar. |
| /? | Exibe a ajuda contextual. |

### <a name="examples"></a>Exemplos

Para importar o arquivo XML *c:\windows\perf_log.xml* do computador *server_1* como um conjunto de coletores de dados chamado *perf_log*, digite:

```
logman import perf_log -s server_1 -xml c:\windows\perf_log.xml
```

## <a name="additional-references"></a>Referências adicionais

- [Chave da sintaxe de linha de comando](command-line-syntax-key.md)

- [comando logman](logman.md)
