---
title: Get-meus dispositivos
description: Tópico de comandos do Windows para Get-meus dispositivos, que exibe as propriedades dos serviços de implantação do Windows de todos os computadores pré-configurados.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 5824b3d2-2df1-4ed6-a289-e6c47c13fea2
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 929a74b6cccaed6e85015648538c1ca875b62208
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80831409"
---
# <a name="get-alldevices"></a>Get-meus dispositivos

>Aplicável a: Windows Server (canal semestral), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Exibe as propriedades dos serviços de implantação do Windows de todos os computadores pré-configurados. Um computador pré-configurado é um computador físico que foi vinculado a uma conta de computador nos serviços de domínio Active Directory.

## <a name="syntax"></a>Sintaxe
```
wdsutil [Options] /Get-AllDevices [/forest:{Yes | No}] [/ReferralServer:<Server name>]
```
### <a name="parameters"></a>Parâmetros
|Parâmetro|Descrição|
|-------|--------|
|[/Forest: {Sim &#124; não}]|Especifica se os serviços de implantação do Windows devem retornar computadores em toda a floresta ou no domínio local. A configuração padrão é **não**, o que significa que somente os computadores no domínio local são retornados.|
|[/ReferralServer:<Server name>]|Retorna somente os computadores que estão pré-configurados para o servidor especificado.|
## <a name="examples"></a><a name=BKMK_examples></a>Disso
Para exibir todos os computadores, digite um dos seguintes:
```
wdsutil /Get-AllDevices
wdsutil /verbose /Get-AllDevices /forest:Yes /ReferralServer:MyWDSServer
```
## <a name="additional-references"></a>Referências adicionais
- [Chave de sintaxe de linha de comando](command-line-syntax-key.md)
[subcomando: set-Device](subcommand-set-device.md)
[usando o comando Add-Device](using-the-add-device-command.md)
[usando o comando Get-Device](using-the-get-device-command.md)
