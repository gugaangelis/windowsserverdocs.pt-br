---
title: secedit generaterollback
description: Artigo de referência para o comando secedit generaterollback, que permite gerar um modelo de reversão para um modelo de configuração especificado.
ms.topic: reference
ms.assetid: 385a6799-51a7-4fe3-bd73-10c7998b6680
ms.author: lizross
author: eross-msft
manager: mtillman
ms.date: 10/16/2017
ms.openlocfilehash: ae2e368ef387ea84095fcbcc51ad1e622225a2cc
ms.sourcegitcommit: e164aeffc01069b8f1f3248bf106fcdb7f64f894
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/26/2020
ms.locfileid: "91388812"
---
# <a name="secedit-generaterollback"></a>/generaterollback secedit

Permite gerar um modelo de reversão para um modelo de configuração especificado. Se existir um modelo de reversão existente, executar esse comando novamente substituirá as informações existentes.

A execução bem-sucedida desse comando registra em log as incompatibilidades entre o modelo de segurança especificado a configuração da política de segurança no arquivo scesrv. log.

## <a name="syntax"></a>Sintaxe

```
secedit /generaterollback /db <database file name> /cfg <configuration file name> /rbk <rollback template file name> [/log <log file name>] [/quiet]
```

### <a name="parameters"></a>Parâmetros

| Parâmetro | Descrição |
|--|--|
| /DB | Obrigatórios. Especifica o caminho e o nome do arquivo do banco de dados que contém a configuração armazenada na qual a análise é executada. Se o nome do arquivo especificar um banco de dados que não tinha um modelo de segurança (como representado pelo arquivo de configuração) associado a ele, a `/cfg <configuration file name>` opção também deverá ser especificada. |
| /cfg | Obrigatórios. Especifica o caminho e o nome do arquivo para o modelo de segurança que será importado para o banco de dados para análise. Essa opção só é válida quando usada com o `/db <database file name>` parâmetro. Se esse parâmetro também não for especificado, a análise será executada em qualquer configuração já armazenada no banco de dados. |
| /rbk | Obrigatórios. Especifica um modelo de segurança no qual as informações de reversão são gravadas. Os modelos de segurança são criados usando o snap-in modelos de segurança. Os arquivos de reversão podem ser criados com este comando. |
| /log | Especifica o caminho e o nome do arquivo de log a ser usado no processo. Se você não especificar um local de arquivo, o arquivo de log padrão `<systemroot>\Documents and Settings\<UserAccount>\My Documents\Security\Logs\<databasename>.log` será usado. |
| /quiet | Suprime a saída de tela e de log. Você ainda pode exibir os resultados da análise usando o snap-in configuração e análise de segurança no console de gerenciamento Microsoft (MMC). |

## <a name="examples"></a>Exemplos

Para criar o arquivo de configuração de reversão, para o arquivo *SecTmplContoso. inf* criado anteriormente, ao salvar as configurações originais e, em seguida, gravar a ação no arquivo de log *SecAnalysisContosoFY11* , digite:

```
secedit /generaterollback /db C:\Security\FY11\SecDbContoso.sdb /cfg sectmplcontoso.inf /rbk sectmplcontosoRBK.inf /log C:\Security\FY11\SecAnalysisContosoFY11.log
```

## <a name="additional-references"></a>Referências adicionais

- [Chave da sintaxe de linha de comando](command-line-syntax-key.md)

- [secedit/Analyze](secedit-analyze.md)

- [secedit/configure](secedit-configure.md)

- [secedit/export](secedit-export.md)

- [secedit/Import](secedit-import.md)

- [/Validate secedit](secedit-validate.md)