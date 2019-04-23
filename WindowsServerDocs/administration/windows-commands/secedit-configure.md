---
title: 'Secedit: configurar'
description: 'Tópico de comandos do Windows para * * *- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: a92e68ca-003c-4219-8655-0e7734f5fab3
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 8285c22c3c64b4f056124d8a1bb02297c7aea3c8
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59853387"
---
# <a name="seceditconfigure"></a>Secedit: configurar



Permite que você configure as configurações atuais do sistema usando as configurações de segurança armazenadas em um banco de dados. Para obter exemplos de como esse comando pode ser usado, consulte [exemplos](#BKMK_Examples).

## <a name="syntax"></a>Sintaxe

```
Secedit /configure /db <database file name> [/cfg <configuration file name>] [/overwrite] [/areas SECURITYPOLICY | GROUP_MGMT | USER_RIGHTS | REGKEYS | FILESTORE | SERVICES] [/log <log file name>] [/quiet]
```

### <a name="parameters"></a>Parâmetros

|Parâmetro|Descrição|
|---------|-----------|
|db|Obrigatório.</br>Especifica o nome de arquivo e caminho de um banco de dados que contém a configuração armazenada.</br>Se o nome do arquivo Especifica um banco de dados que não tenha um modelo de segurança (conforme representado pelo arquivo de configuração) associado a ele, o `/cfg \<configuration file name>` também deve ser especificada a opção de linha de comando.|
|cfg|Opcional.</br>Especifica o nome de arquivo e caminho para o modelo de segurança que será importado para o banco de dados para análise.</br>Essa opção /cfg só é válida quando usado com o `/db \<database file name>` parâmetro. Se não for especificado, a análise é executada em relação a qualquer configuração já armazenada no banco de dados.|
|overwrite|Opcional.</br>Especifica se o modelo de segurança no parâmetro /cfg deve substituir qualquer modelo ou modelo de composição que é armazenado no banco de dados, em vez de acrescentar os resultados ao modelo armazenado.</br>Essa opção de linha de comando é válido somente quando o `/cfg \<configuration file name>` parâmetro também é usado. Se não for especificado, o modelo no parâmetro /cfg é acrescentado ao modelo armazenado.|
|Áreas|Opcional.</br>Especifica as áreas de segurança a ser aplicado ao sistema. Se esse parâmetro não for especificado, todas as configurações de segurança definidas no banco de dados são aplicadas ao sistema. Para configurar várias áreas, separe cada área por um espaço. Há suporte para as seguintes áreas de segurança:</br>-   SecurityPolicy</br>    Diretiva local e diretiva de domínio para o sistema, incluindo diretivas de conta, auditoria de políticas, as opções de segurança e assim por diante.</br>-   Group_Mgmt</br>    Configurações de grupo restrito para quaisquer grupos especificados no modelo de segurança.</br>-User_Rights</br>    Concessão de privilégios e direitos de logon do usuário.</br>-   RegKeys</br>    Segurança nas chaves do Registro local.</br>-   FileStore</br>    Segurança no armazenamento de arquivos local.</br>-Serviços</br>    Segurança para todos os serviços definidos.|
|log|Opcional.</br>Especifica o nome de arquivo e caminho do arquivo de log para o processo.|
|Silencioso|Opcional.</br>Suprime a saída de log e de tela. Você ainda pode exibir resultados da análise, usando a configuração de segurança e análise de snap-in para o Microsoft Management Console (MMC).|

## <a name="remarks"></a>Comentários

Se o caminho para o arquivo de log não for fornecido, o arquivo de log padrão (*systemroot*\Users \*UserAccount*\My Documents\Security\Logs\*DatabaseName*. log) é usado.

Começando com o Windows Server 2008, `Secedit /refreshpolicy` foi substituído por `gpupdate`. Para obter informações sobre como atualizar as configurações de segurança, consulte [Gpupdate](gpupdate.md).

## <a name="BKMK_Examples"></a>Exemplos

Execute a análise para os parâmetros de segurança do banco de dados de segurança, SecDbContoso.sdb, você criou usando o snap-in de análise e configuração de segurança. Direcione a saída para o arquivo SecAnalysisContosoFY11 com solicitar para que você possa verificar o comando foi executado corretamente.
```
Secedit /analyze /db C:\Security\FY11\SecDbContoso.sdb /log C:\Security\FY11\SecAnalysisContosoFY11.log
```
Digamos que a análise revelou alguns inadequações para que o modelo de segurança, SecContoso.inf, foi modificado. Execute o comando novamente para incorporar as alterações, direcionando a saída para o arquivo existente SecAnalysisContosoFY11 com nenhum aviso.
```
Secedit /configure /db C:\Security\FY11\SecDbContoso.sdb /cfg SecContoso.inf /overwrite /log C:\Security\FY11\SecAnalysisContosoFY11.xml /quiet
```

#### <a name="additional-references"></a>Referências adicionais

-   [Secedit](secedit.md)
-   [Secedit: analisar](secedit-analyze.md)
-   [Chave de sintaxe de linha de comando](command-line-syntax-key.md)