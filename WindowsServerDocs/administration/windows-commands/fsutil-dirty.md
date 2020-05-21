---
title: fsutil Dirty
description: Tópico de referência para o comando fsutil Dirty, que consulta ou define um bit sujo de volume.
ms.prod: windows-server
manager: dmoss
ms.author: toklima
author: toklima
ms.technology: storage
ms.assetid: 385a2a7c-d6bd-4f11-9c18-fca0413f9e97
ms.topic: article
ms.date: 10/16/2017
ms.openlocfilehash: 72defd974177675f53e89fb8570f028580b7e167
ms.sourcegitcommit: bf887504703337f8ad685d778124f65fe8c3dc13
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 05/16/2020
ms.locfileid: "83435861"
---
# <a name="fsutil-dirty"></a>fsutil Dirty

> Aplica-se a: Windows Server (canal semestral), Windows Server 2019, Windows Server 2016, Windows 10, Windows Server 2012 R2, Windows 8.1, Windows Server 2012, Windows 8

Consulta ou define um bit sujo de volume. Quando o bit sujo de um volume é definido, o **autochk** verifica automaticamente o volume em busca de erros na próxima vez em que o computador for reiniciado.

## <a name="syntax"></a>Sintaxe

```
fsutil dirty {query | set} <volumepath>
```

### <a name="parameters"></a>Parâmetros

| Parâmetro | Descrição |
| --------- | ----------- |
| Consulta | Consulta o bit sujo do volume especificado. |
| set | Define o bit sujo do volume especificado. |
| `<volumepath>` | Especifica o nome da unidade seguido por dois-pontos ou GUID no seguinte formato: `volume{GUID}` . |

#### <a name="remarks"></a>Comentários

- O bit sujo de um volume indica que o sistema de arquivos pode estar em um estado inconsistente. O bit sujo pode ser definido porque:

    - O volume está online e tem alterações pendentes.

    - Foram feitas alterações no volume e o computador foi desligado antes que as alterações fossem confirmadas no disco.

    - Foi detectado um dano no volume.

- Se o bit sujo for definido quando o computador for reiniciado, **chkdsk** será executado para verificar a integridade do sistema de arquivos e tentar corrigir quaisquer problemas com o volume.

### <a name="examples"></a>Exemplos

Para consultar o bit sujo na unidade C, digite:

```
fsutil dirty query c:
```

    If the volume is dirty, the following output displays:

    `Volume C: is dirty`

    If the volume isn't dirty, the following output displays:

    `Volume C: is not dirty`

Para definir o bit sujo na unidade C, digite:

```
fsutil dirty set C:
```

## <a name="additional-references"></a>Referências adicionais

- [Chave da sintaxe de linha de comando](command-line-syntax-key.md)

- [fsutil](fsutil.md)
