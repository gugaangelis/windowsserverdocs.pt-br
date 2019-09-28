---
title: LCD de FTP
description: 'Tópico de comandos do Windows para * * * *- '
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 60a25808-6abb-408b-8373-0bbdcd0994b4 vhorne
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 47058cc62e4e87d1fcd951ec3b4a7885eeef2ae2
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71376313"
---
# <a name="ftp-lcd"></a>FTP: LCD

>Aplica-se a: Windows Server (canal semestral), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

altera o diretório de trabalho no computador local. Por padrão, o diretório de trabalho é o diretório no qual o **FTP** foi iniciado.   
## <a name="syntax"></a>Sintaxe  
```  
lcd [<directory>]  
```  
### <a name="parameters"></a>Parâmetros  
|Parâmetro|Descrição|  
|-------|--------|  
|[<directory>]|Especifica o diretório no computador local para o qual alterar. Se o *diretório* não for especificado, o diretório de trabalho atual será alterado para o diretório padrão.|  
## <a name="BKMK_Examples"></a>Disso  
Altere o diretório de trabalho no computador local para **C:\Dir1**  
```  
lcd C:\dir1  
```  
## <a name="additional-references"></a>Referências adicionais  
-   [Chave da sintaxe de linha de comando](command-line-syntax-key.md)  
