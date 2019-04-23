---
title: convert gpt
description: 'Tópico de comandos do Windows para * * *- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: b3b1b747-0a7a-4be2-8487-2c4be16ee190
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: e838f68162b6faabf2ecbc7dea2ce840235890c0
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59859107"
---
# <a name="convert-gpt"></a>convert gpt



Converte um disco básico vazio com o estilo de partição (MBR) em um disco básico com o estilo de partição GPT (tabela).

Para obter instruções sobre como usar esse comando, consulte [alterar um disco de registro mestre de inicialização em um disco de tabela de partição GUID](https://go.microsoft.com/fwlink/?LinkId=207049) (https://go.microsoft.com/fwlink/?LinkId=207049).

## <a name="syntax"></a>Sintaxe

```
convert gpt [noerr]
```

## <a name="parameters"></a>Parâmetros

|Parâmetro|Descrição|
|---------|-----------|
|noerr|Somente para scripts. Quando um erro for encontrado, o DiskPart continua a processar comandos como se o erro não tivesse ocorrido. Sem esse parâmetro, um erro causar o DiskPart sair com um código de erro.|

## <a name="remarks"></a>Comentários

> [!IMPORTANT]
> O disco deve estar vazio para convertê-lo em um disco GPT. Fazer backup dos dados e, em seguida, exclua todas as partições ou volumes antes de converter o disco.
-   O tamanho de disco mínimo necessário para a conversão em GPT é de 128 MB.
-   Um disco MBR básico deve ser selecionado para essa operação seja bem-sucedida. Use o **Selecionar disco** comando para selecionar um disco básico e mudar o foco a ele.

## <a name="BKMK_examples"></a>Exemplos

Para converter um disco básico do estilo de partição MBR em estilo de partição GPT, digite:
```
convert gpt
```

#### <a name="additional-references"></a>Referências adicionais

[Chave de sintaxe de linha de comando](command-line-syntax-key.md)

