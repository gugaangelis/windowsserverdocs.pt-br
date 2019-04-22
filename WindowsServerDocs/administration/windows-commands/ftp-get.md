---
title: get de FTP
description: Obter tópico de comandos do Windows para o ftp
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: d70355c4-58ef-43e0-916b-c7ecf77e6ee4 vhorne
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 798317f3921cd0e5ff12b69b972e2ea423fa6b3f
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59816727"
---
# <a name="ftp-get"></a>ftp: get

>Aplica-se a: Windows Server (canal semestral), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Copia um arquivo remoto no computador local usando o tipo de transferência de arquivo atual.   
## <a name="syntax"></a>Sintaxe  
```  
get <remoteFile> [<LocalFile>]  
```  
### <a name="parameters"></a>Parâmetros  
|Parâmetro|Descrição|  
|-------|--------|  
|<remoteFile>|Especifica o arquivo remoto a ser copiado.|  
|[<LocalFile>]|Especifica o nome do arquivo a ser usado no computador local. Se *LocalFile* não for especificado, o arquivo é fornecido o *Arquivo_remoto* nome.|  
## <a name="remarks"></a>Comentários  
O **Obtenha** comando é idêntico de **recv** comando.  
## <a name="BKMK_Examples"></a>Exemplos  
cópia **Test. txt** no computador local usando o tipo de transferência de arquivo atual.  
```  
get test.txt  
```  
cópia **Test. txt** no computador local como **test1.txt** tipo de transferência usando o arquivo atual.  
```  
Get test.txt test1.txt  
```  
## <a name="additional-references"></a>Referências adicionais  
-   [ftp: ascii](ftp-ascii.md)  
-   [ftp: binary](ftp-binary.md)  
-   [Chave de sintaxe de linha de comando](command-line-syntax-key.md)  
