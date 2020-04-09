---
title: LCD de FTP
description: Tópico de comandos do Windows para * * * *-
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 60a25808-6abb-408b-8373-0bbdcd0994b4 vhorne
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 0d7e2e6fc9f6af7655381bfb802dc190e79365bd
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80843399"
---
# <a name="ftp-lcd"></a>FTP: LCD

>Aplicável a: Windows Server (canal semestral), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

altera o diretório de trabalho no computador local. Por padrão, o diretório de trabalho é o diretório no qual o **FTP** foi iniciado.   
## <a name="syntax"></a>Sintaxe  
```  
lcd [<directory>]  
```  
#### <a name="parameters"></a>Parâmetros  
|Parâmetro|Descrição|  
|-------|--------|  
|[<directory>]|Especifica o diretório no computador local para o qual alterar. Se o *diretório* não for especificado, o diretório de trabalho atual será alterado para o diretório padrão.|  
## <a name="examples"></a><a name=BKMK_Examples></a>Disso  
Altere o diretório de trabalho no computador local para **C:\Dir1**  
```  
lcd C:\dir1  
```  
## <a name="additional-references"></a>Referências adicionais  
-   - [Chave da sintaxe de linha de comando](command-line-syntax-key.md)  
