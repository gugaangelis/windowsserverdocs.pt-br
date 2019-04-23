---
title: Disco de atributos
description: Tópico de comandos do Windows para **disco atributos** -exibe, define ou limpa os atributos de um disco.
ms.custom: na
ms.prod: windows-server-threshold
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
ms.openlocfilehash: 7cacc2fb6b47d095f5e452ca470c89f228949594
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59890347"
---
# <a name="attributes-disk"></a>Disco de atributos



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
|somente leitura|Especifica que o disco é somente leitura.|
|noerr|Somente para scripts. Quando um erro for encontrado, o DiskPart continua a processar comandos como se o erro não tivesse ocorrido. Sem esse parâmetro, um erro causar o DiskPart sair com um código de erro.|

## <a name="remarks"></a>Comentários

-   Quando **disco atributos** é usado para exibir os atributos atuais de um disco, o atributo de disco de inicialização denota o disco que é usado para iniciar o computador. Para um espelho dinâmico, ele será exibido para o disco que contém o plex de inicialização do volume de inicialização.
-   Um disco deve ser selecionado para o **disco atributos** comando tenha êxito. Use o **Selecionar disco** comando para selecionar um disco e mudar o foco a ele.

## <a name="BKMK_examples"></a>Exemplos

Para exibir os atributos do disco selecionado, digite:
```
attributes disk
```
Para definir o disco selecionado como somente leitura, digite:
```
attributes disk set readonly
```

#### <a name="additional-references"></a>Referências adicionais

[Chave de sintaxe de linha de comando](command-line-syntax-key.md)

