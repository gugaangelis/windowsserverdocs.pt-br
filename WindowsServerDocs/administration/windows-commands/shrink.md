---
title: shrink
description: Artigo de referência do comando DiskPart Shrink, que reduz o tamanho do volume selecionado pelo valor especificado.
ms.topic: reference
ms.assetid: ec87cc7c-9846-465e-a10d-4ee10db4f4e6
ms.author: lizross
author: eross-msft
manager: mtillman
ms.date: 10/16/2017
ms.openlocfilehash: 8b03c006243f263a4f1c7fe991580d5e74f9a7a7
ms.sourcegitcommit: 720455aad2bac78cf64997d196a13f35ea0acb73
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 10/05/2020
ms.locfileid: "91718313"
---
# <a name="shrink"></a>shrink

> Aplica-se a: Windows Server (canal semestral), Windows Server 2019, Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

O comando de redução do DiskPart reduz o tamanho do volume selecionado pelo valor especificado. Esse comando disponibiliza o espaço livre em disco do espaço não utilizado no final do volume.

Um volume deve ser selecionado para que essa operação seja realizada com sucesso. Use o comando **selecionar volume** para selecionar um volume e deslocar o foco para ele.

> [!NOTE]
> Esse comando funciona em volumes básicos e em volumes dinâmicos simples ou estendidos. Ele não funciona em partições OEM (fabricante original do equipamento), partições de sistema EFI (Extensible Firmware Interface) ou partições de recuperação.

## <a name="syntax"></a>Sintaxe

```
shrink [desired=<n>] [minimum=<n>] [nowait] [noerr]
shrink querymax [noerr]
```

### <a name="parameters"></a>Parâmetros

| Parâmetro | Descrição |
|--|--|
| desejado =`<n>` | Especifica a quantidade desejada de espaço em megabytes (MB) para reduzir o tamanho do volume pelo. |
| mínimo =`<n>` | Especifica a quantidade mínima de espaço em MB para reduzir o tamanho do volume. |
| querymax | Retorna a quantidade máxima de espaço em MB pela qual o volume pode ser reduzido. Esse valor poderá ser alterado se os aplicativos estiverem acessando o volume no momento. |
| nowait | Força o comando a retornar imediatamente enquanto o processo de redução ainda está em andamento. |
| NOERR | Somente para scripts. Quando um erro é encontrado, o DiskPart continua processando comandos como se o erro não tivesse ocorrido. Sem esse parâmetro, um erro faz com que o DiskPart saia com um código de erro. |

#### <a name="remarks"></a>Comentários

- Você pode reduzir o tamanho de um volume somente se ele estiver formatado usando o sistema de arquivos NTFS ou se ele não tiver um sistema de arquivos.

- Se um valor desejado não for especificado, o volume será reduzido pelo valor mínimo (se especificado).

- Se uma quantidade mínima não for especificada, o volume será reduzido pelo valor desejado (se especificado).

- Se nenhum valor mínimo nem um valor desejado for especificado, o volume será reduzido pelo máximo possível.

- Se uma quantidade mínima for especificada, mas não houver espaço livre suficiente disponível, o comando falhará.

## <a name="examples"></a>Exemplos

Para reduzir o tamanho do volume selecionado pelo maior valor possível entre 250 e 500 megabytes, digite:

```
shrink desired=500 minimum=250
```

Para exibir o número máximo de MB pelo qual o volume pode ser reduzido, digite:

```
shrink querymax
```

## <a name="additional-references"></a>Referências adicionais

- [Chave da sintaxe de linha de comando](command-line-syntax-key.md)

- [Resize-Partition](/powershell/module/storage/resize-partition?view=win10-ps&preserve-view=true)
