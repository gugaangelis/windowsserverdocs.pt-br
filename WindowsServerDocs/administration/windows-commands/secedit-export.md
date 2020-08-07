---
title: 'secedit: exportar'
description: Artigo de referência para * * * *-
ms.topic: article
ms.assetid: 49a8b241-aa8c-45b7-844d-67a29fab708e
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 5079ecc2874c1d834fb351ebb51bf140309e4248
ms.sourcegitcommit: 53d526bfeddb89d28af44210a23ba417f6ce0ecf
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/06/2020
ms.locfileid: "87882981"
---
# <a name="seceditexport"></a>secedit: exportar



Exporta as configurações de segurança armazenadas em um banco de dados configurado com modelos de segurança.

## <a name="syntax"></a>Sintaxe

```
Secedit /export /db <database file name> [/mergedpolicy] /cfg <configuration file name> [/areas [securitypolicy | group_mgmt | user_rights | regkeys | filestore | services]] [/log <log file name>] [/quiet]
```

#### <a name="parameters"></a>Parâmetros

|Parâmetro|Descrição|
|---------|-----------|
|db|Obrigatórios.</br>Especifica o caminho e o nome de arquivo de um banco de dados que contém a configuração armazenada na qual a análise será executada.</br>Se o nome do arquivo especificar um banco de dados que não tenha um modelo de segurança (como representado pelo arquivo de configuração) associado a ele, a `/cfg \<configuration file name>` opção de linha de comando também deverá ser especificada.|
|mergedpolicy|Opcional.</br>Mescla e exporta configurações de segurança de diretiva local e de domínio.|
|cfg|Obrigatórios.</br>Especifica o caminho e o nome do arquivo para o modelo de segurança que será importado para o banco de dados para análise.</br>Essa opção de/cfg só é válida quando usada com o `/db \<database file name>` parâmetro. Se isso não for especificado, a análise será executada em qualquer configuração já armazenada no banco de dados.|
|áreas|Opcional.</br>Especifica as áreas de segurança a serem aplicadas ao sistema. Se esse parâmetro não for especificado, todas as configurações de segurança definidas no banco de dados serão aplicadas ao sistema. Para configurar várias áreas, separe cada área por um espaço. Há suporte para as seguintes áreas de segurança:</br>-SecurityPolicy</br>    Política local e política de domínio para o sistema, incluindo políticas de conta, políticas de auditoria, opções de segurança e assim por diante.</br>-Group_Mgmt</br>    Configurações de grupo restrito para todos os grupos especificados no modelo de segurança.</br>-User_Rights</br>    Direitos de logon de usuário e concessão de privilégios.</br>- RegKeys</br>    Segurança em chaves do Registro local.</br>-Filestore</br>    Segurança no armazenamento de arquivos local.</br>-Serviços</br>    Segurança para todos os serviços definidos.|
|log|Opcional.</br>Especifica o caminho e o nome do arquivo de log para o processo.|
|silencioso|Opcional.</br>Suprime a saída de tela e de log. Você ainda pode exibir os resultados da análise usando o snap-in configuração e análise de segurança no console de gerenciamento Microsoft (MMC).|

## <a name="remarks"></a>Comentários

Você pode usar esse comando para fazer backup de suas políticas de segurança em um computador local, além de importar as configurações para outro computador.

Se o caminho para o arquivo de log não for fornecido, o arquivo de log padrão, (*raiz_do_sistema*\Documents and Settings \* USERACCOUNT<em>\Meus Documents\Security\Logs \* DatabaseName</em>. log) será usado.

No Windows Server 2008, foi `Secedit /refreshpolicy` substituído por `gpupdate` . Para obter informações sobre como atualizar as configurações de segurança, consulte [gpupdate](gpupdate.md).

## <a name="examples"></a>Exemplos

Exporte o banco de dados de segurança e as políticas de segurança de domínio para um arquivo inf e, em seguida, importe esse arquivo para um banco de dados diferente para replicar as configurações de política de segurança em outro computador.
```
Secedit /export /db C:\Security\FY11\SecDbContoso.sdb /mergedpolicy /cfg SecContoso.inf /log C:\Security\FY11\SecAnalysisContosoFY11.log /quiet
```
Importe esse arquivo para um banco de dados diferente em outro computador.
```
Secedit /import /db C:\Security\FY12\SecDbContoso.sdb /cfg SecContoso.inf /log C:\Security\FY11\SecAnalysisContosoFY12.log /quiet
```

## <a name="additional-references"></a>Referências adicionais

-   [Secedit:import](secedit-import.md)
-   [Utilitário](secedit.md)
- [Chave da sintaxe de linha de comando](command-line-syntax-key.md)