---
ms.assetid: e5f55f3e-8d2a-4526-8d67-36a539126c22
title: Disposição em camadas do fsutil
ms.prod: windows-server-threshold
manager: dmoss
ms.author: toklima
author: toklima
ms.technology: storage
audience: IT Pro
ms.topic: article
ms.date: 10/16/2017
ms.openlocfilehash: dcb69e4e9c71a723bfd735eb7915472f1232a92b
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59859247"
---
# <a name="fsutil-tiering"></a>Disposição em camadas do fsutil
>Aplica-se a: Windows Server (canal semestral), Windows Server 2016, Windows 10

Permite o gerenciamento de funções de camada de armazenamento, como configuração e desabilitando sinalizadores e listagem das camadas.

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
|clearflags|Desabilita os sinalizadores de comportamento de disposição em camadas de um volume.|
|\<volume>|Especifica o volume.|
|/TrNH|Para volumes com o armazenamento hierárquico, faz com que calor coleta seja desabilitada.<br /><br>Aplica-se em NTFS e ReFS apenas.|
|queryflags|Consulta os sinalizadores de comportamento de disposição em camadas de um volume.|
|regionlist|Lista as regiões em camadas de um volume e suas camadas de armazenamento respectivos.|
|setflags|Permite que os sinalizadores de comportamento de disposição em camadas de um volume.|
|tierlist|Lista o tieres de armazenamento associado a um volume.|


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

Para listar as regiões do volume C e suas camadas de armazenamento respectivos, digite:

```
fsutil tiering regionlist C:
```

Para listar as camadas de volume C, digite:

```
fsutil tiering tierlist C:
```



### <a name="additional-references"></a>Referências adicionais
[Chave de sintaxe de linha de comando](Command-Line-Syntax-Key.md)

[Fsutil](Fsutil.md)

