---
title: binário de FTP
description: Tópico de comandos do Windows para FTP Binary
ms.custom: na
ms.prod: windows-server
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
ms.openlocfilehash: 48579523f44232dec3357a20e8082050cc5175e6
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71376584"
---
# <a name="ftp-binary"></a>FTP: binário

>Aplica-se a: Windows Server (canal semestral), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Define o tipo de transferência de arquivo como Binary.   
## <a name="syntax"></a>Sintaxe  
```  
binary  
```  
### <a name="parameters"></a>Parâmetros  
nenhuma  
## <a name="remarks-optional-section"></a>Comentários <optional section>  
o **FTP** dá suporte a tipos ASCII e de transferência de arquivos de imagem binária. Use Binary ao transferir arquivos executáveis. No modo binário, os arquivos são transferidos em unidades de um byte. Para obter mais informações sobre a transferência de arquivos ASCII, consulte **ftp: ASCII** em referências adicionais.  
## <a name="BKMK_Examples"></a>Disso  
Defina o tipo de transferência de arquivo como binário.  
```  
binary  
```  
## <a name="additional-references"></a>Referências adicionais  
-   [FTP: ASCII](ftp-ascii.md)  
-   [Chave da sintaxe de linha de comando](command-line-syntax-key.md)  
