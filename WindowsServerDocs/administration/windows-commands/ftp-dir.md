---
title: dir de FTP
description: Tópico de comandos do Windows para o diretório de ftp
ms.custom: na
ms.prod: windows-server-threshold
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
ms.openlocfilehash: 78ac8ac5e9fc4894f55401bb234aa98de981adf7
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59835247"
---
# <a name="ftp-dir"></a>ftp: dir

>Aplica-se a: Windows Server (canal semestral), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Exibe uma lista de arquivos do diretório e subdiretórios em um computador remoto.   
## <a name="syntax"></a>Sintaxe  
```  
dir [<remotedirectory>] [<LocalFile>]  
```  
### <a name="parameters"></a>Parâmetros  
|Parâmetro|Descrição|  
|-------|--------|  
|[<remotedirectory>]|Especifica o diretório para o qual você deseja ver uma listagem. Se nenhum diretório for especificado, o diretório de trabalho atual no computador remoto é usado.|  
|[<LocalFile>]|Especifica um arquivo local no qual armazenar a listagem de diretórios. Se um arquivo local não for especificado, os resultados são exibidos na tela.|  
## <a name="BKMK_Examples"></a>Exemplos  
Exibir um listagem de diretórios **dir1** no computador remoto.  
```  
dir dir1  
```  
Salvar uma lista do diretório atual no computador remoto no arquivo local **Lista_pastas**.  
```  
dir . dirlist.txt  
```  
## <a name="additional-references"></a>Referências adicionais  
-   [Chave de sintaxe de linha de comando](command-line-syntax-key.md)  
