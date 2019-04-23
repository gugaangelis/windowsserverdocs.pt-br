---
title: GETIEPROXY e bitsadmin util
description: Tópico de comandos do Windows para **util bitsadmin e getieproxy** -recupera o uso do proxy para a conta de serviço em questão.
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 6d50c7e3-f4eb-4ca5-9f0c-4ed396087db6
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: de4f86340b1163c4d8e3286d9c86c9df794a21c5
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59876867"
---
# <a name="bitsadmin-util-and-getieproxy"></a>GETIEPROXY e bitsadmin util

> Aplica-se a: Windows Server (canal semestral), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Recupera o uso do proxy para a conta de serviço em questão.

## <a name="syntax"></a>Sintaxe

```
bitsadmin /Util /GetIEProxy <Account> [/Conn <ConnectionName>]
```

## <a name="parameters"></a>Parâmetros

|Parâmetro|Descrição|
|-------|--------|
|Conta|Especifica a conta de serviço cujas configurações de proxy que você deseja recuperar. Os valores possíveis são:<br /><br />-   LOCALSYSTEM<br />-NETWORKSERVICE<br />-LOCALSERVICE|
|ConnectionName|Opcional usado com o **/conn** parâmetro para especificar a conexão de modem para usar. Se você não especificar o **/conn** parâmetro, o BITS usa a conexão de rede local. Especifique o nome de conexão de modem imediatamente após o **/conn** parâmetro.|

## <a name="remarks"></a>Comentários

Essa opção mostra o valor para cada uso do proxy, não apenas o uso do proxy especificado para a conta de serviço. Para obter detalhes sobre como definir o uso do proxy para contas de serviço, consulte o [util bitsadmin e setieproxy](bitsadmin-util-and-setieproxy.md) alternar.

## <a name="BKMK_examples"></a>Exemplos

O exemplo a seguir exibe o uso de proxy para a conta de serviço de rede.

```
C:\>bitsadmin /Util /GetIEProxy NETWORKSERVICE
```

## <a name="additional-references"></a>Referências adicionais

[Chave de sintaxe de linha de comando](command-line-syntax-key.md)
