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
ms.openlocfilehash: 18088bcedd077d5c8052bca91c648e2719304a78
ms.sourcegitcommit: ab64dc83fca28039416c26226815502d0193500c
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 05/01/2020
ms.locfileid: "82720088"
---
# <a name="fsutil-transaction"></a>Transação fsutil
> Aplica-se a: Windows Server (canal semestral), Windows Server 2019, Windows Server 2016, Windows 10, Windows Server 2012 R2, Windows 8.1, Windows Server 2012, Windows 8, Windows Server 2008 R2, Windows 7, Windows 2008, Windows Vista

Gerencia transações de NTFS.



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
|   confirmar   |                                                                                                                      Marca o fim de uma transação especificada implícita ou explícita bem-sucedida.                                                                                                                      |
|   <GUID>   |                                                                                                                               Especifica o valor GUID que representa uma transação.                                                                                                                               |
|  FileInfo  |                                                                                                                              Exibe informações de transação para o arquivo especificado.                                                                                                                               |
| <Filename> |                                                                                                                                         Especifica o caminho completo e nome de arquivo.                                                                                                                                          |
|    list    |                                                                                                                                 Exibe uma lista de transações em execução.                                                                                                                                  |
|   Consulta    | Exibe informações para a transação especificada.<p>-Se **fsutil transação consultar arquivos** for especificado, as informações do arquivo serão exibidas somente para a transação especificada.<br />-Se **fsutil transação query** for especificado, todas as informações para a transação serão exibidas. |
|  reversão  |                                                                                                                                Reverte uma transação especificada para o início.                                                                                                                                 |

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

[NTFS transacional](https://go.microsoft.com/fwlink/?LinkID=165402)


