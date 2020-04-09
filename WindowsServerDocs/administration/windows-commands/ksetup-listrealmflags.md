---
title: 'ksetup: listrealmflags'
description: Tópico de comandos do Windows para * * * *-
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: aa96e4da-6b98-4c05-bccf-73cbf33258c2
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 265f988d85deb7602e91677626d207bc3a7873ef
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80841489"
---
# <a name="ksetuplistrealmflags"></a>ksetup: listrealmflags



Lista os sinalizadores de realm disponíveis que podem ser relatados por **ksetup**. Para obter exemplos de como esse comando pode ser usado, consulte [exemplos](#BKMK_Examples).

## <a name="syntax"></a>Sintaxe

```
ksetup /listrealmflags
```

#### <a name="parameters"></a>Parâmetros

Nenhum

## <a name="remarks"></a>Comentários

Os sinalizadores de realm especificam recursos adicionais de um realm Kerberos não baseado no Windows. Computadores que executam o Windows Server 2003, Windows Server 2008 ou Windows Server 2008 R2 podem usar um servidor Kerberos não baseado no Windows para administrar a autenticação em vez de usar um domínio que esteja executando um sistema operacional Windows Server. Esses sistemas participam de um realm Kerberos em vez de um domínio do Windows. Essa entrada estabelece os recursos do realm. A tabela a seguir descreve cada um.

|{1&gt;Valor&lt;1}|Sinalizador de realm|Descrição|
|-----|----------|-----------|
|0xF|Tudo|Todos os sinalizadores de realm estão definidos.|
|0x00|Nenhum|Nenhum sinalizador de realm definido, e nenhum recurso adicional está habilitado.|
|0x01|SendAddress|O endereço IP será incluído dentro dos tíquetes de concessão de tíquetes.|
|0x02|TcpSupported|O protocolo TCP e o UDP (User Datagram Protocol) têm suporte nesse realm.|
|0x04|Delegado|Todos nesse realm são confiáveis para delegação.|
|0x08|NcSupported|Esse Realm dá suporte à canonização de nome, que permite padrões de nomenclatura de DNS e de realm.|
|0x80|RC4|Esse Realm dá suporte à criptografia RC4 para habilitar a relação de confiança entre territórios, o que permite o uso de TLS.|

Os sinalizadores de realm são armazenados no registro em **HKEY_LOCAL_MACHINE \system\currentcontrolset\control\lsa\kerberos\domains\\** <em>território-nome</em>. Essa entrada não existe no Registro por padrão. Você pode usar o comando [Ksetup: addrealmflags](ksetup-addrealmflags.md) para popular o registro.

## <a name="examples"></a><a name=BKMK_Examples></a>Disso

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