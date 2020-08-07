---
title: Update-ServerFiles
description: Artigo de referência para Update-ServerFiles, que atualiza os arquivos na pasta compartilhada reminsd usando os arquivos mais recentes que são armazenados na pasta%Windir%\System32\RemInst do servidor.
ms.topic: article
ms.assetid: 23aa79df-38c6-401e-91bd-cd23811b30b4
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: f60e5b5c5208d5718a287fd2d012368d13fad9f9
ms.sourcegitcommit: 53d526bfeddb89d28af44210a23ba417f6ce0ecf
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/06/2020
ms.locfileid: "87881461"
---
# <a name="update-serverfiles"></a>Update-ServerFiles

Atualiza os arquivos na pasta compartilhada REMINST usando os arquivos mais recentes que são armazenados na pasta%Windir%\System32\RemInst do servidor. Para garantir a validade da instalação dos serviços de implantação do Windows, você deve executar esse comando uma vez após cada atualização do servidor, service pack instalação ou atualização para os arquivos dos serviços de implantação do Windows.

## <a name="syntax"></a>Sintaxe

```
WDSUTIL [Options] /Update-ServerFiles [/Server:<Server name>]
```

### <a name="parameters"></a>Parâmetros

|Parâmetro|Descrição|
|---------|-----------|
|[/Server:\<Server name>]|Especifica o nome do servidor. Pode ser o nome NetBIOS ou o FQDN (nome de domínio totalmente qualificado). Se nenhum nome de servidor for especificado, o servidor local será usado.|

## <a name="examples"></a>Exemplos

Para atualizar os arquivos, digite um dos seguintes:
```
WDSUTIL /Update-ServerFiles
WDSUTIL /Verbose /Progress /Update-ServerFiles /Server:MyWDSServer
```

## <a name="additional-references"></a>Referências adicionais

- [Chave da sintaxe de linha de comando](command-line-syntax-key.md)