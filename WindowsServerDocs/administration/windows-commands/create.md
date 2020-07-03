---
title: create
description: Artigo de referência para o comando Create, que cria uma partição ou partição de sombra em um disco, um volume em um ou mais discos ou um VHD (disco rígido virtual).
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: b45acde1-8f4f-4ec3-b905-d8188f884af8
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 0b5b31f43e58b9e2eddb18f624c1054c9d028f4c
ms.sourcegitcommit: 2afed2461574a3f53f84fc9ec28d86df3b335685
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/02/2020
ms.locfileid: "85929523"
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
