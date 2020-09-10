---
title: create
description: Artigo de referência para o comando Create, que cria uma partição ou partição de sombra em um disco, um volume em um ou mais discos ou um VHD (disco rígido virtual).
ms.topic: reference
ms.assetid: b45acde1-8f4f-4ec3-b905-d8188f884af8
ms.author: lizross
author: eross-msft
manager: mtillman
ms.date: 10/16/2017
ms.openlocfilehash: 8a836320f9fe699beac20990ad60ade5f06e37f6
ms.sourcegitcommit: db2d46842c68813d043738d6523f13d8454fc972
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/10/2020
ms.locfileid: "89629013"
---
# <a name="create"></a>create

Cria uma partição ou sombra em um disco, um volume em um ou mais discos ou um VHD (disco rígido virtual). Se você estiver usando esse comando para criar um volume no disco de sombra, você já deve ter pelo menos um volume no conjunto de cópias de sombra.

## <a name="syntax"></a>Sintaxe

```
create partition
create volume
```

### <a name="parameters"></a>Parâmetros

| Parâmetro | Descrição |
| --------- | ----------- |
| [comando criar partição primária](create-partition-primary.md) | Cria uma partição primária no disco básico com foco. |
| [comando Create Partition EFI](create-partition-efi.md) | Cria uma partição de sistema de EFI (Extensible Firmware Interface) em um disco GPT (tabela de partição GUID) em computadores baseados em Itanium. |
| [comando criar partição estendida](create-partition-extended.md) | Cria uma partição estendida no disco com foco. |
| [comando de criação de partição lógica](create-partition-logical.md) | Cria uma partição lógica em uma partição estendida existente. |
| [comando criar partição MSR](create-partition-msr.md) | Cria uma partição MSR (reservada da Microsoft) em um disco de tabela de partição GUID (GPT). |
| [comando Criar volume simples](create-volume-simple.md) | Cria um volume simples no disco dinâmico especificado. |
| [comando criar espelho de volume](create-volume-mirror.md) | Cria um espelho de volume usando os dois discos dinâmicos especificados. |
| [comando criar RAID de volume](create-volume-raid.md) | Cria um volume RAID-5 usando três ou mais discos dinâmicos especificados. |
| [comando Create volume Stripe](create-volume-stripe.md) | Cria um volume distribuído usando dois ou mais discos dinâmicos especificados. |

## <a name="additional-references"></a>Referências adicionais

- [Chave da sintaxe de linha de comando](command-line-syntax-key.md)
