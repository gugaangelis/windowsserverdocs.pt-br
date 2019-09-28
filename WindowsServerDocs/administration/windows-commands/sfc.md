---
title: sfc
description: 'Tópico de comandos do Windows para * * * *- '
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: c58c25da-e028-42a6-9e10-973484a4b953
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: ca470d519e9f3425c0c58fd0070a76c7038ec9b5
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71384019"
---
# <a name="sfc"></a>sfc

>Aplica-se a: Windows Server (canal semestral), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Verifica e verifica a integridade de todos os arquivos protegidos do sistema e substitui as versões incorretas pelas versões corretas.
Para obter exemplos de como usar esse comando, consulte [exemplos](#BKMK_examples).

## <a name="syntax"></a>Sintaxe
```
sfc [/scannow] [/verifyonly] [/scanfile=<file>] [/verifyfile=<file>] [/offwindir=<offline windows directory> /offbootdir=<offline boot directory>]
```

### <a name="parameters"></a>Parâmetros
|Parâmetro|Descrição|
|-------|--------|
|/scannow|Verifica a integridade de todos os arquivos protegidos do sistema e repara os problemas quando possível.|
|/verifyonly|Verifica a integridade de todos os arquivos protegidos do sistema. Nenhuma operação de reparo é executada.|
|/scanfile|Verifica a integridade do arquivo especificado e repara o arquivo se forem detectados problemas, quando possível.|
|\<file>|Caminho completo e nome de arquivo especificados|
|/verifyfile|verifica a integridade do arquivo especificado. Nenhuma operação de reparo é executada.|
|/offwindir|Especifica o local do diretório offline do Windows, para reparo offline.|
|/offbootdir|Especifica o local do diretório de inicialização offline para offline|
|/?|Exibe a ajuda no prompt de comando.|

## <a name="remarks"></a>Comentários
-   Você deve estar conectado como um membro do grupo Administradores para executar o **Sfc. exe**.
-   Se o **Sfc** descobrir que um arquivo protegido foi substituído, ele recupera a versão correta do arquivo da pasta **systemroot\system32\dllcache** e, em seguida, substitui o arquivo incorreto.
-   Há diferenças funcionais entre o **Sfc** no windows Server 2003, no windows Server 2008 e no windows Server 2008 R2:
-   para obter mais informações sobre o **Sfc** no Windows Server 2003, consulte o [artigo 310747](https://go.microsoft.com/fwlink/?LinkId=227069) na base de dados de conhecimento Microsoft.
-   para obter mais informações sobre o **Sfc** no windows Server 2008 e o windows Server 2008 R2, consulte [Verificador de arquivos do sistema](https://go.microsoft.com/fwlink/?LinkId=227071).

## <a name="BKMK_examples"></a>Disso
Para verificar o **arquivo Kernel32. dll**, digite:
```
sfc /verifyfile=c:\windows\system32\kernel32.dll
```
Para configurar o reparo offline do arquivo **Kernel32. dll** com um diretório de inicialização offline definido como **d:** e o diretório offline do Windows definido como **d:\Windows**, digite:
```
sfc /scanfile=d:\windows\system32\kernel32.dll /offbootdir=d:\ /offwindir=d:\windows
```

## <a name="additional-references"></a>Referências adicionais
-   [Chave da sintaxe de linha de comando](command-line-syntax-key.md)

