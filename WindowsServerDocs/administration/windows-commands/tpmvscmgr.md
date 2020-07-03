---
title: tpmvscmgr
description: Artigo de referência para tpmvscmgr, que é uma ferramenta de linha de comando que permite aos usuários com credenciais administrativas criar e excluir cartões inteligentes virtuais do TPM em um computador.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 8b2c8ff4-5c5d-446d-99e7-4daa1b36a163
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 8741c947220ce2a3f6852c7374bf0817323bb632
ms.sourcegitcommit: 2afed2461574a3f53f84fc9ec28d86df3b335685
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/02/2020
ms.locfileid: "85935592"
---
# <a name="tpmvscmgr"></a>tpmvscmgr

A ferramenta de linha de comando Tpmvscmgr permite que os usuários com credenciais administrativas criem e excluam cartões inteligentes virtuais do TPM em um computador.

## <a name="syntax"></a>Syntax

```
Tpmvscmgr create [/name] [/AdminKey DEFAULT | PROMPT | RANDOM] [/PIN DEFAULT | PROMPT] [/PUK DEFAULT | PROMPT] [/generate] [/machine] [/?]
```
```
Tpmvscmgr destroy [/instance <instance ID>] [/?]
```

#### <a name="parameters-for-create-command"></a>Parâmetros para o comando Create

O comando Create configura novos cartões inteligentes virtuais no sistema do usuário. Ele retornará a ID de instância do cartão recém-criado para referência posterior se a exclusão for necessária. A ID da instância está no formato **ROOT\SMARTCARDREADER\000n** em que **n** começa em 0 e aumenta em 1 cada vez que você cria um novo cartão inteligente virtual.

|Parâmetro|Descrição|
|---------|-----------|
|/Name|Obrigatórios. Indica o nome do novo cartão inteligente virtual.|
|/AdminKey|Indica a chave de administrador desejada que pode ser usada para redefinir o PIN do cartão se o usuário esquecer o PIN.</br>**Padrão** Especifica o valor padrão de 010203040506070801020304050607080102030405060708.</br>**Aviso** Solicita que o usuário insira um valor para a chave de administrador.</br>**Aleatório** Resulta em uma configuração aleatória para a chave de administrador de um cartão que não é retornado ao usuário. Isso cria um cartão que pode não ser gerenciável usando ferramentas de gerenciamento de cartão inteligente. Quando gerado com RANDOM, a chave de administrador deve ser inserida como 48 caracteres hexadecimais.|
|/PIN|Indica o valor de PIN do usuário desejado.</br>**Padrão** Especifica o PIN padrão de 12345678.</br>**Aviso** Solicita que o usuário insira um PIN na linha de comando. O PIN deve ter no mínimo oito caracteres e pode conter numerais, caracteres e caracteres especiais.|
|/PUK|Indica o valor de PUK (chave de desbloqueio de PIN) desejado. O valor de PUK deve ter no mínimo oito caracteres e pode conter numerais, caracteres e caracteres especiais. Se o parâmetro for omitido, o cartão será criado sem um PUK.</br>**Padrão** Especifica o PUK padrão de 12345678.</br>**Aviso** Solicita que o usuário insira um PUK na linha de comando.|
|/generate|Gera os arquivos no armazenamento que são necessários para que o cartão inteligente virtual funcione. Se o parâmetro/Generate for omitido, será equivalente a criar um cartão sem esse sistema de arquivos. Um cartão sem um sistema de arquivos pode ser gerenciado apenas por um sistema de gerenciamento de cartão inteligente, como o Microsoft Configuration Manager.|
|/machine|Permite que você especifique o nome de um computador remoto no qual o cartão inteligente virtual pode ser criado. Isso pode ser usado somente em um ambiente de domínio e depende do DCOM. Para que o comando tenha sucesso na criação de um cartão inteligente virtual em um computador diferente, o usuário que está executando esse comando deve ser um membro do grupo local de administradores no computador remoto.|
|/?|Exibe a ajuda para este comando.|

#### <a name="parameters-for-destroy-command"></a>Parâmetros para o comando Destroy

O comando Destroy exclui com segurança um cartão inteligente virtual do computador do usuário.

> [!WARNING]
> Quando um cartão inteligente virtual é excluído, ele não pode ser recuperado.

|Parâmetro|Descrição|
|---------|-----------|
|/instance|Especifica a ID da instância do cartão inteligente virtual a ser removido. A instanceID foi gerada como saída por Tpmvscmgr.exe quando o cartão foi criado. O parâmetro/instance é um campo obrigatório para o comando Destroy.|
|/?|Exibe a ajuda para este comando.|

## <a name="remarks"></a>Comentários

A associação no grupo **Administradores** (ou equivalente) no computador de destino é o mínimo necessário para executar todos os parâmetros desse comando.

Para entradas alfanuméricas, o conjunto de caracteres ASCII completo de 127 é permitido.

## <a name="examples"></a>Exemplos

O comando a seguir mostra como criar um cartão inteligente virtual que pode ser gerenciado posteriormente por uma ferramenta de gerenciamento de cartão inteligente iniciada em outro computador.
```
tpmvscmgr.exe create /name VirtualSmartCardForCorpAccess /AdminKey DEFAULT /PIN PROMPT
```
Como alternativa, em vez de usar uma chave de administrador padrão, você pode criar uma chave de administrador na linha de comando. O comando a seguir mostra como criar uma chave de administrador.
```
tpmvscmgr.exe create /name VirtualSmartCardForCorpAccess /AdminKey PROMPT /PIN PROMPT
```
O comando a seguir criará o cartão inteligente virtual não gerenciado que pode ser usado para registrar certificados.
```
tpmvscmgr.exe create /name VirtualSmartCardForCorpAccess /AdminKey RANDOM /PIN PROMPT /generate
```
O comando a seguir criará um cartão inteligente virtual com uma chave de administrador aleatória. A chave é automaticamente descartada após a criação do cartão. Isso significa que, se o usuário esquecer o PIN ou quiser alterar o PIN, o usuário precisará excluir o cartão e criá-lo novamente. Para excluir o cartão, o usuário pode executar o comando a seguir.
```
tpmvscmgr.exe destroy /instance <instance ID>
```
em que \<instance ID> é o valor impresso na tela quando o usuário criou o cartão. Especificamente, para o primeiro cartão criado, a ID da instância é ROOT\SMARTCARDREADER\0000.

## <a name="additional-references"></a>Referências adicionais

- [Chave da sintaxe de linha de comando](command-line-syntax-key.md)