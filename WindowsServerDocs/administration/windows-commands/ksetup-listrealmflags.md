---
title: 'ksetup: listrealmflags'
description: Tópico de referência para * * * *-
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: aa96e4da-6b98-4c05-bccf-73cbf33258c2
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: f0646be8daaad4bc3303389cfca1f3a09136fe1a
ms.sourcegitcommit: ab64dc83fca28039416c26226815502d0193500c
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 05/01/2020
ms.locfileid: "82724622"
---
# <a name="ksetuplistrealmflags"></a>ksetup: listrealmflags



Lista os sinalizadores de realm disponíveis que podem ser relatados por **ksetup**.

## <a name="syntax"></a>Sintaxe

```
ksetup /listrealmflags
```

#### <a name="parameters"></a>Parâmetros

Nenhum

## <a name="remarks"></a>Comentários

Os sinalizadores de realm especificam recursos adicionais de um realm Kerberos não baseado no Windows. Computadores que executam o Windows Server 2003, Windows Server 2008 ou Windows Server 2008 R2 podem usar um servidor Kerberos não baseado no Windows para administrar a autenticação em vez de usar um domínio que esteja executando um sistema operacional Windows Server. Esses sistemas participam de um realm Kerberos em vez de um domínio do Windows. Essa entrada estabelece os recursos do realm. A tabela a seguir descreve cada um.

|Valor|Sinalizador de realm|Descrição|
|-----|----------|-----------|
|0xF|Todos|Todos os sinalizadores de realm estão definidos.|
|0x00|Nenhum|Nenhum sinalizador de realm definido, e nenhum recurso adicional está habilitado.|
|0x01|SendAddress|O endereço IP será incluído dentro dos tíquetes de concessão de tíquetes.|
|0x02|TcpSupported|O protocolo TCP e o UDP (User Datagram Protocol) têm suporte nesse realm.|
|0x04|delegado|Todos nesse realm são confiáveis para delegação.|
|0x08|NcSupported|Esse Realm dá suporte à canonização de nome, que permite padrões de nomenclatura de DNS e de realm.|
|0x80|RC4|Esse Realm dá suporte à criptografia RC4 para habilitar a relação de confiança entre territórios, o que permite o uso de TLS.|

Os sinalizadores de realm são armazenados no registro em HKEY_LOCAL_MACHINE<em>nome Realm</em> **\\\system\currentcontrolset\control\lsa\kerberos\domains**. Essa entrada não existe no Registro por padrão. Você pode usar o comando [Ksetup: addrealmflags](ksetup-addrealmflags.md) para popular o registro.

## <a name="examples"></a>Exemplos

Liste os sinalizadores de realm conhecidos neste computador:
```
ksetup /listrealmflags
```
Defina os sinalizadores de realm disponíveis que o **Ksetup** não sabe digitando um dos comandos a seguir na linha de comando:
```
ksetup /setrealmflags CORP.CONTOSO.COM sendaddress tcpsupported delete ncsupported
```
```
ksetup /setrealmflags CORP.CONTOSO.COM 0xF
```

## <a name="additional-references"></a>Referências adicionais

-   [Ksetup:setrealmflags](ksetup-setrealmflags.md)
-   [Ksetup:addrealmflags](ksetup-addrealmflags.md)
-   [Ksetup:delrealmflags](ksetup-delrealmflags.md)
-   [Ksetup](ksetup.md)
-   - [Chave da sintaxe de linha de comando](command-line-syntax-key.md)