---
title: Excluir partição
description: 'Tópico de comandos do Windows para * * *- '
ms.custom: na
ms.prod: windows-server-threshold
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
ms.openlocfilehash: bf9f456ad6ab3010493154da843b2b519754e250
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59816327"
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
|Substituir|Permite que o DiskPart excluir qualquer partição, independentemente do tipo. Normalmente, o DiskPart só permite que você exclua as partições de dados conhecidos.|
|noerr|Somente para scripts. Quando um erro for encontrado, o DiskPart continua a processar comandos como se o erro não tivesse ocorrido. Sem esse parâmetro, um erro causar o DiskPart sair com um código de erro.|

## <a name="remarks"></a>Comentários

> [!CAUTION]
> Exclusão de uma partição em um disco dinâmico pode excluir todos os volumes dinâmicos no disco, portanto, destruir todos os dados e deixando o disco em um estado corrompido. Para excluir um volume dinâmico, sempre use a **Excluir volume** comando em vez disso. As partições podem ser excluídas de discos dinâmicos, mas não devem ser criadas. Por exemplo, é possível excluir uma partição de tabela de partição GUID (GPT) não reconhecido em um disco GPT dinâmico. A exclusão dessa partição não faz com que o espaço livre resultante se torne disponível. Este comando destina-se a permitir que você ao espaço de reclame em um disco dinâmico offline corrompido em uma situação de emergência em que o **limpa** comando DiskPart não pode ser usado.
-   Você não pode excluir a partição do sistema, partição de inicialização ou qualquer partição que contém os Active Directory falha ou arquivo de despejo informações de paginação.
-   Uma partição deve ser selecionada para essa operação seja bem-sucedida. Use o **Selecionar partição** comando para selecionar uma partição e mudar o foco a ele.

## <a name="BKMK_examples"></a>Exemplos

Para excluir a partição com foco, digite:
```
delete partition
```

#### <a name="additional-references"></a>Referências adicionais

[Chave de sintaxe de linha de comando](command-line-syntax-key.md)

