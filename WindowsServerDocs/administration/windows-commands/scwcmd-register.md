---
title: Scwcmd register
description: Tópico de referência para * * * *-
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: fe4d126a-9f27-4076-b7b1-fbefa45f378a
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: b7328a9db29f394bde089988ab0bdbcd84f54b30
ms.sourcegitcommit: ab64dc83fca28039416c26226815502d0193500c
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 05/01/2020
ms.locfileid: "82722137"
---
# <a name="scwcmd-register"></a>Scwcmd: register

> Aplica-se a: Windows Server 2012 R2, Windows Server 2012

Estende ou personaliza o banco de dados de configuração de segurança do ACS (Assistente de configuração de segurança) registrando um arquivo de banco de dados de configuração de segurança que contém definições de função, tarefa, serviço ou porta.

## <a name="syntax"></a>Sintaxe

```
scwcmd register /kbname:<MyApp> [/kbfile:<kb.xml>] [/kb:<path>] [/d]
```

#### <a name="parameters"></a>Parâmetros

|Parâmetro|Descrição|
|---------|-----------|
|/kbname:\<MyApp>|Especifica o nome sob o qual a extensão de banco de dados de configuração de segurança será registrada. Esse parâmetro deve ser especificado.|
|/kbfile:\<KB. xml>|Especifica o caminho e o nome do arquivo de banco de dados de configuração de segurança que será usado para estender ou personalizar o banco de dados de configuração de segurança base. Para validar que o arquivo de banco de dados de configuração de segurança é compatível com o esquema do SCW, use o arquivo de definição de esquema%windir%\security\KBRegistrationInfo.xsd. Essa opção deve ser fornecida a menos que o parâmetro **/d** seja especificado.|
|/KB:\<caminho>|Especifica o caminho para o diretório que contém os arquivos de banco de dados de configuração de segurança do ACS a serem atualizados. Se essa opção não for especificada,%windir%\security\msscw\kbs será usado.|
|/d|Cancela o registro de uma extensão de banco de dados de configuração de segurança do banco de dados de configuração de segurança. A extensão para cancelar o registro é especificada pelo parâmetro/kbname. (O parâmetro **/kbfile** não deve ser especificado.) O banco de dados de configuração de segurança para cancelar o registro da extensão é especificado pelo parâmetro **/KB** .|
|/?|Exibe a ajuda no prompt de comando.|

## <a name="remarks"></a>Comentários

Scwcmd. exe só está disponível em computadores que executam o Windows Server 2008 R2, o Windows Server 2008 ou o Windows Server 2003.

## <a name="examples"></a>Exemplos

Para registrar o arquivo de banco de dados de configuração de segurança chamado SCWKBForMyApp. XML com o \\ \\nome MyApp no local kbserver\kb, digite:
```
scwcmd register /kbfile:d:\SCWKBForMyApp.xml /kbname:MyApp /kb:\\kbserver\kb
```
Para cancelar o registro do banco de dados de configuração \\ \\de segurança MyApp localizado em kbserver\kb, digite:
```
scwcmd register /d /kbname:MyApp /kb:\\kbserver\kb
```

## <a name="additional-references"></a>Referências adicionais

-   - [Chave da sintaxe de linha de comando](command-line-syntax-key.md)