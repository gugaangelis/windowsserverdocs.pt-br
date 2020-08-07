---
title: 'secedit: importar'
description: Artigo de referência para * * * *-
ms.topic: article
ms.assetid: 1dd59d4c-9d48-444a-871b-b957eb682597
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 38f6b55d8c6cec30fbaa7de1fa3295df5cbfc26d
ms.sourcegitcommit: 53d526bfeddb89d28af44210a23ba417f6ce0ecf
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/06/2020
ms.locfileid: "87882943"
---
# <a name="seceditimport"></a>secedit: importar



Importa as configurações de segurança armazenadas em um arquivo inf exportado anteriormente do banco de dados configurado com modelos de segurança.

## <a name="syntax"></a>Sintaxe

```
Secedit /import /db <database file name> /cfg <configuration file name> [/overwrite] [/areas [securitypolicy | group_mgmt | user_rights | regkeys | filestore | services]] [/log <log file name>] [/quiet]
```

#### <a name="parameters"></a>Parâmetros

|Parâmetro|Descrição|
|---------|-----------|
|db|Obrigatórios.</br>Especifica o caminho e o nome de arquivo de um banco de dados que contém a configuração armazenada na qual a importação será executada.</br>Se o nome do arquivo especificar um banco de dados que não tenha um modelo de segurança (como representado pelo arquivo de configuração) associado a ele, a `/cfg \<configuration file name>` opção de linha de comando também deverá ser especificada.|
|substituir|Opcional.</br>Especifica se o modelo de segurança no parâmetro/cfg deve substituir qualquer modelo ou modelo composto armazenado no banco de dados em vez de acrescentar os resultados ao modelo armazenado.</br>Essa opção de linha de comando só é válida quando o `/cfg \<configuration file name>` parâmetro também é usado. Se não for especificado, o modelo no parâmetro/cfg será anexado ao modelo armazenado.|
|cfg|Obrigatórios.</br>Especifica o caminho e o nome do arquivo para o modelo de segurança que será importado para o banco de dados para análise.</br>Essa opção de/cfg só é válida quando usada com o `/db \<database file name>` parâmetro. Se isso não for especificado, a análise será executada em qualquer configuração já armazenada no banco de dados.|
|substituir|Opcional.</br>Especifica se o modelo de segurança no parâmetro/cfg deve substituir qualquer modelo ou modelo composto armazenado no banco de dados em vez de acrescentar os resultados ao modelo armazenado.</br>Essa opção de linha de comando só é válida quando o `/cfg \<configuration file name>` parâmetro também é usado. Se não for especificado, o modelo no parâmetro/cfg será anexado ao modelo armazenado.|
|áreas|Opcional.</br>Especifica as áreas de segurança a serem aplicadas ao sistema. Se esse parâmetro não for especificado, todas as configurações de segurança definidas no banco de dados serão aplicadas ao sistema. Para configurar várias áreas, separe cada área por um espaço. Há suporte para as seguintes áreas de segurança:</br>-SecurityPolicy</br>    Política local e política de domínio para o sistema, incluindo políticas de conta, políticas de auditoria, opções de segurança e assim por diante.</br>-Group_Mgmt</br>    Configurações de grupo restrito para todos os grupos especificados no modelo de segurança.</br>-User_Rights</br>    Direitos de logon de usuário e concessão de privilégios.</br>- RegKeys</br>    Segurança em chaves do Registro local.</br>-Filestore</br>    Segurança no armazenamento de arquivos local.</br>-Serviços</br>    Segurança para todos os serviços definidos.|
|log|Opcional.</br>Especifica o caminho e o nome do arquivo de log para o processo.|
|silencioso|Opcional.</br>Suprime a saída de tela e de log. Você ainda pode exibir os resultados da análise usando o snap-in configuração e análise de segurança no console de gerenciamento Microsoft (MMC).|

## <a name="remarks"></a>Comentários

Antes de importar um arquivo. inf para outro computador, execute o comando secedit/generaterollback no banco de dados em que a importação será executada e secedit/validate no arquivo de importação para verificar sua integridade.

Se o caminho para o arquivo de log não for fornecido, o arquivo de log padrão, (*raiz_do_sistema*\Documents and Settings \* USERACCOUNT<em>\Meus Documents\Security\Logs \* DatabaseName</em>. log) será usado.

No Windows Server 2008, foi `Secedit /refreshpolicy` substituído por `gpupdate` . Para obter informações sobre como atualizar as configurações de segurança, consulte [gpupdate](gpupdate.md).

## <a name="examples"></a>Exemplos

Exporte o banco de dados de segurança e as políticas de segurança de domínio para um arquivo inf e, em seguida, importe esse arquivo para um banco de dados diferente para replicar as configurações de política de segurança em outro computador.
```
Secedit /export /db C:\Security\FY11\SecDbContoso.sdb /mergedpolicy /cfg NetworkShare\Policies\SecContoso.inf /log C:\Security\FY11\SecAnalysisContosoFY11.log /quiet
```
Importe apenas a parte das políticas de segurança do arquivo para um banco de dados diferente em outro computador.
```
Secedit /import /db C:\Security\FY12\SecDbContoso.sdb /cfg NetworkShare\Policies\SecContoso.inf /areas securitypolicy /log C:\Security\FY11\SecAnalysisContosoFY12.log /quiet
```

## <a name="additional-references"></a>Referências adicionais

-   [Secedit:export](secedit-export.md)
-   [Secedit:generaterollback](secedit-generaterollback.md)
-   [Secedit:validate](secedit-validate.md)
-   [Utilitário](secedit.md)
- [Chave da sintaxe de linha de comando](command-line-syntax-key.md)