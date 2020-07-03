---
title: freedisk
description: Artigo de referência para o comando freedisk, que verifica se a quantidade especificada de espaço em disco está disponível antes de continuar com um processo de instalação.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 91c15166-5baa-4b80-9e0c-4cd815d00530
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: d0cfce52c2eaf0917f8169d959b61832bd1779e0
ms.sourcegitcommit: 2afed2461574a3f53f84fc9ec28d86df3b335685
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/02/2020
ms.locfileid: "85924748"
---
# <a name="freedisk"></a>freedisk

> Aplica-se a: Windows Server (canal semestral), Windows Server 2019, Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Verifica se a quantidade especificada de espaço em disco está disponível antes de continuar com um processo de instalação.

## <a name="syntax"></a>Sintaxe

```
freedisk [/s <computer> [/u [<domain>\]<user> [/p [<password>]]]] [/d <drive>] [<value>]
```

### <a name="parameters"></a>Parâmetros

| Parâmetro | Descrição |
| --------- | ----------- |
| /s`<computer>` | Especifica o nome ou o endereço IP de um computador remoto (não use barras invertidas). O padrão é o computador local. Esse parâmetro se aplica a todos os arquivos e pastas especificados no comando. |
| /u`[<domain>\]<user>` | Executa o script com as permissões da conta de usuário especificada. O padrão é permissões do sistema. |
| /p [ <password> ] | Especifica a senha da conta de usuário que é especificada em **/u**. |
| /d`<drive>` | Especifica a unidade para a qual você deseja descobrir a disponibilidade de espaço livre. Você deve especificar `<drive>` para um computador remoto. |
| `<value>` | Verifica se há uma quantidade específica de espaço livre em disco. Você pode especificar `<value>` em bytes, KB, MB, GB, TB, PB, EB, ZB ou YB. |

#### <a name="remarks"></a>Comentários

- Usar as opções de linha de comando **/s**, **/u**e **/p** só estará disponível quando você usar **/s**. Você deve usar **/p** com **/u**para fornecer a senha do usuário.

- Para instalações autônomas, você pode usar **freedisk** em arquivos em lotes de instalação para verificar a quantidade de pré-requisitos de espaço livre antes de continuar com a instalação.

- Quando você usar **freedisk** em um arquivo em lotes, retornará **0** se houver espaço suficiente e um **1** se não houver espaço suficiente.

### <a name="examples"></a>Exemplos

Para determinar se há pelo menos 50 MB de espaço livre disponível na unidade C:, digite:

```
freedisk 50mb
```

Uma saída semelhante à seguinte aparece na tela:

```
INFO: The specified 52,428,800 byte(s) of free space is available on current drive.
```

## <a name="additional-references"></a>Referências adicionais

- [Chave da sintaxe de linha de comando](command-line-syntax-key.md)
