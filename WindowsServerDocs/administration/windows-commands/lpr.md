---
title: lpr
description: Tópico de comandos do Windows para * * * *-
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: afc8790b-8b52-45c4-acdf-be0ffa9da534 jpjofre
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: bf7a674b4517d43deb7c38116424bdb70c100ef6
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80840319"
---
# <a name="lpr"></a>lpr

>Aplicável a: Windows Server (canal semestral), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Envia um arquivo para um computador ou dispositivo de compartilhamento de impressora que executa o Serviço LPD (daemon de impressora de linha) em preparação para impressão.  

## <a name="syntax"></a>Sintaxe  
```  
lpr [-S <ServerName>] -P <printerName> [-C <BannerContent>] [-J <JobName>] [-o | -o l] [-x] [-d] <filename>  
```  
### <a name="parameters"></a>Parâmetros  

|     Parâmetro      |                                                                                                           Descrição                                                                                                           |
|--------------------|---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
|  -S <ServerName>   |                                    Especifica (por nome ou endereço IP) o computador ou o dispositivo de compartilhamento de impressora que hospeda a fila de impressão LPD com um status que você deseja exibir. Obrigatório.                                    |
|  -P <printerName>  |                                                              Especifica (por nome) a impressora para a fila de impressão com um status que você deseja exibir. Obrigatório.                                                              |
| -C <BannerContent> |                Especifica o conteúdo a ser impresso na página de faixa do trabalho de impressão. Se você não incluir esse parâmetro, o nome do computador do qual o trabalho de impressão foi enviado aparecerá na página de faixa.                 |
|    -J <JobName>    |                           Especifica o nome do trabalho de impressão que será impresso na página de faixa. Se você não incluir esse parâmetro, o nome do arquivo que está sendo impresso aparecerá na página de faixa.                            |
| [-o&#124; -o l]  | Especifica o tipo de arquivo que você deseja imprimir. O parâmetro **-o** especifica que você deseja imprimir um arquivo de texto. O parâmetro **-o l** especifica que você deseja imprimir um arquivo binário (por exemplo, um arquivo PostScript). |
|         -d         |              Especifica que o arquivo de dados deve ser enviado antes do arquivo de controle. Use esse parâmetro se a sua impressora exigir que o arquivo de dados seja enviado primeiro. Para obter mais informações, consulte a documentação da sua impressora.               |
|         {1&gt;-&lt;1}x         |                               Especifica que o comando **LPR** deve ser compatível com o sistema operacional Sun Microsystems (chamado de SunOS) para versões até e incluindo 4.1.4_u1.                                |
|     <FileName>     |                                                                                      Especifica (por nome) o arquivo a ser impresso. Obrigatório.                                                                                      |
|         /?         |                                                                                              Exibe a ajuda no prompt de comando.                                                                                               |

## <a name="remarks"></a>Comentários  
- Para localizar o nome da impressora, abra a pasta impressoras.  
- Os parâmetros **-S**, **-P**, **-C**e **-J** diferenciam maiúsculas de minúsculas e devem ser digitados em letras maiúsculas.  
  ## <a name="examples"></a><a name=BKMK_examples></a>Disso  
  Este exemplo mostra como imprimir o arquivo de texto Document. txt na fila de impressora Laserprinter1 em um host LPD em 10.0.0.45:  
  ```  
  lpr -S 10.0.0.45 -P Laserprinter1 -o Document.txt  
  ```  
  Este exemplo mostra como imprimir o arquivo Adobe PostScript PostScript_file. PS na fila de impressora Laserprinter1 em um host LPD em 10.0.0.45:  
  ```  
  lpr -S 10.0.0.45 -P Laserprinter1 -o l PostScript_file.ps  
  ```  

## <a name="additional-references"></a>Referências adicionais  
-   - [Chave da sintaxe de linha de comando](command-line-syntax-key.md)  
-   [Referência de comando de impressão](print-command-reference.md)  
