---
title: showmount
description: Artigo de referência para o comando de getmount, que exibe informações sobre sistemas de arquivos montados exportados pelo servidor para NFS em um computador especificado.
ms.topic: reference
ms.assetid: a6dd562e-e3bd-4ee6-be3b-6d29e29fd20e
ms.author: lizross
author: eross-msft
manager: mtillman
ms.date: 10/16/2017
ms.openlocfilehash: 3af87ba4ed42789cf07a2ae63ce9e720043a196e
ms.sourcegitcommit: 720455aad2bac78cf64997d196a13f35ea0acb73
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 10/05/2020
ms.locfileid: "91718323"
---
# <a name="showmount"></a>showmount

> Aplica-se a: Windows Server (canal semestral), Windows Server 2019, Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Você pode usar a **montagem** para exibir informações sobre sistemas de arquivos montados exportados pelo servidor para NFS em um computador especificado. Se você não especificar um servidor, esse comando exibirá informações sobre o computador no qual o comando de **montagem** é executado.

## <a name="syntax"></a>Sintaxe

```
showmount {-e|-a|-d} <server>
```

### <a name="parameters"></a>Parâmetros

| Parâmetro | Descrição |
|--|--|
| -E | Exibe todos os sistemas de arquivos exportados no servidor. |
| -a | Exibe todos os clientes NFS (sistema de arquivos de rede) e os diretórios no servidor que cada um montou. |
| -d | Exibe todos os diretórios no servidor que estão montados atualmente por clientes NFS. |

## <a name="additional-references"></a>Referências adicionais

- [Chave da sintaxe de linha de comando](command-line-syntax-key.md)

- [Referência aos comandos dos Serviços para Network File System](services-for-network-file-system-command-reference.md)