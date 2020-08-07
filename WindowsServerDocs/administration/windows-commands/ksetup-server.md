---
title: ksetup server
description: Artigo de referência para o comando do servidor ksetup, que permite especificar um nome para um computador que executa o sistema operacional Windows, de modo que as alterações feitas pelo comando ksetup atualizem o computador de destino.
ms.topic: article
ms.assetid: e3407111-ac92-457f-aa1f-a04fe9109d59
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 2bfa894468ee05b983d8c0122a1738982ef4d909
ms.sourcegitcommit: 53d526bfeddb89d28af44210a23ba417f6ce0ecf
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/06/2020
ms.locfileid: "87887781"
---
# <a name="ksetup-server"></a>ksetup server

Permite especificar um nome para um computador que executa o sistema operacional Windows, portanto, as alterações feitas pelo comando **ksetup** atualizam o computador de destino.

O nome do servidor de destino é armazenado no registro em `HKEY_LOCAL_MACHINE\SYSTEM\ControlSet001\Control\LSA\Kerberos` . Essa entrada não é relatada quando você executa o comando **ksetup** .

> [!IMPORTANT]
> Não há como remover o nome do servidor de destino. Em vez disso, você pode alterá-lo de volta para o nome do computador local, que é o padrão.

## <a name="syntax"></a>Sintaxe

```
ksetup /server <servername>
```

### <a name="parameters"></a>Parâmetros

| Parâmetro | Descrição |
| --------- | ----------- |
| `<servername>` | Especifica o nome completo do computador no qual a configuração será efetiva, como *IPops897.Corp.contoso.com*.<p>Se um nome de computador de domínio totalmente qualificado incompleto for especificado, o comando falhará. |

### <a name="examples"></a>Exemplos

Para tornar as configurações do **ksetup** efetivas no computador *IPops897* , que está conectado ao domínio contoso, digite:

```
ksetup /server IPops897.corp.contoso.com
```

## <a name="additional-references"></a>Referências adicionais

- [Chave da sintaxe de linha de comando](command-line-syntax-key.md)

- [comando ksetup](ksetup.md)
