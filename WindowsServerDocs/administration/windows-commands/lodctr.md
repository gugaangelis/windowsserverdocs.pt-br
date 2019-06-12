---
title: lodctr
description: 'Tópico de comandos do Windows para * * *- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 5a849abd-6b31-4833-bc8a-306c05eca29a
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: b67c11b24013adb911309ab2ea9bdbfdfc3408da
ms.sourcegitcommit: eaf071249b6eb6b1a758b38579a2d87710abfb54
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 05/31/2019
ms.locfileid: "66437897"
---
# <a name="lodctr"></a>lodctr

>Aplica-se a: Windows Server (canal semestral), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Permite que você se registrar ou salvar as configurações de registro e o nome do contador de desempenho em um arquivo e designar os serviços confiáveis.
## <a name="syntax"></a>Sintaxe
```
lodctr <filename> [/s:<filename>] [/r:<filename>] [/t:<servicename>]
```
### <a name="parameters"></a>Parâmetros

|    Parâmetro     |                                                                                                                                         Descrição                                                                                                                                          |
|------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
|    <filename>    |                                                                                          Registra as configurações de nome de contador de desempenho e o texto explicativo fornecidos no nome de arquivo do arquivo de inicialização.                                                                                          |
|  /s:<filename>   |                                                                                                       Desempenho de salva as configurações do registro do contador e texto explicativo para arquivo <filename>.                                                                                                       |
|        /r        |                                Restaura as configurações de registro do contador e texto explicativo de configurações do registro atual e os arquivos de desempenho em cache relacionados ao registro.<br /><br />Essa opção está disponível apenas no sistema operacional Windows Server 2003.                                |
|  /r:<filename>   | Desempenho de restaura as configurações do registro do contador e explicar o texto do arquivo <filename>. **Aviso:** se você usar o **lodctr /r** de comando, você substituirá todas as configurações de registro do contador de desempenho e texto explicativo, substituindo-os com a configuração definida no arquivo especificado. |
| /t:<servicename> |                                                                                                                       Indica que o serviço <servicename> é confiável.                                                                                                                       |
|        /?        |                                                                                                                             Exibe a ajuda no prompt de comando.                                                                                                                             |

## <a name="remarks"></a>Comentários
Se as informações que você fornece contiverem espaços, use aspas ao redor do texto (por exemplo, "<filename>").
## <a name="BKMK_Examples"></a>Exemplos
Para salvar as configurações de registro de desempenho atuais e texto explicativo para o arquivo de contador **backup1 de perf. txt**:
```
lodctr /s:"perf backup1.txt"
```
## <a name="additional-references"></a>Referências adicionais
-   [Chave da sintaxe de linha de comando](command-line-syntax-key.md)

