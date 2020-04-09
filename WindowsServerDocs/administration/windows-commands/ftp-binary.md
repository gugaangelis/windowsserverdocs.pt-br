---
title: binário de FTP
description: Tópico de comandos do Windows para FTP Binary
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: ee925b4d-85d2-47b1-b7d6-3832b7ec5505 vhorne
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 20b2f72517826576cfee643eda0c54063b162c94
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80843719"
---
# <a name="ftp-binary"></a>FTP: binário

>Aplicável a: Windows Server (canal semestral), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Define o tipo de transferência de arquivo como Binary.   
## <a name="syntax"></a>Sintaxe  
```  
binary  
```  
#### <a name="parameters"></a>Parâmetros  
none  
## <a name="remarks-optional-section"></a>Comentários <optional section>  
o **FTP** dá suporte a tipos ASCII e de transferência de arquivos de imagem binária. Use Binary ao transferir arquivos executáveis. No modo binário, os arquivos são transferidos em unidades de um byte. Para obter mais informações sobre a transferência de arquivos ASCII, consulte **ftp: ASCII** em referências adicionais.  
## <a name="examples"></a><a name=BKMK_Examples></a>Disso  
Defina o tipo de transferência de arquivo como binário.  
```  
binary  
```  
## <a name="additional-references"></a>Referências adicionais  
-   [FTP: ASCII](ftp-ascii.md)  
-   - [Chave da sintaxe de linha de comando](command-line-syntax-key.md)  
