---
title: recover
description: 'Tópico de comandos do Windows para * * *- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 8cc3a73d-9456-41a0-b375-2b4cc37c3992
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 261bfd79d74323ad324246e21b84a5eb798ebcdf
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59867177"
---
# <a name="recover"></a>recover



Atualiza o estado de todos os discos em um grupo de discos, tentar recuperar os discos em um grupo de disco inválidos e ressincroniza volumes espelhados e volumes RAID-5 que tem dados obsoletos.

> [!IMPORTANT]
> Esse comando DiskPart não está disponível em nenhuma edição do Windows Vista.

## <a name="syntax"></a>Sintaxe

```
recover [noerr]
```

## <a name="parameters"></a>Parâmetros

|Parâmetro|Descrição|
|---------|-----------|
|noerr|Somente para scripts. Quando um erro for encontrado, o DiskPart continua a processar comandos como se o erro não tivesse ocorrido. Sem esse parâmetro, um erro causar o DiskPart sair com um código de erro.|

## <a name="remarks"></a>Comentários

-   Esse comando funciona em um grupo de discos.
-   Esse comando só se aplica a grupos de discos dinâmicos. Se esse comando é usado em um grupo com um disco básico, ele não retornará um erro, mas nenhuma ação será tomada.
-   Esse comando funciona em discos com falha ou com falha. Ela também funciona em volumes com falha, falhando, ou em estado de falha de redundância.
-   Um disco que é parte de um grupo de disco deve ser selecionado para este comando ter êxito. Use o **Selecionar disco** comando para selecionar um disco e mudar o foco a ele.

## <a name="BKMK_examples"></a>Exemplos

Para recuperar o grupo de discos que contém o disco com foco, digite:
```
recover
```

#### <a name="additional-references"></a>Referências adicionais

[Chave de sintaxe de linha de comando](command-line-syntax-key.md)

