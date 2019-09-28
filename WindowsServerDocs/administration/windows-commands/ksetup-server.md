---
title: 'ksetup: servidor'
description: 'Tópico de comandos do Windows para * * * *- '
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: e3407111-ac92-457f-aa1f-a04fe9109d59
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: dd05fd294640c63e633b7b866307197ae6770476
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71374963"
---
# <a name="ksetupserver"></a>ksetup: servidor



Permite especificar um nome para um computador que executa o sistema operacional Windows para que as alterações feitas usando **ksetup** atualizem o computador de destino. Para obter exemplos de como esse comando pode ser usado, consulte [exemplos](#BKMK_Examples).

## <a name="syntax"></a>Sintaxe

```
ksetup /server <ServerName>
```

### <a name="parameters"></a>Parâmetros

|Parâmetro|Descrição|
|---------|-----------|
|\<ServerName >|O nome completo do computador no qual a configuração será efetiva, como IPops897.corp.contoso.com.</br>Se um nome de computador de domínio totalmente qualificado incompleto for especificado, o comando falhará.|

## <a name="remarks"></a>Comentários

Não é possível remover o nome do servidor de destino; Você só pode alterá-lo de volta para o nome do computador local, que é o padrão.

O nome do servidor de destino é armazenado no registro no **HKEY_LOCAL_MACHINE\SYSTEM\ControlSet001\Control\LSA\Kerberos**. Ele não é relatado usando **ksetup**.

## <a name="BKMK_Examples"></a>Disso

Torne suas configurações do **ksetup** efetivas no computador IPops897 que está conectado no domínio contoso:
```
ksetup /server IPops897.corp.contoso.com
```

#### <a name="additional-references"></a>Referências adicionais

-   [Ksetup](ksetup.md)
-   [Chave da sintaxe de linha de comando](command-line-syntax-key.md)