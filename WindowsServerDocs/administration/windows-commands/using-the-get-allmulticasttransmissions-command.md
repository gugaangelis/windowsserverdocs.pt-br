---
title: Get-AllMulticastTransmissions
description: O tópico de comandos do Windows para Get-AllMulticastTransmissions, que exibe informações sobre todas as transmissões multicast em um servidor.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 95b8fb79-7a8a-4f0c-88f4-92bc1111c67f
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: c81db5a5d5ebb9bcc5e2d00165d5c944c687fd97
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80831320"
---
# <a name="get-allmulticasttransmissions"></a>Get-AllMulticastTransmissions

>Aplicável a: Windows Server (canal semestral), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

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
### <a name="parameters"></a>Parâmetros

|        Parâmetro        |                                                                                                                                                                                                                                                                   Explicação                                                                                                                                                                                                                                                                    |
|-------------------------|--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| [/Server:<Server name>] |                                                                                                                                                                                 Especifica o nome do servidor. Pode ser o nome NetBIOS ou o FQDN (nome de domínio totalmente qualificado). Se nenhum nome de servidor for especificado, o servidor local será usado.                                                                                                                                                                                  |
|         /Show         | **Windows Server 2008**<p>/Show: clients – exibe informações sobre os computadores cliente que estão conectados às transmissões multicast.<p>**Windows Server 2008 R2**<p>Mostrar: {boot &#124; install &#124; All}-o tipo de imagem a ser retornado.                                A **inicialização** retorna apenas transmissões de imagem de inicialização.                                  **Install** retorna apenas as transmissões de imagem de instalação. **Todos** retorna ambos os tipos de imagem. |
|                         |                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                  |
|    /Details: clientes     |                                                                                                                                                                                              Com suporte apenas para o Windows Server 2008 R2. Se houver, os clientes que estão conectados à transmissão serão exibidos.                                                                                                                                                                                               |
| [/ExcludedeletePending] |                                                                                                                                                                                                                                              Exclui todas as transmissões desativadas da lista.                                                                                                                                                                                                                                               |

## <a name="examples"></a><a name=BKMK_examples></a>Disso
Para exibir informações sobre todas as transmissões, digite:
- Windows Server 2008: `wdsutil /Get-AllMulticastTransmissions`
- Windows Server 2008 R2: `wdsutil /Get-AllMulticastTransmissions /Show:All` para exibir informações sobre todas as transmissões, exceto as transmissões desativadas, digite:
- Windows Server 2008: `wdsutil /Get-AllMulticastTransmissions /Server:MyWDSServer /Show:Clients /ExcludedeletePending`
- Windows Server 2008 R2: `wdsutil /Get-AllMulticastTransmissions /Server:MyWDSServer /Show:All /details:Clients /ExcludedeletePending`
  ## <a name="additional-references"></a>Referências adicionais
  - A [chave de sintaxe de linha de comando](command-line-syntax-key.md)
  [usando o comando get-MulticastTransmission](using-the-get-multicasttransmission-command.md)
  [usando o comando New-MulticastTransmission](using-the-new-multicasttransmission-command.md)
  [usando o comando Remove-MulticastTransmission](using-the-remove-multicasttransmission-command.md)
  [subcomando: Start-MulticastTransmission](subcommand-start-multicasttransmission.md)
