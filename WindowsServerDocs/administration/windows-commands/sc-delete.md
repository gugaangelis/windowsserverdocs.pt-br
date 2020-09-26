---
title: sc.exe excluir
description: Artigo de referência para o comando sc.exe Delete, que exclui uma subchave de serviço do registro.
ms.topic: reference
ms.assetid: 2fe94fb3-e4d1-47b5-b999-39995ecbb644
ms.author: lizross
author: eross-msft
manager: mtillman
ms.date: 10/16/2017
ms.openlocfilehash: 12c09bd72259a8e4b58f134ca19f1fc825f2f1bb
ms.sourcegitcommit: e164aeffc01069b8f1f3248bf106fcdb7f64f894
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/26/2020
ms.locfileid: "91388457"
---
# <a name="scexe-delete"></a>sc.exe excluir

Exclui uma subchave de serviço do registro. Se o serviço estiver em execução ou se outro processo tiver um identificador aberto para o serviço, o serviço será marcado para exclusão.

> [!NOTE]
> Não recomendamos que você use esse comando para excluir serviços internos do sistema operacional, como DHCP, DNS ou Serviços de Informações da Internet. Para instalar, remover ou reconfigurar funções, serviços e componentes do sistema operacional, consulte [instalar ou desinstalar funções, serviços de função ou recursos](/WindowsServerDocs/administration/server-manager/install-or-uninstall-roles-role-services-or-features.md)

## <a name="syntax"></a>Sintaxe

```
sc.exe [<servername>] delete [<servicename>]
```

### <a name="parameters"></a>Parâmetros

| Parâmetro | Descrição |
|--|--|
| `<servername>` | Especifica o nome do servidor remoto no qual o serviço está localizado. O nome deve usar o formato UNC (Convenção de nomenclatura universal) (por exemplo, \\ meuservidor). Para executar o SC.exe localmente, não use esse parâmetro. |
| `<servicename>` | Especifica o nome do serviço retornado pela operação **GetKeyName** . |
| /? | Exibe a ajuda no prompt de comando. |

## <a name="examples"></a>Exemplos

Para excluir a subchave de serviço **NewServ** do registro no computador local, digite:

```
sc.exe delete NewServ
```

## <a name="additional-references"></a>Referências adicionais

- [Chave da sintaxe de linha de comando](command-line-syntax-key.md)
