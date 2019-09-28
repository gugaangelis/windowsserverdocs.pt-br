---
title: regsvr32
description: 'Tópico de comandos do Windows para * * * *- '
ms.custom: na
ms.prod: windows-server
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
ms.openlocfilehash: 444af0ccf7c9bbe21c013f32b396997b7cb2e00f
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71371635"
---
# <a name="regsvr32"></a>regsvr32



Registra arquivos. dll como componentes de comando no registro.

Para obter exemplos de como usar esse comando, consulte [Exemplos](#BKMK_examples).

## <a name="syntax"></a>Sintaxe

```
regsvr32 [/u] [/s] [/n] [/i[:cmdline]] <DllName>
```

## <a name="parameters"></a>Parâmetros

|Parâmetro|Descrição|
|---------|-----------|
|/u|Cancela o registro do servidor.|
|/s|Executa **regsvr32** sem exibir mensagens.|
|opção|Executa **regsvr32** sem chamar **DllRegisterServer**. (Requer o parâmetro **/i** ).|
|/i: \<cmdline >|Passa uma cadeia de caracteres de linha de comando opcional (*cmdline*) para **DllInstall**. Se você usar esse parâmetro em conjunto com o parâmetro **/u** , ele chamará **DllUninstall**.|
|\<DllName >|O nome do arquivo. dll que será registrado.|
|/?|Exibe a ajuda no prompt de comando.|

## <a name="BKMK_examples"></a>Disso

Para registrar o. dll para o esquema de Active Directory, digite:
```
regsvr32 schmmgmt.dll
```

#### <a name="additional-references"></a>Referências adicionais

[Chave da sintaxe de linha de comando](command-line-syntax-key.md)