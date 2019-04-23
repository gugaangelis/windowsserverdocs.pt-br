---
title: irftp
description: 'Tópico de comandos do Windows para * * *- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: e15c60a7-546d-4e9f-9871-43aaa1b569d6 vhorne
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: d469270b744a9de881efd9b0cfa6f1105f70519a
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59848417"
---
# <a name="irftp"></a>irftp

>Aplica-se a: Windows Server (canal semestral), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Envia arquivos por uma conexão de infravermelho.    
## <a name="syntax"></a>Sintaxe  
```  
irftp [<Drive>:\] [[<path>] <FileName>] [/h][/s]  
```  

### <a name="parameters"></a>Parâmetros  
|Parâmetro|Descrição|  
|-------|--------|  
|Unidade:\|Especifica a unidade que contém os arquivos que você deseja enviar por uma conexão de infravermelho.|  
|[path]FileName|Especifica o local e o nome do arquivo ou conjunto de arquivos que você deseja enviar por uma conexão de infravermelho. Se você especificar um conjunto de arquivos, você deve especificar o caminho completo para cada arquivo.|  
|/h|Especifica o modo oculto. Quando o modo oculto é usado, os arquivos são enviados sem exibir a caixa de diálogo de conexão sem fio.|  
|/s|Abre a caixa de diálogo de conexão sem fio, para que você possa selecionar o arquivo ou conjunto de arquivos que você deseja enviar sem usar a linha de comando para especificar a unidade, caminho e nomes de arquivo.|  

## <a name="remarks"></a>Comentários  
-   Antes de usar esse comando, verifique se que os dispositivos que você deseja se comunicar através de uma conexão de infravermelho tem habilitado a funcionalidade de infravermelho e funcionando corretamente e se uma conexão de infravermelho foi estabelecida entre os dispositivos.  
-   Usado sem parâmetros ou com **/s**, **irftp** abre o **conexão sem fio** caixa de diálogo, onde você pode selecionar os arquivos que você deseja enviar sem usar a linha de comando.  

## <a name="BKMK_Examples"></a>Exemplos  
Envie example através da conexão de infravermelho.  
```  
irftp c:\example.txt  
```  

## <a name="additional-references"></a>Referências adicionais  
-   [Chave de sintaxe de linha de comando](command-line-syntax-key.md)  
