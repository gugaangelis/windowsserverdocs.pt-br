---
title: expandir
description: Tópico de comandos do Windows para * * * *-
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 66de0488-a0c4-40c2-9b03-e40c107ba343
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 0ab4b9bcb5c9da2aeace71a8e535fa762d6d6e95
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80844869"
---
# <a name="expand"></a>expandir

>Aplicável a: Windows Server (canal semestral), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

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
|   {1&gt;source&lt;1}    |                                                                              Especifica os arquivos a serem expandidos. A *origem* pode consistir em uma letra de unidade e dois-pontos, um nome de diretório, um nome de arquivo ou uma combinação desses. Você pode usar caracteres curinga ( **\\** \* ou **?** ).                                                                               |
| destino | Especifica onde os arquivos devem ser expandidos.<p>se a *fonte* consistir em vários arquivos e você não especificar **/r**, o *destino* deverá ser um diretório.<p>O *destino* pode consistir em uma letra de unidade e dois-pontos, um nome de diretório, um nome de arquivo ou uma combinação desses.<p>Especificação do &#124; caminho do arquivo de destino. |
|     /i      |                                                                                                   renomeia arquivos expandidos, mas ignora a estrutura de diretório.<p>Esse parâmetro aplica-se a: Windows Server 2008 R2 e Windows 7.                                                                                                    |
|     /d      |                                                                                                                              Exibe uma lista de arquivos no local de origem. Não expande nem extrai os arquivos.                                                                                                                              |
|     /f:     |                                                                                                                 Especifica os arquivos em um arquivo de gabinete (. cab) que você deseja expandir. Você pode usar caracteres curinga ( **\\** \* ou **?** ).                                                                                                                 |
|     /?      |                                                                                                                                                       Exibe a ajuda no prompt de comando.                                                                                                                                                       |

## <a name="remarks"></a>Comentários  
- Usando **expandir** no console de recuperação  
  O comando **Expand** , com parâmetros diferentes, está disponível no console de recuperação. Para obter mais informações sobre o console de recuperação, consulte o [artigo 314058](https://support.microsoft.com/kb/314058) na base de dados de conhecimento Microsoft.  
  ## <a name="additional-references"></a>Referências adicionais  
  - [Chave da sintaxe de linha de comando](command-line-syntax-key.md)  
