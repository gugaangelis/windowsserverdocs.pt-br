---
title: klist
description: Artigo de referência para o comando klist, que exibe uma lista de tíquetes Kerberos atualmente armazenados em cache.
ms.topic: reference
ms.assetid: 4689b4a9-1740-47dd-9240-02105efca428
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: d5284feae5da9c8c7fcdab90dd34ce7855128d5f
ms.sourcegitcommit: 96d46c702e7a9c3a321bbbb5284f73911c7baa3c
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/27/2020
ms.locfileid: "89037714"
---
# <a name="klist"></a>klist

Exibe uma lista de tíquetes Kerberos atualmente armazenados em cache.

> [!IMPORTANT]
> Você deve ter pelo menos um **administrador de domínio**ou equivalente para executar todos os parâmetros desse comando.

## <a name="syntax"></a>Sintaxe

```
klist [-lh <logonID.highpart>] [-li <logonID.lowpart>] tickets | tgt | purge | sessions | kcd_cache | get | add_bind | query_bind | purge_bind
```

### <a name="parameters"></a>Parâmetros

| Parâmetro | Descrição |
| --------- | ----------- |
| -LH | Denota a parte superior do identificador local exclusivo (LUID) do usuário, expresso em hexadecimal. Se nem **– LH** nem **– li** estiverem presentes, o comando usa como padrão o LUID do usuário que está conectado no momento. |
| -Li | Denota a parte inferior do identificador local exclusivo (LUID) do usuário, expresso em hexadecimal. Se nem **– LH** nem **– li** estiverem presentes, o comando usa como padrão o LUID do usuário que está conectado no momento. |
| ticket | Lista os tíquetes de concessão de tíquetes (TGTs) e Tíquetes de serviço armazenados em cache no momento da sessão de logon especificada. Essa é a opção padrão. |
| TGT | Exibe o TGT Kerberos inicial. |
| depuração | Permite que você exclua todos os tíquetes da sessão de logon especificada. |
| sessões | Exibe uma lista de sessões de logon neste computador. |
| kcd_cache | Exibe as informações de cache de delegação restrita de Kerberos. |
| get | Permite solicitar um tíquete para o computador de destino especificado pelo SPN (nome da entidade de serviço). |
| add_bind | Permite especificar um controlador de domínio preferencial para a autenticação Kerberos. |
| query_bind | Exibe uma lista de controladores de domínio preferenciais em cache para cada domínio que o Kerberos tenha contatado. |
| purge_bind | Remove os controladores de domínio preferenciais em cache para os domínios especificados. |
| kdcoptions | Exibe as opções de centro de distribuição de chaves (KDC) especificadas no RFC 4120. |
| /? | Exibe a ajuda para este comando. |

#### <a name="remarks"></a>Comentários

- Se nenhum parâmetro for fornecido, **klist** recuperará todos os tíquetes para o usuário conectado no momento.

- Os parâmetros exibem as seguintes informações:

  - **tickets** – lista os tíquetes atualmente armazenados em cache dos serviços que você autenticou desde o logon. Exibe os seguintes atributos de todos os tíquetes em cache:

    - **LogonId:** A LUID.

    - **Cliente:** A concatenação do nome do cliente e o nome de domínio do cliente.

    - **Servidor:** A concatenação do nome do serviço e o nome de domínio do serviço.

    - **Tipo de criptografia KerbTicket:** O tipo de criptografia usado para criptografar o tíquete Kerberos.

    - **Sinalizadores de tíquete:** Os sinalizadores de tíquete Kerberos.

    - **Hora de início:** A hora em que o tíquete é válido.

    - **Hora de término:** A hora em que o tíquete se torna não mais válido. Quando um tíquete é passado desta vez, ele não pode mais ser usado para autenticar em um serviço ou ser usado para renovação.

    - **Hora da renovação:** A hora em que uma nova autenticação inicial é necessária.

    - **Tipo de chave de sessão:** O algoritmo de criptografia que é usado para a chave de sessão.

  - **TGT** -lista o TGT Kerberos inicial e os seguintes atributos do tíquete atualmente armazenado em cache:

    - **LogonId:** Identificado em hexadecimal.

    - **ServiceName:** krbtgt

    - **TargetName `<SPN>` :** krbtgt

    - **Nome_do_domínio:** Nome do domínio que emite o TGT.

    - **TargetDomainName:** Domínio ao qual o TGT é emitido.

    - **AltTargetDomainName:** Domínio ao qual o TGT é emitido.

    - **Sinalizadores de tíquete:** Ações de endereço e destino e tipo.

    - **Chave de sessão:** Comprimento da chave e algoritmo de criptografia.

    - **Iníciotime:** Hora do computador local em que o tíquete foi solicitado.

    - **EndTime:** Hora em que o tíquete se torna não mais válido. Quando um tíquete é passado desta vez, ele não pode mais ser usado para autenticar em um serviço.

    - **RenewUntil:** Prazo para renovação do tíquete.

    - **Distorção de horários:** Diferença de tempo com o centro de distribuição de chaves (KDC).

    - **EncodedTicket:** Tíquete codificado.

  - **limpar** – permite que você exclua um tíquete específico. A limpeza de tíquetes destrói todos os tíquetes armazenados em cache, portanto, use esse atributo com cuidado. Ele pode impedir que você possa se autenticar nos recursos. Se isso acontecer, você precisará fazer logoff e logon novamente.

    - **LogonId:** Identificado em hexadecimal.

  - **sessões** – permite que você liste e exiba as informações de todas as sessões de logon neste computador.

    - **LogonId:** Se especificado, exibe a sessão de logon somente pelo valor especificado. Se não for especificado, exibirá todas as sessões de logon neste computador.

  - **kcd_cache** -permite que você exiba as informações de cache de delegação restrita de Kerberos.

    - **LogonId:** Se especificado, exibe as informações de cache para a sessão de logon pelo valor especificado. Se não for especificado, exibe as informações de cache para a sessão de logon do usuário atual.

  - **Get** -permite solicitar um tíquete para o destino especificado pelo SPN.

    - **LogonId:** Se especificado, solicita um tíquete usando a sessão de logon pelo valor especificado. Se não for especificado, o solicitará um tíquete usando a sessão de logon do usuário atual.

    - **kdcoptions:** Solicita um tíquete com as opções de KDC fornecidas

  - **add_bind** -permite que você especifique um controlador de domínio preferencial para a autenticação Kerberos.

  - **query_bind** -permite exibir controladores de domínio preferenciais em cache para os domínios.

  - **purge_bind** -permite remover controladores de domínio preferenciais em cache para os domínios.

  - **kdcoptions** – para obter a lista atual de opções e suas explicações, consulte [RFC 4120](http://www.ietf.org/rfc/rfc4120.txt).

### <a name="examples"></a>Exemplos

Para consultar o cache de tíquete Kerberos para determinar se algum tíquete está ausente, se o servidor ou a conta de destino estiver em erro ou se não houver suporte para o tipo de criptografia devido a um erro de ID de evento 27, digite:

```
klist
```

```
klist –li 0x3e7
```

Para saber mais sobre as especificidades de cada tíquete de concessão de tíquete que é armazenado em cache no computador para uma sessão de logon, digite:

```
klist tgt
```

Para limpar o cache do tíquete Kerberos, faça logoff e, em seguida, faça logon novamente, digite:

```
klist purge
```

```
klist purge –li 0x3e7
```

Para diagnosticar uma sessão de logon e localizar um LogonId para um usuário ou um serviço, digite:

```
klist sessions
```

Para diagnosticar uma falha de delegação restrita de Kerberos e localizar o último erro encontrado, digite:

```
klist kcd_cache
```

Para diagnosticar se um usuário ou um serviço puder obter um tíquete para um servidor ou solicitar um tíquete para um SPN específico, digite:

```
klist get host/%computername%
```

Para diagnosticar problemas de replicação em controladores de domínio, você normalmente precisa do computador cliente para direcionar um controlador de domínio específico. Para direcionar o computador cliente para o controlador de domínio específico, digite:

```
klist add_bind CONTOSO KDC.CONTOSO.COM
```

```
klist add_bind CONTOSO.COM KDC.CONTOSO.COM
```

Para consultar quais controladores de domínio foram contatados recentemente por este computador, digite:

```
klist query_bind
```

Para redescobrir os controladores de domínio ou para liberar o cache antes de criar novas associações de controlador de domínio com o `klist add_bind` , digite:

```
klist purge_bind
```

## <a name="additional-references"></a>Referências adicionais

- [Chave da sintaxe de linha de comando](command-line-syntax-key.md)