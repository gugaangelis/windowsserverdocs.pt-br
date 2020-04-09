---
title: gerenciar o TPM do bde
description: Tópico de comandos do Windows para * * * *-
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 11a8530d-edd7-4fe3-ae81-b943766760fe
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: b6495bfbfedea7219ae175145f72fc12314ce7ae
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80839759"
---
# <a name="manage-bde-tpm"></a>Manage-bde: TPM

> Aplicável a: Windows Server (canal semestral), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012
> 
> [!IMPORTANT]
> Esse comando não tem suporte para uso em computadores que executam o Windows 8, o Windows Server 2012 ou sistemas operacionais posteriores. Para esses computadores, você pode usar os [cmdlets de gerenciamento do TPM para o Windows PowerShell](https://docs.microsoft.com/powershell/module/trustedplatformmodule/).
> Se você estiver usando esse comando no computador que executa o Windows 7 ou o Windows Server 2008, você ainda poderá configurar o Trusted Platform Module do computador (TPM) usando esse comando. Para obter exemplos de como esse comando pode ser usado, consulte [exemplos](#BKMK_Examples).
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
> ## <a name="examples"></a><a name=BKMK_Examples></a>Disso
> O exemplo a seguir ilustra o uso do comando **-TPM** para ativar o TPM.
> ```
> manage-bde  tpm -turnon
> ```
> O exemplo a seguir ilustra o uso do comando **TPM** para apropriar-se do TPM e definir a senha do proprietário como 0wnerP@ss.
> ```
> manage-bde  tpm  takeownership 0wnerP@ss
> ```
> ## <a name="additional-references"></a>Referências adicionais
> -   - [Chave da sintaxe de linha de comando](command-line-syntax-key.md)
> -   [manage-bde](manage-bde.md)
