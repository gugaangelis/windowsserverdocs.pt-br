---
title: secedit:export
description: 'Tópico de comandos do Windows para * * *- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 49a8b241-aa8c-45b7-844d-67a29fab708e
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 398d2fa47f2418aec910569c2eb85aec408ad482
ms.sourcegitcommit: eaf071249b6eb6b1a758b38579a2d87710abfb54
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 05/31/2019
ms.locfileid: "66441595"
---
# <a name="seceditexport"></a>secedit:export



Exporta as configurações de segurança armazenadas no banco de dados configurado com modelos de segurança. Para obter exemplos de como esse comando pode ser usado, consulte [exemplos](#BKMK_Examples).

## <a name="syntax"></a>Sintaxe

```
Secedit /export /db <database file name> [/mergedpolicy] /cfg <configuration file name> [/areas [securitypolicy | group_mgmt | user_rights | regkeys | filestore | services]] [/log <log file name>] [/quiet]
```

### <a name="parameters"></a>Parâmetros

|Parâmetro|Descrição|
|---------|-----------|
|db|Obrigatório.</br>Especifica o nome de arquivo e caminho de um banco de dados que contém a configuração armazenada em relação ao qual a análise será executada.</br>Se o nome do arquivo Especifica um banco de dados que não tenha um modelo de segurança (conforme representado pelo arquivo de configuração) associado a ele, o `/cfg \<configuration file name>` também deve ser especificada a opção de linha de comando.|
|mergedpolicy|Opcional.</br>Mescla e exporta o domínio e configurações de segurança de diretiva local.|
|cfg|Obrigatório.</br>Especifica o nome de arquivo e caminho para o modelo de segurança que será importado para o banco de dados para análise.</br>Essa opção /cfg só é válida quando usado com o `/db \<database file name>` parâmetro. Se não for especificado, a análise é executada em relação a qualquer configuração já armazenada no banco de dados.|
|Áreas|Opcional.</br>Especifica as áreas de segurança a ser aplicado ao sistema. Se esse parâmetro não for especificado, todas as configurações de segurança definidas no banco de dados são aplicadas ao sistema. Para configurar várias áreas, separe cada área por um espaço. Há suporte para as seguintes áreas de segurança:</br>-   SecurityPolicy</br>    Diretiva local e diretiva de domínio para o sistema, incluindo diretivas de conta, auditoria de políticas, as opções de segurança e assim por diante.</br>-   Group_Mgmt</br>    Configurações de grupo restrito para quaisquer grupos especificados no modelo de segurança.</br>-User_Rights</br>    Concessão de privilégios e direitos de logon do usuário.</br>-   RegKeys</br>    Segurança nas chaves do Registro local.</br>-   FileStore</br>    Segurança no armazenamento de arquivos local.</br>-Serviços</br>    Segurança para todos os serviços definidos.|
|log|Opcional.</br>Especifica o nome de arquivo e caminho do arquivo de log para o processo.|
|Silencioso|Opcional.</br>Suprime a saída de log e de tela. Você ainda pode exibir resultados da análise, usando a configuração de segurança e análise de snap-in para o Microsoft Management Console (MMC).|

## <a name="remarks"></a>Comentários

Você pode usar esse comando para fazer backup de suas políticas de segurança em um computador local, além de importar as configurações para outro computador.

Se o caminho para o arquivo de log não for fornecido, o arquivo de log padrão (*systemroot*\Documents and Settings\*UserAccount<em>\My Documents\Security\Logs\*DatabaseName</em>. log) é usado.

No Windows Server 2008, `Secedit /refreshpolicy` foi substituído por `gpupdate`. Para obter informações sobre como atualizar as configurações de segurança, consulte [Gpupdate](gpupdate.md).

## <a name="BKMK_Examples"></a>Exemplos

Exportar o banco de dados de segurança e as políticas de segurança de domínio para um arquivo inf e, em seguida, importar esse arquivo para outro banco de dados para replicar as configurações de política de segurança em outro computador.
```
Secedit /export /db C:\Security\FY11\SecDbContoso.sdb /mergedpolicy /cfg SecContoso.inf /log C:\Security\FY11\SecAnalysisContosoFY11.log /quiet
```
Importe esse arquivo para outro banco de dados em outro computador.
```
Secedit /import /db C:\Security\FY12\SecDbContoso.sdb /cfg SecContoso.inf /log C:\Security\FY11\SecAnalysisContosoFY12.log /quiet
```

#### <a name="additional-references"></a>Referências adicionais

-   [Secedit:import](secedit-import.md)
-   [Secedit](secedit.md)
-   [Chave da sintaxe de linha de comando](command-line-syntax-key.md)