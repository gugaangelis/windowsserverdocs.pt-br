---
title: gerenciar-status do bde
description: 'Tópico de comandos do Windows para * * * *- '
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 1444a360-fabf-4dd3-b67f-188e6ea3fa5b
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 235db54ef2361c0e95c66b15a15be7f188fb74d9
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71373862"
---
# <a name="manage-bde-status"></a>Manage-bde: status



Fornece as seguintes informações sobre todas as unidades no computador; Se eles são ou não protegidos pelo BitLocker:
-   Size
-   Versão do BitLocker
-   Status da conversão
-   Porcentagem criptografada
-   Método de criptografia
-   Status de proteção
-   Status do bloqueio
-   Campo de identificação
-   Protetores de chave

Para obter exemplos de como esse comando pode ser usado, consulte [exemplos](#BKMK_Examples).

## <a name="syntax"></a>Sintaxe

```
manage-bde -status [<Drive>] [-protectionaserrorlevel] [-computername <Name>] [{-?|/?}] [{-help|-h}]
```

### <a name="parameters"></a>Parâmetros

|Parâmetro|Descrição|
|---------|-----------|
|\<Drive >|Representa uma letra de unidade seguida de dois-pontos.|
|-protectionaserrorlevel|Faz com que a ferramenta de linha de comando Manage-bde envie o código de retorno 0 quando o volume é protegido e 1 quando o volume é desprotegido; mais comumente usado para scripts de lote para determinar se uma unidade está protegida pelo BitLocker. Você também pode usar **-p** como uma versão abreviada deste comando.|
|-ComputerName|Especifica que o Manage-bde. exe será usado para modificar a proteção do BitLocker em um computador diferente. Você também pode usar **-CN** como uma versão abreviada desse comando.|
|\<Nome >|Representa o nome do computador no qual a proteção do BitLocker será modificada. Os valores aceitos incluem o nome NetBIOS do computador e o endereço IP do computador.|
|-? ou/?|Exibe a ajuda resumida no prompt de comando.|
|-Help ou-h|Exibe a ajuda completa no prompt de comando.|

## <a name="BKMK_Examples"></a>Disso

O exemplo a seguir ilustra o uso do comando **-status** para exibir o status da unidade C.
```
manage-bde –status C:
```

#### <a name="additional-references"></a>Referências adicionais

-   [Chave da sintaxe de linha de comando](command-line-syntax-key.md)
-   [Manage-bde](manage-bde.md)