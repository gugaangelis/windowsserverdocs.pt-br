---
title: Exibição de scwcmd
description: 'Tópico de comandos do Windows para * * * *- '
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 7995959a-d93e-4865-a6a0-2ab18c2bb47f
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: a69db16696f42950af97b62ba6f28c4083954137
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71371193"
---
# <a name="scwcmd-view"></a>Scwcmd: view

> Aplica-se a: Windows Server 2012 R2, Windows Server 2012

Renderiza um arquivo. xml usando uma transformação. xsl especificada. Esse comando pode ser útil para exibir arquivos. XML do assistente de configuração de segurança (SCW) usando diferentes exibições.

## <a name="syntax"></a>Sintaxe

```
scwcmd view /x:<Xmlfile.xml> [/s:<Xslfile.xsl>]
```

### <a name="parameters"></a>Parâmetros

|Parâmetro|Descrição|
|---------|-----------|
|/x:\<xmlfile. XML >|Especifica o arquivo. XML a ser exibido. Esse parâmetro deve ser especificado.|
|/s:\<xslfile. xsl >|Especifica a transformação. xsl a ser aplicada ao arquivo. xml como parte do processo de renderização. Esse parâmetro é opcional para arquivos ACS. xml. Quando o comando **View** é usado para renderizar um arquivo SCW. xml, ele tenta automaticamente carregar a transformação padrão correta para o arquivo. XML especificado. Se uma transformação. xsl for especificada, a transformação deverá ser escrita sob a suposição de que o arquivo. xml esteja no mesmo diretório que a transformação. Xsl.|
|/?|Exibe a ajuda no prompt de comando.|

## <a name="remarks"></a>Comentários

Scwcmd. exe só está disponível em computadores que executam o Windows Server 2008 R2, o Windows Server 2008 ou o Windows Server 2003.

## <a name="BKMK_Examples"></a>Disso

Para exibir o policyFile. xml usando a transformação Policyview. xsl, digite:
```
scwcmd view /x:C:\policies\Policyfile.xml /s:C:\viewers\Policyview.xsl
```

#### <a name="additional-references"></a>Referências adicionais

-   [Chave da sintaxe de linha de comando](command-line-syntax-key.md)