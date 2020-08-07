---
title: Get-meus dispositivos
description: Artigo de referência para Get-Devices, que exibe as propriedades dos serviços de implantação do Windows de todos os computadores pré-configurados.
ms.topic: article
ms.assetid: 5824b3d2-2df1-4ed6-a289-e6c47c13fea2
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 1e527be333570838ecb675d78742bbf55918eff3
ms.sourcegitcommit: 53d526bfeddb89d28af44210a23ba417f6ce0ecf
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/06/2020
ms.locfileid: "87896974"
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
