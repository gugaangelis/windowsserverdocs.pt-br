---
title: Usando o comando get-AllDevices
description: 'Tópico de comandos do Windows para * * *- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 5824b3d2-2df1-4ed6-a289-e6c47c13fea2
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: e5f51bcc2332cced906be1eec3265541ffd2d225
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59886367"
---
# <a name="using-the-get-alldevices-command"></a>Usando o comando get-AllDevices

>Aplica-se a: Windows Server (canal semestral), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Exibe as propriedades de serviços de implantação do Windows de todos os computadores em pré-teste. Um computador pré-testado é um computador físico que foi vinculado a uma conta de computador nos serviços de domínio do Active Directory.
## <a name="syntax"></a>Sintaxe
```
wdsutil [Options] /Get-AllDevices [/forest:{Yes | No}] [/ReferralServer:<Server name>]
```
## <a name="parameters"></a>Parâmetros
|Parâmetro|Descrição|
|-------|--------|
|[/ floresta: {Sim &#124; não}]|Especifica se os serviços de implantação do Windows deve retornar os computadores em toda a floresta ou domínio local. A configuração padrão é **não**, o que significa que somente os computadores no domínio local são retornados.|
|[/ReferralServer:<Server name>]|Retorna somente os computadores que são pré-configurados para o servidor especificado.|
## <a name="BKMK_examples"></a>Exemplos
Para exibir todos os computadores, digite um dos seguintes:
```
wdsutil /Get-AllDevices
wdsutil /verbose /Get-AllDevices /forest:Yes /ReferralServer:MyWDSServer
```
#### <a name="additional-references"></a>Referências adicionais
[Chave de sintaxe de linha de comando](command-line-syntax-key.md)
[subcomando: dispositivo conjunto](subcommand-set-device.md)
[usando o comando add-Device](using-the-add-device-command.md)
[usando get-dispositivo Comando](using-the-get-device-command.md)
