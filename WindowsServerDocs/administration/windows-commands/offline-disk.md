---
title: Disco offline
description: 'Tópico de comandos do Windows para * * *- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 8fb9b3c3-0b2c-4192-a2e7-f706292653e3
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 617371583a3f0cb3d0cb739845208e4216573d9c
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59834617"
---
# <a name="offline-disk"></a>Disco offline



Usa o disco online com o foco para o estado offline.

> [!IMPORTANT]
> Esse comando DiskPart não está disponível em nenhuma edição do Windows Vista.

## <a name="syntax"></a>Sintaxe

```
offline disk [noerr]
```

## <a name="parameters"></a>Parâmetros

|Parâmetro|Descrição|
|---------|-----------|
|noerr|Somente para scripts. Quando um erro for encontrado, o DiskPart continua a processar comandos como se o erro não tivesse ocorrido. Sem esse parâmetro, um erro causar o DiskPart sair com um código de erro.|

## <a name="remarks"></a>Comentários

-   Esse comando funciona em discos que estão no modo online de SAN. Ele altera o modo de SAN para offline.
-   Se um disco dinâmico em um grupo de discos é colocado offline, o status do disco seja **ausente** e o grupo mostra um disco que esteja offline. O disco ausente é movido para o grupo inválido. Se o disco dinâmico é o último no grupo, o status do disco será alterado para **offline**, e o grupo vazio será removido.
-   Um disco deve ser selecionado para o **disco offline** comando tenha êxito. Use o **Selecionar disco** comando para selecionar um disco e mudar o foco a ele.

## <a name="BKMK_examples"></a>Exemplos

Para colocar o disco com foco off-line, digite:
```
offline disk
```

#### <a name="additional-references"></a>Referências adicionais

[Chave de sintaxe de linha de comando](command-line-syntax-key.md)

