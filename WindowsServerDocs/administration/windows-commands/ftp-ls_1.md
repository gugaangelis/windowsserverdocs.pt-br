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
ms.openlocfilehash: c45e26f6578510837f190ae20e3140e619dc59cb
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59841757"
---
# <a name="ftp-ls1"></a>ftp: ls_1

>Aplica-se a: Windows Server (canal semestral), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012


>Aplica-se a: Windows Server (canal semestral), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Exibe uma lista abreviada de arquivos e subdiretórios do computador remoto.   
## <a name="syntax"></a>Sintaxe  
```  
ls [<remotedirectory>] [<LocalFile>]  
```  
### <a name="parameters"></a>Parâmetros  
|Parâmetro|Descrição|  
|-------|--------|  
|[<remotedirectory>]|Especifica o diretório para o qual você deseja ver uma listagem. Se nenhum diretório for especificado, o diretório de trabalho atual no computador remoto é usado.|  
|[<LocalFile>]|Especifica um arquivo local no qual armazenar a listagem. Se um arquivo local não for especificado, os resultados são exibidos na tela.|  
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
-   [Chave de sintaxe de linha de comando](command-line-syntax-key.md)  
