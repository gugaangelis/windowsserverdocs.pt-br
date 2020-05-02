---
title: 'ksetup: setrealmflags'
description: Tópico de referência para * * * *-
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: bcb2824e-fba7-4ebe-be62-e62b4fae5b17
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 7a32ea03f4f0e76f03c7a0b505563e6bcf972b80
ms.sourcegitcommit: ab64dc83fca28039416c26226815502d0193500c
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 05/01/2020
ms.locfileid: "82724577"
---
# <a name="ksetupsetrealmflags"></a>ksetup: setrealmflags



Define os sinalizadores de realm para o realm especificado.

## <a name="syntax"></a>Sintaxe

```
ksetup /setrealmflags <RealmName> [sendaddress] [tcpsupported] [delegate] [ncsupported] [rc4]
```

#### <a name="parameters"></a>Parâmetros

|Parâmetro|Descrição|
|---------|-----------|
|\<Realmsname>|O nome do realm é declarado como um nome DNS em maiúsculas, como CORP. CONTOSO.COM.|
|Sinalizador de realm|Denota um dos seguintes sinalizadores:</br>- SendAddress</br>- TcpSupported</br>-Delegar</br>- NcSupported</br>-RC4|

## <a name="remarks"></a>Comentários

Os sinalizadores de realm especificam recursos adicionais de um realm Kerberos que não é baseado no sistema operacional Windows Server. Computadores que executam o Windows Server 2003, Windows Server 2008 ou Windows Server 2008 R2 podem usar um servidor Kerberos para administrar a autenticação em vez de usar um domínio que esteja executando um sistema operacional Windows Server, e esses sistemas participam de um realm do Kerberos. Essa entrada estabelece os recursos do realm. A tabela a seguir descreve cada um.

|Valor|Sinalizador de realm|Descrição|
|-----|----------|-----------|
|0xF|Todos|Todos os sinalizadores de realm estão definidos.|
|0x00|Nenhum|Nenhum sinalizador de realm definido, e nenhum recurso adicional está habilitado.|
|0x01|SendAddress|O endereço IP será incluído dentro dos tíquetes de concessão de tíquetes.|
|0x02|TcpSupported|Tanto o protocolo TCP quanto o UDP (User Datagram Protocol) têm suporte nesse realm.|
|0x04|delegado|Todos nesse realm são confiáveis para delegação.|
|0x08|NcSupported|Esse Realm dá suporte à canonização de nome, que permite padrões de nomenclatura de DNS e de realm.|
|0x80|RC4|Esse Realm dá suporte à criptografia RC4 para habilitar a relação de confiança entre territórios, o que permite o uso de TLS.|

Os sinalizadores de realm são armazenados no registro em **HKEY_LOCAL_MACHINE \system\currentcontrolset\control\lsa\kerberos\domains\\**<em>realmsname</em>. Essa entrada não existe no Registro por padrão. Você pode usar o comando [Ksetup: addrealmflags](ksetup-addrealmflags.md) para popular o registro.

Você pode ver quais sinalizadores de realm estão disponíveis e definidos exibindo a saída de **ksetup**.

## <a name="examples"></a>Exemplos

Liste os sinalizadores disponíveis e definir realm para o realm CONTOSO:
```
ksetup
```
Defina dois sinalizadores que não estão definidos no momento:
```
ksetup /setrealmflags CONTOSO ncsupported delegate
```
Execute o comando **ksetup** para verificar se o sinalizador de realm está definido exibindo a saída e procurando por **sinalizadores de realm =**.

## <a name="additional-references"></a>Referências adicionais

-   [Ksetup:listrealmflags](ksetup-listrealmflags.md)
-   [Ksetup:addrealmflags](ksetup-addrealmflags.md)
-   [Ksetup:delrealmflags](ksetup-delrealmflags.md)
-   - [Chave da sintaxe de linha de comando](command-line-syntax-key.md)