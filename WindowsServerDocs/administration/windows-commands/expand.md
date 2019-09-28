---
title: expand
description: 'Tópico de comandos do Windows para * * * *- '
ms.custom: na
ms.prod: windows-server
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
ms.openlocfilehash: 4a88222ffe9ff374626a6406c330a6660d992f96
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71377324"
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

|  Parâmetro  |                                                                                                                                                                   Descrição                                                                                                                                                                    |
|-------------|--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
|     /r      |                                                                                                                                                             renomeia arquivos expandidos.                                                                                                                                                              |
|   source    |                                                                              Especifica os arquivos a serem expandidos. A *origem* pode consistir em uma letra de unidade e dois-pontos, um nome de diretório, um nome de arquivo ou uma combinação desses. Você pode usar caracteres curinga ( **\\** \* ou **?** ).                                                                               |
| destination | Especifica onde os arquivos devem ser expandidos.<br /><br />se a *fonte* consistir em vários arquivos e você não especificar **/r**, o *destino* deverá ser um diretório.<br /><br />O *destino* pode consistir em uma letra de unidade e dois-pontos, um nome de diretório, um nome de arquivo ou uma combinação desses.<br /><br />Especificação do &#124; caminho do arquivo de destino. |
|     /i      |                                                                                                   renomeia arquivos expandidos, mas ignora a estrutura de diretório.<br /><br />Esse parâmetro se aplica a:  Windows Server 2008 R2 e Windows 7.                                                                                                    |
|     /d      |                                                                                                                              Exibe uma lista de arquivos no local de origem. Não expande nem extrai os arquivos.                                                                                                                              |
|     /f:     |                                                                                                                 Especifica os arquivos em um arquivo de gabinete (. cab) que você deseja expandir. Você pode usar caracteres curinga ( **\\** \* ou **?** ).                                                                                                                 |
|     /?      |                                                                                                                                                       Exibe a ajuda no prompt de comando.                                                                                                                                                       |

## <a name="remarks"></a>Comentários  
- Usando **expandir** no console de recuperação  
  O comando **Expand** , com parâmetros diferentes, está disponível no console de recuperação. Para obter mais informações sobre o console de recuperação, consulte o [artigo 314058](https://support.microsoft.com/kb/314058) na base de dados de conhecimento Microsoft.  
  ## <a name="additional-references"></a>Referências adicionais  
  [Chave da sintaxe de linha de comando](command-line-syntax-key.md)  
