---
title: ksetup setrealmflags
description: Artigo de referência para o comando ksetup setrealmflags, que define os sinalizadores de realm para o realm especificado.
ms.topic: reference
ms.assetid: bcb2824e-fba7-4ebe-be62-e62b4fae5b17
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 3f716e1da0a5804df9fa42534d5d4aa0b63672b1
ms.sourcegitcommit: 96d46c702e7a9c3a321bbbb5284f73911c7baa3c
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/27/2020
ms.locfileid: "89025320"
---
# <a name="ksetup-setrealmflags"></a>ksetup setrealmflags

Define os sinalizadores de realm para o realm especificado.

## <a name="syntax"></a>Sintaxe

```
ksetup /setrealmflags <realmname> [sendaddress] [tcpsupported] [delegate] [ncsupported] [rc4]
```

### <a name="parameters"></a>Parâmetros

| Parâmetro | Descrição |
| --------- | ----------- |
| `<realmname>` | Especifica o nome DNS em maiúsculas, como CORP. CONTOSO.COM. |

#### <a name="remarks"></a>Comentários

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

- Os sinalizadores de realm são armazenados no registro em `HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\Lsa\Kerberos\Domains\<realmname>` . Por padrão, essa entrada não existe no registro. Você pode usar o comando [ksetup addrealmflags](ksetup-addrealmflags.md) para popular o registro.

- Você pode ver os sinalizadores de realm disponíveis e definir exibindo a saída de **ksetup** ou `ksetup /dumpstate` .

### <a name="examples"></a>Exemplos

Para listar o disponível e definir os sinalizadores de realm para o realm CONTOSO, digite:

```
ksetup
```

Para definir dois sinalizadores que não estão definidos no momento, digite:

```
ksetup /setrealmflags CONTOSO ncsupported delegate
```

Para verificar se o sinalizador de realm está definido, digite `ksetup` e, em seguida, exiba a saída, procurando o texto, **sinalizadores de realm =**. Se você não vir o texto, significa que o sinalizador não foi definido.

## <a name="additional-references"></a>Referências adicionais

- [Chave da sintaxe de linha de comando](command-line-syntax-key.md)

- [comando ksetup](ksetup.md)

- [comando ksetup listrealmflags](ksetup-listrealmflags.md)

- [comando ksetup addrealmflags](ksetup-addrealmflags.md)

- [comando ksetup delrealmflags](ksetup-delrealmflags.md)

- [comando ksetup dumpstate](ksetup-dumpstate.md)
