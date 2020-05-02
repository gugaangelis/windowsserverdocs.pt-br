---
title: ls_1 FTP
description: Tópico de referência para * * * *-
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 5e03c7db-1e2b-419c-acb2-8a68f3db9615 vhorne
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 0dd661742288bdd43299f379f4eb04016b2ac799
ms.sourcegitcommit: ab64dc83fca28039416c26226815502d0193500c
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 05/01/2020
ms.locfileid: "82725247"
---
# <a name="ftp-ls_1"></a>FTP: ls_1

> Aplica-se a: Windows Server (canal semestral), Windows Server 2019, Windows Server 2016, Windows Server 2012 R2, Windows Server 2012
> 
> 
> Aplica-se a: Windows Server (canal semestral), Windows Server 2019, Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Exibe uma lista abreviada de arquivos e subdiretórios do computador remoto.   
## <a name="syntax"></a>Sintaxe  
```  
ls [<remotedirectory>] [<LocalFile>]  
```  
#### <a name="parameters"></a>Parâmetros  

|      Parâmetro      |                                                                       Descrição                                                                        |
|---------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------|
| [<remotedirectory>] | Especifica o diretório para o qual você deseja ver uma listagem. Se nenhum diretório for especificado, o diretório de trabalho atual no computador remoto será usado. |
|    [<LocalFile>]    |               Especifica um arquivo local no qual armazenar a listagem. Se um arquivo local não for especificado, os resultados serão exibidos na tela.               |

## <a name="examples"></a>Exemplos  
Exibe uma lista abreviada de arquivos e subdiretórios do computador remoto.  
```  
ls  
```  
Obtenha uma listagem de diretório abreviada de **dir1** no computador remoto e salve-o em um arquivo local chamado **DirList. txt**  
```  
ls dir1 dirlist.txt   
```  
## <a name="additional-references"></a>Referências adicionais  
-   - [Chave da sintaxe de linha de comando](command-line-syntax-key.md)  
