---
title: 'secedit: generaterollback'
description: 'Tópico de comandos do Windows para * * * *- '
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 385a6799-51a7-4fe3-bd73-10c7998b6680
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 3ce4bd83e6eda24c10f65bd9d450a204906ff7fd
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71384224"
---
# <a name="seceditgeneraterollback"></a>secedit: generaterollback



Permite gerar um modelo de reversão para um modelo de configuração especificado. Para obter exemplos de como esse comando pode ser usado, consulte [exemplos](#BKMK_Examples).

## <a name="syntax"></a>Sintaxe

```
Secedit /generaterollback /db <database file name> /cfg <configuration file name> /rbk <rollback template file name> [log <log file name>] [/quiet]
```

### <a name="parameters"></a>Parâmetros

|Parâmetro|Descrição|
|---------|-----------|
|db|Obrigatório.</br>Especifica o caminho e o nome de arquivo de um banco de dados que contém a configuração armazenada na qual a análise será executada.</br>Se o nome do arquivo especificar um banco de dados que não tenha um modelo de segurança (como representado pelo arquivo de configuração) associado `/cfg \<configuration file name>` a ele, a opção de linha de comando também deverá ser especificada.|
|cfg|Obrigatório.</br>Especifica o caminho e o nome do arquivo para o modelo de segurança que será importado para o banco de dados para análise.</br>Essa opção de/cfg só é válida quando usada com `/db \<database file name>` o parâmetro. Se isso não for especificado, a análise será executada em qualquer configuração já armazenada no banco de dados.|
|rbk|Obrigatório.</br>Especifica um modelo de segurança no qual as informações de reversão são gravadas. Os modelos de segurança são criados usando o snap-in modelos de segurança. Os arquivos de reversão podem ser criados com este comando.|
|Façam|Opcional.</br>Especifica o caminho e o nome do arquivo de log para o processo.|
|Tranqüilo|Opcional.</br>Suprime a saída de tela e de log. Você ainda pode exibir os resultados da análise usando o snap-in configuração e análise de segurança no console de gerenciamento Microsoft (MMC).|

## <a name="remarks"></a>Comentários

Se o caminho para o arquivo de log não for fornecido, o arquivo de log padrão, ( \*raiz_do_sistema \ usuários USERACCOUNT<em>\*\Meus Documents\Security\Logs DatabaseName</em>. log) será usado.

A partir do Windows Server 2008 `Secedit /refreshpolicy` , foi substituído por `gpupdate`. Para obter informações sobre como atualizar as configurações de segurança, consulte [gpupdate](gpupdate.md).

A execução bem-sucedida desse comando irá declarar "a tarefa foi concluída com êxito". e registra apenas as incompatibilidades entre o modelo de segurança declarado e a configuração da política de segurança. Ele lista essas diferenças no scesrv. log.

Se um modelo de reversão existente for especificado, esse comando irá substituí-lo. Você pode criar um novo modelo de reversão com esse comando. Nenhum parâmetro adicional é necessário para qualquer condição.

## <a name="BKMK_Examples"></a>Disso

Depois de criar o modelo de segurança usando o snap-in configuração e análise de segurança, SecTmplContoso. inf, crie o arquivo de configuração de reversão para salvar as configurações originais. Escreva a ação no arquivo de log FY11.
```
Secedit /generaterollback /db C:\Security\FY11\SecDbContoso.sdb /cfg sectmplcontoso.inf /rbk sectmplcontosoRBK.inf /log C:\Security\FY11\SecAnalysisContosoFY11.log
```

#### <a name="additional-references"></a>Referências adicionais

-   [Utilitário](secedit.md)
-   [Chave da sintaxe de linha de comando](command-line-syntax-key.md)