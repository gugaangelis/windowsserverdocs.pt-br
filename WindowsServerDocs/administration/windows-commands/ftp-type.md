---
title: tipo de FTP
description: Tópico de referência para * * * *-
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 6e96dcd4-08f8-4e7b-90b7-1e1761fea4c7 vhorne
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 5531da30118914599ed0f85bfd10bd02ae89ffcf
ms.sourcegitcommit: ab64dc83fca28039416c26226815502d0193500c
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 05/01/2020
ms.locfileid: "82725084"
---
# <a name="ftp-type"></a>FTP: tipo

> Aplica-se a: Windows Server (canal semestral), Windows Server 2019, Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Define ou exibe o tipo de transferência de arquivo.   
## <a name="syntax"></a>Sintaxe  
```  
type [<typeName>]  
```  
#### <a name="parameters"></a>Parâmetros  

|  Parâmetro   |            Descrição            |
|--------------|-----------------------------------|
| [<typeName>] | Especifica o tipo de transferência de arquivo. |

## <a name="remarks"></a>Comentários  
- Se *TypeName* não for especificado, o tipo atual será exibido.  
- o **FTP** dá suporte a dois tipos de transferência de arquivo, ASCII e binário.  
  O tipo de transferência de arquivo padrão é ASCII.  O comando **ASCII** deve ser usado ao transferir arquivos de texto. No modo ASCII, conversões de caracteres de e para o conjunto de caracteres padrão de rede são executadas. Por exemplo, caracteres de fim de linha são convertidos conforme necessário, com base no sistema operacional no destino.  
  O comando **Binary** deve ser usado ao transferir arquivos executáveis. No modo binário, o arquivo é movido em unidades de um byte.  
  ## <a name="examples"></a>Exemplos  
  Defina o tipo de transferência de arquivo como ASCII.  
  ```  
  type ascii  
  ```  
  Defina o tipo de arquivo de transferência como binário.  
  ```  
  type binary  
  ```  
  ## <a name="additional-references"></a>Referências adicionais  
- - [Chave da sintaxe de linha de comando](command-line-syntax-key.md)  
