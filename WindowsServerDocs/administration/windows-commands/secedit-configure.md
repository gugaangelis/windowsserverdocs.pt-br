---
title: secedit configure
description: Artigo de referência para o comando de configuração do secedit, que permite que você defina as configurações atuais do sistema usando as configurações de segurança armazenadas em um banco de dados.
ms.topic: reference
ms.assetid: a92e68ca-003c-4219-8655-0e7734f5fab3
ms.author: lizross
author: eross-msft
manager: mtillman
ms.date: 10/16/2017
ms.openlocfilehash: 39f69651a69a748acf65727ecec0bcbe4b9c911a
ms.sourcegitcommit: e164aeffc01069b8f1f3248bf106fcdb7f64f894
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/26/2020
ms.locfileid: "91388219"
---
# <a name="secedit-configure"></a>secedit/configure

Permite que você defina as configurações atuais do sistema usando as configurações de segurança armazenadas em um banco de dados.

## <a name="syntax"></a>Sintaxe

```
secedit /configure /db <database file name> [/cfg <configuration file name>] [/overwrite] [/areas [securitypolicy | group_mgmt | user_rights | regkeys | filestore | services]] [/log <log file name>] [/quiet]
```

### <a name="parameters"></a>Parâmetros

| Parâmetro | Descrição |
|--|--|
| /DB | Obrigatórios. Especifica o caminho e o nome do arquivo do banco de dados que contém a configuração armazenada. Se o nome do arquivo especificar um banco de dados que não tinha um modelo de segurança (como representado pelo arquivo de configuração) associado a ele, a `/cfg <configuration file name>` opção também deverá ser especificada. |
| /cfg | Especifica o caminho e o nome do arquivo para o modelo de segurança que será importado para o banco de dados para análise. Essa opção só é válida quando usada com o `/db <database file name>` parâmetro. Se esse parâmetro também não for especificado, a análise será executada em qualquer configuração já armazenada no banco de dados. |
| /overwrite | Especifica se o modelo de segurança no parâmetro **/cfg** deve substituir qualquer modelo ou modelo composto armazenado no banco de dados, em vez de acrescentar os resultados ao modelo armazenado. Essa opção só é válida quando o `/cfg <configuration file name>` parâmetro também é usado. Se esse parâmetro também não for especificado, o modelo no parâmetro **/cfg** será anexado ao modelo armazenado. |
| /areas | Especifica as áreas de segurança a serem aplicadas ao sistema. Se esse parâmetro não for especificado, todas as configurações de segurança definidas no banco de dados serão aplicadas ao sistema. Para configurar várias áreas, separe cada área por um espaço. Há suporte para as seguintes áreas de segurança:<ul><li>**SecurityPolicy:** Política local e política de domínio para o sistema, incluindo políticas de conta, políticas de auditoria, opções de segurança e assim por diante.</li><li>  **GROUP_MGMT:** Configurações de grupo restrito para todos os grupos especificados no modelo de segurança.</li><li>**user_rights:** Direitos de logon de usuário e concessão de privilégios.</li><li>**regkeys:** Segurança em chaves do Registro local.</li><li>**repositório de armazenamento:** Segurança no armazenamento de arquivos local.</li><li>**Serviços:** Segurança para todos os serviços definidos.</li></ul> |
| /log | Especifica o caminho e o nome do arquivo de log a ser usado no processo. Se você não especificar um local de arquivo, o arquivo de log padrão `<systemroot>\Documents and Settings\<UserAccount>\My Documents\Security\Logs\<databasename>.log` será usado. |
| /quiet | Suprime a saída de tela e de log. Você ainda pode exibir os resultados da análise usando o snap-in configuração e análise de segurança no console de gerenciamento Microsoft (MMC). |

## <a name="examples"></a>Exemplos

Para executar a análise dos parâmetros de segurança no banco de dados de segurança, *SecDbContoso. sdb*, e direcionar a saída para o arquivo *SecAnalysisContosoFY11*, incluindo prompts para verificar se o comando foi executado corretamente, digite:

```
secedit /analyze /db C:\Security\FY11\SecDbContoso.sdb /log C:\Security\FY11\SecAnalysisContosoFY11.log
```

Para incorporar as alterações exigidas pelo processo de análise no arquivo *SecContoso. inf* e, em seguida, direcionar a saída para o arquivo existente, *SecAnalysisContosoFY11*, sem aviso, digite:

```
secedit /configure /db C:\Security\FY11\SecDbContoso.sdb /cfg SecContoso.inf /overwrite /log C:\Security\FY11\SecAnalysisContosoFY11.xml /quiet
```

## <a name="additional-references"></a>Referências adicionais

- [Chave da sintaxe de linha de comando](command-line-syntax-key.md)

- [secedit/Analyze](secedit-analyze.md)

- [secedit/export](secedit-export.md)

- [/generaterollback secedit](secedit-generaterollback.md)

- [secedit/Import](secedit-import.md)

- [/Validate secedit](secedit-validate.md)