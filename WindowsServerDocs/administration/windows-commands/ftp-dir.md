---
title: Dir. FTP
description: Tópico de comandos do Windows para dir. FTP
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: a29a92a5-7b79-4e6e-95cf-2ccb38bb6fb2 vhorne
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: c47971c52135d79ce62f935bfed981f6eefcecaa
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71376436"
---
# <a name="ftp-dir"></a>FTP: dir

>Aplica-se a: Windows Server (canal semestral), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Exibe uma lista de arquivos e subdiretórios do diretório em um computador remoto.   
## <a name="syntax"></a>Sintaxe  
```  
dir [<remotedirectory>] [<LocalFile>]  
```  
### <a name="parameters"></a>Parâmetros  
|Parâmetro|Descrição|  
|-------|--------|  
|[<remotedirectory>]|Especifica o diretório para o qual você deseja ver uma listagem. Se nenhum diretório for especificado, o diretório de trabalho atual no computador remoto será usado.|  
|[<LocalFile>]|Especifica um arquivo local no qual armazenar a listagem de diretório. Se um arquivo local não for especificado, os resultados serão exibidos na tela.|  
## <a name="BKMK_Examples"></a>Disso  
Exiba uma listagem de diretório para **dir1** no computador remoto.  
```  
dir dir1  
```  
Salve uma lista do diretório atual no computador remoto no arquivo local **DirList. txt**.  
```  
dir . dirlist.txt  
```  
## <a name="additional-references"></a>Referências adicionais  
-   [Chave da sintaxe de linha de comando](command-line-syntax-key.md)  
