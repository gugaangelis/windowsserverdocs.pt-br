---
title: renomeação de FTP
description: Tópico de comandos do Windows para * * * *-
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 977b7c95-6428-4980-80ec-79c3ae7e8c4d vhorne
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: bbe159f2833ce52921b46e46881a1d7aed8c5df8
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80843019"
---
# <a name="ftp-rename"></a>FTP: renomear

>Aplicável a: Windows Server (canal semestral), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

renomeia arquivos remotos.   
## <a name="syntax"></a>Sintaxe  
```  
rename <FileName> <NewFileName>  
```  
#### <a name="parameters"></a>Parâmetros  

|   Parâmetro   |                 Descrição                 |
|---------------|---------------------------------------------|
|  <FileName>   | Especifica o arquivo que você deseja renomear. |
| <NewFileName> |        Especifica o novo nome de arquivo.         |

## <a name="examples"></a><a name=BKMK_Examples></a>Disso  
Renomeie o arquivo remoto **example. txt** para **example1. txt**  
```  
rename example.txt example1.txt  
```  
## <a name="additional-references"></a>Referências adicionais  
-   - [Chave da sintaxe de linha de comando](command-line-syntax-key.md)  
