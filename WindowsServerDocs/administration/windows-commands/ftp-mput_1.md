---
title: ftp mput_1
description: 'Tópico de comandos do Windows para * * *- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 980f15e7-7cf1-4813-9946-a8cc4edfb198 vhorne
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 99b938618deb2d1e779fd20c504c01a13a2d3f8a
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59868157"
---
# <a name="ftp-mput1"></a>ftp: mput_1

>Aplica-se a: Windows Server (canal semestral), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Tipo de transferência copia arquivos locais para o computador remoto usando o arquivo atual.   
## <a name="syntax"></a>Sintaxe  
```  
mput <LocalFile>[ ]  
```  
### <a name="parameters"></a>Parâmetros  
|Parâmetro|Descrição|  
|-------|--------|  
|<LocalFile>|Especifica o arquivo local para copiar para o computador remoto.|  
## <a name="BKMK_Examples"></a>Exemplos  
cópia **Program1.exe** e **Program2.exe** ao computador remoto usando o tipo de transferência de arquivo atual.  
```  
mput Program1.exe Program2.exe  
```  
## <a name="additional-references"></a>Referências adicionais  
-   [ftp: ascii](ftp-ascii.md)  
-   [ftp: binary](ftp-binary.md)  
-   [Chave de sintaxe de linha de comando](command-line-syntax-key.md)  
