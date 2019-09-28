---
title: Excluir partição
description: 'Tópico de comandos do Windows para * * * *- '
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 65752312-cb16-46f6-870f-1b95c507b101
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 46a214f26e7c21f6ae08eb16d95fd898bd949b0f
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71378657"
---
# <a name="delete-partition"></a>Excluir partição



Exclui a partição com foco.

## <a name="syntax"></a>Sintaxe

```
delete partition [noerr] [override]
```

## <a name="parameters"></a>Parâmetros

|Parâmetro|Descrição|
|---------|-----------|
|substituição|Permite que o DiskPart exclua qualquer partição, independentemente do tipo. Normalmente, o DiskPart só permite que você exclua partições de dados conhecidas.|
|NOERR|Somente para scripts. Quando um erro é encontrado, o DiskPart continua processando comandos como se o erro não tivesse ocorrido. Sem esse parâmetro, um erro faz com que o DiskPart saia com um código de erro.|

## <a name="remarks"></a>Comentários

> [!CAUTION]
> A exclusão de uma partição em um disco dinâmico pode excluir todos os volumes dinâmicos no disco, destruindo os dados e deixando o disco em um estado corrompido. Para excluir um volume dinâmico, sempre use o comando **delete volume** . As partições podem ser excluídas de discos dinâmicos, mas não devem ser criadas. Por exemplo, é possível excluir uma partição GPT (tabela de partição GUID) não reconhecida em um disco GPT dinâmico. Excluir tal partição não faz com que o espaço livre resultante fique disponível. Este comando destina-se a permitir a recuperação de espaço em um disco dinâmico offline corrompido em uma situação de emergência em que o comando **Clean** no DiskPart não pode ser usado.
> -   Não é possível excluir a partição do sistema, a partição de inicialização ou qualquer partição que contenha o arquivo de paginação ativo ou informações de despejo de memória.
> -   Uma partição deve ser selecionada para que essa operação seja realizada com sucesso. Use o comando **selecionar partição** para selecionar uma partição e deslocar o foco para ela.

## <a name="BKMK_examples"></a>Disso

Para excluir a partição com foco, digite:
```
delete partition
```

#### <a name="additional-references"></a>Referências adicionais

[Chave da sintaxe de linha de comando](command-line-syntax-key.md)

