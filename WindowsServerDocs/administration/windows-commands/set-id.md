---
title: set id
description: Artigo de referência para o comando Set ID do DiskPart, que altera o campo de tipo de partição para a partição com foco.
ms.topic: reference
ms.assetid: 5793d7ad-827e-4285-b2c6-ae60eeb0e886
ms.author: lizross
author: eross-msft
manager: mtillman
ms.date: 10/16/2017
ms.openlocfilehash: 0f25498a89cdb7347a40825af70621d5cd09440a
ms.sourcegitcommit: e164aeffc01069b8f1f3248bf106fcdb7f64f894
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/26/2020
ms.locfileid: "91389080"
---
# <a name="set-id-diskpart"></a>definir ID (DiskPart)

> Aplica-se a: Windows Server (canal semestral), Windows Server 2019, Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Altera o campo de tipo de partição para a partição com foco. Esse comando não funciona em discos dinâmicos ou em partições reservadas da Microsoft.

> [!IMPORTANT]
> Este comando destina-se ao uso somente por OEMs (fabricantes originais do equipamento). A alteração de campos de tipo de partição com esse parâmetro pode fazer com que o computador falhe ou não possa ser inicializado. A menos que você seja um OEM ou tenha experiência com discos GPT, não altere os campos de tipo de partição em discos GPT usando esse parâmetro. Em vez disso, sempre use o comando [Create Partition EFI](create-partition-efi.md) para criar partições de sistema EFI, o comando [Create Partition MSR](create-partition-msr.md) para criar partições reservadas da Microsoft e o comando [Create Partition Primary](create-partition-primary.md) sem o parâmetro ID para criar partições primárias em discos GPT.

## <a name="syntax"></a>Sintaxe

```
set id={ <byte> | <GUID> } [override] [noerr]
```

### <a name="parameters"></a>Parâmetros

| Parâmetro | Descrição |
|--|--|
| `<byte>` | Para discos MBR (registro mestre de inicialização), especifica o novo valor para o campo tipo, em formato hexadecimal, para a partição. Qualquer **byte** de tipo de partição pode ser especificado com esse parâmetro, exceto para o tipo 0x42, que especifica uma partição LDM. Observe que o 0x à esquerda é omitido ao especificar o tipo de partição hexadecimal. |
| `<GUID>` | Para discos GPT (tabela de partição GUID), especifica o novo valor de GUID para o campo de tipo da partição. Os GUIDs reconhecidos incluem:<ul><li>**Partição do sistema EFI:** C12A7328-F81F-11D2-BA4B-00A0C93EC93B</li><li>**Partição de dados básica:** ebd0a0a2-b9e5-4433-87c0-68b6b72699c7</li></ul>Qualquer GUID de tipo de partição pode ser especificado com esse parâmetro, exceto o seguinte:<ul><li>**Partição reservada da Microsoft:** e3c9e316-0b5c-4db8-817d-f92df00215ae</li><li>**Partição de metadados LDM em um disco dinâmico:** 5808c8aa-7e8f-42e0-85d2-e1e90434cfb3</li><li>**Partição de dados LDM em um disco dinâmico:** AF9B60A0-1431-4F62-BC68-3311714A69AD</li><li>**Partição de metadados de cluster:** db97dba9-0840-4bae-97f0-ffb9a327c7e1</li></ul> |
| override | força o sistema de arquivos no volume a ser desmontado antes de alterar o tipo de partição. Quando você executa o comando **set ID** , o DiskPart tenta bloquear e desmontar o sistema de arquivos no volume. Se a **substituição** não for especificada e a chamada para bloquear o sistema de arquivos falhar (por exemplo, porque há um identificador aberto), a operação falhará. Se **override** for especificado, o DiskPart forçará a desmontagem mesmo se a chamada para bloquear o sistema de arquivos falhar e quaisquer identificadores abertos para o volume deixarão de ser válidos. |
| NOERR | Usado somente para scripts. Quando um erro é encontrado, o DiskPart continua processando comandos como se o erro não tivesse ocorrido. Sem esse parâmetro, um erro faz com que o DiskPart saia com um código de erro. |

## <a name="remarks"></a>Comentários

- Além das limitações mencionadas anteriormente, o DiskPart não verifica a validade do valor que você especificar (exceto para garantir que seja um byte em formato hexadecimal ou GUID).

## <a name="examples"></a>Exemplos

Para definir o campo de tipo como *0x07* e forçar o sistema de arquivos a ser desmontado, digite:

```
set id=0x07 override
```

Para definir o campo de tipo como uma partição de dados básica, digite:

```
set id=ebd0a0a2-b9e5-4433-87c0-68b6b72699c7
```

## <a name="additional-references"></a>Referências adicionais

- [Chave da sintaxe de linha de comando](command-line-syntax-key.md)
