---
title: scwcmd rollback
description: Artigo de referência para o comando scwcmd rollback, que aplica a política de reversão mais recente disponível e, em seguida, exclui essa política de reversão.
ms.topic: reference
ms.assetid: 4fd9f89b-0420-420a-ad20-4a328636b1e7
ms.author: lizross
author: eross-msft
manager: mtillman
ms.date: 10/16/2017
ms.openlocfilehash: e8f50995131ca01ea2bb58ee9371b12d9ec9a917
ms.sourcegitcommit: e164aeffc01069b8f1f3248bf106fcdb7f64f894
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/26/2020
ms.locfileid: "91388906"
---
# <a name="scwcmd-rollback"></a>scwcmd rollback

> Aplica-se a: Windows Server 2012 R2 e Windows Server 2012

Aplica a política de reversão mais recente disponível e, em seguida, exclui essa política de reversão.

## <a name="syntax"></a>Sintaxe

```
scwcmd rollback /m:<computername> [/u:<username>] [/pw:<password>]
```

### <a name="parameters"></a>Parâmetros

| Parâmetro | Descrição |
|--|--|
| opção`<computername>` | Especifica o nome NetBIOS, o nome DNS ou o endereço IP de um computador em que a operação de reversão deve ser executada. |
| /u`<username>` | Especifica uma conta de usuário alternativa a ser usada ao executar uma reversão remota. O padrão é o usuário conectado. |
| pt`<password>` | Especifica uma credencial de usuário alternativa a ser usada ao executar uma reversão remota. O padrão é o usuário conectado. |
| /? | Exibe a ajuda no prompt de comando. |

## <a name="examples"></a>Exemplos

Para reverter a política de segurança em um computador no endereço IP *172.16.0.0*, digite:

```
scwcmd rollback /m:172.16.0.0
```

## <a name="additional-references"></a>Referências adicionais

- [Chave da sintaxe de linha de comando](command-line-syntax-key.md)

- [comando scwcmd analyze](scwcmd-analyze.md)

- [comando de configuração de scwcmd](scwcmd-configure.md)

- [comando de registro scwcmd](scwcmd-register.md)

- [comando scwcmd transform](scwcmd-transform.md)

- [comando scwcmd View](scwcmd-view.md)