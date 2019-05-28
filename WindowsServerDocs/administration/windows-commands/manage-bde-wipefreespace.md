---
title: Gerenciar-bde WipeFreeSpace
description: 'Tópico de comandos do Windows para * * *- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: b8d83a2a-c5c8-4019-9041-23d1d6abf282
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 7cf99a9124f78189de223018608d9864e51d7897
ms.sourcegitcommit: 08eba714d3ceb5f2dfb5486d6b990da1aa4dcbdd
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 05/14/2019
ms.locfileid: "65564687"
---
# <a name="manage-bde-wipefreespace"></a>Gerenciar-bde: WipeFreeSpace



Apaga o espaço livre no volume, removendo qualquer fragmentos de dados que podem ter existido no espaço. Executar esse comando em um volume que foi criptografado usando o método de criptografia "Apenas do espaço usado" fornece o mesmo nível de proteção que o método de criptografia "Full Volume Encryption". Para obter exemplos de como esse comando pode ser usado, consulte [exemplos](#BKMK_Examples).

## <a name="syntax"></a>Sintaxe

```
manage-bde -WipeFreeSpace|-w [<Drive>] [-Cancel] [-computername <Name>] [{-?|/?}] [{-help|-h}]
```

### <a name="parameters"></a>Parâmetros

|Parâmetro|Descrição|
|---------|-----------|
|\<Drive>|Representa uma letra de unidade seguida por dois-pontos, um caminho GUID de volume ou um volume montado.|
|-Cancelar|Cancela um apagamento de espaço livre que está em processo.|
|-computername|Especifica que gerenciar bde.exe será usado para modificar a proteção do BitLocker em um computador diferente. Você também pode usar **- cn** como uma versão abreviada desse comando.|
|\<Nome >|Representa o nome do computador no qual modificar a proteção do BitLocker. Os valores aceitos incluem o nome do computador NetBIOS e endereço IP do computador.|
|-? ou /?|Exibe uma ajuda breve no prompt de comando.|
|-help ou -h|Exibe uma ajuda detalhada no prompt de comando.|

## <a name="BKMK_Examples"></a>Exemplos

O exemplo a seguir ilustra o uso de **-w** apagar de comando para criar o espaço livre na unidade C.
```
manage-bde -w C:
```
O exemplo a seguir ilustra o uso de **-w** com o **-Cancelar** parâmetro para cancelar o apagamento o espaço livre na unidade C.
```
manage-bde -w -Cancel C:
```

#### <a name="additional-references"></a>Referências adicionais

-   [Chave da sintaxe de linha de comando](command-line-syntax-key.md)
-   [Gerenciar-bde](manage-bde.md)