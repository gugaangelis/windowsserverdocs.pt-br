---
title: Opção Set
description: Artigo de referência para a opção set, que define as opções para a criação da cópia de sombra.
ms.topic: article
ms.assetid: 4d8d4921-9fdd-4a3c-bb0f-9df5458c4b84
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 148efa02509678de65af7b2555094fbcb2a3fb55
ms.sourcegitcommit: 53d526bfeddb89d28af44210a23ba417f6ce0ecf
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/06/2020
ms.locfileid: "87882599"
---
# <a name="set-option"></a>Opção Set

Define as opções para a criação da cópia de sombra. Se usado sem parâmetros, a **opção Set** exibirá a ajuda no prompt de comando.

## <a name="syntax"></a>Sintaxe

```
set option {[differential | plex] [transportable] [[rollbackrecover] [txfrecover] | [noautorecover]]}
```

### <a name="parameters"></a>Parâmetros

|     Parâmetro     |                                                                                                  Descrição                                                                                                  |
|-------------------|---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
|   [diferencial   |                                                                                                     plexos                                                                                                     |
|  transportáveis  |                       Especifica que a cópia de sombra ainda não deve ser importada. O arquivo Metadata. cab pode ser usado posteriormente para importar a cópia de sombra para o mesmo computador ou outro.                       |
| [rollbackrecover] |                     Sinaliza os gravadores para usar a *recuperação automática* durante o evento de **pós-instantâneo** . Isso será útil se a cópia de sombra for usada para reversão (por exemplo, com Data Mining).                      |
|   [txfrecover]    |                                                               Solicita que o VSS torne a cópia de sombra transacionalmente consistente durante a criação.                                                                |
|  [noautorecover]  | Interrompe gravadores e o sistema de arquivos de executar qualquer alteração de recuperação na cópia de sombra para um estado transacionalmente consistente. **Noautorecover** não pode ser usado com **txfrecover** ou **rollbackrecover**. |

## <a name="additional-references"></a>Referências adicionais

- [Chave da sintaxe de linha de comando](command-line-syntax-key.md)