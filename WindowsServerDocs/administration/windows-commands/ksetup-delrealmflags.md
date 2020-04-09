---
title: 'ksetup: delrealmflags'
description: Tópico de comandos do Windows para * * * *-
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 22053041-1eb4-47f5-bed9-3d5681bcde7d
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: a082b2798ffafaca96c19590f02c94b380c3e779
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80841599"
---
# <a name="ksetupdelrealmflags"></a>ksetup: delrealmflags



Remove os sinalizadores de realm do realm especificado.  Para obter exemplos de como esse comando pode ser usado, consulte [exemplos](#BKMK_Examples).

## <a name="syntax"></a>Sintaxe

```
ksetup /delrealmflags <RealmName> [sendaddress] [tcpsupported] [delegate] [ncsupported] [rc4]
```

#### <a name="parameters"></a>Parâmetros

|Parâmetro|Descrição|
|---------|-----------|
|\<Realmsname >|O nome do realm é declarado como um nome DNS em maiúsculas, como CORP. CONTOSO.COM e é listado como o realm padrão quando **Ksetup** é executado.|

## <a name="remarks"></a>Comentários

Os sinalizadores de realm especificam recursos adicionais de um realm Kerberos que não é baseado no sistema operacional Windows Server. Computadores que executam o Windows Server 2003, Windows Server 2008 ou Windows Server 2008 R2 podem usar um servidor Kerberos para administrar a autenticação em vez de usar um domínio que esteja executando um sistema operacional Windows Server, e esses sistemas participam de um realm do Kerberos. Essa entrada estabelece os recursos do realm. A tabela a seguir descreve cada um.

|{1&gt;Valor&lt;1}|Sinalizador de realm|Descrição|
|-----|----------|-----------|
|0xF|Tudo|Todos os sinalizadores de realm estão definidos.|
|0x00|Nenhum|Nenhum sinalizador de realm definido, e nenhum recurso adicional está habilitado.|
|0x01|SendAddress|O endereço IP será incluído nos tíquetes de concessão de tíquetes.|
|0x02|TcpSupported|Tanto o protocolo TCP quanto o UDP (User Datagram Protocol) têm suporte nesse realm.|
|0x04|Delegado|Todos nesse realm são confiáveis para delegação.|
|0x08|NcSupported|Esse Realm dá suporte à canonização de nome, que permite padrões de nomenclatura de DNS e de realm.|
|0x80|RC4|Esse Realm dá suporte à criptografia RC4 para habilitar a relação de confiança entre territórios, o que permite o uso de TLS.|

Os sinalizadores de realm são armazenados no registro em **HKEY_LOCAL_MACHINE \system\currentcontrolset\control\lsa\kerberos\domains\\** <em>territórioname</em>. Essa entrada não existe no Registro por padrão. Você pode usar o comando [Ksetup: addrealmflags](ksetup-addrealmflags.md) para popular o registro.

Você pode ver quais sinalizadores de realm estão disponíveis e definidos exibindo a saída de **ksetup** ou **ksetup/dumpstate**.

## <a name="examples"></a><a name=BKMK_Examples></a>Disso

Lista os sinalizadores de realm disponíveis para o realm CONTOSO:
```
Ksetup /listrealmflags
```
Remova dois sinalizadores que estão atualmente no conjunto:
```
ksetup /delrealmflags CONTOSO ncsupported delegate
```
Execute o comando **ksetup** para verificar se o sinalizador de realm está definido exibindo a saída e procurando por **sinalizadores de realm =** .

## <a name="additional-references"></a>Referências adicionais

-   [Ksetup:listrealmflags](ksetup-listrealmflags.md)
-   [Ksetup:setrealmflags](ksetup-setrealmflags.md)
-   [Ksetup:addrealmflags](ksetup-addrealmflags.md)
-   - [Chave da sintaxe de linha de comando](command-line-syntax-key.md)