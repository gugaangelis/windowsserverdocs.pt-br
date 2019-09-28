---
title: listar gravadores
description: 'Tópico de comandos do Windows para * * * *- '
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 1c30cbc4-f568-4fa7-b564-66c41d3ca82d
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: d00eebe4e562764e97794b3eb1b76ea96c2dc47c
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71374723"
---
# <a name="list-writers"></a>listar gravadores



Lista gravadores que estão no sistema. Se usado sem parâmetros, **list** exibirá a saída para **listar metadados** por padrão.

Para obter exemplos de como usar esse comando, consulte [Exemplos](#BKMK_examples).

## <a name="syntax"></a>Sintaxe

```
list writers [metadata | detailed | status]
```

## <a name="parameters"></a>Parâmetros

|Parâmetro|Descrição|
|---------|-----------|
|Los|Lista a identidade e o status dos gravadores e exibe os metadados, como detalhes do componente e arquivos excluídos. Esse é o parâmetro padrão.|
|Visão|Lista as mesmas informações que os **metadados**, mas **detalhado** inclui a lista completa de arquivos para todos os componentes.|
|status|Lista somente a identidade e o status dos gravadores registrados.|

## <a name="BKMK_examples"></a>Disso

Para listar apenas a identidade e o status dos gravadores, digite:
```
list writers status
```
Saída semelhante às seguintes exibições:
```
Listing writer status ...
* WRITER "System Writer"
        - Status: 5 (VSS_WS_WAITING_FOR_BACKUP_COMPLETE)
        - Writer Failure code: 0x00000000 (S_OK)
        - Writer ID: {e8132975-6f93-4464-a53e-1050253ae220}
        - Instance ID: {7e631031-c695-4229-9da1-a7de057e64cb}
* WRITER "Shadow Copy Optimization Writer"
        - Status: 1 (VSS_WS_STABLE)
        - Writer Failure code: 0x00000000 (S_OK)
        - Writer ID: {4dc3bdd4-ab48-4d07-adb0-3bee2926fd7f}
        - Instance ID: {9e362607-9794-4dd4-a7cd-b3d5de0aad20}
...
...
...
* WRITER "Registry Writer"
        - Status: 1 (VSS_WS_STABLE)
        - Writer Failure code: 0x00000000 (S_OK)
        - Writer ID: {afbab4a2-367d-4d15-a586-71dbb18f8485}
        - Instance ID: {e87ba7e3-f8d8-42d8-b2ee-c76ae26b98e8}
8 writers listed. 
```

#### <a name="additional-references"></a>Referências adicionais

[Chave da sintaxe de linha de comando](command-line-syntax-key.md)