---
title: 'ksetup: ChangePassword'
description: Tópico de referência para * * * *-
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 283078e7-a88f-4875-90e6-f8605e6b7ea7
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 32c48e896b77043820eea42159e20c089bd69fb8
ms.sourcegitcommit: ab64dc83fca28039416c26226815502d0193500c
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 05/01/2020
ms.locfileid: "82724720"
---
# <a name="ksetupchangepassword"></a>ksetup: ChangePassword



Usa o valor de centro de distribuição de chaves (KDC) senha (kpasswd) para alterar a senha do usuário conectado.

## <a name="syntax"></a>Sintaxe

```
ksetup /changepassword <OldPasswd> <NewPasswd>
```

#### <a name="parameters"></a>Parâmetros

|Parâmetro|Descrição|
|---------|-----------|
|\<> OldPasswd|Declara a senha existente do usuário conectado.|
|\<> NewPasswd|Declara a nova senha do usuário conectado.|

## <a name="remarks"></a>Comentários

Esse comando usa o valor de senha do KDC (kpasswd) para alterar a senha do usuário conectado. O kpasswd, se definido, é exibido na saída executando o comando **ksetup/dumpstate** .

A nova senha do usuário deve atender a todos os requisitos de senha definidos neste computador.

Se a conta de usuário não for encontrada no domínio atual, o sistema solicitará que você forneça o nome de domínio onde a conta de usuário reside.

Se você quiser forçar uma alteração de senha no próximo logon, esse comando permitirá o uso do asterisco (*), de modo que o usuário será solicitado a fornecer uma nova senha.

A saída do comando informa o status de êxito ou falha.

## <a name="examples"></a>Exemplos

Altere a senha de um usuário que está conectado no momento neste computador neste domínio:
```
ksetup /changepassword Pas$w0rd Pa$$w0rd
```
Altere a senha de um usuário que está conectado no momento no domínio contoso:
```
ksetup /domain CONTOSO /changepassword Pas$w0rd Pa$$w0rd
```
Forçar o usuário conectado no momento a alterar a senha no próximo logon:
```
ksetup /changepassword Pas$w0rd *
```

## <a name="additional-references"></a>Referências adicionais

-   - [Chave da sintaxe de linha de comando](command-line-syntax-key.md)