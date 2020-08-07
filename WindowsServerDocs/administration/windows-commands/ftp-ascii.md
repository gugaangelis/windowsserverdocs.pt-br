---
title: ftp ascii
description: Artigo de referência para o comando FTP ASCII, que define o tipo de transferência de arquivo como ASCII.
ms.topic: article
ms.assetid: 523be48e-eab0-4237-8fb5-ca222824f0b6
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 76ed369efe992e58304d07e627fdb55bcaa039e0
ms.sourcegitcommit: 53d526bfeddb89d28af44210a23ba417f6ce0ecf
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/06/2020
ms.locfileid: "87889652"
---
# <a name="ftp-ascii"></a>ftp ascii

> Aplica-se a: Windows Server (canal semestral), Windows Server 2019, Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Define o tipo de transferência de arquivo como ASCII. O comando **FTP** dá suporte a tipos ASCII (padrão) e de transferência de arquivo de imagem binária, mas é recomendável usar ASCII ao transferir arquivos de texto. No modo ASCII, conversões de caracteres de e para o conjunto de caracteres padrão de rede são executadas. Por exemplo, caracteres de fim de linha são convertidos conforme necessário, com base no sistema operacional de destino.

## <a name="syntax"></a>Sintaxe

```
ascii
```

### <a name="examples"></a>Exemplos

Para definir o tipo de transferência de arquivo como ASCII, digite:

```
ascii
```

## <a name="additional-references"></a>Referências adicionais

- [Chave da sintaxe de linha de comando](command-line-syntax-key.md)

- [comando Binary FTP](ftp-binary.md)

- [Diretrizes adicionais de FTP](/previous-versions/orphan-topics/ws.10/cc756013(v=ws.10))
