---
title: Usando o comando Get-AllMulticastTransmissions
description: 'Tópico de comandos do Windows para * * * *- '
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 95b8fb79-7a8a-4f0c-88f4-92bc1111c67f
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 644684ffb356ef07120bc391e3d3da2daf768eaf
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71363336"
---
# <a name="using-the-get-allmulticasttransmissions-command"></a>Usando o comando Get-AllMulticastTransmissions

>Aplica-se a: Windows Server (canal semestral), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Exibe informações sobre todas as transmissões multicast em um servidor.
## <a name="syntax"></a>Sintaxe
para o Windows Server 2008:
```
wdsutil /Get-AllMulticastTransmissions [/Server:<Server name>] [/Show:Clients] [/ExcludedeletePending]
```
para o Windows Server 2008 R2:
```
wdsutil /Get-AllMulticastTransmissions [/Server:<Server name>] [/Show:{Boot | Install | All}] [/details:Clients]  [/ExcludedeletePending]
```
## <a name="parameters"></a>Parâmetros

|        Parâmetro        |                                                                                                                                                                                                                                                                   Explicação                                                                                                                                                                                                                                                                    |
|-------------------------|--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| [/Server:<Server name>] |                                                                                                                                                                                 Especifica o nome do servidor. Pode ser o nome NetBIOS ou o FQDN (nome de domínio totalmente qualificado). Se nenhum nome de servidor for especificado, o servidor local será usado.                                                                                                                                                                                  |
|         /Show         | **Windows Server 2008**<br /><br />/Show: clients – exibe informações sobre os computadores cliente que estão conectados às transmissões multicast.<br /><br />**Windows Server 2008 R2**<br /><br />Mostrar: {boot &#124; install &#124; All}-o tipo de imagem a ser retornado.                                A **inicialização** retorna apenas transmissões de imagem de inicialização.                                  **Install** retorna apenas as transmissões de imagem de instalação. **Todos** retorna ambos os tipos de imagem. |
|                         |                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                  |
|    /Details: clientes     |                                                                                                                                                                                              Com suporte apenas para o Windows Server 2008 R2. Se houver, os clientes que estão conectados à transmissão serão exibidos.                                                                                                                                                                                               |
| [/ExcludedeletePending] |                                                                                                                                                                                                                                              Exclui todas as transmissões desativadas da lista.                                                                                                                                                                                                                                               |

## <a name="BKMK_examples"></a>Disso
Para exibir informações sobre todas as transmissões, digite:
- Windows Server 2008: `wdsutil /Get-AllMulticastTransmissions`
- Windows Server 2008 R2: `wdsutil /Get-AllMulticastTransmissions /Show:All` para exibir informações sobre todas as transmissões, exceto as transmissões desativadas, digite:
- Windows Server 2008: `wdsutil /Get-AllMulticastTransmissions /Server:MyWDSServer /Show:Clients /ExcludedeletePending`
- Windows Server 2008 R2: `wdsutil /Get-AllMulticastTransmissions /Server:MyWDSServer /Show:All /details:Clients /ExcludedeletePending`
  #### <a name="additional-references"></a>Referências adicionais
  [Chave de sintaxe de linha de comando](command-line-syntax-key.md)
  [usando o comando get-MulticastTransmission](using-the-get-multicasttransmission-command.md)
  [usando o comando New-MulticastTransmission](using-the-new-multicasttransmission-command.md)
  [usando o comando Remove-MulticastTransmission](using-the-remove-multicasttransmission-command.md)
  [ Subcomando: Start-MulticastTransmission](subcommand-start-multicasttransmission.md)
