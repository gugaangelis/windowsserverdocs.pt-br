---
title: ksetup listrealmflags
description: Artigo de referência para o comando ksetup listrealmflags, que lista os sinalizadores de realm disponíveis que podem ser relatados pelo ksetup.
ms.topic: reference
ms.assetid: aa96e4da-6b98-4c05-bccf-73cbf33258c2
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: d7c522449053a18cdd1e2a9e533dbce5d6e9f17c
ms.sourcegitcommit: 96d46c702e7a9c3a321bbbb5284f73911c7baa3c
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/27/2020
ms.locfileid: "89025470"
---
# <a name="ksetup-listrealmflags"></a>ksetup listrealmflags

Lista os sinalizadores de realm disponíveis que podem ser relatados por **ksetup**.

## <a name="syntax"></a>Sintaxe

```
ksetup /listrealmflags
```

### <a name="remarks"></a>Comentários

- Os sinalizadores de realm especificam recursos adicionais de um realm Kerberos que não se baseiam no sistema operacional Windows Server. Os computadores que executam o Windows Server podem usar um servidor Kerberos para administrar a autenticação no realm do Kerberos, em vez de usar um domínio que esteja executando um sistema operacional Windows Server. Essa entrada estabelece os recursos do Realm e são as seguintes:

| Valor | Sinalizador de realm | Descrição |
| ----- | ---------- | ----------- |
| 0xF | Tudo | Todos os sinalizadores de realm estão definidos. |
| 0x00 | Nenhum | Nenhum sinalizador de realm definido, e nenhum recurso adicional está habilitado. |
| 0x01 | sendaddress | O endereço IP será incluído dentro dos tíquetes de concessão de tíquetes. |
| 0x02 | tcpsupported | Tanto o protocolo TCP quanto o UDP (User Datagram Protocol) têm suporte nesse realm. |
| 0x04 | delegado | Todos nesse realm são confiáveis para delegação. |
| 0x08 | ncsupported | Esse Realm dá suporte à canonização de nome, que permite padrões de nomenclatura de DNS e de realm. |
| 0x80 | RC4 | Esse Realm dá suporte à criptografia RC4 para habilitar a relação de confiança entre territórios, o que permite o uso de TLS. |

- Os sinalizadores de realm são armazenados no registro em `HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\Lsa\Kerberos\Domains\<realmname>` . Por padrão, essa entrada não existe no registro. Você pode usar o [comando ksetup addrealmflags](ksetup-addrealmflags.md) para popular o registro.

## <a name="examples"></a>Exemplos

Para listar os sinalizadores de realm conhecidos neste computador, digite:

```
ksetup /listrealmflags
```

Para definir os sinalizadores de realm disponíveis que o **ksetup** não sabe, digite:

```
ksetup /setrealmflags CORP.CONTOSO.COM sendaddress tcpsupported delete ncsupported
```

**OR**

```
ksetup /setrealmflags CORP.CONTOSO.COM 0xF
```

## <a name="additional-references"></a>Referências adicionais

- [Chave da sintaxe de linha de comando](command-line-syntax-key.md)

- [comando ksetup](ksetup.md)

- [comando ksetup addrealmflags](ksetup-addrealmflags.md)

- [comando ksetup setrealmflags](ksetup-setrealmflags.md)

- [comando ksetup delrealmflags](ksetup-delrealmflags.md)
