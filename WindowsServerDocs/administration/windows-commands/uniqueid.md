---
title: UniqueId
description: 'Tópico de comandos do Windows para * * * *- '
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 64235a4a-b91c-46da-b9b0-68ee90571c2a
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 64f097766daa4c87ec84f42dd53f49792a160bb9
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71363912"
---
# <a name="uniqueid"></a>UniqueId



Exibe ou define o identificador GPT (tabela de partição GUID) ou a assinatura MBR (registro mestre de inicialização) para o disco com foco.

> [!IMPORTANT]
> Esse comando do DiskPart não está disponível em nenhuma edição do Windows Vista.

## <a name="syntax"></a>Sintaxe

```
uniqueid disk [id={<dword> | <GUID>}] [noerr]
```

## <a name="parameters"></a>Parâmetros

|  Parâmetro   |                                                                                             Descrição                                                                                              |
|--------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| ID = {\<dword > |                                                                                               <GUID>}                                                                                                |
|    NOERR     | Somente para scripts. Quando um erro é encontrado, o DiskPart continua processando comandos como se o erro não tivesse ocorrido. Sem esse parâmetro, um erro faz com que o DiskPart saia com um código de erro. |

## <a name="remarks"></a>Comentários

-   Esse comando funciona em discos básicos e dinâmicos.
-   Um disco deve ser selecionado para que esse comando tenha sucesso. Use o comando **selecionar disco** para selecionar um disco e deslocar o foco para ele.

## <a name="BKMK_examples"></a>Disso

Para exibir a assinatura do disco MBR com foco, digite:
```
uniqueid disk
```
Para definir a assinatura do disco MBR com foco para 5f1b2c36, digite:
```
uniqueid disk id=5f1b2c36
```
Para definir o identificador do disco GPT com foco para baf784e7-6bbd-4cfb-AAAC-e86c96e166ee, digite:
```
uniqueid disk id=baf784e7-6bbd-4cfb-aaac-e86c96e166ee
```

#### <a name="additional-references"></a>Referências adicionais

