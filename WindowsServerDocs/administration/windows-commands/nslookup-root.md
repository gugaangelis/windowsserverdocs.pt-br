---
title: nslookup root
description: Artigo de referência para o comando raiz Nslookup, que altera o servidor padrão para o servidor para a raiz do espaço de nome de domínio DNS (sistema de nomes de domínio).
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 9c29edc3-ec49-43f2-bc49-86bf0612d816
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 07dbfcf401314145cb0e7553d71480072da4b574
ms.sourcegitcommit: 2afed2461574a3f53f84fc9ec28d86df3b335685
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/02/2020
ms.locfileid: "85936280"
---
# <a name="nslookup-root"></a>nslookup root

> Aplica-se a: Windows Server (canal semestral), Windows Server 2019, Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Altera o servidor padrão para o servidor para a raiz do espaço de nome de domínio DNS (sistema de nomes de domínio). Atualmente, o servidor de nome ns.nic.ddn.mil é usado. Você pode alterar o nome do servidor raiz usando o comando [nslookup Set root](nslookup-set-root.md) .

> [!NOTE]
> Esse comando é o mesmo que `lserver ns.nic.ddn.mil` .

## <a name="syntax"></a>Sintaxe

```
root
```

### <a name="parameters"></a>Parâmetros

| Parâmetro | Descrição |
| --------- | ----------- |
| /? | Exibe a ajuda no prompt de comando. |
| /help | Exibe a ajuda no prompt de comando. |

## <a name="additional-references"></a>Referências adicionais

- [Chave da sintaxe de linha de comando](command-line-syntax-key.md)

- [nslookup set root](nslookup-set-root.md)
