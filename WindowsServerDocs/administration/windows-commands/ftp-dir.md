---
title: Dir. FTP
description: Tópico de referência para dir. FTP
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: a29a92a5-7b79-4e6e-95cf-2ccb38bb6fb2 vhorne
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 0f2b9c610abe50bf662439a84d9bcbcb17b1a5bb
ms.sourcegitcommit: ab64dc83fca28039416c26226815502d0193500c
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 05/01/2020
ms.locfileid: "82725312"
---
# <a name="ftp-dir"></a>FTP: dir

> Aplica-se a: Windows Server (canal semestral), Windows Server 2019, Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Exibe uma lista de arquivos e subdiretórios do diretório em um computador remoto.   
## <a name="syntax"></a>Sintaxe  
```  
dir [<remotedirectory>] [<LocalFile>]  
```  
#### <a name="parameters"></a>Parâmetros  
|Parâmetro|Descrição|  
|-------|--------|  
|[<remotedirectory>]|Especifica o diretório para o qual você deseja ver uma listagem. Se nenhum diretório for especificado, o diretório de trabalho atual no computador remoto será usado.|  
|[<LocalFile>]|Especifica um arquivo local no qual armazenar a listagem de diretório. Se um arquivo local não for especificado, os resultados serão exibidos na tela.|  
## <a name="examples"></a>Exemplos  
Exiba uma listagem de diretório para **dir1** no computador remoto.  
```  
dir dir1  
```  
Salve uma lista do diretório atual no computador remoto no arquivo local **DirList. txt**.  
```  
dir . dirlist.txt  
```  
## <a name="additional-references"></a>Referências adicionais  
-   - [Chave da sintaxe de linha de comando](command-line-syntax-key.md)  
