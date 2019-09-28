---
ms.assetid: e5f55f3e-8d2a-4526-8d67-36a539126c22
title: Fsutil tiering
ms.prod: windows-server
manager: dmoss
ms.author: toklima
author: toklima
ms.technology: storage
audience: IT Pro
ms.topic: article
ms.date: 10/16/2017
ms.openlocfilehash: 6863940d69e30f4984897a7e03369a834da21d1d
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71376785"
---
# <a name="fsutil-tiering"></a>Fsutil tiering
>Aplica-se a: Windows Server (canal semestral), Windows Server 2016, Windows 10

Habilita o gerenciamento de funções de camada de armazenamento, como a configuração e a desabilitação de sinalizadores e a listagem de camadas.

## <a name="syntax"></a>Sintaxe

```
fsutil tiering [clearflags] <volume> <flags>
fsutil tiering [queryflags] <volume>
fsutil tiering [regionlist] <volume>
fsutil tiering [setflags] <volume> <flags>
fsutil tiering [tierlist] <volume>
```

### <a name="parameters"></a>Parâmetros

|Parâmetro|Descrição|
|-------------|---------------|
|clearflags|Desabilita os sinalizadores de comportamento de camadas de um volume.|
|\<volume >|Especifica o volume.|
|/TrNH|Para volumes com armazenamento em camadas, o faz com que a coleta de calor seja desabilitada.<br /><br>Aplica-se somente a NTFS e ReFS.|
|queryflags|Consulta os sinalizadores de comportamento de camadas de um volume.|
|regiãolist|Lista as regiões em camadas de um volume e suas respectivas camadas de armazenamento.|
|SetFlags|Habilita os sinalizadores de comportamento de camadas de um volume.|
|camadalist|Lista as camadas de armazenamento associadas a um volume.|


### <a name="examples"></a>Exemplos

Para consultar os sinalizadores no volume C, digite:

```
fsutil tiering clearflags C:
```

Para definir os sinalizadores no volume C, digite:

```
fsutil tiering setflags C: /TrNH
```

Para limpar os sinalizadores no volume C, digite:

```
fsutil tiering clearflags C: /TrNH
```

Para listar as regiões do volume C e suas respectivas camadas de armazenamento, digite:

```
fsutil tiering regionlist C:
```

Para listar as camadas do volume C, digite:

```
fsutil tiering tierlist C:
```



### <a name="additional-references"></a>Referências adicionais
[Chave da sintaxe de linha de comando](Command-Line-Syntax-Key.md)

[Fsutil](Fsutil.md)

