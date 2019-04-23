---
title: Exclusão de SC
description: 'Tópico de comandos do Windows para * * *- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 2fe94fb3-e4d1-47b5-b999-39995ecbb644
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: b60127cf957a30d147c9992c74c01e37e5b8bf89
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59871917"
---
# <a name="sc-delete"></a>Exclusão de SC



Exclui uma subchave de serviço do registro. Se o serviço está em execução ou se outro processo tem um identificador aberto para o serviço, o serviço está marcado para exclusão.

Para obter exemplos de como usar esse comando, consulte [Exemplos](#BKMK_examples).

## <a name="syntax"></a>Sintaxe

```
sc [<ServerName>] delete [<ServiceName>]
```

## <a name="parameters"></a>Parâmetros

|Parâmetro|Descrição|
|---------|-----------|
|\<ServerName>|Especifica o nome do servidor remoto no qual o serviço está localizado. O nome deve usar o formato de convenção de nomenclatura Universal (UNC) (por exemplo, \\ \\myserver). Para executar SC.exe localmente, omita este parâmetro.|
|\<ServiceName>|Especifica o nome do serviço retornado pelo **getkeyname** operação.|
|?|Exibe a ajuda no prompt de comando.|

## <a name="remarks"></a>Comentários

Use **adicionar ou remover programas** nos **painel de controle** excluir DHCP, DNS ou qualquer outro serviço interno do sistema operacional. Observe que **adicionar ou remover programas** não removerá apenas a subchave do registro para o serviço, mas ele também desinstale o serviço e exclua todos os atalhos para ele.

## <a name="BKMK_examples"></a>Exemplos

Para excluir a subchave de serviço **NewServ** do registro no computador local, digite:
```
sc delete newserv
```

#### <a name="additional-references"></a>Referências adicionais

[Chave de sintaxe de linha de comando](command-line-syntax-key.md)