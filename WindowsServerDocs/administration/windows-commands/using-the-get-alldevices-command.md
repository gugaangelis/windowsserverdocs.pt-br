---
title: Get-meus dispositivos
description: Artigo de referência para Get-Devices, que exibe as propriedades dos serviços de implantação do Windows de todos os computadores pré-configurados.
ms.topic: reference
ms.assetid: 5824b3d2-2df1-4ed6-a289-e6c47c13fea2
ms.author: lizross
author: eross-msft
manager: mtillman
ms.date: 10/16/2017
ms.openlocfilehash: 5087b67c15b3969085d3c8c66072f09396165614
ms.sourcegitcommit: db2d46842c68813d043738d6523f13d8454fc972
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/10/2020
ms.locfileid: "89621977"
---
# <a name="get-alldevices"></a>Get-meus dispositivos

> Aplica-se a: Windows Server (canal semestral), Windows Server 2019, Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Exibe as propriedades dos serviços de implantação do Windows de todos os computadores pré-configurados. Um computador pré-configurado é um computador físico que foi vinculado a uma conta de computador nos serviços de domínio Active Directory.

## <a name="syntax"></a>Sintaxe
```
wdsutil [Options] /Get-AllDevices [/forest:{Yes | No}] [/ReferralServer:<Server name>]
```
### <a name="parameters"></a>Parâmetros
|Parâmetro|Descrição|
|-------|--------|
|[/Forest: {Sim &#124; não}]|Especifica se os serviços de implantação do Windows devem retornar computadores em toda a floresta ou no domínio local. A configuração padrão é **não**, o que significa que somente os computadores no domínio local são retornados.|
|[/ReferralServer: <Server name> ]|Retorna somente os computadores que estão pré-configurados para o servidor especificado.|
## <a name="examples"></a>Exemplos
Para exibir todos os computadores, digite um dos seguintes:
```
wdsutil /Get-AllDevices
wdsutil /verbose /Get-AllDevices /forest:Yes /ReferralServer:MyWDSServer
```
## <a name="additional-references"></a>Referências adicionais
- Chave de sintaxe [de linha de comando](command-line-syntax-key.md) 
 [Subcomando: Set-Device](subcommand-set-device.md) 
 [Usando o comando](using-the-add-device-command.md) 
 Add-Device [Usando o comando Get-Device](using-the-get-device-command.md)
