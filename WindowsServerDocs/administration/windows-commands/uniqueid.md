---
title: uniqueid
description: Artigo de referência para UniqueId, que exibe ou define o identificador GPT (tabela de partição GUID) ou a assinatura MBR (registro mestre de inicialização) para o disco com foco.
ms.topic: reference
ms.assetid: 64235a4a-b91c-46da-b9b0-68ee90571c2a
ms.author: lizross
author: eross-msft
manager: mtillman
ms.date: 10/16/2017
ms.openlocfilehash: 73721316747ffd0380f178622d37dc4fdbefc2fb
ms.sourcegitcommit: f45640cf4fda621b71593c63517cfdb983d1dc6a
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 10/17/2020
ms.locfileid: "92156295"
---
# <a name="uniqueid"></a>uniqueid

Exibe ou define o identificador GPT (tabela de partição GUID) ou a assinatura MBR (registro mestre de inicialização) para o disco básico ou dinâmico com foco. Um disco básico ou dinâmico deve ser selecionado para que essa operação seja realizada com sucesso. Use o [comando selecionar disco](select-disk.md) para selecionar um disco e deslocar o foco para ele.

## <a name="syntax"></a>Sintaxe

```
uniqueid disk [id={<dword> | <GUID>}] [noerr]
```

### <a name="parameters"></a>Parâmetros

| Parâmetro | Descrição |
|--|--|
| ID =`{<dword> | <GUID>}` | Para discos MBR, esse parâmetro especifica um valor de 4 bytes (DWORD) em formato hexadecimal para a assinatura. Para discos GPT, esse parâmetro especifica um GUID para o identificador. |
| NOERR | Somente para scripts. Quando ocorre um erro, o DiskPart continua processando comandos como se o erro não tivesse ocorrido. Sem esse parâmetro, um erro faz com que o DiskPart saia com um código de erro. |

## <a name="examples"></a>Exemplos

Para exibir a assinatura do disco MBR com foco, digite:

```
uniqueid disk
```

Para definir a assinatura do disco MBR com foco para o valor DWORD *5f1b2c36*, digite:

```
uniqueid disk id=5f1b2c36
```

Para definir o identificador do disco GPT com foco para o valor de GUID baf784e7-6bbd-4cfb-AAAC-e86c96e166ee, digite:

```
uniqueid disk id=baf784e7-6bbd-4cfb-aaac-e86c96e166ee
```

## <a name="additional-references"></a>Referências adicionais

- [Chave da sintaxe de linha de comando](command-line-syntax-key.md)

- [Selecionar comando de disco](select-disk.md)
