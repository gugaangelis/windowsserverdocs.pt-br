---
title: irftp
description: Tópico de referência para * * * *-
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: e15c60a7-546d-4e9f-9871-43aaa1b569d6 vhorne
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 34f912878b97bd00fb1c4ea539c7ea4c1423ea59
ms.sourcegitcommit: ab64dc83fca28039416c26226815502d0193500c
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 05/01/2020
ms.locfileid: "82724812"
---
# <a name="irftp"></a>irftp

> Aplica-se a: Windows Server (canal semestral), Windows Server 2019, Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Envia arquivos por um link de infravermelho.    
## <a name="syntax"></a>Sintaxe  
```  
irftp [<Drive>:\] [[<path>] <FileName>] [/h][/s]  
```  

#### <a name="parameters"></a>Parâmetros  
|Parâmetro|Descrição|  
|-------|--------|  
|Unidade:\|especifica a unidade que contém os arquivos que você deseja enviar por um link de infravermelho.|  
|Multi-Path Nome do arquivo|Especifica o local e o nome do arquivo ou conjunto de arquivos que você deseja enviar por um link de infravermelho. Se você especificar um conjunto de arquivos, deverá especificar o caminho completo de cada arquivo.|  
|/h|Especifica o modo oculto. Quando o modo oculto é usado, os arquivos são enviados sem exibir a caixa de diálogo link sem fio.|  
|/s|Abre a caixa de diálogo link sem fio, para que você possa selecionar o arquivo ou conjunto de arquivos que deseja enviar sem usar a linha de comando para especificar a unidade, o caminho e os nomes de arquivo.|  

## <a name="remarks"></a>Comentários  
-   Antes de usar esse comando, verifique se os dispositivos que você deseja comunicar por meio de um link de infravermelho têm a funcionalidade de infravermelho habilitada e funcionando corretamente, e se um link de infravermelho é estabelecido entre os dispositivos.  
-   Usado sem parâmetros ou usado com **/s**, **irftp** abre a caixa de diálogo **link sem fio** , na qual você pode selecionar os arquivos que deseja enviar sem usar a linha de comando.  

## <a name="examples"></a>Exemplos  
Envie example. txt pelo link de infravermelho.  
```  
irftp c:\example.txt  
```  

## <a name="additional-references"></a>Referências adicionais  
-   - [Chave da sintaxe de linha de comando](command-line-syntax-key.md)  
