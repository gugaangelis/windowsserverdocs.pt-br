---
title: 'ksetup: ChangePassword'
description: Tópico de comandos do Windows para * * * *-
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 283078e7-a88f-4875-90e6-f8605e6b7ea7
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 68b14388ff3c33458873b494c8d5a770b44f7545
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80841779"
---
# <a name="ksetupchangepassword"></a>ksetup: ChangePassword



Usa o valor de centro de distribuição de chaves (KDC) senha (kpasswd) para alterar a senha do usuário conectado. Para obter exemplos de como esse comando pode ser usado, consulte [exemplos](#BKMK_Examples).

## <a name="syntax"></a>Sintaxe

```
ksetup /changepassword <OldPasswd> <NewPasswd>
```

#### <a name="parameters"></a>Parâmetros

|Parâmetro|Descrição|
|---------|-----------|
|\<OldPasswd >|Declara a senha existente do usuário conectado.|
|\<NewPasswd >|Declara a nova senha do usuário conectado.|

## <a name="remarks"></a>Comentários

Esse comando usa o valor de senha do KDC (kpasswd) para alterar a senha do usuário conectado. O kpasswd, se definido, é exibido na saída executando o comando **ksetup/dumpstate** .

A nova senha do usuário deve atender a todos os requisitos de senha definidos neste computador.

Se a conta de usuário não for encontrada no domínio atual, o sistema solicitará que você forneça o nome de domínio onde a conta de usuário reside.

Se você quiser forçar uma alteração de senha no próximo logon, esse comando permitirá o uso do asterisco (*), de modo que o usuário será solicitado a fornecer uma nova senha.

A saída do comando informa o status de êxito ou falha.

## <a name="examples"></a><a name=BKMK_Examples></a>Disso

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