---
title: fondue
description: Tópico de referência para o comando fondue, que habilita os recursos opcionais do Windows baixando os arquivos necessários do Windows Update ou outra fonte especificada por Política de Grupo.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: fc4467f6-ddbb-4d6d-b51e-5a50a957b8c0
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 7a9e751a5ad46d557aa2317ebe4c144fa6f004fa
ms.sourcegitcommit: bf887504703337f8ad685d778124f65fe8c3dc13
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 05/16/2020
ms.locfileid: "83437201"
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
| /Enable-Feature`<feature_name>` | Especifica o nome do recurso opcional do Windows que você deseja habilitar. Você só pode habilitar um recurso por linha de comando. Para habilitar vários recursos, use fondue. exe para cada recurso. |
| /caller-name:`<program_name>` | Especifica o nome do programa ou processo quando você chama fondue. exe de um script ou arquivo em lotes. Você pode usar essa opção para adicionar o nome do programa ao relatório de SQM se houver um erro. |
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
