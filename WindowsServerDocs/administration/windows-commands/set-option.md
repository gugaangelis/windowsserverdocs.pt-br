---
title: opção set
description: Artigo de referência para o comando set option, que define as opções para a criação da cópia de sombra.
ms.topic: reference
ms.assetid: 4d8d4921-9fdd-4a3c-bb0f-9df5458c4b84
ms.author: lizross
author: eross-msft
manager: mtillman
ms.date: 10/16/2017
ms.openlocfilehash: a37e993700169f0268e68822be3a7fbaf6e58361
ms.sourcegitcommit: e164aeffc01069b8f1f3248bf106fcdb7f64f894
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/26/2020
ms.locfileid: "91389052"
---
# <a name="set-option"></a>opção set

Define as opções para a criação da cópia de sombra. Se usado sem parâmetros, a **opção Set** exibirá a ajuda no prompt de comando.

## <a name="syntax"></a>Sintaxe

```
set option {[differential | plex] [transportable] [[rollbackrecover] [txfrecover] | [noautorecover]]}
```

### <a name="parameters"></a>Parâmetros

| Parâmetro | Descrição |
|--|--|
| diferencial | Especifica a criação de um instantâneo pontual de volumes especificados. |
| plexos | Especifica a criação de uma cópia de clonagem pontual dos dados em um volume especificado. |
| transportáveis | Especifica que a cópia de sombra ainda não deve ser importada. O arquivo Metadata. cab pode ser usado posteriormente para importar a cópia de sombra para o mesmo computador ou outro. |
| [rollbackrecover] | Sinaliza os gravadores para usar a *recuperação automática* durante o evento de **pós-instantâneo** . Isso será útil se a cópia de sombra for usada para reversão (por exemplo, com Data Mining). |
| [txfrecover] | Solicita que o VSS torne a cópia de sombra transacionalmente consistente durante a criação. |
| [noautorecover] | Interrompe gravadores e o sistema de arquivos de executar qualquer alteração de recuperação na cópia de sombra para um estado transacionalmente consistente. **Noautorecover** não pode ser usado com **txfrecover** ou **rollbackrecover**. |

## <a name="additional-references"></a>Referências adicionais

- [Chave da sintaxe de linha de comando](command-line-syntax-key.md)

- [Definir comando de contexto](set-context.md)

- [comando Set Metadata](set-metadata.md)

- [Definir comando detalhado](set-verbose.md)
