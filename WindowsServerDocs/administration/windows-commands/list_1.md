---
title: lista
description: 'Tópico de comandos do Windows para * * *- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 69b105a1-9710-4a06-8102-38cc9e475ca5
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: ef9262a04f469f54e43cf3a83efe30fac7ad8580
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59854667"
---
# <a name="list"></a>lista



Exibe uma lista de discos, partições em um disco, volumes em um disco ou de discos rígidos virtuais (VHDs).

## <a name="syntax"></a>Sintaxe

```
list { disk | partition | volume | vdisk }
```

## <a name="parameters"></a>Parâmetros

|Parâmetro|Descrição|
|---------|-----------|
|disco|Exibe uma lista de discos e informações sobre eles, como tamanho, a quantidade de espaço livre disponível, se o disco é básico ou dinâmico e se o disco usa o registro mestre de inicialização (MBR) ou o estilo de partição GUID partição GPT (tabela).|
|partição|Exibe as partições listadas na tabela de partição do disco atual.|
|volume|Exibe uma lista dos volumes básicos e dinâmicos em todos os discos.|
|vdisk|Exibe uma lista dos VHDs que estão anexados e/ou selecionados. Esse comando lista os VHDs desanexados se forem selecionados no momento; No entanto, o tipo de disco é definido como desconhecido até que o VHD está anexado. O VHD marcado com um asterisco (*) tem o foco.</br>Observação: Esse comando só está disponível para Windows 7 e Windows Server 2008 R2.|

## <a name="remarks"></a>Comentários

-   Ao listar as partições em um disco dinâmico, as partições podem não corresponder aos volumes dinâmicos no disco. Essa discrepância ocorre porque os discos dinâmicos contêm entradas na tabela de partição para o volume do sistema ou volume de inicialização (se presente no disco). Eles também contêm uma partição que ocupa o restante do disco para reservar o espaço para uso por volumes dinâmicos.
-   O objeto marcado com um asterisco (*) tem o foco.
-   Ao listar os discos, se um disco estiver ausente, seu número de disco é prefixado com M. Por exemplo, o primeiro disco ausente é numerado M0.

## <a name="BKMK_examples"></a>Exemplos

```
list disk
list partition
list volume
list vdisk
```

#### <a name="additional-references"></a>Referências adicionais

[Chave de sintaxe de linha de comando](command-line-syntax-key.md)

