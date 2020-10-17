---
title: tpmvscmgr
description: Artigo de referência para tpmvscmgr, que é uma ferramenta de linha de comando que permite aos usuários com credenciais administrativas criar e excluir cartões inteligentes virtuais do TPM em um computador.
ms.topic: reference
ms.assetid: 8b2c8ff4-5c5d-446d-99e7-4daa1b36a163
ms.author: lizross
author: eross-msft
manager: mtillman
ms.date: 10/16/2017
ms.openlocfilehash: 1dfa4a51c0ea75092e3abf885476e77d3c35435a
ms.sourcegitcommit: f45640cf4fda621b71593c63517cfdb983d1dc6a
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 10/17/2020
ms.locfileid: "92156422"
---
# <a name="tpmvscmgr"></a>tpmvscmgr

A ferramenta de linha de comando tpmvscmgr permite que os usuários com credenciais administrativas criem e excluam cartões inteligentes virtuais do TPM em um computador.

## <a name="syntax"></a>Syntax

```
tpmvscmgr create [/name] [/adminkey DEFAULT | PROMPT | RANDOM] [/PIN DEFAULT | PROMPT] [/PUK DEFAULT | PROMPT] [/generate] [/machine] [/?]
```

```
tpmvscmgr destroy [/instance <instanceID>] [/?]
```

### <a name="create-parameters"></a>Criar parâmetros

O comando **Create** configura novos cartões inteligentes virtuais no sistema do usuário. Ele também retorna a ID da instância do cartão recém-criado para referência posterior, se a exclusão for necessária. A ID da instância está no formato **ROOT\SMARTCARDREADER\000n** em que **n** começa em 0 e aumenta em 1 cada vez que você cria um novo cartão inteligente virtual.

| Parâmetro | Descrição |
|--|--|
| /Name | Obrigatórios. Indica o nome do novo cartão inteligente virtual. |
| /adminkey | Indica a chave de administrador desejada que pode ser usada para redefinir o PIN do cartão se o usuário esquecer o PIN. Isso pode incluir:<ul><li>**Padrão** -especifica o valor padrão de *010203040506070801020304050607080102030405060708*.</li><li>**Prompt** -solicita que o usuário insira um valor para a chave de administrador.</li><li>**Aleatório** -resulta em uma configuração aleatória para a chave de administrador de um cartão que não é retornado ao usuário. Isso cria um cartão que pode não ser gerenciável usando ferramentas de gerenciamento de cartão inteligente. Ao usar a opção RANDOM, a chave de administrador deve ser inserida como caracteres hexadecimais 48.</li></ul> |
| /PIN | Indica o valor de PIN do usuário desejado.<ul><li>**Padrão** -especifica o PIN padrão de 12345678.</li><li>**Prompt** -solicita que o usuário insira um PIN na linha de comando. O PIN deve ter no mínimo oito caracteres e pode conter numerais, caracteres e caracteres especiais.</li></ul> |
| /PUK | Indica o valor de PUK (chave de desbloqueio de PIN) desejado. O valor de PUK deve ter no mínimo oito caracteres e pode conter numerais, caracteres e caracteres especiais. Se o parâmetro for omitido, o cartão será criado sem um PUK. As opções incluem:<ul><li>**Padrão** -especifica o PUK padrão de *12345678*.</li><li>**Prompt** -solicita ao usuário para inserir um PUK na linha de comando.</li></ul> |
| /generate | Gera os arquivos no armazenamento que são necessários para que o cartão inteligente virtual funcione. Se você não usar o parâmetro **/Generate** , será como você criou o cartão sem o sistema de arquivos subjacente. Um cartão sem um sistema de arquivos pode ser gerenciado apenas por um sistema de gerenciamento de cartão inteligente, como o Microsoft Configuration Manager. |
| /machine | Permite que você especifique o nome de um computador remoto no qual o cartão inteligente virtual pode ser criado. Isso pode ser usado somente em um ambiente de domínio e depende do DCOM. Para que o comando tenha sucesso na criação de um cartão inteligente virtual em um computador diferente, o usuário que está executando esse comando deve ser um membro do grupo local de administradores no computador remoto. |
| /? | Exibe a ajuda para este comando. |

### <a name="destroy-parameters"></a>Destruir parâmetros

O comando **Destroy** exclui com segurança um cartão inteligente virtual do computador do usuário.

> [!WARNING]
> Se um cartão inteligente virtual for excluído, ele não poderá ser recuperado.

| Parâmetro | Descrição |
|--|--|
| /instance | Especifica a ID da instância do cartão inteligente virtual a ser removido. A instanceID foi gerada como saída por tpmvscmgr.exe quando o cartão foi criado. O parâmetro **/Instance** é um campo obrigatório para o comando **Destroy** . |
| /? | Exibe a ajuda no prompt de comando. |

#### <a name="remarks"></a>Comentários

- Para entradas alfanuméricas, o conjunto de caracteres ASCII completo de 127 é permitido.

## <a name="examples"></a>Exemplos

Para criar um cartão inteligente virtual que pode ser gerenciado posteriormente por uma ferramenta de gerenciamento de cartão inteligente iniciada em outro computador, digite:

```
tpmvscmgr.exe create /name VirtualSmartCardForCorpAccess /AdminKey DEFAULT /PIN PROMPT
```

Como alternativa, em vez de usar uma chave de administrador padrão, você pode criar uma chave de administrador na linha de comando. O comando a seguir mostra como criar uma chave de administrador.

```
tpmvscmgr.exe create /name VirtualSmartCardForCorpAccess /AdminKey PROMPT /PIN PROMPT
```

Para criar um cartão inteligente virtual não gerenciado que pode ser usado para registrar certificados, digite:

```
tpmvscmgr.exe create /name VirtualSmartCardForCorpAccess /AdminKey RANDOM /PIN PROMPT /generate
```

Um cartão inteligente virtual é criado com uma chave de administrador aleatória. A chave é automaticamente descartada após a criação do cartão. Isso significa que, se o usuário esquecer o PIN ou quiser alterar o PIN, o usuário precisará excluir o cartão e criá-lo novamente.

Para excluir o cartão, digite:

```
tpmvscmgr.exe destroy /instance <instance ID>
```

Em que `<instanceID>` é o valor impresso na tela quando o usuário criou o cartão. Especificamente, para o primeiro cartão criado, a ID da instância é *ROOT\SMARTCARDREADER\0000*.

## <a name="additional-references"></a>Referências adicionais

- [Chave da sintaxe de linha de comando](command-line-syntax-key.md)
