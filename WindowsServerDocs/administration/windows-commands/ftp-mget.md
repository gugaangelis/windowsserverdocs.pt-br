---
title: FTP mget
description: 'Tópico de comandos do Windows para * * *- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 6c85ae96-ec51-48a9-a227-7f02c7332c69 vhorne
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: e43bf8b6e7067a31b3ec51336b0b43845ab88f63
ms.sourcegitcommit: eaf071249b6eb6b1a758b38579a2d87710abfb54
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 05/31/2019
ms.locfileid: "66438604"
---
# <a name="ftp-mget"></a>FTP: mget

>Aplica-se a: Windows Server (canal semestral), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Tipo de transferência copia arquivos remotos para o computador local usando o arquivo atual.   
## <a name="syntax"></a>Sintaxe  
```  
mget <remoteFile>[ ]  
```  
### <a name="parameters"></a>Parâmetros  

|  Parâmetro   |                        Descrição                        |
|--------------|-----------------------------------------------------------|
| <remoteFile> | Especifica os arquivos remotos a serem copiados para o computador local. |

## <a name="BKMK_Examples"></a>Exemplos  
copiar arquivos remotos **a.exe** e **b.exe** no computador local usando o tipo de transferência de arquivo atual.  
```  
mget a.exe b.exe  
```  
## <a name="additional-references"></a>Referências adicionais  
-   [ftp: ascii](ftp-ascii.md)  
-   [ftp: binary](ftp-binary.md)  
-   [Chave da sintaxe de linha de comando](command-line-syntax-key.md)  
