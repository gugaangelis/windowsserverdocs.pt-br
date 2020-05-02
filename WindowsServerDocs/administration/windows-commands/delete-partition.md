---
title: Excluir partição
description: Tópico de referência para excluir partição, que exclui a partição com foco.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 65752312-cb16-46f6-870f-1b95c507b101
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 259abdfc6e3ba8db22d5582bff08d7a4bc8b807b
ms.sourcegitcommit: ab64dc83fca28039416c26226815502d0193500c
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 05/01/2020
ms.locfileid: "82716724"
---
# <a name="delete-partition"></a>Excluir partição

Exclui a partição com foco.

## <a name="syntax"></a>Sintaxe

```
delete partition [noerr] [override]
```

### <a name="parameters"></a>Parâmetros

|Parâmetro|Descrição|
|---------|-----------|
|override|Permite que o DiskPart exclua qualquer partição, independentemente do tipo. Normalmente, o DiskPart só permite que você exclua partições de dados conhecidas.|
|NOERR|Somente para scripts. Quando um erro é encontrado, o DiskPart continua processando comandos como se o erro não tivesse ocorrido. Sem esse parâmetro, um erro faz com que o DiskPart saia com um código de erro.|

## <a name="remarks"></a>Comentários

> [!CAUTION]
> A exclusão de uma partição em um disco dinâmico pode excluir todos os volumes dinâmicos no disco, destruindo os dados e deixando o disco em um estado corrompido. Para excluir um volume dinâmico, sempre use o comando **delete volume** . As partições podem ser excluídas de discos dinâmicos, mas não devem ser criadas. Por exemplo, é possível excluir uma partição GPT (tabela de partição GUID) não reconhecida em um disco GPT dinâmico. Excluir tal partição não faz com que o espaço livre resultante fique disponível. Este comando destina-se a permitir a recuperação de espaço em um disco dinâmico offline corrompido em uma situação de emergência em que o comando **Clean** no DiskPart não pode ser usado.
> -   Não é possível excluir a partição do sistema, a partição de inicialização ou qualquer partição que contenha o arquivo de paginação ativo ou informações de despejo de memória.
> -   Uma partição deve ser selecionada para que essa operação seja realizada com sucesso. Use o comando **selecionar partição** para selecionar uma partição e deslocar o foco para ela.

## <a name="examples"></a>Exemplos

Para excluir a partição com foco, digite:
```
delete partition
```

## <a name="additional-references"></a>Referências adicionais

- [Chave da sintaxe de linha de comando](command-line-syntax-key.md)

