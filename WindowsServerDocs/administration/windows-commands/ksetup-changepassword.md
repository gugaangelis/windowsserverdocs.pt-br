---
title: ksetup:changepassword
description: 'Tópico de comandos do Windows para * * *- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 283078e7-a88f-4875-90e6-f8605e6b7ea7
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: f629c6c7930777583df38f5af900ed380ec60f9c
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59878527"
---
# <a name="ksetupchangepassword"></a>ksetup:changepassword



Usa o valor de senha (kpasswd) do Centro de distribuição de chaves (KDC) para alterar a senha do usuário conectado. Para obter exemplos de como esse comando pode ser usado, consulte [exemplos](#BKMK_Examples).

## <a name="syntax"></a>Sintaxe

```
ksetup /changepassword <OldPasswd> <NewPasswd>
```

### <a name="parameters"></a>Parâmetros

|Parâmetro|Descrição|
|---------|-----------|
|\<OldPasswd>|Declara o logon de senha do usuário existente.|
|\<NewPasswd>|Declara o fez logon na nova senha do usuário.|

## <a name="remarks"></a>Comentários

Esse comando usa o valor de senha (kpasswd) do KDC para alterar a senha do usuário conectado. Kpasswd, se definido, será exibido na saída, executando o **ksetup /dumpstate** comando.

Nova senha do usuário deve atender a todos os requisitos de senha são definidos neste computador.

Se a conta de usuário não for encontrada no domínio atual, o sistema pedirá que você forneça o nome de domínio onde reside a conta de usuário.

Se você quiser forçar uma alteração de senha no próximo logon, esse comando permite que o uso do asterisco (*) para que o usuário será solicitado para uma nova senha.

A saída do comando informa o status de êxito ou falha.

## <a name="BKMK_Examples"></a>Exemplos

Altere a senha de um usuário que está conectado no momento a este computador neste domínio:
```
ksetup /changepassword Pas$w0rd Pa$$w0rd
```
Altere a senha de um usuário que fez logon no domínio Contoso:
```
ksetup /domain CONTOSO /changepassword Pas$w0rd Pa$$w0rd
```
Força o usuário conectado no momento para alterar a senha no próximo logon:
```
ksetup /changepassword Pas$w0rd *
```

#### <a name="additional-references"></a>Referências adicionais

-   [Chave de sintaxe de linha de comando](command-line-syntax-key.md)