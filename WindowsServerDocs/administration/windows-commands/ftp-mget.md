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
ms.openlocfilehash: 1160ec742dde318141da720bd35b7d60ab805bb1
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59888417"
---
# <a name="ftp-mget"></a>FTP: mget

>Aplica-se a: Windows Server (canal semestral), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Tipo de transferência copia arquivos remotos para o computador local usando o arquivo atual.   
## <a name="syntax"></a>Sintaxe  
```  
mget <remoteFile>[ ]  
```  
### <a name="parameters"></a>Parâmetros  
|Parâmetro|Descrição|  
|-------|--------|  
|<remoteFile>|Especifica os arquivos remotos a serem copiados para o computador local.|  
## <a name="BKMK_Examples"></a>Exemplos  
copiar arquivos remotos **a.exe** e **b.exe** no computador local usando o tipo de transferência de arquivo atual.  
```  
mget a.exe b.exe  
```  
## <a name="additional-references"></a>Referências adicionais  
-   [ftp: ascii](ftp-ascii.md)  
-   [ftp: binary](ftp-binary.md)  
-   [Chave de sintaxe de linha de comando](command-line-syntax-key.md)  
