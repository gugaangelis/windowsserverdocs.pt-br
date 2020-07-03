---
title: uniqueid
description: Artigo de referência para UniqueId, que exibe ou define o identificador GPT (tabela de partição GUID) ou a assinatura MBR (registro mestre de inicialização) para o disco com foco.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 64235a4a-b91c-46da-b9b0-68ee90571c2a
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 5acf29d9a7dfd505a5ecdad2a08dfdb1a9f4d975
ms.sourcegitcommit: 2afed2461574a3f53f84fc9ec28d86df3b335685
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/02/2020
ms.locfileid: "85937280"
---
# <a name="uniqueid"></a>uniqueid

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
| ID = {\<dword> |                                                                                               <GUID>}                                                                                                |
|    NOERR     | Somente para scripts. Quando um erro é encontrado, o DiskPart continua processando comandos como se o erro não tivesse ocorrido. Sem esse parâmetro, um erro faz com que o DiskPart saia com um código de erro. |

## <a name="remarks"></a>Comentários

-   Esse comando funciona em discos básicos e dinâmicos.
-   Um disco deve ser selecionado para que esse comando tenha sucesso. Use o comando **selecionar disco** para selecionar um disco e deslocar o foco para ele.

## <a name="examples"></a>Exemplos

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

