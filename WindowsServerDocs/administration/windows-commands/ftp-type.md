---
title: tipo de FTP
description: 'Tópico de comandos do Windows para * * * *- '
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 6e96dcd4-08f8-4e7b-90b7-1e1761fea4c7 vhorne
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: eb254d1c9b17ac6baadf6b84702d2812f1117a93
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71375901"
---
# <a name="ftp-type"></a>FTP: tipo

>Aplica-se a: Windows Server (canal semestral), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Define ou exibe o tipo de transferência de arquivo.   
## <a name="syntax"></a>Sintaxe  
```  
type [<typeName>]  
```  
### <a name="parameters"></a>Parâmetros  

|  Parâmetro   |            Descrição            |
|--------------|-----------------------------------|
| [<typeName>] | Especifica o tipo de transferência de arquivo. |

## <a name="remarks"></a>Comentários  
- Se *TypeName* não for especificado, o tipo atual será exibido.  
- o **FTP** dá suporte a dois tipos de transferência de arquivo, ASCII e binário.  
  O tipo de transferência de arquivo padrão é ASCII.  O comando **ASCII** deve ser usado ao transferir arquivos de texto. No modo ASCII, conversões de caracteres de e para o conjunto de caracteres padrão de rede são executadas. Por exemplo, caracteres de fim de linha são convertidos conforme necessário, com base no sistema operacional no destino.  
  O comando **Binary** deve ser usado ao transferir arquivos executáveis. No modo binário, o arquivo é movido em unidades de um byte.  
  ## <a name="BKMK_Examples"></a>Disso  
  Defina o tipo de transferência de arquivo como ASCII.  
  ```  
  type ascii  
  ```  
  Defina o tipo de arquivo de transferência como binário.  
  ```  
  type binary  
  ```  
  ## <a name="additional-references"></a>Referências adicionais  
- [Chave da sintaxe de linha de comando](command-line-syntax-key.md)  
