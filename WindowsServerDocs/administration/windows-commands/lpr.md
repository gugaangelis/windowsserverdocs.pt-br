---
title: lpr
description: 'Tópico de comandos do Windows para * * * *- '
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: afc8790b-8b52-45c4-acdf-be0ffa9da534 jpjofre
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: e853933e368f181866963fdcbc1eca5b84bb8c09
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71374156"
---
# <a name="lpr"></a>lpr

>Aplica-se a: Windows Server (canal semestral), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Envia um arquivo para um computador ou dispositivo de compartilhamento de impressora que executa o Serviço LPD (daemon de impressora de linha) em preparação para impressão.  

## <a name="syntax"></a>Sintaxe  
```  
lpr [-S <ServerName>] -P <printerName> [-C <BannerContent>] [-J <JobName>] [-o | "-o l"] [-x] [-d] <filename>  
```  
## <a name="parameters"></a>Parâmetros  

|     Parâmetro      |                                                                                                           Descrição                                                                                                           |
|--------------------|---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
|  -S <ServerName>   |                                    Especifica (por nome ou endereço IP) o computador ou o dispositivo de compartilhamento de impressora que hospeda a fila de impressão LPD com um status que você deseja exibir. Obrigatório.                                    |
|  -P <printerName>  |                                                              Especifica (por nome) a impressora para a fila de impressão com um status que você deseja exibir. Obrigatório.                                                              |
| -C <BannerContent> |                Especifica o conteúdo a ser impresso na página de faixa do trabalho de impressão. Se você não incluir esse parâmetro, o nome do computador do qual o trabalho de impressão foi enviado aparecerá na página de faixa.                 |
|    -J <JobName>    |                           Especifica o nome do trabalho de impressão que será impresso na página de faixa. Se você não incluir esse parâmetro, o nome do arquivo que está sendo impresso aparecerá na página de faixa.                            |
| [-o&#124; "-o l"]  | Especifica o tipo de arquivo que você deseja imprimir. O parâmetro **-o** especifica que você deseja imprimir um arquivo de texto. O parâmetro **"-o l"** especifica que você deseja imprimir um arquivo binário (por exemplo, um arquivo PostScript). |
|         -d         |              Especifica que o arquivo de dados deve ser enviado antes do arquivo de controle. Use esse parâmetro se a sua impressora exigir que o arquivo de dados seja enviado primeiro. Para obter mais informações, consulte a documentação da sua impressora.               |
|         -x         |                               Especifica que o comando **LPR** deve ser compatível com o sistema operacional Sun Microsystems (conhecido como SunOS) para versões até e incluindo 4.1.4 _u1.                                |
|     <FileName>     |                                                                                      Especifica (por nome) o arquivo a ser impresso. Obrigatório.                                                                                      |
|         /?         |                                                                                              Exibe a ajuda no prompt de comando.                                                                                               |

## <a name="remarks"></a>Comentários  
- Para localizar o nome da impressora, abra a pasta impressoras.  
- Os parâmetros **-S**, **-P**, **-C**e **-J** diferenciam maiúsculas de minúsculas e devem ser digitados em letras maiúsculas.  
  ## <a name="BKMK_examples"></a>Disso  
  Este exemplo mostra como imprimir o arquivo de texto "Document. txt" na fila de impressora Laserprinter1 em um host LPD em 10.0.0.45:  
  ```  
  lpr -S 10.0.0.45 -P Laserprinter1 -o Document.txt  
  ```  
  Este exemplo mostra como imprimir o arquivo PostScript Adobe "PostScript_file. PS" na fila de impressora Laserprinter1 em um host LPD em 10.0.0.45:  
  ```  
  lpr -S 10.0.0.45 -P Laserprinter1 "-o l" PostScript_file.ps  
  ```  

#### <a name="additional-references"></a>Referências adicionais  
-   [Chave da sintaxe de linha de comando](command-line-syntax-key.md)  
-   [Referência de comando de impressão](print-command-reference.md)  
