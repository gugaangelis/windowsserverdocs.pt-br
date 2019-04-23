---
title: ftp open_1
description: 'Tópico de comandos do Windows para * * *- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 4b61926a-dc60-4b4c-96d3-64e5c91c18ba vhorne
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: da7926914e2cbdbb4909093d90c33025ae5cd695
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59882467"
---
# <a name="ftp-open1"></a>ftp: open_1

>Aplica-se a: Windows Server (canal semestral), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Conecta-se ao servidor ftp especificado.   
## <a name="syntax"></a>Sintaxe  
```  
open <computer> [<Port>]  
```  
### <a name="parameters"></a>Parâmetros  
|Parâmetro|Descrição|  
|-------|--------|  
|<computer>|Especifica o computador remoto ao qual você está tentando se conectar.|  
|[<Port>]|Especifica o número da porta TCP para usar para se conectar a um servidor ftp. Por padrão, a porta TCP 21 é usada.|  
## <a name="remarks"></a>Comentários  
Você pode usar um nome de computador ou endereço IP (neste caso, um arquivo de Hosts ou um servidor DNS deve estar disponível) para especificar **computador**.  
## <a name="BKMK_Examples"></a>Exemplos  
Conectar-se ao servidor ftp **ftp.microsoft.com**.  
```  
Open ftp.microsoft.com  
```  
Conectar-se ao servidor ftp **ftp.microsoft.com** que está escutando na porta TCP 755.  
```  
open ftp.microsoft.com 755  
```  
## <a name="additional-references"></a>Referências adicionais  
-   [Chave de sintaxe de linha de comando](command-line-syntax-key.md)  
