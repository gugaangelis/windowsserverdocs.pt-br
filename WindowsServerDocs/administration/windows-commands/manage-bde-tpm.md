---
title: bde gerenciar tpm
description: 'Tópico de comandos do Windows para * * *- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 11a8530d-edd7-4fe3-ae81-b943766760fe
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 0d41c846034ad421d0da81bda57acbcd419c1ae1
ms.sourcegitcommit: eaf071249b6eb6b1a758b38579a2d87710abfb54
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 05/31/2019
ms.locfileid: "66437394"
---
# <a name="manage-bde-tpm"></a>Gerenciar-bde: tpm

> Aplica-se a: Windows Server (canal semestral), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012
> 
> [!IMPORTANT]
> Não há suporte para esse comando para uso em computadores que executam o Windows 8, Windows Server 2012 ou sistemas operacionais posteriores. Para esses computadores, você pode usar o [os cmdlets de gerenciamento do TPM para o Windows PowerShell](https://docs.microsoft.com/en-us/powershell/module/trustedplatformmodule/).
> Se você estiver usando este comando no computador executando o Windows 7 ou Windows Server 2008, você ainda pode configurar Trusted Platform Module (TPM do computador) usando este comando. Para obter exemplos de como esse comando pode ser usado, consulte [exemplos](#BKMK_Examples).
> ## <a name="syntax"></a>Sintaxe
> ```
> manage-bde -tpm [-turnon] [-takeownership <OwnerPassword>] [-computername <Name>] [{-?|/?}] [{-help|-h}]
> ```
> ### <a name="parameters"></a>Parâmetros
> 
> |    Parâmetro    |                                                                              Descrição                                                                               |
> |-----------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
> |     -turnon     |              Habilita e ativa o TPM, permitindo que a senha de proprietário do TPM seja definido. Você também pode usar **-t** como uma versão abreviada desse comando.              |
> | -takeownership  |                      Assume a propriedade do TPM, definindo uma senha de proprietário. Você também pode usar **-o** como uma versão abreviada desse comando.                       |
> | <OwnerPassword> |                                                      Representa a senha do proprietário que você especificar para o TPM.                                                       |
> |  -computername  | Especifica que bde.exe gerenciar será usado para modificar a proteção do BitLocker em um computador diferente. Você também pode usar **- cn** como uma versão abreviada desse comando. |
> |     <Name>      |    Representa o nome do computador no qual modificar a proteção do BitLocker. Os valores aceitos incluem o nome do computador NetBIOS e endereço IP do computador.     |
> |    -? ou /?     |                                                               Exibe uma breve ajuda no prompt de comando.                                                               |
> |   -help ou -h   |                                                             Exibe concluir ajuda no prompt de comando.                                                              |
> 
> ## <a name="BKMK_Examples"></a>Exemplos
> O exemplo a seguir ilustra o uso de **- tpm** comando para ativar o TPM.
> ```
> manage-bde  tpm -turnon
> ```
> O exemplo a seguir ilustra o uso de **tpm** comando para assumir a propriedade do TPM e definir a senha de proprietário 0wnerP@ss.
> ```
> manage-bde  tpm  takeownership 0wnerP@ss
> ```
> ## <a name="additional-references"></a>Referências adicionais
> -   [Chave da sintaxe de linha de comando](command-line-syntax-key.md)
> -   [manage-bde](manage-bde.md)
