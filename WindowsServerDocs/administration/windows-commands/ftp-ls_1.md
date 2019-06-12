---
title: ls_1 de FTP
description: 'Tópico de comandos do Windows para * * *- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 5e03c7db-1e2b-419c-acb2-8a68f3db9615 vhorne
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 6abf8466f90ac29846f2e1ee7d305e7e4280231e
ms.sourcegitcommit: eaf071249b6eb6b1a758b38579a2d87710abfb54
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 05/31/2019
ms.locfileid: "66438637"
---
# <a name="ftp-ls1"></a>ftp: ls_1

> Aplica-se a: Windows Server (canal semestral), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012
> 
> 
> Aplica-se a: Windows Server (canal semestral), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Exibe uma lista abreviada de arquivos e subdiretórios do computador remoto.   
## <a name="syntax"></a>Sintaxe  
```  
ls [<remotedirectory>] [<LocalFile>]  
```  
### <a name="parameters"></a>Parâmetros  

|      Parâmetro      |                                                                       Descrição                                                                        |
|---------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------|
| [<remotedirectory>] | Especifica o diretório para o qual você deseja ver uma listagem. Se nenhum diretório for especificado, o diretório de trabalho atual no computador remoto é usado. |
|    [<LocalFile>]    |               Especifica um arquivo local no qual armazenar a listagem. Se um arquivo local não for especificado, os resultados são exibidos na tela.               |

## <a name="BKMK_Examples"></a>Exemplos  
Exiba uma lista abreviada de arquivos e subdiretórios do computador remoto.  
```  
ls  
```  
Obter uma listagem de diretório abreviada de **dir1** no computador remoto e salve-o em um arquivo local chamado **Lista_pastas**  
```  
ls dir1 dirlist.txt   
```  
## <a name="additional-references"></a>Referências adicionais  
-   [Chave da sintaxe de linha de comando](command-line-syntax-key.md)  
