---
title: copiar-controlador de driver
description: Artigo de referência do comando copy-Driver, que duplica um grupo de drivers existente no servidor, incluindo os filtros, os pacotes de driver e o status habilitado/desabilitado.
ms.topic: reference
ms.assetid: 0aaf6fa5-8b5b-4a1e-ae9b-8b5c6d89f571
ms.author: lizross
author: eross-msft
manager: mtillman
ms.date: 10/16/2017
ms.openlocfilehash: 924f6181e424dad88a33f1421098e658275d1fa3
ms.sourcegitcommit: 554d274fea48a4d47c19845d969a9ec93dec82de
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 10/24/2020
ms.locfileid: "92524911"
---
# <a name="copy-drivergroup"></a>copiar-controlador de driver

Duplica um grupo de drivers existente no servidor, incluindo os filtros, os pacotes de driver e o status habilitado/desabilitado.

## <a name="syntax"></a>Sintaxe

```
wdsutil /Copy-DriverGroup [/Server:<Server name>] /DriverGroup:<Source Groupname> /GroupName:<New Groupname>
```

### <a name="parameters"></a>Parâmetros

| Parâmetro | Descrição |
|--|--|
| Servidor`<Servername>` | Especifica o nome do servidor. Pode ser o nome NetBIOS ou o FQDN. Se nenhum nome de servidor for especificado, o servidor local será usado. |
| /DriverGroup:`<Source Groupname>` | Especifica o nome do grupo de drivers de origem. |
| GroupName`<New Groupname>` | Especifica o nome do novo grupo de drivers. |

## <a name="examples"></a>Exemplos

Para copiar um grupo de drivers, digite:

```
wdsutil /Copy-DriverGroup /Server:MyWdsServer /DriverGroup:PrinterDrivers /GroupName:X86PrinterDrivers
```

```
wdsutil /Copy-DriverGroup /DriverGroup:PrinterDrivers /GroupName:ColorPrinterDrivers
```

## <a name="additional-references"></a>Referências adicionais

- [Chave da sintaxe de linha de comando](command-line-syntax-key.md)

- [Cmdlets dos serviços de implantação do Windows](/powershell/module/wds)
