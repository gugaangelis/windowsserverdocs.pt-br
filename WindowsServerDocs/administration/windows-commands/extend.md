---
title: extend
description: Artigo de referência para o comando Extend, que estende o volume ou a partição com foco e seu sistema de arquivos para o espaço livre (não alocado) em um disco.
ms.topic: article
ms.assetid: 2414e21d-fc0b-40e8-9e33-3e072f8ad76b
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: a64de5c0215568827b5440a3720946a86c7a891e
ms.sourcegitcommit: 53d526bfeddb89d28af44210a23ba417f6ce0ecf
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/06/2020
ms.locfileid: "87890386"
---
# <a name="extend"></a>extend

> Aplica-se a: Windows Server (canal semestral), Windows Server 2019, Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Estende o volume ou a partição com foco e seu sistema de arquivos para espaço livre (não alocado) em um disco.

## <a name="syntax"></a>Sintaxe

```
extend [size=<n>] [disk=<n>] [noerr]
extend filesystem [noerr]
```

### <a name="parameters"></a>Parâmetros

| Parâmetro | Descrição |
| --------- | ----------- |
| tamanho =`<n>` | Especifica a quantidade de espaço em megabytes (MB) a ser adicionada ao volume ou à partição atual. Se nenhum tamanho for fornecido, todo o espaço livre contíguo disponível no disco será usado. |
| disco =`<n>` | Especifica o disco no qual o volume ou a partição é estendida. Se nenhum disco for especificado, o volume ou a partição será estendido no disco atual. |
| filesystem | Estende o sistema de arquivos do volume com foco. Para uso somente em discos em que o sistema de arquivos não foi estendido com o volume. |
| NOERR | Somente para scripts. Quando um erro é encontrado, o DiskPart continua processando comandos como se o erro não tivesse ocorrido. Sem esse parâmetro, um erro faz com que o DiskPart saia com um código de erro. |

#### <a name="remarks"></a>Comentários

- Em discos básicos, o espaço livre deve estar no mesmo disco que o volume ou a partição com foco. Ele também deve seguir imediatamente o volume ou a partição com foco (ou seja, ele deve iniciar no próximo deslocamento do setor).

- Em discos dinâmicos com volumes simples ou estendidos, um volume pode ser estendido para qualquer espaço livre em qualquer disco dinâmico. Usando esse comando, você pode converter um volume dinâmico simples em um volume dinâmico estendido. Volumes espelhados, RAID-5 e distribuídos não podem ser estendidos.

- Se a partição tiver sido formatada anteriormente com o sistema de arquivos NTFS, o sistema de arquivos será estendido automaticamente para preencher a partição maior e não ocorrerá nenhuma perda de dados.

- Se a partição tiver sido formatada anteriormente com um sistema de arquivos diferente de NTFS, o comando falhará sem alteração na partição.

- Se a partição não tiver sido formatada anteriormente com um sistema de arquivos, a partição ainda será estendida.

- A partição deve ter um volume associado para que possa ser estendida.

## <a name="examples"></a>Exemplos

Para estender o volume ou a partição com foco em 500 megabytes, no disco 3, digite:

```
extend size=500 disk=3
```

Para estender o sistema de arquivos de um volume depois que ele foi estendido, digite:

```
extend filesystem
```

## <a name="additional-references"></a>Referências adicionais

- [Chave da sintaxe de linha de comando](command-line-syntax-key.md)
