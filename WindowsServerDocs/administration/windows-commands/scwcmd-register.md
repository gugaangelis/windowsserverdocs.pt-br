---
title: Scwcmd register
description: 'Tópico de comandos do Windows para * * * *- '
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: fe4d126a-9f27-4076-b7b1-fbefa45f378a
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 2e892f7c08461e88d12a072dfb171f9523558ef7
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71371216"
---
# <a name="scwcmd-register"></a>Scwcmd: register

> Aplica-se a: Windows Server 2012 R2, Windows Server 2012

Estende ou personaliza o banco de dados de configuração de segurança do ACS (Assistente de configuração de segurança) registrando um arquivo de banco de dados de configuração de segurança que contém definições de função, tarefa, serviço ou porta.

## <a name="syntax"></a>Sintaxe

```
scwcmd register /kbname:<MyApp> [/kbfile:<kb.xml>] [/kb:<path>] [/d]
```

### <a name="parameters"></a>Parâmetros

|Parâmetro|Descrição|
|---------|-----------|
|/kbname:\<MyApp >|Especifica o nome sob o qual a extensão de banco de dados de configuração de segurança será registrada. Esse parâmetro deve ser especificado.|
|/kbfile:\<KB. XML >|Especifica o caminho e o nome do arquivo de banco de dados de configuração de segurança que será usado para estender ou personalizar o banco de dados de configuração de segurança base. Para validar que o arquivo de banco de dados de configuração de segurança é compatível com o esquema do SCW, use o arquivo de definição de esquema%windir%\security\KBRegistrationInfo.xsd. Essa opção deve ser fornecida a menos que o parâmetro **/d** seja especificado.|
|/KB: caminho de\<>|Especifica o caminho para o diretório que contém os arquivos de banco de dados de configuração de segurança do ACS a serem atualizados. Se essa opção não for especificada,%windir%\security\msscw\kbs será usado.|
|/d|Cancela o registro de uma extensão de banco de dados de configuração de segurança do banco de dados de configuração de segurança. A extensão para cancelar o registro é especificada pelo parâmetro/kbname. (O parâmetro **/kbfile** não deve ser especificado.) O banco de dados de configuração de segurança para cancelar o registro da extensão é especificado pelo parâmetro **/KB** .|
|/?|Exibe a ajuda no prompt de comando.|

## <a name="remarks"></a>Comentários

Scwcmd. exe só está disponível em computadores que executam o Windows Server 2008 R2, o Windows Server 2008 ou o Windows Server 2003.

## <a name="BKMK_Examples"></a>Disso

Para registrar o arquivo de banco de dados de configuração de segurança chamado SCWKBForMyApp. XML com o nome MyApp no local \\\\kbserver\kb, digite:
```
scwcmd register /kbfile:d:\SCWKBForMyApp.xml /kbname:MyApp /kb:\\kbserver\kb
```
Para cancelar o registro do banco de dados de configuração de segurança MyApp localizado em \\\\kbserver\kb, digite:
```
scwcmd register /d /kbname:MyApp /kb:\\kbserver\kb
```

#### <a name="additional-references"></a>Referências adicionais

-   [Chave da sintaxe de linha de comando](command-line-syntax-key.md)