---
title: status do bde gerenciar
description: 'Tópico de comandos do Windows para * * *- '
ms.custom: na
ms.prod: windows-server-threshold
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
ms.openlocfilehash: d81808b57b1833ca30b95dc9d4b6aa0b0a4bdbaa
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59836557"
---
# <a name="manage-bde-status"></a>manage-bde: status



Fornece as seguintes informações sobre todas as unidades no computador. Se elas são protegidas pelo BitLocker:
-   Tamanho
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
|\<Drive>|Representa uma letra de unidade seguida de dois-pontos.|
|-protectionaserrorlevel|Faz com que a ferramenta de linha de comando Manage-bde enviar o código de retorno de 0 quando o volume é protegido e 1, quando o volume está desprotegido; mais comumente usado para scripts em lotes para determinar se uma unidade protegida pelo BitLocker. Você também pode usar **-p** como uma versão abreviada desse comando.|
|-computername|Especifica que gerenciar bde.exe será usado para modificar a proteção do BitLocker em um computador diferente. Você também pode usar **- cn** como uma versão abreviada desse comando.|
|\<Nome >|Representa o nome do computador no qual modificar a proteção do BitLocker. Os valores aceitos incluem o nome do computador NetBIOS e endereço IP do computador.|
|-? ou /?|Exibe uma ajuda breve no prompt de comando.|
|-help ou -h|Exibe uma ajuda detalhada no prompt de comando.|

## <a name="BKMK_Examples"></a>Exemplos

O exemplo a seguir ilustra o uso de **-status** comando para exibir o status da unidade C.
```
manage-bde –status C:
```

#### <a name="additional-references"></a>Referências adicionais

-   [Chave de sintaxe de linha de comando](command-line-syntax-key.md)
-   [Gerenciar-bde](manage-bde.md)