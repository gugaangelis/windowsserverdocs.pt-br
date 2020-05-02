---
title: Excluir SC
description: Tópico de referência para * * * *-
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 2fe94fb3-e4d1-47b5-b999-39995ecbb644
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: dd40b5eb82def3b3c437cbdb5b60d279529d25a0
ms.sourcegitcommit: ab64dc83fca28039416c26226815502d0193500c
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 05/01/2020
ms.locfileid: "82722187"
---
# <a name="sc-delete"></a>Excluir SC



Exclui uma subchave de serviço do registro. Se o serviço estiver em execução ou se outro processo tiver um identificador aberto para o serviço, o serviço será marcado para exclusão.

Para obter exemplos de como usar esse comando, consulte [Exemplos](#examples).

## <a name="syntax"></a>Sintaxe

```
sc [<ServerName>] delete [<ServiceName>]
```

### <a name="parameters"></a>Parâmetros

|Parâmetro|Descrição|
|---------|-----------|
|\<ServerName>|Especifica o nome do servidor remoto no qual o serviço está localizado. O nome deve usar o formato UNC (Convenção de nomenclatura universal) (por exemplo \\ \\, meuservidor). Para executar o SC. exe localmente, omita esse parâmetro.|
|\<> do ServiceName|Especifica o nome do serviço retornado pela operação **GetKeyName** .|
|?|Exibe a ajuda no prompt de comando.|

## <a name="remarks"></a>Comentários

Use **Adicionar ou remover programas** no **painel de controle** para excluir DHCP, DNS ou qualquer outro serviço interno do sistema operacional. Observe que **Adicionar ou remover programas** não apenas removerá a subchave do registro para o serviço, mas também desinstalará o serviço e excluirá todos os atalhos para ele.

## <a name="examples"></a>Exemplos

Para excluir a subchave de serviço **NewServ** do registro no computador local, digite:
```
sc delete newserv
```

## <a name="additional-references"></a>Referências adicionais

- [Chave da sintaxe de linha de comando](command-line-syntax-key.md)