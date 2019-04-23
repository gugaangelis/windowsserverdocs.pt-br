---
title: Scwcmd register
description: 'Tópico de comandos do Windows para * * *- '
ms.custom: na
ms.prod: windows-server-threshold
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
ms.openlocfilehash: cc8b4a06af519b0da01dfcab8de0139b12cc68f2
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59834967"
---
# <a name="scwcmd-register"></a>Scwcmd: register

> Aplica-se a: Windows Server 2012 R2, Windows Server 2012

Estende ou personaliza o banco de dados de configuração de segurança do Assistente de configuração de segurança (ACS), registrando um arquivo de banco de dados de configuração de segurança que contém a função, tarefa, serviço ou definições de porta.

## <a name="syntax"></a>Sintaxe

```
scwcmd register /kbname:<MyApp> [/kbfile:<kb.xml>] [/kb:<path>] [/d]
```

### <a name="parameters"></a>Parâmetros

|Parâmetro|Descrição|
|---------|-----------|
|/kbname:\<MyApp>|Especifica o nome sob a qual a extensão de banco de dados de configuração de segurança será registrada. Esse parâmetro deve ser especificado.|
|/kbfile:\<Kb.xml>|Especifica o nome de arquivo e caminho do arquivo de banco de dados de configuração de segurança que será usado para estender ou personalizar o banco de dados de configuração de segurança base. Para validar que o arquivo de banco de dados de configuração de segurança está em conformidade com o esquema do ACS, use o arquivo de definição de esquema %windir%\security\KBRegistrationInfo.xsd. Essa opção deve ser fornecida, a menos que o **/d** parâmetro for especificado.|
|/kb:\<Path>|Especifica o caminho para o diretório que contém os arquivos de banco de dados de configuração de segurança ACS a ser atualizado. Se essa opção não for especificada, %windir%\security\msscw\kbs será usado.|
|/d|Cancela o registro de uma extensão de banco de dados de configuração de segurança do banco de dados de configuração de segurança. A extensão para cancelar o registro é especificada pelo parâmetro /kbname. (O **/kbfile** parâmetro não deve ser especificado.) O banco de dados de configuração de segurança para cancelar o registro da extensão é especificado pela **/KB.** parâmetro.|
|/?|Exibe a ajuda no prompt de comando.|

## <a name="remarks"></a>Comentários

Scwcmd.exe só está disponível em computadores que executam o Windows Server 2008 R2, Windows Server 2008 ou Windows Server 2003.

## <a name="BKMK_Examples"></a>Exemplos

Para registrar o arquivo de banco de dados de configuração de segurança chamado SCWKBForMyApp.xml sob o nome de MyApp no local \\ \\kbserver\kb, tipo:
```
scwcmd register /kbfile:d:\SCWKBForMyApp.xml /kbname:MyApp /kb:\\kbserver\kb
```
Para cancelar o registro de MyApp de banco de dados de configuração segurança localizado em \\ \\kbserver\kb, tipo:
```
scwcmd register /d /kbname:MyApp /kb:\\kbserver\kb
```

#### <a name="additional-references"></a>Referências adicionais

-   [Chave de sintaxe de linha de comando](command-line-syntax-key.md)