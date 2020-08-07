---
title: create partition efi
description: Artigo de referência para o comando Create Partition EFI, que cria uma partição de sistema de EFI (Extensible Firmware Interface) em um disco GPT (tabela de partição GUID) em computadores baseados em Itanium.
ms.topic: article
ms.assetid: 3cfc1fca-6515-4a4d-bfae-615fa8045ea9
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: e0307410648453a42c66e7327b5c671a702017e2
ms.sourcegitcommit: 53d526bfeddb89d28af44210a23ba417f6ce0ecf
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/06/2020
ms.locfileid: "87880006"
---
# <a name="create-partition-efi"></a>create partition efi

> Aplica-se a: Windows Server (canal semestral), Windows Server 2019, Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Cria uma partição de sistema de EFI (Extensible Firmware Interface) em um disco GPT (tabela de partição GUID) em computadores baseados em Itanium. Depois que a partição é criada, o foco é fornecido para a nova partição.

>[!NOTE]
> Um disco GPT deve ser selecionado para que essa operação tenha sucesso. Use o comando [selecionar disco](select-disk.md) para selecionar um disco e deslocar o foco para ele.

## <a name="syntax"></a>Sintaxe

```
create partition efi [size=<n>] [offset=<n>] [noerr]
```

### <a name="parameters"></a>Parâmetros

| Parâmetro | Descrição |
| --------- | ----------- |
| tamanho =`<n>` | O tamanho da partição em megabytes (MB). Se nenhum tamanho for fornecido, a partição continuará até que não haja mais espaço livre na região atual. |
| deslocamento =`<n>` | O deslocamento em kilobytes (KB), no qual a partição é criada. Se nenhum deslocamento for fornecido, a partição será colocada na primeira extensão de disco grande o suficiente para contê-la. |
| NOERR | Somente para scripts. Quando um erro é encontrado, o DiskPart continua processando comandos como se o erro não tivesse ocorrido. Sem esse parâmetro, um erro faz com que o DiskPart saia com um código de erro. |

#### <a name="remarks"></a>Comentários

- Você deve adicionar pelo menos um volume com o comando **Add volume** antes de poder usar o comando **Create** .

- Depois de executar o comando **Create** , você pode usar o comando **exec** para executar um script de duplicação para backup da cópia de sombra.

- Você pode usar o comando **Iniciar backup** para especificar um backup completo, em vez de um backup de cópia.

## <a name="examples"></a>Exemplos

Para criar uma partição EFI de 1000 megabytes no disco selecionado, digite:

```
create partition efi size=1000
```

## <a name="additional-references"></a>Referências adicionais

- [Chave da sintaxe de linha de comando](command-line-syntax-key.md)

- [comando Create](create.md)

- [select disk](select-disk.md)
