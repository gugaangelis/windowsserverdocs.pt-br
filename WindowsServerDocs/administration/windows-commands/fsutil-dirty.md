---
ms.assetid: 385a2a7c-d6bd-4f11-9c18-fca0413f9e97
title: fsutil suja
ms.prod: windows-server-threshold
manager: dmoss
ms.author: toklima
author: toklima
ms.technology: storage
audience: IT Pro
ms.topic: article
ms.date: 10/16/2017
ms.openlocfilehash: c308b0497a5a39a25384b22441b733143df8727b
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59852127"
---
# <a name="fsutil-dirty"></a>fsutil suja
>Aplica-se a: Windows Server (canal semestral), Windows Server 2016, Windows 10, Windows Server 2012 R2, Windows 8.1, Windows Server 2012, Windows 8, Windows Server 2008 R2, Windows 7

As consultas ou define o bit de um volume incorreto. Quando um volume's sujo bit estiver definido, **autochk** verifica automaticamente o volume de erros na próxima vez que o computador for reiniciado.

Para obter exemplos de como usar esse comando, consulte [Exemplos](#BKMK_examples).

## <a name="syntax"></a>Sintaxe

```
fsutil dirty {query | set} <VolumePath>
```

## <a name="parameters"></a>Parâmetros

|Parâmetro|Descrição|
|-------------|---------------|
|consulta|Consulta o bit incorreto do volume especificado.|
|set|Define o bit incorreto do volume especificado.|
|\<VolumePath>|Especifica o nome da unidade seguido por uma vírgula ou um GUID no formato a seguir: **Volume{***GUID***}**.|

## <a name="remarks"></a>Comentários

-   Bit sujo de um volume indica que o sistema de arquivos pode estar em um estado inconsistente. O bit incorreto pode ser definido porque:

    -   O volume estiver online e ele tem alterações pendentes.

    -   As alterações foram feitas no volume e o computador foi desligado antes que as alterações foram confirmadas no disco.

    -   Corrupção foi detectada no volume.

-   Se o bit incorreto é definido quando o computador for reiniciado, **chkdsk** é executado para verificar a integridade do sistema de arquivo e tentar corrigir quaisquer problemas com o volume.

## <a name="BKMK_examples"></a>Exemplos
Para consultar o bit incorreto na unidade C, digite:

```
fsutil dirty query c:
```

-   Se o volume está sujo, exibe a saída a seguir:

    `Volume C: is dirty`

-   Se o volume não está sujo, exibe a saída a seguir:

    `Volume C: is not dirty`

Para definir o bit incorreto na unidade C, digite:

```
fsutil dirty set C:
```

#### <a name="additional-references"></a>Referências adicionais
[Chave de sintaxe de linha de comando](Command-Line-Syntax-Key.md)

[Fsutil](Fsutil.md)


