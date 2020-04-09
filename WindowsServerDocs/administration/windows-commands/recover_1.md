---
title: recover
description: Tópico de comandos do Windows para * * * *-
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 8cc3a73d-9456-41a0-b375-2b4cc37c3992
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 4c9b691b2f0cbad101f7caeb63011724dcf7594d
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80836569"
---
# <a name="recover"></a>recover



Atualiza o estado de todos os discos em um grupo de discos, tenta recuperar discos em um grupo de discos inválido e sincroniza novamente os volumes espelhados e os volumes RAID-5 que têm dados obsoletos.

> [!IMPORTANT]
> Esse comando do DiskPart não está disponível em nenhuma edição do Windows Vista.

## <a name="syntax"></a>Sintaxe

```
recover [noerr]
```

### <a name="parameters"></a>Parâmetros

|Parâmetro|Descrição|
|---------|-----------|
|NOERR|somente para scripts. Quando um erro é encontrado, o DiskPart continua processando comandos como se o erro não tivesse ocorrido. Sem esse parâmetro, um erro faz com que o DiskPart saia com um código de erro.|

## <a name="remarks"></a>Comentários

-   Esse comando opera em um grupo de discos.
-   Esse comando aplica-se somente a grupos de discos dinâmicos. Se esse comando for usado em um grupo com um disco básico, ele não retornará um erro, mas nenhuma ação será executada.
-   Esse comando opera em discos que falharam ou falharam. Ele também opera em volumes que falharam, falham ou estão em estado de redundância com falha.
-   Um disco que faz parte de um grupo de discos deve ser selecionado para que esse comando seja executado com sucesso. Use o comando **selecionar disco** para selecionar um disco e deslocar o foco para ele.

## <a name="examples"></a><a name=BKMK_examples></a>Disso

Para recuperar o grupo de discos que contém o disco com foco, digite:
```
recover
```

## <a name="additional-references"></a>Referências adicionais

- [Chave da sintaxe de linha de comando](command-line-syntax-key.md)

