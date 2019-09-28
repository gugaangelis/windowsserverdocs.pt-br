---
title: 'secedit: configurar'
description: 'Tópico de comandos do Windows para * * * *- '
ms.custom: na
ms.prod: windows-server
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
ms.openlocfilehash: f0e1b900d01ad7f0e84d3235f24a00fe108eaa36
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71384272"
---
# <a name="seceditconfigure"></a>secedit: configurar



Permite que você defina as configurações atuais do sistema usando as configurações de segurança armazenadas em um banco de dados. Para obter exemplos de como esse comando pode ser usado, consulte [exemplos](#BKMK_Examples).

## <a name="syntax"></a>Sintaxe

```
Secedit /configure /db <database file name> [/cfg <configuration file name>] [/overwrite] [/areas SECURITYPOLICY | GROUP_MGMT | USER_RIGHTS | REGKEYS | FILESTORE | SERVICES] [/log <log file name>] [/quiet]
```

### <a name="parameters"></a>Parâmetros

|Parâmetro|Descrição|
|---------|-----------|
|db|Obrigatório.</br>Especifica o caminho e o nome de arquivo de um banco de dados que contém a configuração armazenada.</br>Se o nome do arquivo especificar um banco de dados que não tenha um modelo de segurança (como representado pelo arquivo de configuração) associado `/cfg \<configuration file name>` a ele, a opção de linha de comando também deverá ser especificada.|
|cfg|Opcional.</br>Especifica o caminho e o nome do arquivo para o modelo de segurança que será importado para o banco de dados para análise.</br>Essa opção de/cfg só é válida quando usada com `/db \<database file name>` o parâmetro. Se isso não for especificado, a análise será executada em qualquer configuração já armazenada no banco de dados.|
|overwrite|Opcional.</br>Especifica se o modelo de segurança no parâmetro/cfg deve substituir qualquer modelo ou modelo composto armazenado no banco de dados em vez de acrescentar os resultados ao modelo armazenado.</br>Essa opção de linha de comando só é válida quando `/cfg \<configuration file name>` o parâmetro também é usado. Se não for especificado, o modelo no parâmetro/cfg será anexado ao modelo armazenado.|
|Área|Opcional.</br>Especifica as áreas de segurança a serem aplicadas ao sistema. Se esse parâmetro não for especificado, todas as configurações de segurança definidas no banco de dados serão aplicadas ao sistema. Para configurar várias áreas, separe cada área por um espaço. Há suporte para as seguintes áreas de segurança:</br>-SecurityPolicy</br>    Política local e política de domínio para o sistema, incluindo políticas de conta, políticas de auditoria, opções de segurança e assim por diante.</br>- Group_Mgmt</br>    Configurações de grupo restrito para todos os grupos especificados no modelo de segurança.</br>- User_Rights</br>    Direitos de logon de usuário e concessão de privilégios.</br>- RegKeys</br>    Segurança em chaves do Registro local.</br>-Filestore</br>    Segurança no armazenamento de arquivos local.</br>-Serviços</br>    Segurança para todos os serviços definidos.|
|Façam|Opcional.</br>Especifica o caminho e o nome do arquivo de log para o processo.|
|Tranqüilo|Opcional.</br>Suprime a saída de tela e de log. Você ainda pode exibir os resultados da análise usando o snap-in configuração e análise de segurança no console de gerenciamento Microsoft (MMC).|

## <a name="remarks"></a>Comentários

Se o caminho para o arquivo de log não for fornecido, o arquivo de log padrão, ( \*raiz_do_sistema \ usuários USERACCOUNT<em>\*\Meus Documents\Security\Logs DatabaseName</em>. log) será usado.

A partir do Windows Server 2008 `Secedit /refreshpolicy` , foi substituído por `gpupdate`. Para obter informações sobre como atualizar as configurações de segurança, consulte [gpupdate](gpupdate.md).

## <a name="BKMK_Examples"></a>Disso

Execute a análise dos parâmetros de segurança no banco de dados de segurança, SecDbContoso. sdb, criado usando o snap-in configuração e análise de segurança. Direcione a saída para o arquivo SecAnalysisContosoFY11 com a solicitação para que você possa verificar se o comando foi executado corretamente.
```
Secedit /analyze /db C:\Security\FY11\SecDbContoso.sdb /log C:\Security\FY11\SecAnalysisContosoFY11.log
```
Digamos que a análise revelou alguns inadequacies para que o modelo de segurança, SecContoso. inf, tenha sido modificado. Execute o comando novamente para incorporar as alterações, direcionando a saída para o arquivo existente SecAnalysisContosoFY11 sem aviso.
```
Secedit /configure /db C:\Security\FY11\SecDbContoso.sdb /cfg SecContoso.inf /overwrite /log C:\Security\FY11\SecAnalysisContosoFY11.xml /quiet
```

#### <a name="additional-references"></a>Referências adicionais

-   [Utilitário](secedit.md)
-   [Secedit:analyze](secedit-analyze.md)
-   [Chave da sintaxe de linha de comando](command-line-syntax-key.md)