---
title: 'Secedit: analisar'
description: 'Tópico de comandos do Windows para * * *- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 3430cf9d-1411-48b1-b5a9-2e47701dc87f
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 324da8de153a5487c9d71872cd154928cc24c285
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59848817"
---
# <a name="seceditanalyze"></a>Secedit: analisar



Permite que você analise as configurações de sistemas atuais em relação a configurações de linha de base que são armazenadas em um banco de dados. Para obter exemplos de como esse comando pode ser usado, consulte [exemplos](#BKMK_Examples).

## <a name="syntax"></a>Sintaxe

```
Secedit /analyze /db <database file name> [/cfg <configuration file name>] [/overwrite] [/log <log file name>] [/quiet}]
```

### <a name="parameters"></a>Parâmetros

|Parâmetro|Descrição|
|---------|-----------|
|db|Obrigatório.</br>Especifica o nome de arquivo e caminho de um banco de dados que contém a configuração armazenada em relação ao qual a análise será executada.</br>Se o nome do arquivo Especifica um banco de dados que não tenha um modelo de segurança (conforme representado pelo arquivo de configuração) associado a ele, o `/cfg \<configuration file name>` também deve ser especificada a opção de linha de comando.|
|cfg|Opcional.</br>Especifica o nome de arquivo e caminho para o modelo de segurança que será importado para o banco de dados para análise.</br>Essa opção /cfg só é válida quando usado com o `/db \<database file name>` parâmetro. Se não for especificado, a análise é executada em relação a qualquer configuração já armazenada no banco de dados.|
|overwrite|Opcional.</br>Especifica se o modelo de segurança no parâmetro /cfg deve substituir qualquer modelo ou modelo de composição que é armazenado no banco de dados, em vez de acrescentar os resultados ao modelo armazenado.</br>Essa opção de linha de comando é válido somente quando o `/cfg \<configuration file name>` parâmetro também é usado. Se não for especificado, o modelo no parâmetro /cfg é acrescentado ao modelo armazenado.|
|log|Opcional.</br>Especifica o nome de arquivo e caminho do arquivo de log a ser usado no processo.|
|Silencioso|Opcional.</br>Suprime a saída de tela. Você ainda pode exibir resultados da análise, usando a configuração de segurança e análise de snap-in para o Microsoft Management Console (MMC).|

## <a name="remarks"></a>Comentários

Os resultados da análise são armazenados em uma área separada do banco de dados e podem ser exibidos na configuração de segurança e análise snap-in do MMC.

Se o caminho para o arquivo de log não for fornecido, o arquivo de log padrão (*systemroot*\Documents and Settings\*UserAccount*\My Documents\Security\Logs\*DatabaseName*. log) é usado.

No Windows Server 2008, `Secedit /refreshpolicy` foi substituído por `gpupdate`. Para obter informações sobre como atualizar as configurações de segurança, consulte [Gpupdate](gpupdate.md).

## <a name="BKMK_Examples"></a>Exemplos

Execute a análise para os parâmetros de segurança do banco de dados de segurança, SecDbContoso.sdb, você criou usando o snap-in de análise e configuração de segurança. Direcione a saída para o arquivo SecAnalysisContosoFY11 com solicitar para que você possa verificar o comando foi executado corretamente.
```
Secedit /analyze /db C:\Security\FY11\SecDbContoso.sdb /log C:\Security\FY11\SecAnalysisContosoFY11.log
```
Digamos que a análise revelou alguns inadequações para que o modelo de segurança, SecContoso.inf, foi modificado. Execute o comando novamente para incorporar as alterações, direcionando a saída para o arquivo existente SecAnalysisContosoFY11 com nenhum aviso.
```
Secedit /analyze /db C:\Security\FY11\SecDbContoso.sdb /cfg SecContoso.inf /overwrite /log C:\Security\FY11\SecAnalysisContosoFY11.xml /quiet
```

#### <a name="additional-references"></a>Referências adicionais

-   [Secedit](secedit.md)
-   [Chave de sintaxe de linha de comando](command-line-syntax-key.md)