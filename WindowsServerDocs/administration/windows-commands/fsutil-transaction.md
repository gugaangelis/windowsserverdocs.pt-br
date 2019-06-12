---
ms.assetid: f2eefaaf-2817-4ac7-abac-d2b65fa971dc
title: Transação fsutil
ms.prod: windows-server-threshold
manager: dmoss
ms.author: toklima
author: toklima
ms.technology: storage
audience: IT Pro
ms.topic: article
ms.date: 10/16/2017
ms.openlocfilehash: c225c99919a2558559b1ec7a47b61d716e199a73
ms.sourcegitcommit: eaf071249b6eb6b1a758b38579a2d87710abfb54
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 05/31/2019
ms.locfileid: "66439005"
---
# <a name="fsutil-transaction"></a>Transação fsutil
>Aplica-se a: Windows Server (canal semestral), Windows Server 2016, Windows 10, Windows Server 2012 R2, Windows 8.1, Windows Server 2012, Windows 8, Windows Server 2008 R2, Windows 7, Windows 2008, Windows Vista

Gerencia transações de NTFS.

Para obter exemplos de como usar esse comando, consulte [exemplos](#BKMK_examples) .

## <a name="syntax"></a>Sintaxe

```
fsutil transaction [commit] <GUID>
fsutil transaction [fileinfo] <Filename>
fsutil transaction [list]
fsutil transaction [query] [{Files|All}] <GUID>
fsutil transaction [rollback] <GUID>
```

### <a name="parameters"></a>Parâmetros

| Parâmetro  |                                                                                                                                                     Descrição                                                                                                                                                     |
|------------|---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
|   confirmação   |                                                                                                                      Marca o fim de uma transação especificada implícita ou explícita bem-sucedida.                                                                                                                      |
|   <GUID>   |                                                                                                                               Especifica o valor GUID que representa uma transação.                                                                                                                               |
|  FileInfo  |                                                                                                                              Exibe informações de transação para o arquivo especificado.                                                                                                                               |
| <Filename> |                                                                                                                                         Especifica o caminho completo e nome de arquivo.                                                                                                                                          |
|    lista    |                                                                                                                                 Exibe uma lista de transações em execução.                                                                                                                                  |
|   consulta    | Exibe informações para a transação especificada.<br /><br />-Se **fsutil transação consultar arquivos** for especificado, as informações do arquivo serão exibidas somente para a transação especificada.<br />-Se **fsutil transação query** for especificado, todas as informações para a transação serão exibidas. |
|  reversão  |                                                                                                                                Reverte uma transação especificada para o início.                                                                                                                                 |

### <a name="remarks"></a>Comentários

-   O NTFS Transacional foi introduzido no Windows Server 2008.

### <a name="BKMK_examples"></a>Exemplos
Para exibir informações de transação para o arquivo c:\test.txt., digite:

```
fsutil transaction fileinfo c:\test.txt  
```

### <a name="additional-references"></a>Referências adicionais
[Chave da sintaxe de linha de comando](Command-Line-Syntax-Key.md)

[Fsutil](Fsutil.md)

[NTFS Transacional](https://go.microsoft.com/fwlink/?LinkID=165402)


