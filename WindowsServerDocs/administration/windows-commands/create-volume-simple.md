---
title: create volume simple
description: Artigo de referência para o comando Create volume simples, que cria um volume simples no disco dinâmico especificado.
ms.topic: article
ms.assetid: da0f208d-7fda-471a-9db2-5de5ba5207c6
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: a434cc959eac79011cf57e2aca101ffc536b7633
ms.sourcegitcommit: 53d526bfeddb89d28af44210a23ba417f6ce0ecf
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/06/2020
ms.locfileid: "87891623"
---
# <a name="create-volume-simple"></a>create volume simple

> Aplica-se a: Windows Server (canal semestral), Windows Server 2019, Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Cria um volume simples no disco dinâmico especificado. Depois de criar o volume, o foco mudará automaticamente para o novo volume.

## <a name="syntax"></a>Sintaxe

```
create volume simple [size=<n>] [disk=<n>] [align=<n>] [noerr]
```

### <a name="parameters"></a>Parâmetros

| Parâmetro | Descrição |
| --------- | ----------- |
| tamanho =`<n>`  | O tamanho do volume em megabytes (MB). Se nenhum tamanho for fornecido, o novo volume ocupará o espaço livre restante no disco. |
| disco =`<n>`  | O disco dinâmico no qual o volume é criado. Se nenhum disco for especificado, o disco atual será usado. |
| align =`<n>` | Alinha todas as extensões de volume ao limite de alinhamento mais próximo. Normalmente usado com matrizes de LUN (número de unidade lógica) RAID de hardware para melhorar o desempenho. `<n>`é o número de kilobytes (KB) desde o início do disco até o limite de alinhamento mais próximo. |
| NOERR | Somente para scripts. Quando um erro é encontrado, o DiskPart continua processando comandos como se o erro não tivesse ocorrido. Sem esse parâmetro, um erro faz com que o DiskPart saia com um código de erro. |

## <a name="examples"></a>Exemplos

Para criar um volume de 1000 megabytes de tamanho, no disco 1, digite:

```
create volume simple size=1000 disk=1
```

## <a name="additional-references"></a>Referências adicionais

- [Chave da sintaxe de linha de comando](command-line-syntax-key.md)

- [comando Create](create.md)
