---
title: lpr
description: 'Tópico de comandos do Windows para * * *- '
ms.custom: na
ms.prod: windows-server-threshold
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
ms.openlocfilehash: ddc64f958dcb38129d81becfc8950059e055d8e5
ms.sourcegitcommit: eaf071249b6eb6b1a758b38579a2d87710abfb54
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 05/31/2019
ms.locfileid: "66437484"
---
# <a name="lpr"></a>lpr

>Aplica-se a: Windows Server (canal semestral), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Envia um arquivo em um computador ou dispositivo que executa o serviço de impressora de linha LPD (Daemon) em preparação para impressão de compartilhamento de impressora.  

## <a name="syntax"></a>Sintaxe  
```  
lpr [-S <ServerName>] -P <printerName> [-C <BannerContent>] [-J <JobName>] [-o | "-o l"] [-x] [-d] <filename>  
```  
## <a name="parameters"></a>Parâmetros  

|     Parâmetro      |                                                                                                           Descrição                                                                                                           |
|--------------------|---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
|  -S <ServerName>   |                                    Especifica (por nome ou endereço IP), o computador ou dispositivo que hospeda a fila de impressão LPD com um status que você deseja exibir de compartilhamento de impressora. Obrigatório.                                    |
|  -P <printerName>  |                                                              Especifica (por nome) da impressora para a fila de impressão com um status que você deseja exibir. Obrigatório.                                                              |
| -C <BannerContent> |                Especifica o conteúdo a ser impresso na página de faixa do trabalho de impressão. Se você não incluir esse parâmetro, o nome do computador do qual o trabalho de impressão foi enviado será exibida na página de faixa.                 |
|    -J <JobName>    |                           Especifica o nome do trabalho de impressão que será impresso na página de faixa. Se você não incluir esse parâmetro, o nome do arquivo que está sendo impresso aparece na página de faixa.                            |
| [-o&#124; "-o l"]  | Especifica o tipo de arquivo que você deseja imprimir. O parâmetro **-o** Especifica que você deseja imprimir um arquivo de texto. O parâmetro **"-s l"** Especifica que você deseja imprimir um arquivo binário (por exemplo, um arquivo PostScript). |
|         -d         |              Especifica que o arquivo de dados deve ser enviado antes que o arquivo de controle. Use esse parâmetro se sua impressora requer o arquivo de dados a serem enviados pela primeira vez. Para obter mais informações, consulte a documentação da impressora.               |
|         -x         |                               Especifica que o **lpr** comando deve ser compatível com a Sun Microsystems (conhecido como SunOS) do sistema de operacional para as versões até e incluindo 4.1.4_u1.                                |
|     <FileName>     |                                                                                      Especifica o arquivo a ser impresso (por nome). Obrigatório.                                                                                      |
|         /?         |                                                                                              Exibe a ajuda no prompt de comando.                                                                                               |

## <a name="remarks"></a>Comentários  
- Para localizar o nome da impressora, abra a pasta Impressoras.  
- O **-S**, **-P**, **- C**, e **-J** parâmetros diferenciam maiusculas de minúsculas e devem ser digitados em letras maiusculas.  
  ## <a name="BKMK_examples"></a>Exemplos  
  Este exemplo mostra como imprimir o arquivo de texto "Document. txt" para a fila da impressora Laserprinter1 em um host LPD em 10.0.0.45:  
  ```  
  lpr -S 10.0.0.45 -P Laserprinter1 -o Document.txt  
  ```  
  Este exemplo mostra como imprimir o arquivo "PostScript_file.ps" Adobe PostScript à fila da impressora Laserprinter1 em um host LPD em 10.0.0.45:  
  ```  
  lpr -S 10.0.0.45 -P Laserprinter1 "-o l" PostScript_file.ps  
  ```  

#### <a name="additional-references"></a>Referências adicionais  
-   [Chave da sintaxe de linha de comando](command-line-syntax-key.md)  
-   [Referência do comando Imprimir](print-command-reference.md)  
