---
title: tskill
description: Artigo de referência para o comando tskill, que encerra um processo em execução em uma sessão em um servidor de Host da Sessão da Área de Trabalho Remota.
ms.topic: reference
ms.assetid: 08986e6a-6900-4ece-85a1-8f73b14db1b3
ms.author: lizross
author: eross-msft
manager: mtillman
ms.date: 10/16/2017
ms.openlocfilehash: b281df4852bc5fbc0756e7b052d82ea2f9bab6d5
ms.sourcegitcommit: f45640cf4fda621b71593c63517cfdb983d1dc6a
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 10/17/2020
ms.locfileid: "92156374"
---
# <a name="tskill"></a>tskill

> Aplica-se a: Windows Server (canal semestral), Windows Server 2019, Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Encerra um processo em execução em uma sessão em um servidor de Host da Sessão da Área de Trabalho Remota.

> [!NOTE]
> Você pode usar esse comando para finalizar somente os processos que pertencem a você, a menos que você seja um administrador. Os administradores têm acesso total a todas as funções **tskill** e podem finalizar processos que estão em execução em outras sessões de usuário.
>
> Para descobrir as novidades da versão mais recente, consulte Novidades do [serviços de área de trabalho remota no Windows Server](/previous-versions/windows/it-pro/windows-server-2012-r2-and-2012/dn283323(v=ws.11)).

## <a name="syntax"></a>Sintaxe

```
tskill {<processID> | <processname>} [/server:<servername>] [/id:<sessionID> | /a] [/v]
```

### <a name="parameters"></a>Parâmetros

| Parâmetro | Descrição |
|--|--|
| `<processID>` | Especifica a ID do processo que você deseja encerrar. |
| `<processname>` | Especifica o nome do processo que você deseja encerrar. Esse parâmetro pode incluir caracteres curinga. |
| /server:`<servername>` | Especifica o servidor de terminal que contém o processo que você deseja encerrar. Se **/Server** não for especificado, o servidor de host da sessão da área de trabalho remota atual será usado. |
| /ID`<sessionID>` | Encerra o processo que está sendo executado na sessão especificada. |
| /a | Encerra o processo que está sendo executado em todas as sessões. |
| /v | Exibe informações sobre as ações que estão sendo executadas. |
| /? | Exibe a ajuda no prompt de comando. |

#### <a name="remarks"></a>Comentários

- Quando todos os processos que estão sendo executados em uma sessão terminam, a sessão também termina.

- Se você usar os `<processname>` parâmetros e `/server:<servername>` , também deverá especificar o `/id:<sessionID>` parâmetro ou o **/a** .

## <a name="examples"></a>Exemplos

Para encerrar o processo 6543, digite:

```
tskill 6543
```

Para finalizar o Gerenciador de processos em execução na sessão 5, digite:

```
tskill explorer /id:5
```

## <a name="additional-references"></a>Referências adicionais

- [Chave da sintaxe de linha de comando](command-line-syntax-key.md)

- [Referência aos comandos dos Serviços de Área de Trabalho Remota (Serviços de Terminal)](remote-desktop-services-terminal-services-command-reference.md)
