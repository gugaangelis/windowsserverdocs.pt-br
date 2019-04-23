---
title: ksetup:listrealmflags
description: 'Tópico de comandos do Windows para * * *- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: aa96e4da-6b98-4c05-bccf-73cbf33258c2
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 1bc4be8be747c31d60d75c90ad3aa831dd8dff93
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59838297"
---
# <a name="ksetuplistrealmflags"></a>ksetup:listrealmflags



Lista os sinalizadores de território disponíveis que podem ser informados por **ksetup**. Para obter exemplos de como esse comando pode ser usado, consulte [exemplos](#BKMK_Examples).

## <a name="syntax"></a>Sintaxe

```
ksetup /listrealmflags
```

### <a name="parameters"></a>Parâmetros

Nenhuma

## <a name="remarks"></a>Comentários

Os sinalizadores de território especificam recursos adicionais do realm do Kerberos não baseados em Windows. Computadores que executam o Windows Server 2003, Windows Server 2008 ou Windows Server 2008 R2 podem usar um servidor de Kerberos não são baseados em Windows para administrar a autenticação em vez de usar um domínio que esteja executando um sistema operacional Windows Server. Esses sistemas de participarem de um realm Kerberos em vez de um domínio do Windows. Essa entrada estabelece os recursos do território. A tabela a seguir descreve cada um.

|Valor|Sinalizador de realm|Descrição|
|-----|----------|-----------|
|0xF|Todas|Todos os sinalizadores de território são definidos.|
|0x00|Nenhuma|Nenhum sinalizador de território está definido e não há recursos adicionais estão habilitados.|
|0x01|SendAddress|O endereço IP será incluído dentro de tíquetes de concessão de tíquete.|
|0x02|TcpSupported|O protocolo TCP (Transmission Control) e o protocolo UDP (User Datagram) têm suporte nesse realm.|
|0x04|Delegar|Todas as pessoas esse realm são confiável para delegação.|
|0x08|NcSupported|Esse realm dá suporte à canonização do nome, o que permite o DNS e o realm padrões de nomenclatura.|
|0x80|RC4|Esse realm dá suporte a criptografia RC4 para habilitar a relação de confiança entre territórios, que permite o uso do TLS.|

Sinalizadores de território são armazenadas no registro em **HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\Lsa\Kerberos\Domains\*** Realm-nome *. Essa entrada não existe no Registro por padrão. Você pode usar o [Ksetup:addrealmflags](ksetup-addrealmflags.md) comando preencher o registro.

## <a name="BKMK_Examples"></a>Exemplos

Lista os sinalizadores de território conhecidos neste computador:
```
ksetup /listrealmflags
```
Definir os sinalizadores de território disponíveis que **Ksetup** não sabe, digitando um dos comandos a seguir na linha de comando:
```
ksetup /setrealmflags CORP.CONTOSO.COM sendaddress tcpsupported delete ncsupported
```
```
ksetup /setrealmflags CORP.CONTOSO.COM 0xF
```

#### <a name="additional-references"></a>Referências adicionais

-   [Ksetup:setrealmflags](ksetup-setrealmflags.md)
-   [Ksetup:addrealmflags](ksetup-addrealmflags.md)
-   [Ksetup:delrealmflags](ksetup-delrealmflags.md)
-   [Ksetup](ksetup.md)
-   [Chave de sintaxe de linha de comando](command-line-syntax-key.md)