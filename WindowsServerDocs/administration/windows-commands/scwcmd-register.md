---
title: scwcmd register
description: Artigo de referência para o comando scwcmd register, que estende ou personaliza o banco de dados de configuração de segurança do ACS (Assistente de configuração de segurança) registrando um arquivo de banco de dados de configuração de segurança que contém definições de função, tarefa, serviço ou porta.
ms.topic: reference
ms.assetid: fe4d126a-9f27-4076-b7b1-fbefa45f378a
ms.author: lizross
author: eross-msft
manager: mtillman
ms.date: 10/16/2017
ms.openlocfilehash: b1e981ef2a428174406fd793d5c959de57a8067c
ms.sourcegitcommit: e164aeffc01069b8f1f3248bf106fcdb7f64f894
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/26/2020
ms.locfileid: "91388572"
---
# <a name="scwcmd-register"></a>scwcmd register

> Aplica-se a: Windows Server 2012 R2 e Windows Server 2012

Estende ou personaliza o banco de dados de configuração de segurança do ACS (Assistente de configuração de segurança) registrando um arquivo de banco de dados de configuração de segurança que contém definições de função, tarefa, serviço ou porta.

## <a name="syntax"></a>Sintaxe

```
scwcmd register /kbname:<MyApp> [/kbfile:<kb.xml>] [/kb:<path>] [/d]
```

### <a name="parameters"></a>Parâmetros

| Parâmetro | Descrição |
|--|--|
| /kbname:`<MyApp>` | Especifica o nome sob o qual a extensão de banco de dados de configuração de segurança será registrada. Esse parâmetro deve ser especificado. |
| /kbfile:`<kb.xml>` | Especifica o caminho e o nome do arquivo do banco de dados de configuração de segurança usado para estender ou personalizar o banco de dados de configuração de segurança base. Para validar que o arquivo de banco de dados de configuração de segurança está em conformidade com o esquema do SCW, use o `%windir%\security\KBRegistrationInfo.xsd` arquivo de definição de esquema. Essa opção deve ser fornecida a menos que o parâmetro **/d** seja especificado. |
| quilobyte`<path>` | Especifica o caminho para o diretório que contém os arquivos de banco de dados de configuração de segurança do ACS a serem atualizados. Se essa opção não for especificada, `%windir%\security\msscw\kbs` será usada. |
| /d | Cancela o registro de uma extensão de banco de dados de configuração de segurança do banco de dados de configuração de segurança. A extensão para cancelar o registro é especificada pelo parâmetro **/kbname** . (O parâmetro **/kbfile** não deve ser especificado.) O banco de dados de configuração de segurança para cancelar o registro da extensão é especificado pelo parâmetro **/KB** . |
| /? | Exibe a ajuda no prompt de comando. |

## <a name="examples"></a>Exemplos

Para registrar o arquivo de banco de dados de configuração de segurança chamado *SCWKBForMyApp.xml* sob o nome *MyApp* no local `\\kbserver\kb` , digite:

```
scwcmd register /kbfile:d:\SCWKBForMyApp.xml /kbname:MyApp /kb:\\kbserver\kb
```

Para cancelar o registro do banco de dados de configuração de segurança *MyApp*, localizado em `\\kbserver\kb` , digite:

```
scwcmd register /d /kbname:MyApp /kb:\\kbserver\kb
```

## <a name="additional-references"></a>Referências adicionais

- [Chave da sintaxe de linha de comando](command-line-syntax-key.md)

- [comando scwcmd analyze](scwcmd-analyze.md)

- [comando de configuração de scwcmd](scwcmd-configure.md)

- [comando scwcmd rollback](scwcmd-rollback.md)

- [comando scwcmd transform](scwcmd-transform.md)

- [comando scwcmd View](scwcmd-view.md)