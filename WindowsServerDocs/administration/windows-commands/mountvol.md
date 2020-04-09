---
title: mountvol
description: Tópico de comandos do Windows para * * * *-
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: fea8ad4d-f04a-4aaa-a3e5-75931e867b39
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 34a98a273274f7982bfdd970710c04178fed4f5a
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80839379"
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

### <a name="parameters"></a>Parâmetros

|Parâmetro|Descrição|
|---------|-----------|
|[\<drive >:]<Path>|Especifica o diretório NTFS existente no qual o ponto de montagem residirá.|
|\<VolumeName >|Especifica o nome do volume que é o destino do ponto de montagem. O nome do volume usa a seguinte sintaxe, em que *GUID* é um identificador globalmente exclusivo:</br>`\\\\?\Volume\{GUID}\`</br>Os colchetes {} são necessários.|
|/d|Remove o ponto de montagem de volume da pasta especificada.|
|/l|Lista o nome do volume montado para a pasta especificada.|
|/p|Remove o ponto de montagem de volume do diretório especificado, desmonta o volume básico e coloca o volume básico offline, tornando-o não montável. Se outros processos estiverem usando o volume, o **Mountvol** fechará qualquer identificador aberto antes de desmontar o volume.|
|/r|Remove os diretórios de ponto de montagem de volume e configurações de registro para volumes que não estão mais no sistema, impedindo que eles sejam montados automaticamente e recebam seus antigos pontos de montagem de volume quando forem adicionados de volta ao sistema.|
|/n|Desabilita a montagem automática de novos volumes básicos. Novos volumes não são montados automaticamente quando adicionados ao sistema.|
|/e|Habilita novamente a montagem automática de novos volumes básicos.|
|/s|Monta a partição do sistema EFI na unidade especificada.|
|/?|Exibe a ajuda no prompt de comando.|

## <a name="remarks"></a>Comentários

-   O **Mountvol** permite vincular volumes sem a necessidade de uma letra de unidade.
-   Os volumes desmontados usando **/p** são listados na lista volumes como não montados até que um ponto de montagem de volume seja criado. Se o volume tiver mais de um ponto de montagem, use **/d** para remover os pontos de montagem adicionais antes de usar **/p**. Você pode tornar o volume básico montável novamente atribuindo um ponto de montagem de volume.
-   Se você precisar expandir o espaço do volume sem reformatar ou substituir um disco rígido, poderá adicionar um caminho de montagem a outro volume. O benefício de usar um volume com vários caminhos de montagem é que você pode acessar todos os volumes locais usando uma única letra de unidade (como `C:`). Você não precisa se lembrar de qual volume corresponde a qual letra de unidade — embora ainda possa montar volumes locais e atribuir a eles letras de unidade.

## <a name="examples"></a><a name=BKMK_examples></a>Disso

Para criar um ponto de montagem, digite:
```
mountvol \sysmount \\?\Volume\{2eca078d-5cbc-43d3-aff8-7e8511f60d0e}\
```

## <a name="additional-references"></a>Referências adicionais

- [Chave da sintaxe de linha de comando](command-line-syntax-key.md)
