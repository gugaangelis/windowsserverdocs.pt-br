---
title: mls_1 de FTP
description: 'Tópico de comandos do Windows para * * *- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 4738fd49-0e80-4bdf-a773-0f973db3a710 vhorne
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 6cf81018fa590d38e55778d60b0cb0e849ab83de
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59835697"
---
# <a name="ftp-mls1"></a>ftp: mls_1

>Aplica-se a: Windows Server (canal semestral), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Exibe uma lista abreviada de arquivos e subdiretórios em um diretório remoto.   
## <a name="syntax"></a>Sintaxe  
```  
mls <remoteFile>[ ] <LocalFile>  
```  
### <a name="parameters"></a>Parâmetros  
|Parâmetro|Descrição|  
|-------|--------|  
|<remoteFile>|Especifica o arquivo para o qual você deseja ver uma listagem.|  
|<LocalFile>|Especifica um arquivo local no qual armazenar a listagem.|  
## <a name="remarks"></a>Comentários  
-   Especificando *Arquivos_remotos*  
    Digite um hífen (**-**) para usar o diretório de trabalho atual no computador remoto.  
-   Especificando *LocalFile*  
    Digite um hífen (**-**) para exibir a listagem na tela.  
## <a name="BKMK_Examples"></a>Exemplos  
Exibir uma lista abreviada de arquivos e subdiretórios **dir1** e **dir2**.  
```  
mls dir1 dir2 -  
```  
Salvar uma lista abreviada de arquivos e subdiretórios **dir1** e **dir2** no arquivo local **Lista_pastas**  
```  
mls dir1 dir2 dirlist.txt   
```  
## <a name="additional-references"></a>Referências adicionais  
-   [Chave de sintaxe de linha de comando](command-line-syntax-key.md)  
