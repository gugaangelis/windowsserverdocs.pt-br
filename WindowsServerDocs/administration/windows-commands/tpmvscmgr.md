---
title: tpmvscmgr
description: 'Tópico de comandos do Windows para * * *- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 8b2c8ff4-5c5d-446d-99e7-4daa1b36a163
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: f6cfc15b7c089c9b6ad4d9a267b951373e63a63a
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59888587"
---
# <a name="tpmvscmgr"></a>tpmvscmgr



A ferramenta de linha de comando Tpmvscmgr permite que os usuários com credenciais administrativas criar e excluir os cartões inteligentes virtuais TPM em um computador. Para obter exemplos de como esse comando pode ser usado, consulte [exemplos](#BKMK_Examples).

## <a name="syntax"></a>Sintaxe

```
Tpmvscmgr create [/name] [/AdminKey DEFAULT | PROMPT | RANDOM] [/PIN DEFAULT | PROMPT] [/PUK DEFAULT | PROMPT] [/generate] [/machine] [/?]
```
```
Tpmvscmgr destroy [/instance <instance ID>] [/?]
```

### <a name="parameters-for-create-command"></a>Parâmetros do comando Create

O comando Create define novos cartões inteligentes virtuais no sistema do usuário. Ele retorna a ID da instância do cartão recém-criado para referência posterior se a exclusão for necessária. A instância de ID está no formato **ROOT\SMARTCARDREADER\000n** onde **n** começa em 0 e é aumentado em 1 sempre que você cria um novo cartão inteligente virtual.

|Parâmetro|Descrição|
|---------|-----------|
|/name|Obrigatório. Indica o nome do novo cartão inteligente virtual.|
|/AdminKey|Indica a chave de administrador desejado pode ser usada para redefinir o PIN do cartão, se o usuário esquecer o PIN.</br>**PADRÃO** Especifica o valor padrão de 010203040506070801020304050607080102030405060708.</br>**PROMPT** solicita que o usuário insira um valor para a chave de administrador.</br>**ALEATÓRIO** resulta em uma configuração aleatória para a chave de administrador para um cartão que não é devolvido ao usuário. Isso cria um cartão que não pode ser gerenciado usando ferramentas de gerenciamento de cartão inteligente. Quando gerado com RANDOM, a chave de administrador deve ser inserida como 48 caracteres hexadecimais.|
|/PIN|Indica o valor PIN do usuário desejado.</br>**PADRÃO** Especifica o PIN 12345678 padrão.</br>**PROMPT** solicita ao usuário inserir um PIN na linha de comando. O PIN deve ter um mínimo de oito caracteres e pode conter números, caracteres e caracteres especiais.|
|/PUK|Indica o valor de chave de desbloqueio do PIN (PUK) desejado. O valor PUK deve ser um mínimo de oito caracteres e pode conter números, caracteres e caracteres especiais. Se o parâmetro for omitido, o cartão é criado sem um PUK.</br>**PADRÃO** Especifica o padrão PUK 12345678.</br>**PROMPT** solicita ao usuário inserir um PUK na linha de comando.|
|/generate|Gera os arquivos no armazenamento que são necessárias ao funcionamento do cartão inteligente virtual. Se o / gerar parâmetro for omitido, ele é equivalente a criar um cartão sem esse sistema de arquivos. Uma placa sem um sistema de arquivos pode ser gerenciada somente por um sistema de gerenciamento de cartão inteligente, como o Microsoft Configuration Manager.|
|/machine|Permite que você especifique o nome de um computador remoto no qual o cartão inteligente virtual pode ser criado. Isso pode ser usado em um ambiente de domínio e ele se baseia no DCOM. Para o comando tenha êxito na criação de um cartão inteligente virtual em um computador diferente, o usuário que executa este comando deve ser um membro do grupo de administradores local no computador remoto.|
|/?|Exibe a Ajuda para este comando.|

### <a name="parameters-for-destroy-command"></a>Parâmetros do comando Destroy

O comando destruir exclui com segurança um cartão inteligente virtual do computador do usuário.

> [!WARNING]
> Quando um cartão inteligente virtual é excluído, ele não pode ser recuperado.

|Parâmetro|Descrição|
|---------|-----------|
|/instance|Especifica a ID da instância do cartão inteligente virtual a ser removido. A instanceID foi gerada como saída pelo Tpmvscmgr.exe quando o cartão foi criado. O parâmetro /instance é um campo obrigatório para o comando destruir.|
|/?|Exibe a Ajuda para este comando.|

## <a name="remarks"></a>Comentários

Associação a **administradores** grupo (ou equivalente) no computador de destino é o mínimo necessário para executar todos os parâmetros desse comando.

Para entradas alfanuméricas, o conjunto completo de 127 caracteres ASCII é permitido.

## <a name="BKMK_Examples"></a>Exemplos

O comando a seguir mostra como criar um cartão inteligente virtual que podem ser gerenciado mais tarde por uma ferramenta de gerenciamento de cartão inteligente iniciada a partir de outro computador.
```
tpmvscmgr.exe create /name "VirtualSmartCardForCorpAccess" /AdminKey DEFAULT /PIN PROMPT
```
Como alternativa, em vez de usar uma chave de administrador padrão, você pode criar uma chave de administrador na linha de comando. O comando a seguir mostra como criar uma chave de administrador.
```
tpmvscmgr.exe create /name "VirtualSmartCardForCorpAccess" /AdminKey PROMPT /PIN PROMPT
```
O comando a seguir criará o cartão inteligente virtual não gerenciado que pode ser usado para registrar certificados.
```
tpmvscmgr.exe create /name "VirtualSmartCardForCorpAccess" /AdminKey RANDOM /PIN PROMPT /generate
```
O comando a seguir criará um cartão inteligente virtual com uma chave aleatória de administrador. A chave será automaticamente descartada após o cardis criado. Isso significa que, se o usuário esquece o PIN ou quer a alteração de PIN, o usuário precisa excluir o cartão e criá-lo novamente. Para excluir o cartão, o usuário pode executar o comando a seguir.
```
tpmvscmgr.exe destroy /instance <instance ID> 
```
onde \<ID da instância > é o valor impresso na tela quando o usuário criou o cartão. Especificamente, para o primeiro cartão criado, a ID da instância é ROOT\SMARTCARDREADER\0000.

#### <a name="additional-references"></a>Referências adicionais

-   [Chave de sintaxe de linha de comando](command-line-syntax-key.md)