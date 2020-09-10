---
title: fondue
description: Artigo de referência para o comando fondue, que habilita os recursos opcionais do Windows baixando os arquivos necessários do Windows Update ou outra fonte especificada por Política de Grupo.
ms.topic: reference
ms.assetid: fc4467f6-ddbb-4d6d-b51e-5a50a957b8c0
ms.author: lizross
author: eross-msft
manager: mtillman
ms.date: 10/16/2017
ms.openlocfilehash: c80c6b1aef9ea37bdb4ff497ff7a7b5e54f8c468
ms.sourcegitcommit: db2d46842c68813d043738d6523f13d8454fc972
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/10/2020
ms.locfileid: "89634841"
---
# <a name="fondue"></a>fondue

> Aplica-se a: Windows Server (canal semestral), Windows Server 2019, Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Habilita recursos opcionais do Windows baixando os arquivos necessários do Windows Update ou outra fonte especificada por Política de Grupo. O arquivo de manifesto do recurso já deve estar instalado na imagem do Windows.

## <a name="syntax"></a>Sintaxe

```
fondue.exe /enable-feature:<feature_name> [/caller-name:<program_name>] [/hide-ux:{all | rebootrequest}]
```

### <a name="parameters"></a>Parâmetros

| Parâmetro | Descrição |
| --------- | ----------- |
| /Enable-Feature`<feature_name>` | Especifica o nome do recurso opcional do Windows que você deseja habilitar. Você só pode habilitar um recurso por linha de comando. Para habilitar vários recursos, use fondue.exe para cada recurso. |
| /caller-name:`<program_name>` | Especifica o nome do programa ou processo quando você chama fondue.exe de um script ou arquivo em lotes. Você pode usar essa opção para adicionar o nome do programa ao relatório de SQM se houver um erro. |
| /hide-ux:`{all | rebootrequest}` | Use **tudo** para ocultar todas as mensagens para o usuário, incluindo as solicitações de progresso e permissão para acessar Windows Update. Se a permissão for necessária, a operação falhará.<p>Use **rebootrequest** para ocultar apenas as mensagens do usuário solicitando permissão para reinicializar o computador. Use esta opção se você tiver um script que controla solicitações de reinicialização. |

### <a name="examples"></a>Exemplos

Para habilitar Microsoft .NET Framework 4,8, digite:

```
fondue.exe /enable-feature:NETFX4
```

Para habilitar Microsoft .NET Framework 4,8, adicione o nome do programa ao relatório de SQM e não exiba mensagens para o usuário, digite:

```
fondue.exe /enable-feature:NETFX4 /caller-name:Admin.bat /hide-ux:all
```

## <a name="additional-references"></a>Referências adicionais

- [Chave da sintaxe de linha de comando](command-line-syntax-key.md)

- [Download do Microsoft .NET Framework 4,8](https://dotnet.microsoft.com/download/dotnet-framework/net48)
