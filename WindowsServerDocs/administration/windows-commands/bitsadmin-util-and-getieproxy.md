---
title: Bitsadmin util e getieproxy
description: O tópico de comandos do Windows para **Bitsadmin util e getieproxy** -recupera o uso de proxy para a conta de serviço determinada.
ms.custom: na
ms.prod: windows-server
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
ms.openlocfilehash: 6936e088ddcf467b5a8f931bc8217ba9da4662c2
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71380274"
---
# <a name="bitsadmin-util-and-getieproxy"></a>Bitsadmin util e getieproxy

> Aplica-se a: Windows Server (canal semestral), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Recupera o uso de proxy para a conta de serviço determinada.

## <a name="syntax"></a>Sintaxe

```
bitsadmin /Util /GetIEProxy <Account> [/Conn <ConnectionName>]
```

## <a name="parameters"></a>Parâmetros

|Parâmetro|Descrição|
|-------|--------|
|Conta|Especifica a conta de serviço cujas configurações de proxy você deseja recuperar. Os valores possíveis são:<br /><br />-LOCALSYSTEM<br />-NETWORKSERVICE<br />-LOCALSERVICE|
|ConnectionName|Opcional usado com o parâmetro **/Conn** para especificar a conexão de modem a ser usada. Se você não especificar o parâmetro **/Conn** , o bits usará a conexão LAN. Especifique o nome da conexão do modem imediatamente após o parâmetro **/Conn** .|

## <a name="remarks"></a>Comentários

Essa opção mostra o valor para cada uso de proxy, não apenas o uso de proxy especificado para a conta de serviço. Para obter detalhes sobre como definir o uso de proxy para contas de serviço, consulte a opção [Bitsadmin util e setieproxy](bitsadmin-util-and-setieproxy.md) .

## <a name="BKMK_examples"></a>Disso

O exemplo a seguir exibe o uso de proxy para a conta de serviço de rede.

```
C:\>bitsadmin /Util /GetIEProxy NETWORKSERVICE
```

## <a name="additional-references"></a>Referências adicionais

[Chave da sintaxe de linha de comando](command-line-syntax-key.md)
