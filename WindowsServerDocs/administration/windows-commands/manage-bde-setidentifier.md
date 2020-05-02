---
title: gerenciar-bde setidentifier
description: Tópico de referência para * * * *-
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 7092d18f-4ac9-4c73-a20f-1246ca60e75e
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: dec19003f9a3421cfd2c73ba892f68aebfb8e133
ms.sourcegitcommit: ab64dc83fca28039416c26226815502d0193500c
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 05/01/2020
ms.locfileid: "82724093"
---
# <a name="manage-bde-setidentifier"></a>Manage-bde: setidentifier



Define o campo identificador da unidade na unidade para o valor especificado na configuração **fornecer os identificadores exclusivos para sua organização** política de grupo.

## <a name="syntax"></a>Sintaxe

```
manage-bde –setidentifier <Drive> [-computername <Name>] [{-?|/?}] [{-help|-h}]
```

#### <a name="parameters"></a>Parâmetros

|Parâmetro|Descrição|
|---------|-----------|
|\<> da unidade|Representa uma letra de unidade seguida de dois-pontos.|
|-ComputerName|Especifica que o Manage-bde. exe será usado para modificar a proteção do BitLocker em um computador diferente. Você também pode usar **-CN** como uma versão abreviada desse comando.|
|\<Name>|Representa o nome do computador no qual a proteção do BitLocker será modificada. Os valores aceitos incluem o nome NetBIOS do computador e o endereço IP do computador.|
|-? ou/?|Exibe a ajuda resumida no prompt de comando.|
|-Help ou-h|Exibe a ajuda completa no prompt de comando.|

## <a name="examples"></a>Exemplos

Para ilustrações usando o comando **-setidentifier** para definir o campo do identificador de unidade do BitLocker para C.
```
manage-bde –setidentifier C:
```

## <a name="additional-references"></a>Referências adicionais

-   - [Chave da sintaxe de linha de comando](command-line-syntax-key.md)
-   [Manage-bde](manage-bde.md)
-   [Usando agentes de recuperação de dados com o BitLocker](https://technet.microsoft.com/library/dd875560(WS.10).aspx)