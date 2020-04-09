---
title: sfc
description: Tópico de comandos do Windows para o SFC, que verifica e verifica a integridade de todos os arquivos protegidos do sistema e substitui as versões incorretas pelas versões corretas.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: c58c25da-e028-42a6-9e10-973484a4b953
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 7663c8e3527995e2d3ec874dff6fa972e7e83ddd
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80834319"
---
# <a name="sfc"></a>sfc

>Aplicável a: Windows Server (canal semestral), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Verifica e verifica a integridade de todos os arquivos protegidos do sistema e substitui as versões incorretas pelas versões corretas.

Para obter exemplos de como usar esse comando, consulte [Exemplos](#BKMK_examples).

## <a name="syntax"></a>Sintaxe
```
sfc [/scannow] [/verifyonly] [/scanfile=<file>] [/verifyfile=<file>] [/offwindir=<offline windows directory> /offbootdir=<offline boot directory>]
```

#### <a name="parameters"></a>Parâmetros
|Parâmetro|Descrição|
|-------|--------|
|/scannow|Verifica a integridade de todos os arquivos protegidos do sistema e repara os problemas quando possível.|
|/verifyonly|Verifica a integridade de todos os arquivos protegidos do sistema. Nenhuma operação de reparo é executada.|
|/scanfile|Verifica a integridade do arquivo especificado e repara o arquivo se forem detectados problemas, quando possível.|
|arquivo de \<>|Caminho completo e nome de arquivo especificados|
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

## <a name="examples"></a><a name=BKMK_examples></a>Disso
Para verificar o **arquivo Kernel32. dll**, digite:
```
sfc /verifyfile=c:\windows\system32\kernel32.dll
```
Para configurar o reparo offline do arquivo **Kernel32. dll** com um diretório de inicialização offline definido como **d:** e o diretório offline do Windows definido como **d:\Windows**, digite:
```
sfc /scanfile=d:\windows\system32\kernel32.dll /offbootdir=d:\ /offwindir=d:\windows
```

## <a name="additional-references"></a>Referências adicionais
-   - [Chave da sintaxe de linha de comando](command-line-syntax-key.md)

