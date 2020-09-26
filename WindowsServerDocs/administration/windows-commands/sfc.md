---
title: sfc
description: Artigo de referência do comando SFC, que verifica e verifica a integridade de todos os arquivos protegidos do sistema e substitui as versões incorretas pelas versões corretas.
ms.topic: reference
ms.assetid: c58c25da-e028-42a6-9e10-973484a4b953
ms.author: lizross
author: eross-msft
manager: mtillman
ms.date: 10/16/2017
ms.openlocfilehash: 829c6e328ad0ea993e11cb5eb5d96d99f0d52476
ms.sourcegitcommit: e164aeffc01069b8f1f3248bf106fcdb7f64f894
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/26/2020
ms.locfileid: "91388936"
---
# <a name="sfc"></a>sfc

> Aplica-se a: Windows Server (canal semestral), Windows Server 2019, Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Verifica e verifica a integridade de todos os arquivos protegidos do sistema e substitui as versões incorretas pelas versões corretas. Se esse comando descobrir que um arquivo protegido foi substituído, ele recupera a versão correta do arquivo da pasta **systemroot\system32\dllcache** e, em seguida, substitui o arquivo incorreto.

> [!IMPORTANT]
> Você deve estar conectado como um membro do grupo Administradores para executar esse comando.

## <a name="syntax"></a>Sintaxe

```
sfc [/scannow] [/verifyonly] [/scanfile=<file>] [/verifyfile=<file>] [/offwindir=<offline windows directory> /offbootdir=<offline boot directory>]
```

### <a name="parameters"></a>Parâmetros

| Parâmetro | Descrição |
|--|--|
| /scannow | Verifica a integridade de todos os arquivos protegidos do sistema e repara os problemas quando possível. |
| /verifyonly | Verifica a integridade de todos os arquivos protegidos do sistema, sem executar reparos. |
| /scanfile `<file>` | Verifica a integridade do arquivo especificado (caminho completo e nome do arquivo) e tenta reparar os problemas se eles forem detectados. |
| /verifyfile `<file>` | Verifica a integridade do arquivo especificado (caminho completo e nome do arquivo), sem executar reparos. |
| /offwindir `<offline windows directory>` | Especifica o local do diretório offline do Windows, para reparo offline. |
| /offbootdir `<offline boot directory>` | Especifica o local do diretório de inicialização offline para o reparo offline. |
| /? | Exibe a ajuda no prompt de comando. |

## <a name="examples"></a>Exemplos

Para verificar o * arquivo dekernel32.dll*, digite:

```
sfc /verifyfile=c:\windows\system32\kernel32.dll
```

Para configurar o reparo offline do arquivo de *kernel32.dll* com um diretório de inicialização offline definido como * D: \* e um diretório offline do Windows definido como *D:\Windows*, digite:

```
sfc /scanfile=D:\windows\system32\kernel32.dll /offbootdir=D:\ /offwindir=d:\windows
```

## <a name="additional-references"></a>Referências adicionais

- [Chave da sintaxe de linha de comando](command-line-syntax-key.md)
