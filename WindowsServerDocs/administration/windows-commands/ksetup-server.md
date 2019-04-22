---
title: ksetup:server
description: 'Tópico de comandos do Windows para * * *- '
ms.custom: na
ms.prod: windows-server-threshold
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
ms.openlocfilehash: f370d4dede278e1facdda829503ea3793502b9e6
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59814567"
---
# <a name="ksetupserver"></a>ksetup:server



Permite que você especifique um nome para um computador que executa o sistema operacional Windows para que as alterações feitas por meio **ksetup** atualizará o computador de destino. Para obter exemplos de como esse comando pode ser usado, consulte [exemplos](#BKMK_Examples).

## <a name="syntax"></a>Sintaxe

```
ksetup /server <ServerName>
```

### <a name="parameters"></a>Parâmetros

|Parâmetro|Descrição|
|---------|-----------|
|\<ServerName>|O nome completo do computador no qual a configuração será eficaz, como IPops897.corp.contoso.com.</br>Se um incompleta totalmente qualificado do nome do computador de domínio for especificado, o comando falhará.|

## <a name="remarks"></a>Comentários

Não é possível remover o nome do servidor de destino; Você só pode alterá-la novamente para o nome do computador local, que é o padrão.

O nome do servidor de destino é armazenado no registro em **HKEY_LOCAL_MACHINE\SYSTEM\ControlSet001\Control\LSA\Kerberos**. Ele não será relatado por meio **ksetup**.

## <a name="BKMK_Examples"></a>Exemplos

Verifique sua **ksetup** configurações efetivas no computador IPops897 que está conectado no domínio Contoso:
```
ksetup /server IPops897.corp.contoso.com
```

#### <a name="additional-references"></a>Referências adicionais

-   [Ksetup](ksetup.md)
-   [Chave de sintaxe de linha de comando](command-line-syntax-key.md)