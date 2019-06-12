---
title: Usando o comando get-AllMulticastTransmissions
description: 'Tópico de comandos do Windows para * * *- '
ms.custom: na
ms.prod: windows-server-threshold
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
ms.openlocfilehash: b05f8802a288d80960cf79356675cb9adce9c260
ms.sourcegitcommit: eaf071249b6eb6b1a758b38579a2d87710abfb54
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 05/31/2019
ms.locfileid: "66440529"
---
# <a name="using-the-get-allmulticasttransmissions-command"></a>Usando o comando get-AllMulticastTransmissions

>Aplica-se a: Windows Server (canal semestral), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Exibe informações sobre todas as transmissões multicast em um servidor.
## <a name="syntax"></a>Sintaxe
for Windows Server 2008:
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
|         [/Show]         | **Windows Server 2008**<br /><br />/Show:clients - Exibe informações sobre computadores cliente que estão conectados às transmissões multicast.<br /><br />**Windows Server 2008 R2**<br /><br />Mostrar: {inicialização &#124; instalar &#124; todas as}-o tipo de imagem para retornar.                                **Inicialização** retorna somente as transmissões de imagem de inicialização.                                  **Instalar** retorna instala somente as transmissões de imagem. **Todos os** retorna os dois tipos de imagem. |
|                         |                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                  |
|    /details:clients     |                                                                                                                                                                                              Suporte apenas para Windows Server 2008 R2. Se estiver presente, os clientes que estão conectados a transmissão serão exibidos.                                                                                                                                                                                               |
| [/ExcludedeletePending] |                                                                                                                                                                                                                                              Exclui qualquer transmissões desativadas na lista.                                                                                                                                                                                                                                               |

## <a name="BKMK_examples"></a>Exemplos
Para exibir informações sobre todas as transmissões, digite:
- Windows Server 2008: `wdsutil /Get-AllMulticastTransmissions`
- Windows Server 2008 R2: `wdsutil /Get-AllMulticastTransmissions /Show:All` Para exibir informações sobre todas as transmissões, exceto as transmissões de desativado, digite:
- Windows Server 2008: `wdsutil /Get-AllMulticastTransmissions /Server:MyWDSServer /Show:Clients /ExcludedeletePending`
- Windows Server 2008 R2: `wdsutil /Get-AllMulticastTransmissions /Server:MyWDSServer /Show:All /details:Clients /ExcludedeletePending`
  #### <a name="additional-references"></a>Referências adicionais
  [Chave de sintaxe de linha de comando](command-line-syntax-key.md)
  [usando o comando get-MulticastTransmission](using-the-get-multicasttransmission-command.md)
  [usando o comando /New-MulticastTransmission](using-the-new-multicasttransmission-command.md) 
   [Usando o comando remove-MulticastTransmission](using-the-remove-multicasttransmission-command.md)
  [subcomando: início MulticastTransmission](subcommand-start-multicasttransmission.md)
