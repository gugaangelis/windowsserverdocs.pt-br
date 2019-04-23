---
title: attrib
description: Tópico de comandos do Windows para **attrib** -exibe, define ou Remove atributos atribuídos a arquivos ou diretórios.
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 5e763ca5-21a2-45d2-b26d-a9c44c99091a
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 0f640af2c7957e43dd3f31dfa732bfc887112651
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59841087"
---
# <a name="attrib"></a>attrib



Exibe, define ou remove os atributos atribuídos a arquivos ou diretórios. Se usado sem parâmetros, **attrib** exibe os atributos de todos os arquivos no diretório atual.

Para obter exemplos de como usar esse comando, consulte [Exemplos](#BKMK_examples).

## <a name="syntax"></a>Sintaxe

```
attrib [{+|-}r] [{+|-}a] [{+|-}s] [{+|-}h] [{+|-}i] [<Drive>:][<Path>][<FileName>] [/s [/d] [/l]]
```

## <a name="parameters"></a>Parâmetros

|Parâmetro|Descrição|
|---------|-----------|
|{+\|-}r|Conjuntos (**+**) ou limpa (**-**) o atributo de arquivo somente leitura.|
|{+\|-}a|Conjuntos (**+**) ou limpa (**-**) o atributo de arquivo.|
|{+\|-}s|Conjuntos (**+**) ou limpa (**-**) o atributo de arquivo do sistema.|
|{+\|-}h|Conjuntos (**+**) ou limpa (**-**) o atributo de arquivo oculto.|
|{+\|-}i|Conjuntos (**+**) ou limpa (**-**) o atributo de arquivo não conteúdo indexado.|
|[\<Drive>:][<Path>][<FileName>]|Especifica o local e o nome do diretório, arquivo ou grupo de arquivos para o qual você deseja exibir ou alterar os atributos. Você pode usar o **?** e **&#42;** caracteres curinga no *FileName* parâmetro para exibir ou alterar os atributos de um grupo de arquivos.|
|/s|Aplica-se **attrib** e quaisquer opções de linha de comando para arquivos correspondentes no diretório atual e todos os seus subdiretórios.|
|/d|Aplica-se **attrib** e quaisquer opções de linha de comando para diretórios.|
|/l|Aplica-se **attrib** e quaisquer opções de linha de comando para o Link simbólico, em vez do destino do Link simbólico.|
|/?|Exibe a ajuda no prompt de comando.|

## <a name="remarks"></a>Comentários

-   Você pode usar caracteres curinga (**?** e **&#42;**) com o *FileName* parâmetro para exibir ou alterar os atributos de um grupo de arquivos.
-   Se um arquivo tiver o sistema (**s**) ou oculto (**h**) conjunto de atributos, você deve limpar o atributo antes de alterar todos os outros atributos para esse arquivo.
-   O atributo Archive (**um**) marca os arquivos que foram alterados desde a última vez em que foi feito o backup. Observe que o **xcopy** usa atributos de arquivo de comando.

## <a name="BKMK_examples"></a>Exemplos

Para exibir os atributos de um arquivo denominado INFOS86 está localizado no diretório atual, digite:
```
attrib news86 
```
Para atribuir o atributo somente leitura para o arquivo denominado report. txt, digite:
```
attrib +r report.txt 
```
Para remover o atributo somente leitura de arquivos na pasta pública e seus subdiretórios em um disco na unidade B, digite:
```
attrib -r b:\public\*.* /s 
```
Para definir o atributo de arquivo morto para todos os arquivos da unidade A e, em seguida, desmarque o atributo de arquivo para arquivos com a extensão. bak, digite:
```
attrib +a a:*.* & attrib -a a:*.bak 
```
