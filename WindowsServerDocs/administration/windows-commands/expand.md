---
title: expand
description: Tópico de referência para * * * *-
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 66de0488-a0c4-40c2-9b03-e40c107ba343
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: b757f630e08249b1c716803a07cd9a163d7398d2
ms.sourcegitcommit: ab64dc83fca28039416c26226815502d0193500c
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 05/01/2020
ms.locfileid: "82725687"
---
# <a name="expand"></a>expand

> Aplica-se a: Windows Server (canal semestral), Windows Server 2019, Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

expande um ou mais arquivos compactados. Você pode usar esse comando para recuperar arquivos compactados dos discos de distribuição.  
## <a name="syntax"></a>Sintaxe  
```  
expand [/r] <source> <destination>  
expand /r <source> [<destination>]  
expand /i <source> [<destination>]  
expand /d <source>.cab [/f:<files>]  
expand <source>.cab /f:<files> <destination>  
```  
#### <a name="parameters"></a>Parâmetros  

|  Parâmetro  |                                                                                                                                                                   Descrição                                                                                                                                                                    |
|-------------|--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
|     /r      |                                                                                                                                                             renomeia arquivos expandidos.                                                                                                                                                              |
|   source    |                                                                              Especifica os arquivos a serem expandidos. A *origem* pode consistir em uma letra de unidade e dois-pontos, um nome de diretório, um nome de arquivo ou uma combinação desses. Você pode usar caracteres curinga (**\\** \* ou **?**).                                                                               |
| destino | Especifica onde os arquivos devem ser expandidos.<p>se a *fonte* consistir em vários arquivos e você não especificar **/r**, o *destino* deverá ser um diretório.<p>O *destino* pode consistir em uma letra de unidade e dois-pontos, um nome de diretório, um nome de arquivo ou uma combinação desses.<p>Especificação do caminho do &#124; do arquivo de destino. |
|     /i      |                                                                                                   renomeia arquivos expandidos, mas ignora a estrutura de diretório.<p>Esse parâmetro aplica-se a: Windows Server 2008 R2 e Windows 7.                                                                                                    |
|     /d      |                                                                                                                              Exibe uma lista de arquivos no local de origem. Não expande nem extrai os arquivos.                                                                                                                              |
|     /f:     |                                                                                                                 Especifica os arquivos em um arquivo de gabinete (. cab) que você deseja expandir. Você pode usar caracteres curinga (**\\** \* ou **?**).                                                                                                                 |
|     /?      |                                                                                                                                                       Exibe a ajuda no prompt de comando.                                                                                                                                                       |

## <a name="remarks"></a>Comentários  
- Usando **expandir** no console de recuperação  
  O comando **Expand** , com parâmetros diferentes, está disponível no console de recuperação. Para obter mais informações sobre o console de recuperação, consulte o [artigo 314058](https://support.microsoft.com/kb/314058) na base de dados de conhecimento Microsoft.  
  ## <a name="additional-references"></a>Referências adicionais  
  - [Chave da sintaxe de linha de comando](command-line-syntax-key.md)  
