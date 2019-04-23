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
ms.openlocfilehash: f99b3a43192a48e8adffaa60c25b46cfcaa8e3c2
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59861857"
---
# <a name="ftp-rename"></a>ftp: rename

>Aplica-se a: Windows Server (canal semestral), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Renomeia arquivos remotos.   
## <a name="syntax"></a>Sintaxe  
```  
rename <FileName> <NewFileName>  
```  
### <a name="parameters"></a>Parâmetros  
|Parâmetro|Descrição|  
|-------|--------|  
|<FileName>|Especifica o arquivo que você deseja renomear.|  
|<NewFileName>|Especifica o novo nome de arquivo.|  
## <a name="BKMK_Examples"></a>Exemplos  
Renomeie o arquivo remoto **example** para **example1.txt**  
```  
rename example.txt example1.txt  
```  
## <a name="additional-references"></a>Referências adicionais  
-   [Chave de sintaxe de linha de comando](command-line-syntax-key.md)  
