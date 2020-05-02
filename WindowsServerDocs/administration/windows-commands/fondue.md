---
title: fondue
description: Tópico de referência para * * * *-
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: fc4467f6-ddbb-4d6d-b51e-5a50a957b8c0
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: e2e3b64ff05ab9a539b7734f37b7515c6f9ef123
ms.sourcegitcommit: ab64dc83fca28039416c26226815502d0193500c
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 05/01/2020
ms.locfileid: "82725594"
---
# <a name="fondue"></a>fondue

> Aplica-se a: Windows Server (canal semestral), Windows Server 2019, Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Habilita recursos opcionais do Windows baixando os arquivos necessários do Windows Update ou outra fonte especificada por Política de Grupo. O arquivo de manifesto do recurso já deve estar instalado na imagem do Windows. 
## <a name="syntax"></a>Sintaxe
```
fondue.exe /enable-feature:<feature_name> [/caller-name:<program_name>] [/hide-ux:{all | rebootRequest}]
```
#### <a name="parameters"></a>Parâmetros

|              Parâmetro              |                                                                                                                                                                     Descrição                                                                                                                                                                     |
|-------------------------------------|-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
|  /Enable-Feature: <*feature_name*>   |                                                                               Especifica o nome do recurso opcional do Windows que você deseja habilitar. Você só pode habilitar um recurso por linha de comando. Para habilitar vários recursos, use fondue. exe para cada recurso.                                                                                |
|    /Caller-Name: <*program_name*>    |                                                                                 Especifica o nome do programa ou processo quando você chama fondue. exe de um script ou arquivo em lotes. Você pode usar essa opção para adicionar o nome do programa ao relatório de SQM se houver um erro.                                                                                 |
| /Hide-UX: {ALL &#124; rebootRequest} | Use **tudo** para ocultar todas as mensagens para o usuário, incluindo as solicitações de progresso e permissão para acessar Windows Update. Se a permissão for necessária, a operação falhará.<p>Use **rebootRequest** para ocultar apenas as mensagens do usuário solicitando permissão para reinicializar o computador. Use esta opção se você tiver um script que controla solicitações de reinicialização. |

## <a name="examples"></a>Exemplos
Para habilitar Microsoft .NET Framework 3,5, digite:
```
fondue.exe /enable-feature:NETFX3
```
Para habilitar Microsoft .NET Framework 3,5, adicione o nome do programa ao relatório de SQM e não exiba mensagens para o usuário, digite:
```
fondue.exe /enable-feature:NETFX3 /caller-name:Admin.bat /hide-ux:all
```
## <a name="additional-references"></a>Referências adicionais
- - [Chave da sintaxe de linha de comando](command-line-syntax-key.md)
  ## <a name="see-also"></a>Consulte Também
  [Considerações de implantação do Microsoft .NET Framework 3.5](https://go.microsoft.com/fwlink/?LinkId=248869)
