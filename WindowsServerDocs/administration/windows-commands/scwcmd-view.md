---
title: scwcmd view
description: Artigo de referência para o comando scwcmd View, que renderiza um arquivo. xml usando uma transformação. xsl especificada.
ms.topic: reference
ms.assetid: 7995959a-d93e-4865-a6a0-2ab18c2bb47f
ms.author: lizross
author: eross-msft
manager: mtillman
ms.date: 10/16/2017
ms.openlocfilehash: e411bf06015684e7dedb94d109f5e4294f46af84
ms.sourcegitcommit: e164aeffc01069b8f1f3248bf106fcdb7f64f894
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/26/2020
ms.locfileid: "91388926"
---
# <a name="scwcmd-view"></a>scwcmd view

> Aplica-se a: Windows Server 2012 R2 e Windows Server 2012

Renderiza um arquivo. xml usando uma transformação. xsl especificada. Esse comando pode ser útil para exibir arquivos. XML do assistente de configuração de segurança (SCW) usando diferentes exibições.

## <a name="syntax"></a>Sintaxe

```
scwcmd view /x:<Xmlfile.xml> [/s:<Xslfile.xsl>]
```

### <a name="parameters"></a>Parâmetros

| Parâmetro | Descrição |
|--|--|
| /x`<Xmlfile.xml>` | Especifica o arquivo. XML a ser exibido. Esse parâmetro deve ser especificado. |
| /s`<Xslfile.xsl>` | Especifica a transformação. xsl a ser aplicada ao arquivo. xml como parte do processo de renderização. Esse parâmetro é opcional para arquivos ACS. xml. Quando o comando **View** é usado para renderizar um arquivo SCW. xml, ele tenta automaticamente carregar a transformação padrão correta para o arquivo. XML especificado. Se uma transformação. xsl for especificada, a transformação deverá ser escrita sob a suposição de que o arquivo. xml esteja no mesmo diretório que a transformação. Xsl. |
| /? | Exibe a ajuda no prompt de comando. |

## <a name="example"></a>Exemplo

Para exibir *Policyfile.xml* usando a transformação *Policyview. xsl* , digite:

```
scwcmd view /x:C:\policies\Policyfile.xml /s:C:\viewers\Policyview.xsl
```

## <a name="additional-references"></a>Referências adicionais

- [Chave da sintaxe de linha de comando](command-line-syntax-key.md)

- [comando scwcmd analyze](scwcmd-analyze.md)

- [comando de configuração de scwcmd](scwcmd-configure.md)

- [comando de registro scwcmd](scwcmd-register.md)

- [comando scwcmd rollback](scwcmd-rollback.md)

- [comando scwcmd transform](scwcmd-transform.md)