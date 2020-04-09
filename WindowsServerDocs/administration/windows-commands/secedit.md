---
title: secedit
description: Tópico de comandos do Windows para * * * *-
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 58ed57ed-08e3-403d-a363-0620b358637a
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: e80217c201cde4dc1df58c0e8976fbe1422511fe
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80834839"
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

#### <a name="parameters"></a>Parâmetros

|Parâmetro|Descrição|
|---------|-----------|
|[Secedit:analyze](secedit-analyze.md)|Permite que você analise as configurações atuais de sistemas em relação às configurações de linha de base que são armazenadas em um banco de dados.  Os resultados da análise são armazenados em uma área separada do banco de dados e podem ser exibidos no snap-in configuração de segurança e análise.|
|[Secedit:configure](secedit-configure.md)|Permite que você configure um sistema com as configurações de segurança armazenadas em um banco de dados.|
|[Secedit:export](secedit-export.md)|Permite exportar configurações de segurança armazenadas em um banco de dados do.|
|[Secedit:generaterollback](secedit-generaterollback.md)|Permite gerar um modelo de reversão em relação a um modelo de configuração.|
|[Secedit:import](secedit-import.md)|Permite que você importe um modelo de segurança para um banco de dados para que as configurações especificadas no modelo possam ser aplicadas a um sistema ou analisadas em um sistema.|
|[Secedit:validate](secedit-validate.md)|Permite validar a sintaxe de um modelo de segurança.|

## <a name="remarks"></a>Comentários

Para todos os nomes de FileName, o diretório atual será usado se nenhum caminho for especificado.

Quando um modelo de segurança é criado usando o snap-in modelo de segurança e a configuração de segurança e o snap-in de análise são executados, os seguintes arquivos são criados:


|           Arquivo           |                                                                                                                                                                                                                                                               Descrição                                                                                                                                                                                                                                                                |
|--------------------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
|        Scesrv. log        |                                                                                                                             **Local**:%WINDIR%\Security\Logs</br>**Criado por**: sistema operacional</br>**Tipo de arquivo**: texto</br>**Taxa de atualização**: substituída quando secedit/Analyze,/configure,/Export ou/Import são executados.</br>**Conteúdo**: contém os resultados da análise agrupados por tipo de política.                                                                                                                             |
| *Nome do usuário selecionado*. sdb |                                                                                    **Local**:% windir%\*conta de usuário<em>\Documents\Security\Database</br></em>*criado por*<em>: executando o snap-in de configuração e análise de segurança</br>*tipo de arquivo* de </em><em>: proprietário</br><em>da *taxa de atualização* de </em>: atualizado sempre que um novo modelo de segurança é criado.</br>\*de </em>de *conteúdo* : políticas de segurança local e modelos de segurança criados pelo usuário.                                                                                    |
| *Nome do usuário selecionado*. log | **Local**: definido pelo usuário, mas o padrão é% windir%\*conta de usuário<em>\Documents\Security\Logs</br></em>*criado por*<em>: executando os subcomando/Analyze e/configure (ou usando o snap-in de configuração e análise de segurança)</br>*tipo de arquivo* de </em><em>: texto</br><em>da *taxa de atualização* de </em>: executando os subcomando/Analyze e/configure (ou usando o snap-in de configuração e análise de segurança); substituído.</br>\*de </em>de *conteúdo* :</br>1. nome do arquivo de log</br>2. Data e hora</br>3. resultados da análise ou investigação. |
| *Nome do usuário selecionado*. inf |                                                                                     **Local**:% windir%\*conta de usuário<em>\Documents\Security\Templates</br></em>*criado por*<em>: executando o snap-in do modelo de segurança</br>*tipo de arquivo* de </em><em>: texto</br><em>da *taxa de atualização* de </em>: sempre que o modelo de segurança for atualizado</br>\*de *conteúdo* de </em>: contém as informações de configuração do modelo para cada política selecionada usando o snap-in.                                                                                     |

> [!NOTE]
> O MMC (console de gerenciamento Microsoft) e o snap-in de análise e configuração de segurança não estão disponíveis no Server Core.

## <a name="additional-references"></a>Referências adicionais

Para obter exemplos de como esse comando pode ser usado, consulte a seção exemplos em qualquer um dos arquivos de subcomando.
-   - [Chave da sintaxe de linha de comando](command-line-syntax-key.md)