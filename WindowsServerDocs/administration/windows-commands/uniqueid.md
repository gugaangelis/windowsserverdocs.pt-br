---
title: UniqueId
description: O tópico de comandos do Windows para UniqueId, que exibe ou define o identificador GPT (tabela de partição GUID) ou a assinatura MBR (registro mestre de inicialização) para o disco com foco.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 64235a4a-b91c-46da-b9b0-68ee90571c2a
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 29d7bf0498e76d5192e986aadabb77d575a8102b
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80832309"
---
# <a name="uniqueid"></a>UniqueId

Exibe ou define o identificador GPT (tabela de partição GUID) ou a assinatura MBR (registro mestre de inicialização) para o disco com foco.

> [!IMPORTANT]
> Esse comando do DiskPart não está disponível em nenhuma edição do Windows Vista.

## <a name="syntax"></a>Sintaxe

```
uniqueid disk [id={<dword> | <GUID>}] [noerr]
```

### <a name="parameters"></a>Parâmetros

|  Parâmetro   |                                                                                             Descrição                                                                                              |
|--------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| ID = {\<DWORD > |                                                                                               <GUID>}                                                                                                |
|    NOERR     | somente para scripts. Quando um erro é encontrado, o DiskPart continua processando comandos como se o erro não tivesse ocorrido. Sem esse parâmetro, um erro faz com que o DiskPart saia com um código de erro. |

## <a name="remarks"></a>Comentários

-   Esse comando funciona em discos básicos e dinâmicos.
-   Um disco deve ser selecionado para que esse comando tenha sucesso. Use o comando **selecionar disco** para selecionar um disco e deslocar o foco para ele.

## <a name="examples"></a><a name=BKMK_examples></a>Disso

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

## <a name="additional-references"></a>Referências adicionais

