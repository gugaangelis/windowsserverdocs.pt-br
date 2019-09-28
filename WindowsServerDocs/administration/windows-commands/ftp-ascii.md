---
title: ASCII de FTP
description: 'Tópico de comandos do Windows para FTP ASCII '
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 523be48e-eab0-4237-8fb5-ca222824f0b6 vhorne
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: d5ae0064f9c1679bb8b386271f042d589b158c73
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71376624"
---
# <a name="ftp-ascii"></a>FTP: ASCII

>Aplica-se a: Windows Server (canal semestral), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Define o tipo de transferência de arquivo como ASCII.   
## <a name="syntax"></a>Sintaxe  
```  
ascii  
```  
### <a name="parameters"></a>Parâmetros  
nenhuma  
## <a name="remarks"></a>Comentários  
- O tipo de transferência de arquivo padrão é ASCII.  
- No modo ASCII, conversões de caracteres de e para o conjunto de caracteres padrão de rede são executadas. Por exemplo, caracteres de fim de linha são convertidos conforme necessário, com base no sistema operacional de destino.  
- o **FTP** dá suporte a tipos ASCII e de transferência de arquivos de imagem binária. Use ASCII ao transferir arquivos de texto. Para obter mais informações sobre a transferência de arquivos binários, consulte **ftp: Binary** em referências adicionais.  
  ## <a name="BKMK_Examples"></a>Disso  
  Defina o tipo de transferência de arquivo como ASCII.  
  ```  
  ascii  
  ```  
  ## <a name="additional-references"></a>Referências adicionais  
- [FTP: binário](ftp-binary.md)  
- [Chave da sintaxe de linha de comando](command-line-syntax-key.md)  
