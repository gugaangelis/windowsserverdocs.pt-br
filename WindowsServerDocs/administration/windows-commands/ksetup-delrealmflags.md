---
title: ksetup delrealmflags
description: Artigo de referência para o comando ksetup delrealmflags, que remove os sinalizadores de realm do realm especificado.
ms.topic: reference
ms.assetid: 22053041-1eb4-47f5-bed9-3d5681bcde7d
ms.author: lizross
author: eross-msft
manager: mtillman
ms.date: 10/16/2017
ms.openlocfilehash: 90ebf697ae19cd31b45dc7744ba29f5dc0b9f597
ms.sourcegitcommit: db2d46842c68813d043738d6523f13d8454fc972
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/10/2020
ms.locfileid: "89640054"
---
# <a name="ksetup-delrealmflags"></a>ksetup delrealmflags

Remove os sinalizadores de realm do realm especificado.

## <a name="syntax"></a>Sintaxe

```
ksetup /delrealmflags <realmname> [sendaddress] [tcpsupported] [delegate] [ncsupported] [rc4]
```

### <a name="parameters"></a>Parâmetros

| Parâmetro | Descrição |
| --------- | ----------- |
| `<realmname>` | Especifica o nome DNS em maiúsculas, como CORP. CONTOSO.COM e é listado como o realm ou **Realm padrão =** quando **ksetup** é executado. |

#### <a name="remarks"></a>Comentários

- Os sinalizadores de realm especificam recursos adicionais de um realm Kerberos que não se baseiam no sistema operacional Windows Server. Os computadores que executam o Windows Server podem usar um servidor Kerberos para administrar a autenticação no realm do Kerberos, em vez de usar um domínio que esteja executando um sistema operacional Windows Server. Essa entrada estabelece os recursos do Realm e são as seguintes:

| Valor | Sinalizador de realm | Descrição |
| ----- | ---------- | ----------- |
| 0xF | Todos | Todos os sinalizadores de realm estão definidos. |
| 0x00 | Nenhum | Nenhum sinalizador de realm definido, e nenhum recurso adicional está habilitado. |
| 0x01 | sendaddress | O endereço IP será incluído dentro dos tíquetes de concessão de tíquetes. |
| 0x02 | tcpsupported | Tanto o protocolo TCP quanto o UDP (User Datagram Protocol) têm suporte nesse realm. |
| 0x04 | delegado | Todos nesse realm são confiáveis para delegação. |
| 0x08 | ncsupported | Esse Realm dá suporte à canonização de nome, que permite padrões de nomenclatura de DNS e de realm. |
| 0x80 | RC4 | Esse Realm dá suporte à criptografia RC4 para habilitar a relação de confiança entre territórios, o que permite o uso de TLS. |

- Os sinalizadores de realm são armazenados no registro em `HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\Lsa\Kerberos\Domains\<realmname>` . Por padrão, essa entrada não existe no registro. Você pode usar o [comando ksetup addrealmflags](ksetup-addrealmflags.md) para popular o registro.

- Você pode ver os sinalizadores de realm disponíveis e definir exibindo a saída de **ksetup** ou `ksetup /dumpstate` .

### <a name="examples"></a>Exemplos

Para listar os sinalizadores de realm disponíveis para o realm CONTOSO, digite:

```
ksetup /listrealmflags
```

Para remover dois sinalizadores atualmente no conjunto, digite:

```
ksetup /delrealmflags CONTOSO ncsupported delegate
```

Para verificar se os sinalizadores de realm foram removidos, digite `ksetup` e, em seguida, exiba a saída, procurando o texto, **sinalizadores de realm =**.

## <a name="additional-references"></a>Referências adicionais

- [Chave da sintaxe de linha de comando](command-line-syntax-key.md)

- [comando ksetup](ksetup.md)

- [comando ksetup listrealmflags](ksetup-listrealmflags.md)

- [comando ksetup setrealmflags](ksetup-setrealmflags.md)

- [comando ksetup addrealmflags](ksetup-addrealmflags.md)

- [comando ksetup dumpstate](ksetup-dumpstate.md)
