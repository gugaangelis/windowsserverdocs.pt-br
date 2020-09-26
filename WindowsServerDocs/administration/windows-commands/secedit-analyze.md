---
title: secedit analyze
description: Artigo de referência para o comando de análise de secedit, que permite analisar as configurações atuais de sistemas em relação às configurações de linha de base armazenadas em um banco de dados.
ms.topic: reference
ms.assetid: 3430cf9d-1411-48b1-b5a9-2e47701dc87f
ms.author: lizross
author: eross-msft
manager: mtillman
ms.date: 10/16/2017
ms.openlocfilehash: 783f9d1042b860adefc49f58f38f66e0571468e3
ms.sourcegitcommit: e164aeffc01069b8f1f3248bf106fcdb7f64f894
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/26/2020
ms.locfileid: "91388552"
---
# <a name="secedit-analyze"></a>secedit/Analyze

Permite que você analise as configurações atuais de sistemas em relação às configurações de linha de base que são armazenadas em um banco de dados.

## <a name="syntax"></a>Sintaxe

```
secedit /analyze /db <database file name> [/cfg <configuration file name>] [/overwrite] [/log <log file name>] [/quiet}]
```

### <a name="parameters"></a>Parâmetros

| Parâmetro | Descrição |
|--|--|
| /DB | Obrigatórios. Especifica o caminho e o nome do arquivo do banco de dados que contém a configuração armazenada na qual a análise é executada. Se o nome do arquivo especificar um banco de dados que não tinha um modelo de segurança (como representado pelo arquivo de configuração) associado a ele, a `/cfg <configuration file name>` opção também deverá ser especificada. |
| /cfg | Especifica o caminho e o nome do arquivo para o modelo de segurança que será importado para o banco de dados para análise. Essa opção só é válida quando usada com o `/db <database file name>` parâmetro. Se esse parâmetro também não for especificado, a análise será executada em qualquer configuração já armazenada no banco de dados. |
| /overwrite | Especifica se o modelo de segurança no parâmetro **/cfg** deve substituir qualquer modelo ou modelo composto armazenado no banco de dados, em vez de acrescentar os resultados ao modelo armazenado. Essa opção só é válida quando o `/cfg <configuration file name>` parâmetro também é usado. Se esse parâmetro também não for especificado, o modelo no parâmetro **/cfg** será anexado ao modelo armazenado. |
| /log | Especifica o caminho e o nome do arquivo de log a ser usado no processo. Se você não especificar um local de arquivo, o arquivo de log padrão `<systemroot>\Documents and Settings\<UserAccount>\My Documents\Security\Logs\<databasename>.log` será usado. |
| /quiet | Suprime a saída da tela. Você ainda pode exibir os resultados da análise usando o snap-in configuração e análise de segurança no console de gerenciamento Microsoft (MMC). |

## <a name="examples"></a>Exemplos

Para executar a análise dos parâmetros de segurança no banco de dados de segurança, *SecDbContoso. sdb*, e direcionar a saída para o arquivo *SecAnalysisContosoFY11*, incluindo prompts para verificar se o comando foi executado corretamente, digite:

```
secedit /analyze /db C:\Security\FY11\SecDbContoso.sdb /log C:\Security\FY11\SecAnalysisContosoFY11.log
```

Para incorporar as alterações exigidas pelo processo de análise no arquivo *SecContoso. inf* e, em seguida, direcionar a saída para o arquivo existente, *SecAnalysisContosoFY11*, sem aviso, digite:

```
secedit /analyze /db C:\Security\FY11\SecDbContoso.sdb /cfg SecContoso.inf /overwrite /log C:\Security\FY11\SecAnalysisContosoFY11.xml /quiet
```

## <a name="additional-references"></a>Referências adicionais

- [Chave da sintaxe de linha de comando](command-line-syntax-key.md)

- [secedit/configure](secedit-configure.md)

- [secedit/export](secedit-export.md)

- [/generaterollback secedit](secedit-generaterollback.md)

- [secedit/Import](secedit-import.md)

- [/Validate secedit](secedit-validate.md)