---
ms.assetid: f2eefaaf-2817-4ac7-abac-d2b65fa971dc
title: Transação fsutil
ms.prod: windows-server
manager: dmoss
ms.author: toklima
author: toklima
ms.technology: storage
audience: IT Pro
ms.topic: article
ms.date: 10/16/2017
ms.openlocfilehash: aa6692dbb8af1ec832650971c6723c060fc2cd56
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80844009"
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

#### <a name="parameters"></a>Parâmetros

| Parâmetro  |                                                                                                                                                     Descrição                                                                                                                                                     |
|------------|---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
|   confirmação   |                                                                                                                      Marca o fim de uma transação especificada implícita ou explícita bem-sucedida.                                                                                                                      |
|   <GUID>   |                                                                                                                               Especifica o valor GUID que representa uma transação.                                                                                                                               |
|  FileInfo  |                                                                                                                              Exibe informações de transação para o arquivo especificado.                                                                                                                               |
| <Filename> |                                                                                                                                         Especifica o caminho completo e nome de arquivo.                                                                                                                                          |
|    {1&gt;list&lt;1}    |                                                                                                                                 Exibe uma lista de transações em execução.                                                                                                                                  |
|   query    | Exibe informações para a transação especificada.<p>-Se **fsutil transação consultar arquivos** for especificado, as informações do arquivo serão exibidas somente para a transação especificada.<br />-Se **fsutil transação query** for especificado, todas as informações para a transação serão exibidas. |
|  reverter  |                                                                                                                                Reverte uma transação especificada para o início.                                                                                                                                 |

### <a name="remarks"></a>Comentários

-   O NTFS Transacional foi introduzido no Windows Server 2008.

### <a name="examples"></a><a name="BKMK_examples"></a>Disso
Para exibir informações de transação para o arquivo c:\test.txt, digite:

```
fsutil transaction fileinfo c:\test.txt  
```

### <a name="additional-references"></a>Referências adicionais
- [Chave da sintaxe de linha de comando](command-line-syntax-key.md)

[Fsutil](Fsutil.md)

[NTFS Transacional](https://go.microsoft.com/fwlink/?LinkID=165402)


