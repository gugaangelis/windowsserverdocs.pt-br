---
title: regsvr32
description: Tópico de comandos do Windows para * * * *-
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 3345e964-7d3e-42b8-abeb-42ed6edfe2b2
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: d3b775b0c49e4191a9fee6dc9e2e91f968142085
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80836209"
---
# <a name="regsvr32"></a>regsvr32



Registra arquivos. dll como componentes de comando no registro.

Para obter exemplos de como usar esse comando, consulte [Exemplos](#BKMK_examples).

## <a name="syntax"></a>Sintaxe

```
regsvr32 [/u] [/s] [/n] [/i[:cmdline]] <DllName>
```

### <a name="parameters"></a>Parâmetros

|Parâmetro|Descrição|
|---------|-----------|
|/u|Cancela o registro do servidor.|
|/s|Executa **regsvr32** sem exibir mensagens.|
|/n|Executa **regsvr32** sem chamar **DllRegisterServer**. (Requer o parâmetro **/i** ).|
|/i:\<cmdline >|Passa uma cadeia de caracteres de linha de comando opcional (*cmdline*) para **DllInstall**. Se você usar esse parâmetro em conjunto com o parâmetro **/u** , ele chamará **DllUninstall**.|
|\<DllName >|O nome do arquivo. dll que será registrado.|
|/?|Exibe a ajuda no prompt de comando.|

## <a name="examples"></a><a name=BKMK_examples></a>Disso

Para registrar o. dll para o esquema de Active Directory, digite:
```
regsvr32 schmmgmt.dll
```

## <a name="additional-references"></a>Referências adicionais

- [Chave da sintaxe de linha de comando](command-line-syntax-key.md)