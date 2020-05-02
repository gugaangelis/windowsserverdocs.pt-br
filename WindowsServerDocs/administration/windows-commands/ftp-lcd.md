---
title: LCD de FTP
description: Tópico de referência para * * * *-
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 60a25808-6abb-408b-8373-0bbdcd0994b4 vhorne
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 646cbfe3feadb63388694218758dae165ffb49c4
ms.sourcegitcommit: ab64dc83fca28039416c26226815502d0193500c
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 05/01/2020
ms.locfileid: "82725260"
---
# <a name="ftp-lcd"></a>FTP: LCD

> Aplica-se a: Windows Server (canal semestral), Windows Server 2019, Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

altera o diretório de trabalho no computador local. Por padrão, o diretório de trabalho é o diretório no qual o **FTP** foi iniciado.   
## <a name="syntax"></a>Sintaxe  
```  
lcd [<directory>]  
```  
#### <a name="parameters"></a>Parâmetros  
|Parâmetro|Descrição|  
|-------|--------|  
|[<directory>]|Especifica o diretório no computador local para o qual alterar. Se o *diretório* não for especificado, o diretório de trabalho atual será alterado para o diretório padrão.|  
## <a name="examples"></a>Exemplos  
Altere o diretório de trabalho no computador local para **C:\Dir1**  
```  
lcd C:\dir1  
```  
## <a name="additional-references"></a>Referências adicionais  
-   - [Chave da sintaxe de linha de comando](command-line-syntax-key.md)  
