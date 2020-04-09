---
title: irftp
description: Tópico de comandos do Windows para * * * *-
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: e15c60a7-546d-4e9f-9871-43aaa1b569d6 vhorne
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 28cf722a5e630cb05b0348ebf2d4f582217b5497
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80841979"
---
# <a name="irftp"></a>irftp

>Aplicável a: Windows Server (canal semestral), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

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

## <a name="examples"></a><a name=BKMK_Examples></a>Disso  
Envie example. txt pelo link de infravermelho.  
```  
irftp c:\example.txt  
```  

## <a name="additional-references"></a>Referências adicionais  
-   - [Chave da sintaxe de linha de comando](command-line-syntax-key.md)  
