---
title: renomeação de FTP
description: Tópico de referência para * * * *-
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 977b7c95-6428-4980-80ec-79c3ae7e8c4d vhorne
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 5dc5006c82df8417a8652a9c0ba20f7f1a002e7f
ms.sourcegitcommit: ab64dc83fca28039416c26226815502d0193500c
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 05/01/2020
ms.locfileid: "82725107"
---
# <a name="ftp-rename"></a>FTP: renomear

> Aplica-se a: Windows Server (canal semestral), Windows Server 2019, Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

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

## <a name="examples"></a>Exemplos  
Renomeie o arquivo remoto **example. txt** para **example1. txt**  
```  
rename example.txt example1.txt  
```  
## <a name="additional-references"></a>Referências adicionais  
-   - [Chave da sintaxe de linha de comando](command-line-syntax-key.md)  
