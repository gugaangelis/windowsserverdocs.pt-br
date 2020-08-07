---
title: gerenciar-status do bde
description: Artigo de referência para o comando status Manage-bde, que fornece informações sobre todas as unidades no computador, independentemente se elas estão protegidas pelo BitLocker.
ms.topic: article
ms.assetid: 1444a360-fabf-4dd3-b67f-188e6ea3fa5b
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: cab8f552cd633e5e71a13993fd2223062125f02f
ms.sourcegitcommit: 53d526bfeddb89d28af44210a23ba417f6ce0ecf
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/06/2020
ms.locfileid: "87886694"
---
# <a name="manage-bde-status"></a>gerenciar-status do bde

Fornece informações sobre todas as unidades no computador; Se eles são ou não protegidos pelo BitLocker, incluindo:

- Tamanho

- Versão do BitLocker

- Status da conversão

- Porcentagem criptografada

- Método de criptografia

- Status de proteção

- Status do bloqueio

- Campo de identificação

- Protetores de chave

## <a name="syntax"></a>Sintaxe

```
manage-bde -status [<drive>] [-protectionaserrorlevel] [-computername <name>] [{-?|/?}] [{-help|-h}]
```

### <a name="parameters"></a>Parâmetros

| Parâmetro | Descrição |
| --------- | ----------- |
| `<drive>` | Representa uma letra de unidade seguida de dois-pontos. |
| -protectionaserrorlevel | Faz com que a ferramenta de linha de comando Manage-bde envie o código de retorno **0** se o volume estiver protegido e **1** se o volume estiver desprotegido; mais comumente usado para scripts de lote para determinar se uma unidade está protegida pelo BitLocker. Você também pode usar **-p** como uma versão abreviada deste comando. |
| -ComputerName | Especifica que manage-bde.exe será usado para modificar a proteção do BitLocker em um computador diferente. Você também pode usar **-CN** como uma versão abreviada desse comando. |
| `<name>` | Representa o nome do computador no qual a proteção do BitLocker será modificada. Os valores aceitos incluem o nome NetBIOS do computador e o endereço IP do computador. |
| -? ou/? | Exibe a ajuda resumida no prompt de comando. |
| -Help ou-h | Exibe a ajuda completa no prompt de comando. |

### <a name="examples"></a>Exemplos

Para exibir o status da unidade C, digite:

```
manage-bde –status C:
```

## <a name="additional-references"></a>Referências adicionais

- [Chave da sintaxe de linha de comando](command-line-syntax-key.md)

- [comando Manage-bde](manage-bde.md)
