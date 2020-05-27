---
title: ksetup addrealmflags
description: Tópico de referência para o comando ksetup addrealmflags, que adiciona sinalizadores de realm adicionais ao realm especificado.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 80ca1e16-8871-494b-b9be-6bc9d63de860
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: c0862462f47189f4904421943e4d3de55c856ace
ms.sourcegitcommit: 4f407b82435afe3111c215510b0ef797863f9cb4
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 05/24/2020
ms.locfileid: "83818026"
---
# <a name="ksetup-addrealmflags"></a>ksetup addrealmflags

Adiciona sinalizadores de realm adicionais ao realm especificado.

## <a name="syntax"></a>Sintaxe

```
ksetup /addrealmflags <realmname> [sendaddress] [tcpsupported] [delegate] [ncsupported] [rc4]
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

Para listar os sinalizadores de realm disponíveis para o realm CONTOSO, digite:

```
ksetup /listrealmflags
```

Para definir dois sinalizadores para o realm CONTOSO, digite:

```
ksetup /setrealmflags CONTOSO ncsupported delegate
```

Para adicionar mais um sinalizador que não está atualmente no conjunto, digite:

```
ksetup /addrealmflags CONTOSO SendAddress
```

Para verificar se o sinalizador de realm está definido, digite `ksetup` e, em seguida, exiba a saída, procurando o texto, **sinalizadores de realm =**. Se você não vir o texto, significa que o sinalizador não foi definido.

## <a name="additional-references"></a>Referências adicionais

- [Chave da sintaxe de linha de comando](command-line-syntax-key.md)

- [comando ksetup](ksetup.md)

- [comando ksetup listrealmflags](ksetup-listrealmflags.md)

- [comando ksetup setrealmflags](ksetup-setrealmflags.md)

- [comando ksetup delrealmflags](ksetup-delrealmflags.md)

- [comando ksetup dumpstate](ksetup-dumpstate.md)
