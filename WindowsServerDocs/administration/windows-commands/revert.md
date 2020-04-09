---
title: voltar
description: Tópico de comandos do Windows para * * * *-
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 75ad40e4-502a-401e-b11e-8b31e00424b5
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: d7dae609e4615868b03e4b5ea9a681f553aa0667
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80835749"
---
# <a name="revert"></a>voltar



Reverte um volume de volta para uma cópia de sombra especificada. Isso tem suporte apenas para cópias de sombra no contexto CLIENTACCESSIBLE. Essas cópias de sombra são persistentes e só podem ser feitas pelo provedor do sistema. Se usado sem parâmetros, **REVERT** exibe a ajuda no prompt de comando.

## <a name="syntax"></a>Sintaxe

```
revert <ShadowCopyID>
```

### <a name="parameters"></a>Parâmetros

|Parâmetro|Descrição|
|---------|-----------|
|\<ShadowCopyID >|Especifica a ID da cópia de sombra para a qual reverter o volume.|

## <a name="additional-references"></a>Referências adicionais

- [Chave da sintaxe de linha de comando](command-line-syntax-key.md)