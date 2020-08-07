---
title: Get-AllMulticastTransmissions
description: Artigo de referência para Get-AllMulticastTransmissions, que exibe informações sobre todas as transmissões multicast em um servidor.
ms.topic: article
ms.assetid: 95b8fb79-7a8a-4f0c-88f4-92bc1111c67f
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: c732559e1f302278f21044884ef93f7efdf05ce7
ms.sourcegitcommit: 53d526bfeddb89d28af44210a23ba417f6ce0ecf
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/06/2020
ms.locfileid: "87879839"
---
# <a name="get-allmulticasttransmissions"></a>Get-AllMulticastTransmissions

> Aplica-se a: Windows Server (canal semestral), Windows Server 2019, Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

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
|         /Show         | **Windows Server 2008**<p>/Show: clients – exibe informações sobre os computadores cliente que estão conectados às transmissões multicast.<p>**Windows Server 2008 R2**<p>Show: {boot &#124; instalar &#124; All}-o tipo de imagem a ser retornado.                                A **inicialização** retorna apenas transmissões de imagem de inicialização.                                  **Install** retorna apenas as transmissões de imagem de instalação. **Todos** retorna ambos os tipos de imagem. |
|                         |                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                  |
|    /Details: clientes     |                                                                                                                                                                                              Com suporte apenas para o Windows Server 2008 R2. Se houver, os clientes que estão conectados à transmissão serão exibidos.                                                                                                                                                                                               |
| [/ExcludedeletePending] |                                                                                                                                                                                                                                              Exclui todas as transmissões desativadas da lista.                                                                                                                                                                                                                                               |

## <a name="examples"></a>Exemplos
Para exibir informações sobre todas as transmissões, digite:
- Windows Server 2008:`wdsutil /Get-AllMulticastTransmissions`
- Windows Server 2008 R2: `wdsutil /Get-AllMulticastTransmissions /Show:All` para exibir informações sobre todas as transmissões, exceto as transmissões desativadas, digite:
- Windows Server 2008:`wdsutil /Get-AllMulticastTransmissions /Server:MyWDSServer /Show:Clients /ExcludedeletePending`
- Windows Server 2008 R2:`wdsutil /Get-AllMulticastTransmissions /Server:MyWDSServer /Show:All /details:Clients /ExcludedeletePending`
  ## <a name="additional-references"></a>Referências adicionais
  - Chave de sintaxe [de linha de comando](command-line-syntax-key.md) 
   [Usando o comando](using-the-get-multicasttransmission-command.md) 
   Get-MulticastTransmission [Usando o comando](using-the-new-multicasttransmission-command.md) 
   New-MulticastTransmission [Usando o comando](using-the-remove-multicasttransmission-command.md) 
   Remove-MulticastTransmission [Subcomando: Start-MulticastTransmission](subcommand-start-multicasttransmission.md)
