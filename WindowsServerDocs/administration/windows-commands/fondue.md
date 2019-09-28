---
title: fondue
description: 'Tópico de comandos do Windows para * * * *- '
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: fc4467f6-ddbb-4d6d-b51e-5a50a957b8c0
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: d75d2d9fb57f8888cfc5bf50e2f7796aefc66102
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71377090"
---
# <a name="fondue"></a>fondue

>Aplica-se a: Windows Server (canal semestral), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Habilita recursos opcionais do Windows baixando os arquivos necessários do Windows Update ou outra fonte especificada por Política de Grupo. O arquivo de manifesto do recurso já deve estar instalado na imagem do Windows. 
## <a name="syntax"></a>Sintaxe
```
fondue.exe /enable-feature:<feature_name> [/caller-name:<program_name>] [/hide-ux:{all | rebootRequest}]
```
### <a name="parameters"></a>Parâmetros

|              Parâmetro              |                                                                                                                                                                     Descrição                                                                                                                                                                     |
|-------------------------------------|-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
|  /Enable-Feature: <*feature_name*>   |                                                                               Especifica o nome do recurso opcional do Windows que você deseja habilitar. Você só pode habilitar um recurso por linha de comando. Para habilitar vários recursos, use fondue. exe para cada recurso.                                                                                |
|    /Caller-Name: <*program_name*>    |                                                                                 Especifica o nome do programa ou processo quando você chama fondue. exe de um script ou arquivo em lotes. Você pode usar essa opção para adicionar o nome do programa ao relatório de SQM se houver um erro.                                                                                 |
| /Hide-UX: {todos &#124; os rebootRequest} | Use **tudo** para ocultar todas as mensagens para o usuário, incluindo as solicitações de progresso e permissão para acessar Windows Update. Se a permissão for necessária, a operação falhará.<br /><br />Use **rebootRequest** para ocultar apenas as mensagens do usuário solicitando permissão para reinicializar o computador. Use esta opção se você tiver um script que controla solicitações de reinicialização. |

## <a name="BKMK_Examples"></a>Disso
Para habilitar Microsoft .NET Framework 3,5, digite:
```
fondue.exe /enable-feature:NETFX3
```
Para habilitar Microsoft .NET Framework 3,5, adicione o nome do programa ao relatório de SQM e não exiba mensagens para o usuário, digite:
```
fondue.exe /enable-feature:NETFX3 /caller-name:Admin.bat /hide-ux:all
```
## <a name="additional-references"></a>Referências adicionais
- [Chave da sintaxe de linha de comando](command-line-syntax-key.md)
  ## <a name="see-also"></a>Consulte também
  [Considerações de implantação do Microsoft .NET Framework 3,5](https://go.microsoft.com/fwlink/?LinkId=248869)
