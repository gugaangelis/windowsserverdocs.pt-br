---
title: secedit:generaterollback
description: 'Tópico de comandos do Windows para * * *- '
ms.custom: na
ms.prod: windows-server-threshold
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
ms.openlocfilehash: 3229e6ccb07c925a900b298a8332c5e48cefefe7
ms.sourcegitcommit: 08eba714d3ceb5f2dfb5486d6b990da1aa4dcbdd
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 05/14/2019
ms.locfileid: "65564673"
---
# <a name="seceditgeneraterollback"></a>secedit:generaterollback



Permite que você gerar um modelo de reversão para um modelo de configuração especificado. Para obter exemplos de como esse comando pode ser usado, consulte [exemplos](#BKMK_Examples).

## <a name="syntax"></a>Sintaxe

```
Secedit /generaterollback /db <database file name> /cfg <configuration file name> /rbk <rollback template file name> [log <log file name>] [/quiet]
```

### <a name="parameters"></a>Parâmetros

|Parâmetro|Descrição|
|---------|-----------|
|db|Obrigatório.</br>Especifica o nome de arquivo e caminho de um banco de dados que contém a configuração armazenada em relação ao qual a análise será executada.</br>Se o nome do arquivo Especifica um banco de dados que não tenha um modelo de segurança (conforme representado pelo arquivo de configuração) associado a ele, o `/cfg \<configuration file name>` também deve ser especificada a opção de linha de comando.|
|cfg|Obrigatório.</br>Especifica o nome de arquivo e caminho para o modelo de segurança que será importado para o banco de dados para análise.</br>Essa opção /cfg só é válida quando usado com o `/db \<database file name>` parâmetro. Se não for especificado, a análise é executada em relação a qualquer configuração já armazenada no banco de dados.|
|rbk|Obrigatório.</br>Especifica um modelo de segurança no qual as informações de reversão são gravadas. Modelos de segurança são criados usando o snap-in de modelos de segurança. Arquivos de conversão podem ser criados com esse comando.|
|log|Opcional.</br>Especifica o nome de arquivo e caminho do arquivo de log para o processo.|
|Silencioso|Opcional.</br>Suprime a saída de log e de tela. Você ainda pode exibir resultados da análise, usando a configuração de segurança e análise de snap-in para o Microsoft Management Console (MMC).|

## <a name="remarks"></a>Comentários

Se o caminho para o arquivo de log não for fornecido, o arquivo de log padrão (*systemroot*\Users \*UserAccount *\My Documents\Security\Logs\*DatabaseName*. log) é usado.

Começando com o Windows Server 2008, `Secedit /refreshpolicy` foi substituído por `gpupdate`. Para obter informações sobre como atualizar as configurações de segurança, consulte [Gpupdate](gpupdate.md).

A execução bem-sucedida desse comando informará "a tarefa foi concluída com êxito". e apenas as incompatibilidades entre o modelo de segurança estabelecidas e a configuração de política de segurança de logs. Ele lista dessas incompatibilidades no scesrv.

Se um modelo de reversão existente for especificado, esse comando irá substituí-lo. Você pode criar um novo modelo de reversão com este comando. Sem parâmetros adicionais são necessários para uma dessas condições.

## <a name="BKMK_Examples"></a>Exemplos

Depois de criar o modelo de segurança usando a configuração de segurança e análise snap-in, SecTmplContoso.inf, crie o arquivo de configuração de reversão para salvar as configurações originais. Gravar a ação para o arquivo de log FY11.
```
Secedit /generaterollback /db C:\Security\FY11\SecDbContoso.sdb /cfg sectmplcontoso.inf /rbk sectmplcontosoRBK.inf /log C:\Security\FY11\SecAnalysisContosoFY11.log
```

#### <a name="additional-references"></a>Referências adicionais

-   [Secedit](secedit.md)
-   [Chave da sintaxe de linha de comando](command-line-syntax-key.md)