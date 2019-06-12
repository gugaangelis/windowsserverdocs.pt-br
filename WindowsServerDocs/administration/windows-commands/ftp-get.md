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
ms.openlocfilehash: 28961ccf0ae04b52586728f9c68a9b2ca3e69b1d
ms.sourcegitcommit: eaf071249b6eb6b1a758b38579a2d87710abfb54
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 05/31/2019
ms.locfileid: "66438773"
---
# <a name="ftp-get"></a>ftp: get

>Aplica-se a: Windows Server (canal semestral), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Copia um arquivo remoto no computador local usando o tipo de transferência de arquivo atual.   
## <a name="syntax"></a>Sintaxe  
```  
get <remoteFile> [<LocalFile>]  
```  
### <a name="parameters"></a>Parâmetros  

|   Parâmetro   |                                                              Descrição                                                               |
|---------------|----------------------------------------------------------------------------------------------------------------------------------------|
| <remoteFile>  |                                                   Especifica o arquivo remoto a ser copiado.                                                   |
| [<LocalFile>] | Especifica o nome do arquivo a ser usado no computador local. Se *LocalFile* não for especificado, o arquivo é fornecido o *Arquivo_remoto* nome. |

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
-   [Chave da sintaxe de linha de comando](command-line-syntax-key.md)  
