---
title: disco offline
description: Tópico de comandos do Windows para * * * *-
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 8fb9b3c3-0b2c-4192-a2e7-f706292653e3
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: bd7991f1f5967970690c7051612395fb47a764ec
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80837979"
---
# <a name="offline-disk"></a>disco offline



Coloca o disco online com foco no estado offline.

> [!IMPORTANT]
> Esse comando do DiskPart não está disponível em nenhuma edição do Windows Vista.

## <a name="syntax"></a>Sintaxe

```
offline disk [noerr]
```

### <a name="parameters"></a>Parâmetros

|Parâmetro|Descrição|
|---------|-----------|
|NOERR|somente para scripts. Quando um erro é encontrado, o DiskPart continua processando comandos como se o erro não tivesse ocorrido. Sem esse parâmetro, um erro faz com que o DiskPart saia com um código de erro.|

## <a name="remarks"></a>Comentários

-   Esse comando opera em discos que estão no modo SAN online. Ele altera seu modo SAN para offline.
-   Se um disco dinâmico em um grupo de discos for colocado offline, o status do disco será alterado para **ausente** e o grupo mostrará um disco que está offline. O disco ausente é movido para o grupo inválido. Se o disco dinâmico for o último disco do grupo, o status do disco será alterado para **offline**e o grupo vazio será removido.
-   Um disco deve ser selecionado para que o comando de **disco offline** seja executado com sucesso. Use o comando **selecionar disco** para selecionar um disco e deslocar o foco para ele.

## <a name="examples"></a><a name=BKMK_examples></a>Disso

Para colocar o disco com foco offline, digite:
```
offline disk
```

## <a name="additional-references"></a>Referências adicionais

- [Chave da sintaxe de linha de comando](command-line-syntax-key.md)

