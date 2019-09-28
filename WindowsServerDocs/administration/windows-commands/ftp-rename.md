---
title: renomeação de FTP
description: 'Tópico de comandos do Windows para * * * *- '
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 977b7c95-6428-4980-80ec-79c3ae7e8c4d vhorne
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 977baa042a6b0d9c23db7cb398bee997c2049227
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71376017"
---
# <a name="ftp-rename"></a>FTP: renomear

>Aplica-se a: Windows Server (canal semestral), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

renomeia arquivos remotos.   
## <a name="syntax"></a>Sintaxe  
```  
rename <FileName> <NewFileName>  
```  
### <a name="parameters"></a>Parâmetros  

|   Parâmetro   |                 Descrição                 |
|---------------|---------------------------------------------|
|  <FileName>   | Especifica o arquivo que você deseja renomear. |
| <NewFileName> |        Especifica o novo nome de arquivo.         |

## <a name="BKMK_Examples"></a>Disso  
Renomeie o arquivo remoto **example. txt** para **example1. txt**  
```  
rename example.txt example1.txt  
```  
## <a name="additional-references"></a>Referências adicionais  
-   [Chave da sintaxe de linha de comando](command-line-syntax-key.md)  
