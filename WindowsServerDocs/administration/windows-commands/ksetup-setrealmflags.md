---
title: ksetup:setrealmflags
description: 'Tópico de comandos do Windows para * * *- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: bcb2824e-fba7-4ebe-be62-e62b4fae5b17
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 249eb82bb40890e071bd7d1eca3a0201064fa01e
ms.sourcegitcommit: eaf071249b6eb6b1a758b38579a2d87710abfb54
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 05/31/2019
ms.locfileid: "66437944"
---
# <a name="ksetupsetrealmflags"></a>ksetup:setrealmflags



Define os sinalizadores de realm para o realm especificado. Para obter exemplos de como esse comando pode ser usado, consulte [exemplos](#BKMK_Examples).

## <a name="syntax"></a>Sintaxe

```
ksetup /setrealmflags <RealmName> [sendaddress] [tcpsupported] [delegate] [ncsupported] [rc4]
```

### <a name="parameters"></a>Parâmetros

|Parâmetro|Descrição|
|---------|-----------|
|\<RealmName>|O nome do território é declarado como um nome DNS de letras maiusculas, como CORP. CONTOSO.COM.|
|Sinalizador de realm|Denota um dos seguintes sinalizadores:</br>-SendAddress</br>-   TcpSupported</br>-Delegate</br>-NcSupported</br>-   RC4|

## <a name="remarks"></a>Comentários

Os sinalizadores de território especificam recursos adicionais de um realm Kerberos que não se baseia no sistema operacional Windows Server. Computadores que executam o Windows Server 2003, Windows Server 2008 ou Windows Server 2008 R2 podem usar um servidor de Kerberos para autenticação em vez de usar um domínio que esteja executando um sistema operacional Windows Server de administrar e esses sistemas de participarem de um Realm de Kerberos. Essa entrada estabelece os recursos do território. A tabela a seguir descreve cada um.

|Valor|Sinalizador de realm|Descrição|
|-----|----------|-----------|
|0xF|Todas|Todos os sinalizadores de território são definidos.|
|0x00|Nenhuma|Nenhum sinalizador de território está definido e não há recursos adicionais estão habilitados.|
|0x01|SendAddress|O endereço IP será incluído dentro de tíquetes de concessão de tíquete.|
|0x02|TcpSupported|O protocolo TCP (Transmission Control) e o protocolo UDP (User Datagram) têm suporte nesse realm.|
|0x04|Delegar|Todas as pessoas esse realm são confiável para delegação.|
|0x08|NcSupported|Esse realm dá suporte à canonização do nome, o que permite o DNS e o Realm padrões de nomenclatura.|
|0x80|RC4|Esse realm dá suporte a criptografia RC4 para habilitar a relação de confiança entre territórios, que permite o uso do TLS.|

Sinalizadores de território são armazenadas no registro em **HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\Lsa\Kerberos\Domains\\** <em>RealmName</em>. Essa entrada não existe no Registro por padrão. Você pode usar o [Ksetup:addrealmflags](ksetup-addrealmflags.md) comando preencher o registro.

Você pode ver quais sinalizadores de território são disponível e defina exibindo a saída de **ksetup**.

## <a name="BKMK_Examples"></a>Exemplos

Lista os sinalizadores de território disponível e defina o realm CONTOSO:
```
ksetup
```
Defina dois sinalizadores que não estão definidas no momento:
```
ksetup /setrealmflags CONTOSO ncsupported delegate
```
Execute o **ksetup** comando para verificar se o sinalizador de território está definido exibindo a saída e procurando **sinalizadores de território =** .

#### <a name="additional-references"></a>Referências adicionais

-   [Ksetup:listrealmflags](ksetup-listrealmflags.md)
-   [Ksetup:addrealmflags](ksetup-addrealmflags.md)
-   [Ksetup:delrealmflags](ksetup-delrealmflags.md)
-   [Chave da sintaxe de linha de comando](command-line-syntax-key.md)