---
title: gerenciar o TPM do bde
description: Artigo de referência para o comando do TPM Manage-bde, que configura o Trusted Platform Module do computador (TPM).
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 11a8530d-edd7-4fe3-ae81-b943766760fe
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: bed203c5de5351162f4c465e43631a4869f0e9a1
ms.sourcegitcommit: 2afed2461574a3f53f84fc9ec28d86df3b335685
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/02/2020
ms.locfileid: "85922192"
---
# <a name="manage-bde-tpm"></a>gerenciar o TPM do bde

> Aplica-se a: Windows Server (canal semestral), Windows Server 2019, Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Configura o Trusted Platform Module do computador (TPM).

## <a name="syntax"></a>Sintaxe

```
manage-bde -tpm [-turnon] [-takeownership <ownerpassword>] [-computername <name>] [{-?|/?}] [{-help|-h}]
```

### <a name="parameters"></a>Parâmetros

| Parâmetro | Descrição |
| --------- | ----------- |
| -ativação | Habilita e ativa o TPM, permitindo que a senha de proprietário do TPM seja definida. Você também pode usar **-t** como uma versão abreviada desse comando. |
| -TakeOwnership | Apropria-se do TPM definindo uma senha de proprietário. Você também pode usar **-o** como uma versão abreviada deste comando. |
| `<ownerpassword>` | Representa a senha do proprietário que você especificar para o TPM. |
| -ComputerName | Especifica que manage-bde.exe será usado para modificar a proteção do BitLocker em um computador diferente. Você também pode usar **-CN** como uma versão abreviada desse comando. |
| `<name>` | Representa o nome do computador no qual a proteção do BitLocker será modificada. Os valores aceitos incluem o nome NetBIOS do computador e o endereço IP do computador. |
| -? ou/? | Exibe a ajuda resumida no prompt de comando. |
| -Help ou-h | Exibe a ajuda completa no prompt de comando. |

### <a name="examples"></a>Exemplos

Para ativar o TPM, digite:

```
manage-bde  tpm -turnon
```

Para apropriar-se do TPM e definir a senha do proprietário como *0wnerP@ss* , digite:

```
manage-bde  tpm  takeownership 0wnerP@ss
```

## <a name="additional-references"></a>Referências adicionais

- [Chave da sintaxe de linha de comando](command-line-syntax-key.md)

- [Cmdlets de gerenciamento do TPM para Windows PowerShell](https://docs.microsoft.com/powershell/module/trustedplatformmodule/)

- [comando Manage-bde](manage-bde.md)
