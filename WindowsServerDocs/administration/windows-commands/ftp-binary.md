---
title: binário de FTP
description: Tópico de comandos do Windows para o binário de ftp
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: ee925b4d-85d2-47b1-b7d6-3832b7ec5505 vhorne
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: cadd59bff3bd2acf5c6d700caef66ca5c871b523
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59821917"
---
# <a name="ftp-binary"></a>ftp: binary

>Aplica-se a: Windows Server (canal semestral), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Define o tipo de transferência de arquivo como binário.   
## <a name="syntax"></a>Sintaxe  
```  
binary  
```  
### <a name="parameters"></a>Parâmetros  
nenhuma  
## <a name="remarks-optional-section"></a>Comentários <optional section>  
**FTP** suporta ASCII e tipos de transferência de arquivo de imagem binária. Usar binário ao transferir arquivos executáveis. No modo binário, os arquivos são transferidos em unidades de um byte. Para obter mais informações sobre a transferência de arquivos ASCII, consulte **ftp: ascii** nas referências adicionais.  
## <a name="BKMK_Examples"></a>Exemplos  
Defina o tipo de transferência de arquivo binário.  
```  
binary  
```  
## <a name="additional-references"></a>Referências adicionais  
-   [ftp: ascii](ftp-ascii.md)  
-   [Chave de sintaxe de linha de comando](command-line-syntax-key.md)  
