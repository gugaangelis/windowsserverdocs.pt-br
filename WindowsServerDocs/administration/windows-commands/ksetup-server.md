---
title: servidor ksetup
description: Tópico de referência para o comando do servidor ksetup, que permite especificar um nome para um computador que executa o sistema operacional Windows, de modo que as alterações feitas pelo comando ksetup atualizem o computador de destino.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: e3407111-ac92-457f-aa1f-a04fe9109d59
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: e39a3fbef4b99848d2a90c81007c526597c77275
ms.sourcegitcommit: 4f407b82435afe3111c215510b0ef797863f9cb4
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 05/24/2020
ms.locfileid: "83817516"
---
# <a name="ksetup-server"></a>servidor ksetup

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
