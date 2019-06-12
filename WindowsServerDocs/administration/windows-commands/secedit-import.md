---
title: Secedit:Import
description: 'Tópico de comandos do Windows para * * *- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 1dd59d4c-9d48-444a-871b-b957eb682597
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 305b915a0d7e8ab152b072ff131854f56b9b0386
ms.sourcegitcommit: eaf071249b6eb6b1a758b38579a2d87710abfb54
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 05/31/2019
ms.locfileid: "66441529"
---
# <a name="seceditimport"></a>Secedit:Import



Importa as configurações de segurança armazenadas em um arquivo inf previamente exportado de um banco de dados configurado com modelos de segurança. Para obter exemplos de como esse comando pode ser usado, consulte [exemplos](#BKMK_Examples).

## <a name="syntax"></a>Sintaxe

```
Secedit /import /db <database file name> /cfg <configuration file name> [/overwrite] [/areas [securitypolicy | group_mgmt | user_rights | regkeys | filestore | services]] [/log <log file name>] [/quiet]
```

### <a name="parameters"></a>Parâmetros

|Parâmetro|Descrição|
|---------|-----------|
|db|Obrigatório.</br>Especifica o nome de arquivo e caminho de um banco de dados que contém a configuração armazenada no qual será realizada a importação.</br>Se o nome do arquivo Especifica um banco de dados que não tenha um modelo de segurança (conforme representado pelo arquivo de configuração) associado a ele, o `/cfg \<configuration file name>` também deve ser especificada a opção de linha de comando.|
|overwrite|Opcional.</br>Especifica se o modelo de segurança no parâmetro /cfg deve substituir qualquer modelo ou modelo de composição que é armazenado no banco de dados, em vez de acrescentar os resultados ao modelo armazenado.</br>Essa opção de linha de comando é válido somente quando o `/cfg \<configuration file name>` parâmetro também é usado. Se não for especificado, o modelo no parâmetro /cfg é acrescentado ao modelo armazenado.|
|cfg|Obrigatório.</br>Especifica o nome de arquivo e caminho para o modelo de segurança que será importado para o banco de dados para análise.</br>Essa opção /cfg só é válida quando usado com o `/db \<database file name>` parâmetro. Se não for especificado, a análise é executada em relação a qualquer configuração já armazenada no banco de dados.|
|overwrite|Opcional.</br>Especifica se o modelo de segurança no parâmetro /cfg deve substituir qualquer modelo ou modelo de composição que é armazenado no banco de dados, em vez de acrescentar os resultados ao modelo armazenado.</br>Essa opção de linha de comando é válido somente quando o `/cfg \<configuration file name>` parâmetro também é usado. Se não for especificado, o modelo no parâmetro /cfg é acrescentado ao modelo armazenado.|
|Áreas|Opcional.</br>Especifica as áreas de segurança a ser aplicado ao sistema. Se esse parâmetro não for especificado, todas as configurações de segurança definidas no banco de dados são aplicadas ao sistema. Para configurar várias áreas, separe cada área por um espaço. Há suporte para as seguintes áreas de segurança:</br>-   SecurityPolicy</br>    Diretiva local e diretiva de domínio para o sistema, incluindo diretivas de conta, auditoria de políticas, as opções de segurança e assim por diante.</br>-   Group_Mgmt</br>    Configurações de grupo restrito para quaisquer grupos especificados no modelo de segurança.</br>-User_Rights</br>    Concessão de privilégios e direitos de logon do usuário.</br>-   RegKeys</br>    Segurança nas chaves do Registro local.</br>-   FileStore</br>    Segurança no armazenamento de arquivos local.</br>-Serviços</br>    Segurança para todos os serviços definidos.|
|log|Opcional.</br>Especifica o nome de arquivo e caminho do arquivo de log para o processo.|
|Silencioso|Opcional.</br>Suprime a saída de log e de tela. Você ainda pode exibir resultados da análise, usando a configuração de segurança e análise de snap-in para o Microsoft Management Console (MMC).|

## <a name="remarks"></a>Comentários

Antes de importar um arquivo. inf em outro computador, execute o /generaterollback secedit de comando no banco de dados qual será realizada a importação e secedit / validar no arquivo de importação para verificar sua integridade.

Se o caminho para o arquivo de log não for fornecido, o arquivo de log padrão (*systemroot*\Documents and Settings\*UserAccount<em>\My Documents\Security\Logs\*DatabaseName</em>. log) é usado.

No Windows Server 2008, `Secedit /refreshpolicy` foi substituído por `gpupdate`. Para obter informações sobre como atualizar as configurações de segurança, consulte [Gpupdate](gpupdate.md).

## <a name="BKMK_Examples"></a>Exemplos

Exportar o banco de dados de segurança e as políticas de segurança de domínio para um arquivo inf e, em seguida, importar esse arquivo para outro banco de dados para replicar as configurações de política de segurança em outro computador.
```
Secedit /export /db C:\Security\FY11\SecDbContoso.sdb /mergedpolicy /cfg NetworkShare\Policies\SecContoso.inf /log C:\Security\FY11\SecAnalysisContosoFY11.log /quiet
```
Importe apenas a parte de políticas de segurança do arquivo para outro banco de dados em outro computador.
```
Secedit /import /db C:\Security\FY12\SecDbContoso.sdb /cfg NetworkShare\Policies\SecContoso.inf /areas securitypolicy /log C:\Security\FY11\SecAnalysisContosoFY12.log /quiet
```

#### <a name="additional-references"></a>Referências adicionais

-   [Secedit:export](secedit-export.md)
-   [Secedit:generaterollback](secedit-generaterollback.md)
-   [Secedit:validate](secedit-validate.md)
-   [Secedit](secedit.md)
-   [Chave da sintaxe de linha de comando](command-line-syntax-key.md)