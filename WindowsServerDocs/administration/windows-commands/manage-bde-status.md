---
title: gerenciar-status do bde
description: Tópico de comandos do Windows para * * * *-
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 1444a360-fabf-4dd3-b67f-188e6ea3fa5b
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 1c8248333944b030dc8868ba4408024727a08c3a
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80839839"
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

#### <a name="parameters"></a>Parâmetros

|Parâmetro|Descrição|
|---------|-----------|
|Unidade de \<>|Representa uma letra de unidade seguida de dois-pontos.|
|-protectionaserrorlevel|Faz com que a ferramenta de linha de comando Manage-bde envie o código de retorno 0 quando o volume é protegido e 1 quando o volume é desprotegido; mais comumente usado para scripts de lote para determinar se uma unidade está protegida pelo BitLocker. Você também pode usar **-p** como uma versão abreviada deste comando.|
|-ComputerName|Especifica que o Manage-bde. exe será usado para modificar a proteção do BitLocker em um computador diferente. Você também pode usar **-CN** como uma versão abreviada desse comando.|
|Nome do \<>|Representa o nome do computador no qual a proteção do BitLocker será modificada. Os valores aceitos incluem o nome NetBIOS do computador e o endereço IP do computador.|
|-? ou/?|Exibe a ajuda resumida no prompt de comando.|
|-Help ou-h|Exibe a ajuda completa no prompt de comando.|

## <a name="examples"></a><a name=BKMK_Examples></a>Disso

O exemplo a seguir ilustra o uso do comando **-status** para exibir o status da unidade C.
```
manage-bde –status C:
```

## <a name="additional-references"></a>Referências adicionais

-   - [Chave da sintaxe de linha de comando](command-line-syntax-key.md)
-   [Manage-bde](manage-bde.md)