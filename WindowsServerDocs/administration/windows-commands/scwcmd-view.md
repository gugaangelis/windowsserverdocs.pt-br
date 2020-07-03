---
title: Exibição de scwcmd
description: Artigo de referência para * * * *-
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 7995959a-d93e-4865-a6a0-2ab18c2bb47f
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: cbae5f3d0157424fb9281d47cdf126bf106447c3
ms.sourcegitcommit: 2afed2461574a3f53f84fc9ec28d86df3b335685
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/02/2020
ms.locfileid: "85932614"
---
# <a name="scwcmd-view"></a>Scwcmd: view

> Aplica-se a: Windows Server 2012 R2, Windows Server 2012

Renderiza um arquivo. xml usando uma transformação. xsl especificada. Esse comando pode ser útil para exibir arquivos. XML do assistente de configuração de segurança (SCW) usando diferentes exibições.

## <a name="syntax"></a>Sintaxe

```
scwcmd view /x:<Xmlfile.xml> [/s:<Xslfile.xsl>]
```

#### <a name="parameters"></a>Parâmetros

|Parâmetro|Descrição|
|---------|-----------|
|/x\<Xmlfile.xml>|Especifica o arquivo. XML a ser exibido. Esse parâmetro deve ser especificado.|
|/s\<Xslfile.xsl>|Especifica a transformação. xsl a ser aplicada ao arquivo. xml como parte do processo de renderização. Esse parâmetro é opcional para arquivos ACS. xml. Quando o comando **View** é usado para renderizar um arquivo SCW. xml, ele tenta automaticamente carregar a transformação padrão correta para o arquivo. XML especificado. Se uma transformação. xsl for especificada, a transformação deverá ser escrita sob a suposição de que o arquivo. xml esteja no mesmo diretório que a transformação. Xsl.|
|/?|Exibe a ajuda no prompt de comando.|

## <a name="remarks"></a>Comentários

Scwcmd.exe só está disponível em computadores que executam o Windows Server 2008 R2, o Windows Server 2008 ou o Windows Server 2003.

## <a name="examples"></a>Exemplos

Para exibir Policyfile.xml usando a transformação Policyview. xsl, digite:
```
scwcmd view /x:C:\policies\Policyfile.xml /s:C:\viewers\Policyview.xsl
```

## <a name="additional-references"></a>Referências adicionais

- [Chave da sintaxe de linha de comando](command-line-syntax-key.md)