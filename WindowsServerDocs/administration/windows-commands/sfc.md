---
title: sfc
description: 'Tópico de comandos do Windows para * * *- '
ms.custom: na
ms.prod: windows-server-threshold
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
ms.openlocfilehash: 1db0ab81c9469c88ddb64a367a9dc98a1fd9b70c
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59832387"
---
# <a name="sfc"></a>sfc

>Aplica-se a: Windows Server (canal semestral), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Examina e verifica a integridade do sistema protegido todos os arquivos e substitui as versões incorretas com as versões corretas.
Para obter exemplos de como usar esse comando, consulte [exemplos](#BKMK_examples).

## <a name="syntax"></a>Sintaxe
```
sfc [/scannow] [/verifyonly] [/scanfile=<file>] [/verifyfile=<file>] [/offwindir=<offline windows directory> /offbootdir=<offline boot directory>]
```

### <a name="parameters"></a>Parâmetros
|Parâmetro|Descrição|
|-------|--------|
|/scannow|Verifica a integridade de todos os arquivos protegidos do sistema e repara os arquivos com problemas quando possível.|
|/verifyonly|Verificações de integridade de todos os arquivos protegidos do sistema. Nenhuma operação de reparo é executada.|
|/scanfile|Verifica a integridade do arquivo especificado e repara o arquivo se problemas forem detectados, quando possível.|
|\<file>|Nome de arquivo e caminho completo especificado|
|/verifyfile|verifica a integridade do arquivo especificado. Nenhuma operação de reparo é executada.|
|/offwindir|Especifica o local do diretório do windows offline, para reparo offline.|
|/offbootdir|Especifica o local do diretório de inicialização offline off-line|
|/?|Exibe a ajuda no prompt de comando.|

## <a name="remarks"></a>Comentários
-   Você deve estar conectado como um membro do grupo Administradores para executar **sfc.exe**.
-   Se **sfc** detecta que um arquivo protegido foi substituído, ele recupera a versão correta do arquivo a partir de **systemroot\system32\dllcache** pasta e, em seguida, substitui o arquivo incorreto.
-   Há diferenças funcionais entre **sfc** no Windows Server 2003, Windows Server 2008 e Windows Server 2008 R2:
-   Para obter mais informações sobre **sfc** no Windows Server 2003, consulte [310747 do artigo](https://go.microsoft.com/fwlink/?LinkId=227069) na Base de dados de Conhecimento Microsoft.
-   Para obter mais informações sobre **sfc** no Windows Server 2008 e Windows Server 2008 R2, consulte [System File Checker](https://go.microsoft.com/fwlink/?LinkId=227071).

## <a name="BKMK_examples"></a>Exemplos
Para verificar se o **kernel32.dll arquivo**, tipo:
```
sfc /verifyfile=c:\windows\system32\kernel32.dll
```
Para reparo de instalação offline do **kernel32.dll** arquivo com um diretório de inicialização offline definido como **unidade d:** e o diretório offline do windows definida como **d:\windows**, tipo:
```
sfc /scanfile=d:\windows\system32\kernel32.dll /offbootdir=d:\ /offwindir=d:\windows
```

## <a name="additional-references"></a>Referências adicionais
-   [Chave de sintaxe de linha de comando](command-line-syntax-key.md)

