---
title: tipo de FTP
description: 'Tópico de comandos do Windows para * * *- '
ms.custom: na
ms.prod: windows-server-threshold
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
ms.openlocfilehash: 3524c7c772cdcd54a131d8a7e8c8714fad9ce563
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59858897"
---
# <a name="ftp-type"></a>FTP: tipo

>Aplica-se a: Windows Server (canal semestral), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Define ou exibe o tipo de transferência de arquivo.   
## <a name="syntax"></a>Sintaxe  
```  
type [<typeName>]  
```  
### <a name="parameters"></a>Parâmetros  
|Parâmetro|Descrição|  
|-------|--------|  
|[<typeName>]|Especifica o tipo de transferência de arquivo.|  
## <a name="remarks"></a>Comentários  
-   Se *typeName* não for especificado, o tipo atual é exibido.  
-   **FTP** dá suporte a dois tipos de transferência, ASCII e binário do arquivo.  
    O tipo de transferência de arquivo padrão é ASCII.  O **ascii** comando deve ser usado durante a transferência de arquivos de texto. No modo ASCII, as conversões de caracteres de e para o conjunto de caracteres padrão de rede são executadas. Por exemplo, os caracteres de final de linha são convertidos quando necessário, com base no sistema operacional no destino.  
    O **binário** comando deve ser usado durante a transferência de arquivos executáveis. No modo binário, o arquivo é movido em unidades de um byte.  
## <a name="BKMK_Examples"></a>Exemplos  
Defina o tipo de transferência de arquivo como ASCII.  
```  
type ascii  
```  
Defina a transferência de tipo de arquivo binário.  
```  
type binary  
```  
## <a name="additional-references"></a>Referências adicionais  
-   [Chave de sintaxe de linha de comando](command-line-syntax-key.md)  
