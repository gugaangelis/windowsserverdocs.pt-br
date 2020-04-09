---
title: Bitsadmin util e getieproxy
description: Tópico de comandos do Windows para Bitsadmin util e getieproxy, que recupera o uso de proxy para a conta de serviço específica.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 6d50c7e3-f4eb-4ca5-9f0c-4ed396087db6
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: d0d2a8634f3b42d655632a280cb9b998111c800b
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80848969"
---
# <a name="bitsadmin-util-and-getieproxy"></a>Bitsadmin util e getieproxy

> Aplicável a: Windows Server (canal semestral), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Recupera o uso de proxy para a conta de serviço determinada.

## <a name="syntax"></a>Sintaxe

```
bitsadmin /Util /GetIEProxy <Account> [/Conn <ConnectionName>]
```

### <a name="parameters"></a>Parâmetros

|Parâmetro|Descrição|
|-------|--------|
|Account|Especifica a conta de serviço cujas configurações de proxy você deseja recuperar. Os valores possíveis são:<p>-LOCALSYSTEM<br />-NETWORKSERVICE<br />-LOCALSERVICE|
|ConnectionName|Opcional usado com o parâmetro **/Conn** para especificar a conexão de modem a ser usada. Se você não especificar o parâmetro **/Conn** , o bits usará a conexão LAN. Especifique o nome da conexão do modem imediatamente após o parâmetro **/Conn** .|

## <a name="remarks"></a>Comentários

Essa opção mostra o valor para cada uso de proxy, não apenas o uso de proxy especificado para a conta de serviço. Para obter detalhes sobre como definir o uso de proxy para contas de serviço, consulte a opção [Bitsadmin util e setieproxy](bitsadmin-util-and-setieproxy.md) .

## <a name="examples"></a><a name=BKMK_examples></a>Disso

O exemplo a seguir exibe o uso de proxy para a conta de serviço de rede.

```
C:\>bitsadmin /Util /GetIEProxy NETWORKSERVICE
```

## <a name="additional-references"></a>Referências adicionais

- [Chave da sintaxe de linha de comando](command-line-syntax-key.md)
