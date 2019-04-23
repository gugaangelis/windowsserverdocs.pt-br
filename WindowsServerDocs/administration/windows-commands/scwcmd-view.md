---
title: Modo de exibição scwcmd
description: 'Tópico de comandos do Windows para * * *- '
ms.custom: na
ms.prod: windows-server-threshold
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
ms.openlocfilehash: 2ef1dd72903108edd6c5fb450c536a9325fcf546
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59889547"
---
# <a name="scwcmd-view"></a>Scwcmd: view

> Aplica-se a: Windows Server 2012 R2, Windows Server 2012

Renderiza um arquivo. XML usando uma transformação XSL especificado. Esse comando pode ser útil para exibir os arquivos do Assistente de configuração de segurança (ACS). XML, usando diferentes modos de exibição.

## <a name="syntax"></a>Sintaxe

```
scwcmd view /x:<Xmlfile.xml> [/s:<Xslfile.xsl>]
```

### <a name="parameters"></a>Parâmetros

|Parâmetro|Descrição|
|---------|-----------|
|/x:\<Xmlfile.xml>|Especifica o arquivo. XML a ser exibida. Esse parâmetro deve ser especificado.|
|/s:\<Xslfile.xsl>|Especifica a transformação XSL para aplicar ao arquivo. XML como parte do processo de renderização. Esse parâmetro é opcional para arquivos. XML do ACS. Quando o **exibição** comando é usado para processar um arquivo. XML do ACS, ele automaticamente tenta carregar a transformação padrão correto para o arquivo. XML especificado. Se uma transformação XSL for especificada, a transformação deve ser escrita com a premissa de que o arquivo. XML está no mesmo diretório que a transformação. xsl.|
|/?|Exibe a ajuda no prompt de comando.|

## <a name="remarks"></a>Comentários

Scwcmd.exe só está disponível em computadores que executam o Windows Server 2008 R2, Windows Server 2008 ou Windows Server 2003.

## <a name="BKMK_Examples"></a>Exemplos

Para exibir Policyfile.xml usando a transformação Policyview.xsl, digite:
```
scwcmd view /x:C:\policies\Policyfile.xml /s:C:\viewers\Policyview.xsl
```

#### <a name="additional-references"></a>Referências adicionais

-   [Chave de sintaxe de linha de comando](command-line-syntax-key.md)