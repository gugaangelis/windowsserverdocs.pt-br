---
title: ftp recv
description: 'Tópico de comandos do Windows para * * *- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: f249ce61-247d-421b-9b93-48bce5108800 vhorne
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 5bfd68dcb745ebf7ef239883aa1c5322241b32df
ms.sourcegitcommit: eaf071249b6eb6b1a758b38579a2d87710abfb54
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 05/31/2019
ms.locfileid: "66438436"
---
# <a name="ftp-recv"></a>ftp: recv

>Aplica-se a: Windows Server (canal semestral), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Copia um arquivo remoto no computador local usando o tipo de transferência de arquivo atual.   
## <a name="syntax"></a>Sintaxe  
```  
recv <remoteFile> [<LocalFile>]  
```  
### <a name="parameters"></a>Parâmetros  

|   Parâmetro   |                   Descrição                    |
|---------------|--------------------------------------------------|
| <remoteFile>  |        Especifica o arquivo remoto a ser copiado.        |
| [<LocalFile>] | Especifica o nome a ser usado no computador local. |

## <a name="remarks"></a>Comentários  
- O **recv** comando é idêntico de **obter** comando.  
- Se *LocalFile* não for especificado, o arquivo é fornecido o *Arquivo_remoto* nome.  
  ## <a name="BKMK_Examples"></a>Exemplos  
  cópia **Test. txt** no computador local usando o tipo de transferência de arquivo atual.  
  ```  
  recv test.txt  
  ```  
  cópia **Test. txt** no computador local como **test1.txt** tipo de transferência usando o arquivo atual.  
  ```  
  recv test.txt test1.txt  
  ```  
  ## <a name="additional-references"></a>Referências adicionais  
- [ftp: ascii](ftp-ascii.md)  
- [ftp: binary](ftp-binary.md)  
- [ftp: get](ftp-get.md)  
- [Chave da sintaxe de linha de comando](command-line-syntax-key.md)  
