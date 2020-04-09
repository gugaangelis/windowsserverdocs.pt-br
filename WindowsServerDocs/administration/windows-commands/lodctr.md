---
title: lodctr
description: Tópico de comandos do Windows para * * * *-
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 5a849abd-6b31-4833-bc8a-306c05eca29a
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 33c14970a669d24f1cc803003e8530712311c564
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80840949"
---
# <a name="lodctr"></a>lodctr

>Aplicável a: Windows Server (canal semestral), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Permite registrar ou salvar o nome do contador de desempenho e as configurações do registro em um arquivo e designar serviços confiáveis.
## <a name="syntax"></a>Sintaxe
```
lodctr <filename> [/s:<filename>] [/r:<filename>] [/t:<servicename>]
```
#### <a name="parameters"></a>Parâmetros

|    Parâmetro     |                                                                                                                                         Descrição                                                                                                                                          |
|------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
|    <filename>    |                                                                                          Registra as configurações do nome do contador de desempenho e o texto explicativo fornecido no arquivo de inicialização FileName.                                                                                          |
|  /s:<filename>   |                                                                                                       Salva as configurações do registro do contador de desempenho e explica o texto em <filename>de arquivo.                                                                                                       |
|        /r        |                                Restaura as configurações do registro do contador e explica o texto das configurações atuais do registro e os arquivos de desempenho em cache relacionados ao registro.<p>Essa opção está disponível apenas no sistema operacional Windows Server 2003.                                |
|  /r:<filename>   | Restaura as configurações do registro do contador de desempenho e explica o texto do arquivo <filename>. **AVISO:** se você usar o comando **lodctr/r** , substituirá todas as configurações do registro do contador de desempenho e explicará o texto, substituindo-os pela configuração definida no arquivo especificado. |
| /t:<servicename> |                                                                                                                       Indica que o serviço <servicename> é confiável.                                                                                                                       |
|        /?        |                                                                                                                             Exibe a ajuda no prompt de comando.                                                                                                                             |

## <a name="remarks"></a>Comentários
Se as informações fornecidas contiverem espaços, use aspas ao contrário do texto (por exemplo, <filename>).
## <a name="examples"></a><a name=BKMK_Examples></a>Disso
Para salvar as configurações atuais do registro de desempenho e o contador de texto explicativo para o arquivo **perf backup1. txt**:
```
lodctr /s:perf backup1.txt
```
## <a name="additional-references"></a>Referências adicionais
-   - [Chave da sintaxe de linha de comando](command-line-syntax-key.md)

