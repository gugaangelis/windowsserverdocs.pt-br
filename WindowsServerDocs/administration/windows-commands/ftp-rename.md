---
title: renomeação de FTP
description: 'Tópico de comandos do Windows para * * *- '
ms.custom: na
ms.prod: windows-server-threshold
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
ms.openlocfilehash: 80d1a15f038017444c7654a44748bfd22be8e487
ms.sourcegitcommit: eaf071249b6eb6b1a758b38579a2d87710abfb54
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 05/31/2019
ms.locfileid: "66438383"
---
# <a name="ftp-rename"></a>ftp: rename

>Aplica-se a: Windows Server (canal semestral), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Renomeia arquivos remotos.   
## <a name="syntax"></a>Sintaxe  
```  
rename <FileName> <NewFileName>  
```  
### <a name="parameters"></a>Parâmetros  

|   Parâmetro   |                 Descrição                 |
|---------------|---------------------------------------------|
|  <FileName>   | Especifica o arquivo que você deseja renomear. |
| <NewFileName> |        Especifica o novo nome de arquivo.         |

## <a name="BKMK_Examples"></a>Exemplos  
Renomeie o arquivo remoto **example** para **example1.txt**  
```  
rename example.txt example1.txt  
```  
## <a name="additional-references"></a>Referências adicionais  
-   [Chave da sintaxe de linha de comando](command-line-syntax-key.md)  
