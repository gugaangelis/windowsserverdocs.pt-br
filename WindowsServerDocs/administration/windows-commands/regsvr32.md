---
title: regsvr32
description: 'Tópico de comandos do Windows para * * *- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 3345e964-7d3e-42b8-abeb-42ed6edfe2b2
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 87d9291755ddb4484e85248cb01ad78b01a25965
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59889987"
---
# <a name="regsvr32"></a>regsvr32



Registra os arquivos. dll como componentes de comando no registro.

Para obter exemplos de como usar esse comando, consulte [Exemplos](#BKMK_examples).

## <a name="syntax"></a>Sintaxe

```
regsvr32 [/u] [/s] [/n] [/i[:cmdline]] <DllName>
```

## <a name="parameters"></a>Parâmetros

|Parâmetro|Descrição|
|---------|-----------|
|/u|Cancela o registro do servidor.|
|/s|Execuções **Regsvr32** sem exibir mensagens.|
|/n|Execuções **Regsvr32** sem chamar **DllRegisterServer**. (Requer o **/i** parâmetro.)|
|/i:\<cmdline>|Passa uma cadeia de caracteres de linha de comando opcional (*cmdline*) para **DllInstall**. Se você usar esse parâmetro junto com o **/u** parâmetro, ele chama **DllUninstall**.|
|\<DllName>|O nome do arquivo. dll que será registrado.|
|/?|Exibe a ajuda no prompt de comando.|

## <a name="BKMK_examples"></a>Exemplos

Para registrar o arquivo. dll para o esquema do Active Directory, digite:
```
regsvr32 schmmgmt.dll
```

#### <a name="additional-references"></a>Referências adicionais

[Chave de sintaxe de linha de comando](command-line-syntax-key.md)