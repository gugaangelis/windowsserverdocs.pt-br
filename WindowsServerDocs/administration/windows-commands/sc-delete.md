---
title: Excluir SC. exe
description: Saiba como cancelar o registro de serviços usando o utilitário SC. exe
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 2fe94fb3-e4d1-47b5-b999-39995ecbb644
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 284012cf6799df52832e62c3eea1b2f0fcd84805
ms.sourcegitcommit: 95b60384b0b070263465eaffb27b8e3bb052a4de
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 05/06/2020
ms.locfileid: "82850107"
---
# <a name="scexe-delete"></a>Excluir SC. exe

Exclui uma subchave de serviço do registro. Se o serviço estiver em execução ou se outro processo tiver um identificador aberto para o serviço, o serviço será marcado para exclusão.

Para obter exemplos de como usar esse comando, consulte [Exemplos](#examples).

## <a name="syntax"></a>Sintaxe

```
sc.exe [<ServerName>] delete [<ServiceName>]
```

### <a name="parameters"></a>Parâmetros

|Parâmetro|Descrição|
|---------|-----------|
|\<ServerName>|Especifica o nome do servidor remoto no qual o serviço está localizado. O nome deve usar o formato UNC (Convenção de nomenclatura universal) (por exemplo \\ \\, meuservidor). Para executar o SC. exe localmente, omita esse parâmetro.|
|\<> do ServiceName|Especifica o nome do serviço retornado pela operação **GetKeyName** .|
|?|Exibe a ajuda no prompt de comando.|

## <a name="remarks"></a>Comentários

Não é recomendável usar SC. exe para excluir serviços internos do sistema operacional, como DHCP, DNS ou Serviços de Informações da Internet. Para instalar, remover ou reconfigurar funções, serviços e componentes do sistema operacional, consulte [instalar ou desinstalar funções, serviços de função ou recursos](/WindowsServerDocs/administration/server-manager/install-or-uninstall-roles-role-services-or-features.md)

## <a name="examples"></a>Exemplos

Para excluir a subchave de serviço **NewServ** do registro no computador local, digite:
```
sc.exe delete newserv
```

## <a name="additional-references"></a>Referências adicionais

- [Chave da sintaxe de linha de comando](command-line-syntax-key.md)
