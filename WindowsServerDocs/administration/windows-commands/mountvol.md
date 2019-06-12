---
title: mountvol
description: 'Tópico de comandos do Windows para * * *- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: fea8ad4d-f04a-4aaa-a3e5-75931e867b39
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 03e7cefc7c7a00338972fc365b7c25d9c795c83e
ms.sourcegitcommit: eaf071249b6eb6b1a758b38579a2d87710abfb54
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 05/31/2019
ms.locfileid: "66437285"
---
# <a name="mountvol"></a>mountvol



Cria, exclui ou lista um ponto de montagem de volume.

Para obter exemplos de como usar esse comando, consulte [exemplos](#BKMK_examples).

## <a name="syntax"></a>Sintaxe

```
mountvol [<Drive>:]<Path VolumeName>
mountvol [<Drive>:]<Path> /d
mountvol [<Drive>:]<Path> /l
mountvol [<Drive>:]<Path> /p
mountvol /r
mountvol [/n | /e]
mountvol <Drive>: /s
```

## <a name="parameters"></a>Parâmetros

|     Parâmetro     |                                                                                                                           Descrição                                                                                                                            |
|-------------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| [\<Drive>:]<Path> |                                                                                             Especifica a pasta NTFS existente em que o ponto de montagem residirá.                                                                                             |
|   \<VolumeName>   |                     Especifica o nome do volume que é o destino do ponto de montagem. O nome do volume usa a sintaxe a seguir, onde *GUID* é um identificador globalmente exclusivo:</br>`\\\\?\Volume\{GUID}\`</br>O {} colchetes são necessários.                      |
|        /d         |                                                                                                    Remove o ponto de montagem do volume da pasta especificada.                                                                                                     |
|        /l         |                                                                                                     Lista o nome do volume montado para a pasta especificada.                                                                                                      |
|        /p         | Remove o ponto de montagem de volume do diretório especificado, desmonta o volume básico e coloca o volume básico off-line, tornando-o não montado. Se outros processos estão usando o volume **mountvol** fecha os identificadores abertos antes de desmontar o volume. |
|        /r         |             Remove os diretórios de ponto de montagem de volume e configurações do registro para volumes que não estão mais no sistema, impedindo que sejam montados automaticamente e recebam seu antigo montagem de volume (s) quando adicionado de volta ao sistema.              |
|        /n         |                                                                      Desabilita a montagem automática de novos volumes básicos. Novos volumes não são montados automaticamente quando adicionados ao sistema.                                                                       |
|        /e         |                                                                                                       Habilita novamente a montagem automática de novos volumes básicos.                                                                                                        |
|        /s         |                                                                                Monta a partição do sistema EFI na unidade especificada. Disponível em apenas a computadores baseados em Itanium.                                                                                |
|        /?         |                                                                                                               Exibe a ajuda no prompt de comando.                                                                                                               |

## <a name="remarks"></a>Comentários

-   **Mountvol** permite que você vincule volumes sem a necessidade de uma letra de unidade.
-   Volumes que são desmontados por meio **/p** são listados na lista de volumes, como "Não MOUNTED até um VOLUME de ponto de montagem é criado." Se o volume tiver mais de uma montagem de ponto, use **/d** para remover os pontos de montagem adicionais antes de usar **p**. Você pode tornar o volume básico montável novamente atribuindo um ponto de montagem de volume.
-   Se você precisar expandir o espaço do volume sem reformatar ou substituir um disco rígido, você pode adicionar um caminho de montagem para outro volume. A vantagem de usar um volume com vários caminhos de montagem é que você pode acessar todos os volumes locais usando uma única letra de unidade (como `C:`). Você não precisa se lembrar que volume corresponde a qual letra de unidade — Embora você ainda pode montar volumes locais e atribuí-los em letras de unidade.

## <a name="BKMK_examples"></a>Exemplos

Para criar um ponto de montagem, digite:
```
mountvol \sysmount \\?\Volume\{2eca078d-5cbc-43d3-aff8-7e8511f60d0e}\
```

#### <a name="additional-references"></a>Referências adicionais

[Chave da sintaxe de linha de comando](command-line-syntax-key.md)