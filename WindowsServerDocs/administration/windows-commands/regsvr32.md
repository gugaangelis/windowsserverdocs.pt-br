---
title: regsvr32
description: Tópico de referência para * * * *-
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 3345e964-7d3e-42b8-abeb-42ed6edfe2b2
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: beadc9e9e614e2fe4cffad5dc263cfb1d4aecf67
ms.sourcegitcommit: ab64dc83fca28039416c26226815502d0193500c
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 05/01/2020
ms.locfileid: "82722485"
---
# <a name="regsvr32"></a>regsvr32



Registra arquivos. dll como componentes de comando no registro.



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
|/i:\<> cmdline|Passa uma cadeia de caracteres de linha de comando opcional (*cmdline*) para **DllInstall**. Se você usar esse parâmetro em conjunto com o parâmetro **/u** , ele chamará **DllUninstall**.|
|\<DllName>|O nome do arquivo. dll que será registrado.|
|/?|Exibe a ajuda no prompt de comando.|

## <a name="examples"></a>Exemplos

Para registrar o. dll para o esquema de Active Directory, digite:
```
regsvr32 schmmgmt.dll
```

## <a name="additional-references"></a>Referências adicionais

- [Chave da sintaxe de linha de comando](command-line-syntax-key.md)