---
title: expand
description: 'Tópico de comandos do Windows para * * *- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 66de0488-a0c4-40c2-9b03-e40c107ba343
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 5df078af23c77f54ccb2da83b1057c5d7042593a
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59825957"
---
# <a name="expand"></a>expand

>Aplica-se a: Windows Server (canal semestral), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

expande um ou mais arquivos compactados. Você pode usar esse comando para recuperar arquivos compactados dos discos de distribuição.  
## <a name="syntax"></a>Sintaxe  
```  
expand [/r] <source> <destination>  
expand /r <source> [<destination>]  
expand /i <source> [<destination>]  
expand /d <source>.cab [/f:<files>]  
expand <source>.cab /f:<files> <destination>  
```  
### <a name="parameters"></a>Parâmetros  
|Parâmetro|Descrição|  
|-------|--------|  
|/r|Renomeia arquivos expandidos.|  
|Código-fonte|Especifica os arquivos para expandir. *Origem* pode consistir em uma letra de unidade e dois-pontos, um nome de diretório, um nome de arquivo ou uma combinação desses elementos. Você pode usar caracteres curinga (**\*** ou **?**).|  
|Destino|Especifica quais arquivos devem ser expandida.<br /><br />Se *fonte* consiste em vários arquivos e você não especificar **/r**, *destino* deve ser um diretório.<br /><br />*Destino* pode consistir em uma letra de unidade e dois-pontos, um nome de diretório, um nome de arquivo ou uma combinação desses elementos.<br /><br />Arquivo de destino &#124; especificação de caminho.|  
|/i|Renomeia arquivos expandidos, mas ignora a estrutura do diretório.<br /><br />Esse parâmetro se aplica a:  Windows Server 2008 R2 e Windows 7.|  
|/d|Exibe uma lista de arquivos no local de origem. Não expanda ou extrair os arquivos.|  
|/f:|Especifica os arquivos em um arquivo de gabinete (. cab) que você deseja expandir. Você pode usar caracteres curinga (**\*** ou **?**).|  
|/?|Exibe a ajuda no prompt de comando.|  
## <a name="remarks"></a>Comentários  
-   Usando o **expandir** no Console de recuperação  
    O **expandir** comando, com parâmetros diferentes, está disponível no Console de recuperação. Para obter mais informações sobre a Console de recuperação, consulte [314058 do artigo](https://support.microsoft.com/kb/314058) na Base de dados de Conhecimento Microsoft.  
## <a name="additional-references"></a>Referências adicionais  
[Chave de sintaxe de linha de comando](command-line-syntax-key.md)  
