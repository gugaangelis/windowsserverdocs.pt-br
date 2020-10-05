---
title: progresso do WDSUTIL
description: Artigo de referência para o progresso do WDSUTIL, que exibe o progresso enquanto um comando está em execução.
ms.topic: reference
ms.assetid: 8ce5e77b-e13f-4ac3-948d-31770a6c7e25
ms.author: lizross
author: eross-msft
manager: mtillman
ms.date: 10/16/2017
ms.openlocfilehash: 4a7ddc18db35b110c8b5c513f798e3408aafd93f
ms.sourcegitcommit: 720455aad2bac78cf64997d196a13f35ea0acb73
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 10/05/2020
ms.locfileid: "91729615"
---
# <a name="wdsutil-progress"></a>WDSUTIL/Progress

Exibe o progresso enquanto um comando está em execução. Você pode usar o **/Progress** com outros comandos do WDSUTIL que você executar. Se você quiser ativar o log detalhado para este comando, deverá especificar **/Verbose** e **/Progress** diretamente após **WDSUTIL**.

## <a name="syntax"></a>Syntax

```
wdsutil /progress <commands>
```

## <a name="examples"></a>Exemplos

Para inicializar o servidor e exibir o progresso, digite:

```
wdsutil /verbose /progress /Initialize-Server /Server:MyWDSServer /RemInst:C:\RemoteInstall
```

## <a name="additional-references"></a>Referências adicionais

- [Chave da sintaxe de linha de comando](command-line-syntax-key.md)