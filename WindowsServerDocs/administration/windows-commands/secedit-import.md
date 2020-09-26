---
title: secedit import
description: Artigo de referência para o comando de importação secedit, que importa as configurações de segurança (arquivo. inf), exportadas anteriormente do banco de dados configurado com modelos de segurança.
ms.topic: reference
ms.assetid: 1dd59d4c-9d48-444a-871b-b957eb682597
ms.author: lizross
author: eross-msft
manager: mtillman
ms.date: 10/16/2017
ms.openlocfilehash: 35705012d32a196934c0834b3de0c67f7210270b
ms.sourcegitcommit: e164aeffc01069b8f1f3248bf106fcdb7f64f894
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/26/2020
ms.locfileid: "91388229"
---
# <a name="secedit-import"></a>secedit/Import

Importa as configurações de segurança (arquivo. inf), exportadas anteriormente do banco de dados configurado com modelos de segurança.

> [!IMPORTANT]
> Antes de importar um arquivo. inf para outro computador, você deve executar o `secedit /generaterollback` comando no banco de dados no qual a importação será executada.
>
> Você também deve executar o `secedit /validate` comando no arquivo de importação para verificar sua integridade.

## <a name="syntax"></a>Sintaxe

```
secedit /import /db <database file name> /cfg <configuration file name> [/overwrite] [/areas [securitypolicy | group_mgmt | user_rights | regkeys | filestore | services]] [/log <log file name>] [/quiet]
```

### <a name="parameters"></a>Parâmetros

| Parâmetro | Descrição |
|--|--|
| /DB | Obrigatórios. Especifica o caminho e o nome do arquivo do banco de dados que contém a configuração armazenada na qual a importação é executada. Se o nome do arquivo especificar um banco de dados que não tinha um modelo de segurança (como representado pelo arquivo de configuração) associado a ele, a `/cfg <configuration file name>` opção também deverá ser especificada. |
| /overwrite | Especifica se o modelo de segurança no parâmetro **/cfg** deve substituir qualquer modelo ou modelo composto armazenado no banco de dados, em vez de acrescentar os resultados ao modelo armazenado. Essa opção só é válida quando o `/cfg <configuration file name>` parâmetro também é usado. Se esse parâmetro também não for especificado, o modelo no parâmetro **/cfg** será anexado ao modelo armazenado. |
| /cfg | Obrigatórios. Especifica o caminho e o nome do arquivo para o modelo de segurança que será importado para o banco de dados para análise. Essa opção só é válida quando usada com o `/db <database file name>` parâmetro. Se esse parâmetro também não for especificado, a análise será executada em qualquer configuração já armazenada no banco de dados. |
| /areas | Especifica as áreas de segurança a serem aplicadas ao sistema. Se esse parâmetro não for especificado, todas as configurações de segurança definidas no banco de dados serão aplicadas ao sistema. Para configurar várias áreas, separe cada área por um espaço. Há suporte para as seguintes áreas de segurança:<ul><li>**SecurityPolicy:** Política local e política de domínio para o sistema, incluindo políticas de conta, políticas de auditoria, opções de segurança e assim por diante.</li><li>  **GROUP_MGMT:** Configurações de grupo restrito para todos os grupos especificados no modelo de segurança.</li><li>**user_rights:** Direitos de logon de usuário e concessão de privilégios.</li><li>**regkeys:** Segurança em chaves do Registro local.</li><li>**repositório de armazenamento:** Segurança no armazenamento de arquivos local.</li><li>**Serviços:** Segurança para todos os serviços definidos.</li></ul> |
| /log | Especifica o caminho e o nome do arquivo de log a ser usado no processo. Se você não especificar um local de arquivo, o arquivo de log padrão `<systemroot>\Documents and Settings\<UserAccount>\My Documents\Security\Logs\<databasename>.log` será usado. |
| /quiet | Suprime a saída de tela e de log. Você ainda pode exibir os resultados da análise usando o snap-in configuração e análise de segurança no console de gerenciamento Microsoft (MMC). |

## <a name="examples"></a>Exemplos

Para exportar o banco de dados de segurança e as políticas de segurança de domínio para um arquivo. inf e, em seguida, importar esse arquivo para um banco de dados diferente para replicar as configurações de política em outro computador, digite:

```
secedit /export /db C:\Security\FY11\SecDbContoso.sdb /mergedpolicy /cfg NetworkShare\Policies\SecContoso.inf /log C:\Security\FY11\SecAnalysisContosoFY11.log /quiet
```

Para importar apenas a parte das políticas de segurança do arquivo para um banco de dados diferente em outro computador, digite:

```
secedit /import /db C:\Security\FY12\SecDbContoso.sdb /cfg NetworkShare\Policies\SecContoso.inf /areas securitypolicy /log C:\Security\FY11\SecAnalysisContosoFY12.log /quiet
```

## <a name="additional-references"></a>Referências adicionais

- [Chave da sintaxe de linha de comando](command-line-syntax-key.md)

- [secedit/Analyze](secedit-analyze.md)

- [secedit/configure](secedit-configure.md)

- [secedit/export](secedit-export.md)

- [/generaterollback secedit](secedit-generaterollback.md)

- [/Validate secedit](secedit-validate.md)