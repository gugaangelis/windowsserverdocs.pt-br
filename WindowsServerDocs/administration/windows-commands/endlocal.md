---
title: endlocal
description: Tópico de comandos do Windows para * * * *-
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 765fae3c-0c0a-4639-99a4-cf613489b949
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 4958c5419ed4f6374f7c6ecf09bdf67f61134d93
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80845109"
---
# <a name="endlocal"></a>endlocal



Encerra a localização de alterações de ambiente em um arquivo em lotes e restaura as variáveis de ambiente para seus valores antes da execução do comando **setlocal** correspondente.

Para obter exemplos de como usar esse comando, consulte [Exemplos](#BKMK_examples).

## <a name="syntax"></a>Sintaxe

```
endlocal
```

### <a name="parameters"></a>Parâmetros

|Parâmetro|Descrição|
|---------|-----------|
|/?|Exibe a ajuda no prompt de comando.|

## <a name="remarks"></a>Comentários

-   O comando **ENDLOCAL** não tem nenhum efeito fora de um script ou arquivo em lotes.
-   Há um comando **ENDLOCAL** implícito no final de um arquivo em lotes.
-   Se as extensões de comando estiverem habilitadas (as extensões de comando são habilitadas por padrão), o comando **ENDLOCAL** restaura o estado das extensões de comando (isto é, habilitado ou desabilitado) para o que era antes de o comando **setlocal** correspondente ser executado.

> [!NOTE]
> Para obter mais informações sobre como habilitar e desabilitar extensões de comando, consulte [cmd](cmd.md).

## <a name="examples"></a><a name=BKMK_examples></a>Disso

Você pode localizar variáveis de ambiente em um arquivo em lotes. Por exemplo, o programa a seguir inicia o programa superapp batch na rede, direciona a saída para um arquivo e exibe o arquivo no bloco de notas:
```
@echo off
setlocal
path=g:\programs\superapp;%path%
call superapp>c:\superapp.out
endlocal
start notepad c:\superapp.out
```

## <a name="additional-references"></a>Referências adicionais

- [Chave da sintaxe de linha de comando](command-line-syntax-key.md)