---
title: arp
description: Tópico de comandos do Windows para **ARP** – exibe e modifica entradas no cache ARP (protocolo de resolução de endereço) usado para armazenar endereços IP e seus endereços físicos resolvidos.
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 827e96eb-1945-483f-980f-714703456f7c
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 8354df3f52790840e0cb0c5c9834da2722d27d43
ms.sourcegitcommit: 0e3c2473a54f915d35687d30d1b4b1ac2bae4068
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/09/2019
ms.locfileid: "68914690"
---
# <a name="arp"></a>arp

>Aplica-se a: Windows Server (canal semestral), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Exibe e modifica entradas no cache do ARP (protocolo de resolução de endereço). O cache ARP contém uma ou mais tabelas que são usadas para armazenar endereços IP e seus endereços físicos de token ou Ethernet resolvidos. Há uma tabela separada para cada adaptador de rede Ethernet ou token ring instalado no computador. Usado sem parâmetros, o **ARP** exibe informações de ajuda.
## <a name="syntax"></a>Sintaxe
```
arp [/a [<Inetaddr>] [/n <ifaceaddr>]] [/g [<Inetaddr>] [-n <ifaceaddr>]] [/d <Inetaddr> [<ifaceaddr>]] [/s <Inetaddr> <Etheraddr> [<ifaceaddr>]]
```
### <a name="parameters"></a>Parâmetros

|                Parâmetro                |                                                                                                                                                                                                                                                               Descrição                                                                                                                                                                                                                                                               |
|-----------------------------------------|-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
|    /a [<Inetaddr>] [/n <ifaceaddr>]     | Exibe as tabelas de cache ARP atuais para todas as interfaces. O parâmetro/n diferencia maiúsculas de minúsculas.<br /><br />Para exibir a entrada de cache ARP para um endereço IP específico, use **ARP/a** com o parâmetro *Inetaddr* , em que *Inetaddr* é um endereço IP. Se *Inetaddr* não for especificado, a primeira interface aplicável será usada.<br /><br />Para exibir a tabela de cache ARP para uma interface específica, use o parâmetro **/n** _ em conjunto com o parâmetro **/a** , em que *ifaceaddr* é o endereço IP atribuído à interface. |
|    /g [<Inetaddr>] [/n <ifaceaddr>]     |                                                                                                                                                                                                                                                          Idêntico a **/a**.                                                                                                                                                                                                                                                           |
|      [/d <Inetaddr> [<ifaceaddr>]       |                                                                                           exclui uma entrada com um endereço IP específico, em que *Inetaddr* é o endereço IP.<br /><br />Para excluir uma entrada em uma tabela para uma interface específica, use o parâmetro *ifaceaddr* , em que *ifaceaddr* é o endereço IP atribuído à interface.<br /><br />Para excluir todas as entradas, use o caractere\*curinga asterisco () no lugar de *Inetaddr*.                                                                                           |
| /s <Inetaddr> [<Etheraddr> ]<ifaceaddr> |                                                                                                                     Adiciona uma entrada estática ao cache ARP que resolve o endereço IP *Inetaddr* para o endereço físico *Etheraddr*.<br /><br />Para adicionar uma entrada de cache ARP estática à tabela para uma interface específica, use o parâmetro *ifaceaddr* , em que *ifaceaddr* é um endereço IP atribuído à interface.                                                                                                                     |
|                   /?                    |                                                                                                                                                                                                                                                  Exibe a ajuda no prompt de comando.                                                                                                                                                                                                                                                   |

## <a name="remarks"></a>Comentários
- Os endereços IP para *Inetaddr* e *ifaceaddr* são expressos em notação decimal pontilhada.
- O endereço físico para *Etheraddr* consiste em seis bytes expressos em notação hexadecimal e separados por hifens (por exemplo, 00-AA-00-4F-2A-9C).
- As entradas adicionadas com o parâmetro **/s** são estáticas e não têm tempo limite do cache ARP. As entradas serão removidas se o protocolo TCP/IP for interrompido e iniciado. Para criar entradas de cache ARP estáticas permanentes, coloque os comandos **ARP** apropriados em um arquivo em lotes e use as tarefas agendadas para executar o arquivo em lotes na inicialização.
  ## <a name="BKMK_Examples"></a>Disso
  Para exibir as tabelas de cache ARP para todas as interfaces, digite:
  ```
  arp /a
  ```
  Para exibir a tabela de cache ARP para a interface que recebe o endereço IP 10.0.0.99, digite:
  ```
  arp /a /n 10.0.0.99
  ```
  Para adicionar uma entrada de cache ARP estática que resolve o endereço IP 10.0.0.80 para o endereço físico 00-AA-00-4F-2A-9C, digite:
  ```
  arp /s 10.0.0.80 00-AA-00-4F-2A-9C 
  ```
  ## <a name="additional-references"></a>referências adicionais
- [Chave da sintaxe de linha de comando](command-line-syntax-key.md)
