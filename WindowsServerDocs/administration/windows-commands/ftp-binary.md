---
title: binário de FTP
description: Tópico de referência para binário de FTP
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: ee925b4d-85d2-47b1-b7d6-3832b7ec5505 vhorne
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 60a3e84bf9256dd5c71dd4444b5939980eecc512
ms.sourcegitcommit: ab64dc83fca28039416c26226815502d0193500c
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 05/01/2020
ms.locfileid: "82725377"
---
# <a name="ftp-binary"></a>FTP: binário

> Aplica-se a: Windows Server (canal semestral), Windows Server 2019, Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Define o tipo de transferência de arquivo como Binary.   
## <a name="syntax"></a>Sintaxe  
```  
binary  
```  
#### <a name="parameters"></a>Parâmetros  
none  
## <a name="remarks-optional-section"></a>Comentários<optional section>  
o **FTP** dá suporte a tipos ASCII e de transferência de arquivos de imagem binária. Use Binary ao transferir arquivos executáveis. No modo binário, os arquivos são transferidos em unidades de um byte. Para obter mais informações sobre a transferência de arquivos ASCII, consulte **ftp: ASCII** em referências adicionais.  
## <a name="examples"></a>Exemplos  
Defina o tipo de transferência de arquivo como binário.  
```  
binary  
```  
## <a name="additional-references"></a>Referências adicionais  
-   [FTP: ASCII](ftp-ascii.md)  
-   - [Chave da sintaxe de linha de comando](command-line-syntax-key.md)  
