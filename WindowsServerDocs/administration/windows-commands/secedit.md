---
title: secedit
description: 'Tópico de comandos do Windows para * * *- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 58ed57ed-08e3-403d-a363-0620b358637a
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 4c70211cc029cec7e6bb0290877089ecb9a86f22
ms.sourcegitcommit: eaf071249b6eb6b1a758b38579a2d87710abfb54
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 05/31/2019
ms.locfileid: "66441464"
---
# <a name="secedit"></a>secedit



Configura e analisa a segurança do sistema comparando a configuração atual com os modelos de segurança especificados.

## <a name="syntax"></a>Sintaxe

```
secedit 
[/analyze /db <database file name> /cfg <configuration file name> [/overwrite] /log <log file name> [/quiet]]
[/configure /db <database file name> [/cfg <configuration filename>] [/overwrite] [/areas [securitypolicy | group_mgmt | user_rights | regkeys | filestore | services]] [/log <log file name>] [/quiet]]
[/export /db <database file name> [/mergedpolicy] /cfg <configuration file name> [/areas [securitypolicy | group_mgmt | user_rights | regkeys | filestore | services]] [/log <log file name>]]
[/generaterollback /db <database file name> /cfg <configuration file name> /rbk <rollback file name> [/log <log file name>] [/quiet]]
[/import /db <database file name> /cfg <configuration file name> [/overwrite] [/areas [securitypolicy | group_mgmt | user_rights | regkeys | filestore | services]] [/log <log file name>] [/quiet]]
[/validate <configuration file name>]
```

### <a name="parameters"></a>Parâmetros

|Parâmetro|Descrição|
|---------|-----------|
|[Secedit:analyze](secedit-analyze.md)|Permite que você analise as configurações de sistemas atuais em relação a configurações de linha de base que são armazenadas em um banco de dados.  Os resultados da análise são armazenados em uma área separada do banco de dados e podem ser exibidos no snap-in de análise e configuração de segurança.|
|[Secedit:configure](secedit-configure.md)|Permite que você configure um sistema com configurações de segurança armazenadas em um banco de dados.|
|[Secedit:export](secedit-export.md)|Permite que você exporte as configurações de segurança armazenadas em um banco de dados.|
|[Secedit:generaterollback](secedit-generaterollback.md)|Permite que você gerar um modelo de reversão em relação a um modelo de configuração.|
|[Secedit:import](secedit-import.md)|Permite que você importe um modelo de segurança para um banco de dados para que as configurações especificadas no modelo podem ser aplicadas a um sistema ou analisadas em relação a um sistema.|
|[Secedit:validate](secedit-validate.md)|Permite que você valide a sintaxe de um modelo de segurança.|

## <a name="remarks"></a>Comentários

Para todos os nomes de arquivo, o diretório atual é usado se nenhum caminho for especificado.

Quando um modelo de segurança é criado usando o snap-in do modelo de segurança e a configuração de segurança e o snap-in de análise é executada, os seguintes arquivos são criados:


|           Arquivo           |                                                                                                                                                                                                                                                               Descrição                                                                                                                                                                                                                                                                |
|--------------------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
|        Scesrv.log        |                                                                                                                             **Local**: %windir%\security\logs.</br>**Criado por**: sistema operacional</br>**Tipo de arquivo**: texto</br>**Taxa de atualização**: Substituído quando secedit / analisar, / configurar/exportar ou /import são executados.</br>**Conteúdo**: Contém os resultados da análise agrupados por tipo de política.                                                                                                                             |
| *Nome de usuário selecionado*. sdb |                                                                                    **Local**: % windir %\*conta de usuário<em>\Documents\Security\Database</br></em>*Criado por*<em>: executando o snap-in de análise e a configuração de segurança</br></em>*Tipo de arquivo*<em>: proprietários</br></em>*Taxa de atualização*<em>: Atualizado sempre que um novo modelo de segurança é criado.</br></em>*Conteúdo*\*: Políticas de segurança locais e modelos de segurança criados pelo usuário.                                                                                    |
| *Nome de usuário selecionado*. log | **Local**: Definido pelo usuário, mas o padrão é % windir %\*conta de usuário<em>\Documents\Security\Logs</br></em>*Criado por*<em>: Executando o /ANALYZE e / configurar subcomandos (ou usando o snap-in de análise e a configuração de segurança)</br></em>*Tipo de arquivo*<em>: texto</br></em>*Taxa de atualização*<em>: Executando o /ANALYZE e / configurar subcomandos (ou usando o snap-in de análise e a configuração de segurança); substituído.</br></em>*Conteúdo*\*:</br>1.  Nome do arquivo de log</br>2.  Data e hora</br>3.  Resultados da análise ou investigação. |
| *Nome de usuário selecionado*. inf |                                                                                     **Local**: % windir %\*conta de usuário<em>\Documents\Security\Templates</br></em>*Criado por*<em>: executando o snap-in do modelo de segurança</br></em>*Tipo de arquivo*<em>: texto</br></em>*Taxa de atualização*<em>: cada vez que o modelo de segurança é atualizado</br></em>*Conteúdo*\*: Contém informações para o modelo para cada política selecionado usando o snap-in de configuração.                                                                                     |

> [!NOTE]
> O Microsoft Management Console (MMC) e a configuração de segurança e snap-in de análise não estão disponíveis no Server Core.

#### <a name="additional-references"></a>Referências adicionais

Para obter exemplos de como esse comando pode ser usado, consulte a seção de exemplos em qualquer um dos arquivos subcomandos.
-   [Chave da sintaxe de linha de comando](command-line-syntax-key.md)