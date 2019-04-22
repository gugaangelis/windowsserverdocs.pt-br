---
title: fondue
description: 'Tópico de comandos do Windows para * * *- '
ms.custom: na
ms.prod: windows-server-threshold
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
ms.openlocfilehash: 09708c239b5399f3284c42877970443cc2605cbe
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59817147"
---
# <a name="fondue"></a>fondue

>Aplica-se a: Windows Server (canal semestral), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Permite que os recursos opcionais do Windows, baixando os arquivos necessários do Windows Update ou outra fonte especificado pela diretiva de grupo. O arquivo de manifesto para o recurso já deve estar instalado em sua imagem do Windows. 
## <a name="syntax"></a>Sintaxe
```
fondue.exe /enable-feature:<feature_name> [/caller-name:<program_name>] [/hide-ux:{all | rebootRequest}]
```
### <a name="parameters"></a>Parâmetros
|Parâmetro|Descrição|
|-------|--------|
|/Enable-Feature: <*nome_do_recurso*>|Especifica o nome do recurso opcional do Windows que você deseja habilitar. Só é possível habilitar um recurso por linha de comando. Para habilitar vários recursos, use fondue.exe para cada recurso.|
|/caller-name:<*program_name*>|Especifica o nome do programa ou processo quando você chama fondue.exe de um arquivo de script ou lote. Você pode usar essa opção para adicionar o nome do programa para o relatório SQM, se houver um erro.|
|/hide-ux:{all &#124; rebootRequest}|Use **todos os** para ocultar todas as mensagens para o usuário, incluindo solicitações de andamento e a permissão para acessar o Windows Update. Se a permissão é necessária, a operação falhará.<br /><br />Use **rebootRequest** apenas ocultar mensagens de usuário solicitando permissão para reinicializar o computador. Use esta opção se você tiver um script que controles reinicialize as solicitações.|
## <a name="BKMK_Examples"></a>Exemplos
Para habilitar o Microsoft .NET Framework 3.5, digite:
```
fondue.exe /enable-feature:NETFX3
```
Para habilitar o Microsoft .NET Framework 3.5, adicione o nome do programa para o relatório SQM e exibe mensagens para o usuário, tipo:
```
fondue.exe /enable-feature:NETFX3 /caller-name:Admin.bat /hide-ux:all
```
## <a name="additional-references"></a>Referências adicionais
-   [Chave de sintaxe de linha de comando](command-line-syntax-key.md)
## <a name="see-also"></a>Consulte também
[Considerações de implantação do Microsoft .NET Framework 3.5](https://go.microsoft.com/fwlink/?LinkId=248869)
