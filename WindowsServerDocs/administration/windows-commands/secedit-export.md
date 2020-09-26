---
title: secedit export
description: Artigo de referência para a exportação do secedit, que exporta as configurações de segurança armazenadas em um banco de dados configurado com modelos de segurança.
ms.topic: reference
ms.assetid: 49a8b241-aa8c-45b7-844d-67a29fab708e
ms.author: lizross
author: eross-msft
manager: mtillman
ms.date: 10/16/2017
ms.openlocfilehash: dfd766674a4e4ac2e9552d36b4cb706117c173bd
ms.sourcegitcommit: e164aeffc01069b8f1f3248bf106fcdb7f64f894
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/26/2020
ms.locfileid: "91388239"
---
# <a name="secedit-export"></a>secedit/export

Exporta as configurações de segurança armazenadas em um banco de dados configurado com modelos de segurança. Você pode usar esse comando para fazer backup de suas políticas de segurança em um computador local, além de importar as configurações para outro computador.

## <a name="syntax"></a>Sintaxe

```
secedit /export /db <database file name> [/mergedpolicy] /cfg <configuration file name> [/areas [securitypolicy | group_mgmt | user_rights | regkeys | filestore | services]] [/log <log file name>] [/quiet]
```

### <a name="parameters"></a>Parâmetros

| Parâmetro | Descrição |
|--|--|
| /DB | Obrigatórios. Especifica o caminho e o nome do arquivo do banco de dados que contém a configuração armazenada na qual a exportação é executada. Se o nome do arquivo especificar um banco de dados que não tinha um modelo de segurança (como representado pelo arquivo de configuração) associado a ele, a `/cfg <configuration file name>` opção também deverá ser especificada. |
| /mergedpolicy | Mescla e exporta configurações de segurança de diretiva local e de domínio. |
| /cfg | Obrigatórios. Especifica o caminho e o nome do arquivo para o modelo de segurança que será importado para o banco de dados para análise. Essa opção só é válida quando usada com o `/db <database file name>` parâmetro. Se esse parâmetro também não for especificado, a análise será executada em qualquer configuração já armazenada no banco de dados. |
| /areas | Especifica as áreas de segurança a serem aplicadas ao sistema. Se esse parâmetro não for especificado, todas as configurações de segurança definidas no banco de dados serão aplicadas ao sistema. Para configurar várias áreas, separe cada área por um espaço. Há suporte para as seguintes áreas de segurança:<ul><li>**SecurityPolicy:** Política local e política de domínio para o sistema, incluindo políticas de conta, políticas de auditoria, opções de segurança e assim por diante.</li><li>  **GROUP_MGMT:** Configurações de grupo restrito para todos os grupos especificados no modelo de segurança.</li><li>**user_rights:** Direitos de logon de usuário e concessão de privilégios.</li><li>**regkeys:** Segurança em chaves do Registro local.</li><li>**repositório de armazenamento:** Segurança no armazenamento de arquivos local.</li><li>**Serviços:** Segurança para todos os serviços definidos.</li></ul> |
| /log | Especifica o caminho e o nome do arquivo de log a ser usado no processo. Se você não especificar um local de arquivo, o arquivo de log padrão `<systemroot>\Documents and Settings\<UserAccount>\My Documents\Security\Logs\<databasename>.log` será usado. |
| /quiet | Suprime a saída de tela e de log. Você ainda pode exibir os resultados da análise usando o snap-in configuração e análise de segurança no console de gerenciamento Microsoft (MMC). |

## <a name="examples"></a>Exemplos

Para exportar o banco de dados de segurança e as políticas de segurança de domínio para um arquivo inf e, em seguida, importar esse arquivo para um banco de dados diferente para replicar as configurações de política de segurança em outro computador, digite:

```
secedit /export /db C:\Security\FY11\SecDbContoso.sdb /mergedpolicy /cfg SecContoso.inf /log C:\Security\FY11\SecAnalysisContosoFY11.log /quiet
```

Para importar o arquivo de exemplo para um banco de dados diferente em outro computador, digite:

```
secedit /import /db C:\Security\FY12\SecDbContoso.sdb /cfg SecContoso.inf /log C:\Security\FY11\SecAnalysisContosoFY12.log /quiet
```

## <a name="additional-references"></a>Referências adicionais

- [Chave da sintaxe de linha de comando](command-line-syntax-key.md)

- [secedit/Analyze](secedit-analyze.md)

- [secedit/configure](secedit-configure.md)

- [/generaterollback secedit](secedit-generaterollback.md)

- [secedit/Import](secedit-import.md)

- [/Validate secedit](secedit-validate.md)