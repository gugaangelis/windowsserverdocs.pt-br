---
title: inativos
description: Artigo de referência para o comando inativo, que marca a partição do sistema ou a partição de inicialização com foco como inativa em discos básicos de MBR (registro mestre de inicialização).
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: f4fb4695-4e66-4166-b4ab-2c86a4605580
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: db7d279577b8120137b4397551a480ea2d2a0e8a
ms.sourcegitcommit: 2afed2461574a3f53f84fc9ec28d86df3b335685
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/02/2020
ms.locfileid: "85924411"
---
# <a name="inactive"></a>inativos

Marca a partição do sistema ou a partição de inicialização com foco como inativa em discos básicos de MBR (registro mestre de inicialização).

Um sistema ativo ou uma partição de inicialização deve ser selecionada para que essa operação seja realizada com sucesso. Use o comando [selecionar partição](select-partition.md) para selecionar a partição ativa e deslocar o foco para ela.

> [!CAUTION]
> Seu computador pode não iniciar sem uma partição ativa. Não marque uma partição de inicialização ou de sistema como inativa, a menos que você seja um usuário experiente com uma compreensão completa da família de sistemas operacionais Windows.<p>Se não for possível iniciar o computador depois de marcar o sistema ou a partição de inicialização como inativa, insira o CD do Instalação do Windows na unidade de CD-ROM, reinicie o computador e repare a partição usando os comandos **FIXMBR** e **fixboot** no console de recuperação.
>
> Depois de marcar a partição do sistema ou a partição de inicialização como inativa, seu computador será iniciado da próxima opção especificada no BIOS, como a unidade de CD-ROM ou um PXE (Pre-Boot eXecution Environment).

## <a name="syntax"></a>Syntax

```
inactive
```

### <a name="examples"></a>Exemplos

```
inactive
```

## <a name="additional-references"></a>Referências adicionais

- [Chave da sintaxe de linha de comando](command-line-syntax-key.md)

- [Selecionar comando de partição](select-partition.md)

- [Solução de problemas avançada para erros de inicialização do Windows](https://docs.microsoft.com/windows/client-management/advanced-troubleshooting-boot-problems)
