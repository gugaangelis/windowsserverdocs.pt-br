---
title: ID exclusiva
description: 'Tópico de comandos do Windows para * * *- '
ms.custom: na
ms.prod: windows-server-threshold
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
ms.openlocfilehash: 4cad6607e13d2657433e4e78ce8e65beff73aa9d
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59857507"
---
# <a name="uniqueid"></a>ID exclusiva



Exibe ou define a GUID partição GPT (tabela) identificador mestre de inicialização MBR (registro) assinatura ou para o disco com foco.

> [!IMPORTANT]
> Esse comando DiskPart não está disponível em nenhuma edição do Windows Vista.

## <a name="syntax"></a>Sintaxe

```
uniqueid disk [id={<dword> | <GUID>}] [noerr]
```

## <a name="parameters"></a>Parâmetros

|Parâmetro|Descrição|
|---------|-----------|
|id={\<dword> | <GUID>}|Para discos MBR, especifica um valor de (DWORD) de quatro bytes em formato hexadecimal para a assinatura.</br>Para discos GPT, especifica um GUID para o identificador.|
|noerr|Somente para scripts. Quando um erro for encontrado, o DiskPart continua a processar comandos como se o erro não tivesse ocorrido. Sem esse parâmetro, um erro causar o DiskPart sair com um código de erro.|

## <a name="remarks"></a>Comentários

-   Esse comando funciona em discos básicos e dinâmicos.
-   Um disco deve ser selecionado para este comando ter êxito. Use o **Selecionar disco** comando para selecionar um disco e mudar o foco a ele.

## <a name="BKMK_examples"></a>Exemplos

Para exibir a assinatura do disco MBR com foco, digite:
```
uniqueid disk
```
Para definir a assinatura do disco MBR com foco 5f1b2c36, digite:
```
uniqueid disk id=5f1b2c36
```
Para definir o identificador do disco GPT com foco baf784e7-6bbd-4cfb-aaac-e86c96e166ee, digite:
```
uniqueid disk id=baf784e7-6bbd-4cfb-aaac-e86c96e166ee
```

#### <a name="additional-references"></a>Referências adicionais

