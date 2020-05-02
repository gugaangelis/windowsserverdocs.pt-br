---
title: ASCII de FTP
description: Tópico de referência para FTP ASCII
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 523be48e-eab0-4237-8fb5-ca222824f0b6 vhorne
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: aa33637f43ef8d26635f36b40dbd21cfe2bff42e
ms.sourcegitcommit: ab64dc83fca28039416c26226815502d0193500c
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 05/01/2020
ms.locfileid: "82725389"
---
# <a name="ftp-ascii"></a>FTP: ASCII

> Aplica-se a: Windows Server (canal semestral), Windows Server 2019, Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Define o tipo de transferência de arquivo como ASCII.   
## <a name="syntax"></a>Sintaxe  
```  
ascii  
```  
#### <a name="parameters"></a>Parâmetros  
none  
## <a name="remarks"></a>Comentários  
- O tipo de transferência de arquivo padrão é ASCII.  
- No modo ASCII, conversões de caracteres de e para o conjunto de caracteres padrão de rede são executadas. Por exemplo, caracteres de fim de linha são convertidos conforme necessário, com base no sistema operacional de destino.  
- o **FTP** dá suporte a tipos ASCII e de transferência de arquivos de imagem binária. Use ASCII ao transferir arquivos de texto. Para obter mais informações sobre a transferência de arquivos binários, consulte **ftp: Binary** em referências adicionais.  
  ## <a name="examples"></a>Exemplos  
  Defina o tipo de transferência de arquivo como ASCII.  
  ```  
  ascii  
  ```  
  ## <a name="additional-references"></a>Referências adicionais  
- [FTP: binário](ftp-binary.md)  
- - [Chave da sintaxe de linha de comando](command-line-syntax-key.md)  
