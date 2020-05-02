---
title: gerenciar o TPM do bde
description: Tópico de referência para * * * *-
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 11a8530d-edd7-4fe3-ae81-b943766760fe
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: afb3859a1795959c90e71391b2926164165ef9ce
ms.sourcegitcommit: ab64dc83fca28039416c26226815502d0193500c
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 05/01/2020
ms.locfileid: "82724073"
---
# <a name="manage-bde-tpm"></a>Manage-bde: TPM

> Aplica-se a: Windows Server (canal semestral), Windows Server 2019, Windows Server 2016, Windows Server 2012 R2, Windows Server 2012
> 
> [!IMPORTANT]
> Esse comando não tem suporte para uso em computadores que executam o Windows 8, o Windows Server 2012 ou sistemas operacionais posteriores. Para esses computadores, você pode usar os [cmdlets de gerenciamento do TPM para o Windows PowerShell](https://docs.microsoft.com/powershell/module/trustedplatformmodule/).
> Se você estiver usando esse comando no computador que executa o Windows 7 ou o Windows Server 2008, você ainda poderá configurar o Trusted Platform Module do computador (TPM) usando esse comando.
> ## <a name="syntax"></a>Sintaxe
> ```
> manage-bde -tpm [-turnon] [-takeownership <OwnerPassword>] [-computername <Name>] [{-?|/?}] [{-help|-h}]
> ```
> #### <a name="parameters"></a>Parâmetros
> 
> |    Parâmetro    |                                                                              Descrição                                                                               |
> |-----------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
> |     -ativação     |              Habilita e ativa o TPM, permitindo que a senha de proprietário do TPM seja definida. Você também pode usar **-t** como uma versão abreviada desse comando.              |
> | -TakeOwnership  |                      Apropria-se do TPM definindo uma senha de proprietário. Você também pode usar **-o** como uma versão abreviada deste comando.                       |
> | <OwnerPassword> |                                                      Representa a senha do proprietário que você especificar para o TPM.                                                       |
> |  -ComputerName  | Especifica que o Manage-bde. exe será usado para modificar a proteção do BitLocker em um computador diferente. Você também pode usar **-CN** como uma versão abreviada desse comando. |
> |     <Name>      |    Representa o nome do computador no qual a proteção do BitLocker será modificada. Os valores aceitos incluem o nome NetBIOS do computador e o endereço IP do computador.     |
> |    -? ou/?     |                                                               Exibe a ajuda resumida no prompt de comando.                                                               |
> |   -Help ou-h   |                                                             Exibe a ajuda completa no prompt de comando.                                                              |
> 
> ## <a name="examples"></a>Exemplos
> Ilustra o uso do comando **-TPM** para ativar o TPM.
> ```
> manage-bde  tpm -turnon
> ```
> Para ilustrar o uso do comando **TPM** para apropriar-se do TPM e definir a 0wnerP@sssenha do proprietário como.
> ```
> manage-bde  tpm  takeownership 0wnerP@ss
> ```
> ## <a name="additional-references"></a>Referências adicionais
> -   - [Chave da sintaxe de linha de comando](command-line-syntax-key.md)
> -   [manage-bde](manage-bde.md)
