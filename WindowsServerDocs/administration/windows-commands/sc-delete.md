---
title: Sc.exe excluir
description: Saiba como cancelar o registro de serviços usando o utilitário sc.exe
ms.topic: reference
ms.assetid: 2fe94fb3-e4d1-47b5-b999-39995ecbb644
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 09a3f43824c3e0c895331326341b92c7c6aa5727
ms.sourcegitcommit: 96d46c702e7a9c3a321bbbb5284f73911c7baa3c
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/27/2020
ms.locfileid: "89037534"
---
# <a name="scexe-delete"></a>Sc.exe excluir

Exclui uma subchave de serviço do registro. Se o serviço estiver em execução ou se outro processo tiver um identificador aberto para o serviço, o serviço será marcado para exclusão.

Para obter exemplos de como usar esse comando, consulte [Exemplos](#examples).

## <a name="syntax"></a>Sintaxe

```
sc.exe [<ServerName>] delete [<ServiceName>]
```

### <a name="parameters"></a>Parâmetros

|Parâmetro|Descrição|
|---------|-----------|
|\<ServerName>|Especifica o nome do servidor remoto no qual o serviço está localizado. O nome deve usar o formato UNC (Convenção de nomenclatura universal) (por exemplo, \\ \\ meuservidor). Para executar SC.exe localmente, omita esse parâmetro.|
|\<ServiceName>|Especifica o nome do serviço retornado pela operação **GetKeyName** .|
|?|Exibe a ajuda no prompt de comando.|

## <a name="remarks"></a>Comentários

Não é recomendável usar sc.exe para excluir serviços internos do sistema operacional, como DHCP, DNS ou Serviços de Informações da Internet. Para instalar, remover ou reconfigurar funções, serviços e componentes do sistema operacional, consulte [instalar ou desinstalar funções, serviços de função ou recursos](/WindowsServerDocs/administration/server-manager/install-or-uninstall-roles-role-services-or-features.md)

## <a name="examples"></a>Exemplos

Para excluir a subchave de serviço **NewServ** do registro no computador local, digite:
```
sc.exe delete newserv
```

## <a name="additional-references"></a>Referências adicionais

- [Chave da sintaxe de linha de comando](command-line-syntax-key.md)
