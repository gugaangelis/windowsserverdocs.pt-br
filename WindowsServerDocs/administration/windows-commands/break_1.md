---
title: break
description: O tópico de comandos do Windows para break_1, que define ou limpa a verificação estendida CTRL + C em sistemas MS-DOS.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: c89b7357-d69e-4141-826e-73c9ba0fc630
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 809a9321b8b4f8b2d201582f767da132076826d4
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80848359"
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

## <a name="additional-references"></a>Referências adicionais

- [Chave da sintaxe de linha de comando](command-line-syntax-key.md)