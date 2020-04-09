---
title: Dir. FTP
description: Tópico de comandos do Windows para dir. FTP
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: a29a92a5-7b79-4e6e-95cf-2ccb38bb6fb2 vhorne
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 64f227ea9806f169c2df1698382cfce6e7ac3257
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80843569"
---
# <a name="ftp-dir"></a>FTP: dir

>Aplicável a: Windows Server (canal semestral), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

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
## <a name="examples"></a><a name=BKMK_Examples></a>Disso  
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
