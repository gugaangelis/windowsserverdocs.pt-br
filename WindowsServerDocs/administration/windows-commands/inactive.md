---
title: inativos
description: Tópico de referência para * * * *-
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: f4fb4695-4e66-4166-b4ab-2c86a4605580
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 4f1c0e0cd5ebbf92638a221852bc3133116f4911
ms.sourcegitcommit: ab64dc83fca28039416c26226815502d0193500c
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 05/01/2020
ms.locfileid: "82724830"
---
# <a name="inactive"></a>inativos



Em discos básicos de MBR (registro mestre de inicialização), o marca a partição do sistema ou a partição de inicialização com foco como inativa.

## <a name="syntax"></a>Sintaxe

```
inactive
```

## <a name="remarks"></a>Comentários

> [!CAUTION]
> Seu computador pode não iniciar sem uma partição ativa. Não marque uma partição de inicialização ou de sistema como inativa, a menos que você seja um usuário experiente com uma compreensão completa da família de sistemas operacionais Windows.</br>> se não for possível iniciar o computador depois de marcar o sistema ou a partição de inicialização como inativa, insira o CD do Instalação do Windows na unidade de CD-ROM, reinicie o computador e repare a partição usando os comandos **FIXMBR** e **fixboot** no console de recuperação.
> -   Depois de marcar a partição do sistema ou a partição de inicialização como inativa, seu computador será iniciado da próxima opção especificada no BIOS, como a unidade de CD-ROM ou um PXE (Pre-Boot eXecution Environment).
> -   Um sistema ativo ou uma partição de inicialização deve ser selecionada para que essa operação seja realizada com sucesso. Use o comando **selecionar partição** para selecionar a partição ativa e deslocar o foco para ela.

## <a name="examples"></a>Exemplos

```
inactive
```

## <a name="additional-references"></a>Referências adicionais

- [Chave da sintaxe de linha de comando](command-line-syntax-key.md)

