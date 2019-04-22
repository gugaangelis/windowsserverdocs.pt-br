---
title: arp
description: Tópico de comandos do Windows para **arp** -exibe e modifica as entradas no cache de Resolution Protocol (arp) de endereço usado para armazenar endereços IP e seus endereços físicos resolvidos.
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
ms.openlocfilehash: 5cd84269a5ac1a85d4b6cf359cc97f478a500c4f
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59825977"
---
# <a name="arp"></a>arp

>Aplica-se a: Windows Server (canal semestral), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Exibe e modifica as entradas no cache de protocolo de resolução de endereço (ARP). O cache ARP contém uma ou mais tabelas que são usadas para armazenar os endereços IP e seus endereços físicos Ethernet ou Token Ring resolvidos. Há uma tabela separada para cada adaptador de rede Ethernet ou Token Ring instalado em seu computador. Usado sem parâmetros, **arp** exibe informações de Ajuda.
## <a name="syntax"></a>Sintaxe
```
arp [/a [<Inetaddr>] [/n <ifaceaddr>]] [/g [<Inetaddr>] [-n <ifaceaddr>]] [/d <Inetaddr> [<ifaceaddr>]] [/s <Inetaddr> <Etheraddr> [<ifaceaddr>]]
```
### <a name="parameters"></a>Parâmetros
|Parâmetro|Descrição|
|-------|--------|
|/a [<Inetaddr>] [/n <ifaceaddr>]|Exibe as tabelas do cache arp atual para todas as interfaces. O parâmetro /n diferencia maiusculas de minúsculas.<br /><br />Para exibir a entrada de cache arp para um endereço IP específico, use **/a arp** com o *End_ip_da_rede* parâmetro, onde *End_ip_da_rede* é um endereço IP. Se *End_ip_da_rede* não for especificado, a primeira interface aplicável é usada.<br /><br />Para exibir a tabela de cache arp para uma interface específica, use o **/n * * * End_da_Interf* parâmetro junto com o **/a** parâmetro em que *End_da_Interf* é o endereço IP atribuído à interface.|
|/g [<Inetaddr>] [/n <ifaceaddr>]|Idêntico ao **/a**.|
|[/d <Inetaddr> [<ifaceaddr>]|Exclui uma entrada com um endereço IP específico, onde *End_ip_da_rede* é o endereço IP.<br /><br />Para excluir uma entrada em uma tabela para uma interface específica, use o *End_da_Interf* parâmetro onde *End_da_Interf* é o endereço IP atribuído à interface.<br /><br />Para excluir todas as entradas, use o asterisco (\*) o caractere curinga no lugar de *End_ip_da_rede*.|
|/s <Inetaddr> <Etheraddr> [<ifaceaddr>]|Adiciona uma entrada estática ao cache arp que resolve o endereço IP *End_ip_da_rede* para o endereço físico *End_ether*.<br /><br />Para adicionar uma entrada de cache arp estáticas para a tabela para uma interface específica, use o *End_da_Interf* parâmetro onde *End_da_Interf* é um endereço IP atribuído à interface.|
|/?|Exibe a ajuda no prompt de comando.|
## <a name="remarks"></a>Comentários
-   Os endereços IP para *End_ip_da_rede* e *End_da_Interf* são expressos em notação de ponto decimal.
-   O endereço físico *End_ether* consiste em seis bytes expressos em notação hexadecimal e separados por hifens (por exemplo, 00-AA-00-4F-2A-9C).
-   As entradas adicionadas com o **/s** parâmetro são estático e não especificar tempo fora do cache arp. As entradas serão removidas se o protocolo TCP/IP for interrompido e iniciado. Para criar entradas de cache de arp estáticas permanentes, coloque apropriado **arp** comandos em um lote de arquivos e usar tarefas agendadas para executar o arquivo em lotes na inicialização.
## <a name="BKMK_Examples"></a>Exemplos
Para exibir as tabelas de cache arp para todas as interfaces, digite:
```
arp /a
```
Para exibir a tabela de cache arp para a interface que é atribuída o endereço IP 10.0.0.99, digite:
```
arp /a /n 10.0.0.99
```
Para adicionar uma entrada de cache arp estáticas que resolve o endereço IP 10.0.0.80 para o endereço físico 00-AA-00-4F-2A-9C, digite:
```
arp /s 10.0.0.80 00-AA-00-4F-2A-9C 
```
## <a name="additional-references"></a>Referências adicionais
-   [Chave de sintaxe de linha de comando](command-line-syntax-key.md)
