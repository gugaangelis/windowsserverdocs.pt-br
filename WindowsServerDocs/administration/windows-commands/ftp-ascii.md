---
title: ftp ascii
description: 'Tópico de comandos do Windows para ascii de ftp '
ms.custom: na
ms.prod: windows-server-threshold
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
ms.openlocfilehash: b7bcca3f29cec8ff5c30256dfd123acc7fbb804d
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59849187"
---
# <a name="ftp-ascii"></a>ftp: ascii

>Aplica-se a: Windows Server (canal semestral), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Define o tipo de transferência de arquivos como ASCII.   
## <a name="syntax"></a>Sintaxe  
```  
ascii  
```  
### <a name="parameters"></a>Parâmetros  
nenhuma  
## <a name="remarks"></a>Comentários  
-   O tipo de transferência de arquivo padrão é ASCII.  
-   No modo ASCII, as conversões de caracteres de e para o conjunto de caracteres padrão de rede são executadas. Por exemplo, os caracteres de final de linha são convertidos conforme necessário, com base no sistema operacional de destino.  
-   **FTP** suporta ASCII e tipos de transferência de arquivo de imagem binária. Use ASCII quando transferir arquivos de texto. Para obter mais informações sobre a transferência de arquivo binário, consulte **ftp: binário** nas referências adicionais.  
## <a name="BKMK_Examples"></a>Exemplos  
Defina o tipo de transferência de arquivo como ASCII.  
```  
ascii  
```  
## <a name="additional-references"></a>Referências adicionais  
-   [ftp: binary](ftp-binary.md)  
-   [Chave de sintaxe de linha de comando](command-line-syntax-key.md)  
