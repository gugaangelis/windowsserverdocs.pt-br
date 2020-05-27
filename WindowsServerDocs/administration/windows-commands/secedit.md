---
title: secedit
description: Tópico de referência para * * * *-
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 58ed57ed-08e3-403d-a363-0620b358637a
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 56c9cdecfe2a5553230f1c8120c827e7f05d0ba9
ms.sourcegitcommit: 4f407b82435afe3111c215510b0ef797863f9cb4
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 05/24/2020
ms.locfileid: "83821086"
---
# <a name="secedit"></a>secedit



Configura e analisa a segurança do sistema comparando sua configuração atual com os modelos de segurança especificados.

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
| *Nome do usuário selecionado*. sdb |                                                                                    **Local**:% WINDIR% \* \Documents\Security\Database da conta de usuário <em></br></em>*Criado por* <em> : executando o snap-in de configuração e análise de segurança</br></em>*Tipo de arquivo* <em> : proprietário</br></em>*Taxa de atualização* <em> : atualizada sempre que um novo modelo de segurança é criado.</br></em>*Conteúdo* \* : políticas de segurança local e modelos de segurança criados pelo usuário.                                                                                    |
| *Nome do usuário selecionado*. log | **Local**: definido pelo usuário, mas o padrão é% windir% \* \Documents\Security\Logs da conta de usuário <em></br></em>*Criado por* <em> : executando os subcomando/Analyze e/configure (ou usando o snap-in de configuração e análise de segurança)</br></em>*Tipo de arquivo* <em> : texto</br></em>*Taxa de atualização* <em> : executando os subcomandos/analyze e/configure (ou usando o snap-in de configuração e análise de segurança); substituído.</br></em>*Conteúdo* \* :</br>1. nome do arquivo de log</br>2. Data e hora</br>3. resultados da análise ou investigação. |
| *Nome do usuário selecionado*. inf |                                                                                     **Local**:% WINDIR% \* \Documents\Security\Templates da conta de usuário <em></br></em>*Criado por* <em> : executando o snap-in do modelo de segurança</br></em>*Tipo de arquivo* <em> : texto</br></em>*Taxa de atualização* <em> : cada vez que o modelo de segurança é atualizado</br></em>*Conteúdo* \* : contém as informações de configuração do modelo para cada política selecionada usando o snap-in.                                                                                     |

> [!NOTE]
> O MMC (console de gerenciamento Microsoft) e o snap-in de análise e configuração de segurança não estão disponíveis no Server Core.

## <a name="additional-references"></a>Referências adicionais

Para obter exemplos de como esse comando pode ser usado, consulte a seção exemplos em qualquer um dos arquivos de subcomando.
- [Chave da sintaxe de linha de comando](command-line-syntax-key.md)