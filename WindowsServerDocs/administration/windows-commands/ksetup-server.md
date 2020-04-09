---
title: 'ksetup: servidor'
description: Tópico de comandos do Windows para * * * *-
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: e3407111-ac92-457f-aa1f-a04fe9109d59
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 7889e1a03d3c0eec1958bf1d6356c67e9371a80f
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80841439"
---
# <a name="ksetupserver"></a>ksetup: servidor



Permite especificar um nome para um computador que executa o sistema operacional Windows para que as alterações feitas usando **ksetup** atualizem o computador de destino. Para obter exemplos de como esse comando pode ser usado, consulte [exemplos](#BKMK_Examples).

## <a name="syntax"></a>Sintaxe

```
ksetup /server <ServerName>
```

#### <a name="parameters"></a>Parâmetros

|Parâmetro|Descrição|
|---------|-----------|
|\<ServerName >|O nome completo do computador no qual a configuração será efetiva, como IPops897.corp.contoso.com.</br>Se um nome de computador de domínio totalmente qualificado incompleto for especificado, o comando falhará.|

## <a name="remarks"></a>Comentários

Não é possível remover o nome do servidor de destino; Você só pode alterá-lo de volta para o nome do computador local, que é o padrão.

O nome do servidor de destino é armazenado no registro no **HKEY_LOCAL_MACHINE \system\controlset001\control\lsa\kerberos**. Ele não é relatado usando **ksetup**.

## <a name="examples"></a><a name=BKMK_Examples></a>Disso

Torne suas configurações do **ksetup** efetivas no computador IPops897 que está conectado no domínio contoso:
```
ksetup /server IPops897.corp.contoso.com
```

## <a name="additional-references"></a>Referências adicionais

-   [Ksetup](ksetup.md)
-   - [Chave da sintaxe de linha de comando](command-line-syntax-key.md)