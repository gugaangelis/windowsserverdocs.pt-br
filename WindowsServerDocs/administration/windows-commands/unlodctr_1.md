---
title: unlodctr
description: Artigo de referência para o Unlodctr, que remove nomes de contadores de desempenho e texto explicativo para um serviço ou driver de dispositivo do registro do sistema
ms.topic: reference
ms.assetid: fc8aa6f0-c1d9-47ea-937a-28152148e774
ms.author: lizross
author: eross-msft
manager: mtillman
ms.date: 10/16/2017
ms.openlocfilehash: e0494d0b4509c4b75af9ca8473e3ae65353e02c1
ms.sourcegitcommit: db2d46842c68813d043738d6523f13d8454fc972
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/10/2020
ms.locfileid: "89626565"
---
# <a name="unlodctr"></a>unlodctr

> Aplica-se a: Windows Server (canal semestral), Windows Server 2019, Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Remove os nomes de **contadores de desempenho** e o texto **explicativo** para um serviço ou driver de dispositivo do registro do sistema.

## <a name="syntax"></a>Sintaxe
```
Unlodctr <DriverName>
```
#### <a name="parameters"></a>Parâmetros
|Parâmetro|Descrição|
|-------|--------|
|\<DriverName>|Remove as configurações do nome do contador de desempenho e o texto explicativo para driver ou serviço <DriverName> do registro do Windows Server 2003.|
|/?|Exibe a ajuda no prompt de comando.|

## <a name="remarks"></a>Comentários
> [!WARNING]
> A edição incorreta do Registro pode causar danos graves ao sistema. Antes de alterar o Registro, faça backup de todos os dados importantes do computador.

Se as informações fornecidas contiverem espaços, use aspas em volta do texto (por exemplo, <DriverName> ).

## <a name="examples"></a>Exemplos
Para remover as configurações atuais do registro de desempenho e o texto explicativo do contador para o serviço SMTP:
```
unlodctr SMTPSVC
```
## <a name="additional-references"></a>Referências adicionais
- [Chave da sintaxe de linha de comando](command-line-syntax-key.md)

