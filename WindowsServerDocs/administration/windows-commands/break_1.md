---
title: break
description: 'O tópico de comandos do Windows para **break_1** -define ou limpa a verificação estendida CTRL + C em sistemas MS-dos. Se usado sem parâmetros, **Break** exibirá a configuração atual. '
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: c89b7357-d69e-4141-826e-73c9ba0fc630
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 73afdac29efbfd9efec88d297cf4185ca1b92d62
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71380494"
---
# <a name="break"></a>break



Define ou limpa a verificação estendida CTRL + C em sistemas MS-DOS. Se usado sem parâmetros, **Break** exibirá a configuração atual.

> [!NOTE]
> Este comando não está mais em uso. Ele é incluído somente para preservar a compatibilidade com arquivos de MS-DOS existentes, mas não tem nenhum efeito na linha de comando porque a funcionalidade é automática.

## <a name="syntax"></a>Sintaxe

```
break=[on|off]
```

## <a name="remarks"></a>Comentários

Se as extensões de comando estiverem habilitadas e em execução na plataforma Windows, inserir o comando **Break** em um arquivo em lotes entrará em um ponto de interrupção embutido em código, se estiver sendo depurado por um depurador.

#### <a name="additional-references"></a>Referências adicionais

[Chave da sintaxe de linha de comando](command-line-syntax-key.md)