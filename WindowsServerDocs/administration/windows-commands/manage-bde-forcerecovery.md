---
title: Gerenciar-bde forcerecovery
description: 'Tópico de comandos do Windows para * * *- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: eecae37c-c9a3-46c5-b615-a0ace1f1d778
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 5bf7a913c9c496aebded349731ef965a53c98ac7
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59858637"
---
# <a name="manage-bde-forcerecovery"></a>manage-bde: forcerecovery



Força uma unidade protegida pelo BitLocker no modo de recuperação na reinicialização. Esse comando exclui todos os protetores de chave TPM Trusted Platform Module relacionados da unidade. Quando o computador for reiniciado, somente uma senha ou chave de recuperação pode ser usada para desbloquear a unidade. Para obter exemplos de como esse comando pode ser usado, consulte [exemplos](#BKMK_Examples).

## <a name="syntax"></a>Sintaxe

```
manage-bde –forcerecovery <Drive> [-computername <Name>] [{-?|/?}] [{-help|-h}]
```

### <a name="parameters"></a>Parâmetros

|Parâmetro|Descrição|
|---------|-----------|
|\<Drive>|Representa uma letra de unidade seguida de dois-pontos.|
|-computername|Especifica que gerenciar bde.exe será usado para modificar a proteção do BitLocker em um computador diferente. Você também pode usar **- cn** como uma versão abreviada desse comando.|
|\<Nome >|Representa o nome do computador no qual modificar a proteção do BitLocker. Os valores aceitos incluem o nome do computador NetBIOS e endereço IP do computador.|
|-? ou /?|Exibe uma ajuda breve no prompt de comando.|
|-help ou -h|Exibe uma ajuda detalhada no prompt de comando.|

## <a name="BKMK_Examples"></a>Exemplos

O exemplo a seguir ilustra o uso de **forcerecovery -** comando para fazer com que o BitLocker seja inicializado no modo de recuperação na unidade C.
```
manage-bde –forcerecovery C:
```

#### <a name="additional-references"></a>Referências adicionais

-   [Chave de sintaxe de linha de comando](command-line-syntax-key.md)
-   [Gerenciar-bde](manage-bde.md)