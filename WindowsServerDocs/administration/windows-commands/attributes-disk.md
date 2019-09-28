---
title: disco de atributos
description: Tópico de comandos do Windows para **atributos disco** – exibe, define ou limpa os atributos de um disco.
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: eed57071-c1c6-4394-9542-62b52a878c92
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 415125208b13d82adeed736107f59fda9489a953
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71382574"
---
# <a name="attributes-disk"></a>disco de atributos



Exibe, define ou limpa os atributos de um disco.

> [!IMPORTANT]
> Esse parâmetro não está disponível em nenhuma edição do Windows Vista.

## <a name="syntax"></a>Sintaxe

```
attributes disk [{set | clear}] [readonly] [noerr]
```

## <a name="parameters"></a>Parâmetros

|Parâmetro|Descrição|
|---------|-----------|
|set|Define o atributo especificado do disco com foco.|
|clear|Limpa o atributo especificado do disco com foco.|
|readonly|Especifica que o disco é somente leitura.|
|NOERR|Somente para scripts. Quando um erro é encontrado, o DiskPart continua processando comandos como se o erro não tivesse ocorrido. Sem esse parâmetro, um erro faz com que o DiskPart saia com um código de erro.|

## <a name="remarks"></a>Comentários

-   Quando o **disco de atributos** é usado para exibir os atributos atuais de um disco, o atributo de disco de inicialização denota o disco que é usado para iniciar o computador. Para um espelho dinâmico, ele é exibido para o disco que contém o Plex de inicialização do volume de inicialização.
-   Um disco deve ser selecionado para que o comando **atributos de disco** tenha sucesso. Use o comando **selecionar disco** para selecionar um disco e deslocar o foco para ele.

## <a name="BKMK_examples"></a>Disso

Para exibir os atributos do disco selecionado, digite:
```
attributes disk
```
Para definir o disco selecionado como somente leitura, digite:
```
attributes disk set readonly
```

#### <a name="additional-references"></a>Referências adicionais

[Chave da sintaxe de linha de comando](command-line-syntax-key.md)

