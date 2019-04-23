---
title: tracert
description: 'Tópico de comandos do Windows para * * *- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 9032a032-2e5e-49d4-9e86-f821600e4ba6
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 6a97c7656e646a22892eee5caa13d0163d05293d
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59837177"
---
# <a name="tracert"></a>tracert

>Aplica-se a: Windows Server (canal semestral), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Determina o caminho tomado para um destino, enviando mensagens de solicitação ou ICMPv6 de eco de ICMP Internet Control Message Protocol () para o destino com o aumento do tempo para valores de campo de vida (TTL) incrementalmente. O caminho exibido é a lista de interfaces do roteador near/side dos roteadores no caminho entre um host de origem e um destino. A interface de próximo/lado é a interface do roteador que está mais próximo ao host de envio no caminho. Usado sem parâmetros, tracert exibe a Ajuda.   

## <a name="syntax"></a>Sintaxe  
```  
tracert [/d] [/h <MaximumHops>] [/j <Hostlist>] [/w <timeout>] [/R] [/S <Srcaddr>] [/4][/6] <TargetName>  
```  
### <a name="parameters"></a>Parâmetros  
|Parâmetro|Descrição|  
|-------|--------|  
|/d|Impede **tracert** da tentativa de resolver os endereços IP dos roteadores intermediários para seus nomes. Isso pode acelerar a exibição de **tracert** resultados.|  
|/h \<MaximumHops>|Especifica o número máximo de saltos no caminho de pesquisa para o destino (destino). O padrão é 30 saltos.|  
|/j \<Hostlist>|Especifica que recuperar mensagens de solicitação de usam a opção de rota no cabeçalho IP com o conjunto de destinos intermediários especificado em *Hostlist*. Com o roteamento de origem flexível, os destinos intermediários sucessivos podem ser separados por um ou vários roteadores. O número máximo de endereços ou nomes na lista de hosts é 9. O *Hostlist* é uma série de endereços IP (em notação decimal pontilhada) separada por espaços. Use esse parâmetro somente quando o rastreamento de endereços IPv4.|  
|/w \<timeout>|Especifica a quantidade de tempo em milissegundos para aguardar o tempo ICMP excedido ou correspondente a uma mensagem de solicitação de eco determinado para ser recebida a mensagem de resposta de eco. Se não receber dentro do tempo limite, um asterisco (*) é exibido. O tempo limite padrão é 4000 (4 segundos).|  
|/R|Especifica que o cabeçalho de extensão de roteamento IPv6 ser usado para enviar um mensagem de solicitação de eco ao host local, usando o destino como um destino intermediário e testando a rota inversa.|  
|/S \<Srcaddr>|Especifica o endereço de origem para usar o eco de mensagens de solicitação. Use esse parâmetro somente quando o rastreamento de endereços IPv6.|  
|/4|Especifica que tracert.exe que pode usar somente IPv4 para este rastreamento.|  
|/6|Especifica que tracert.exe que pode usar apenas o IPv6 para este rastreamento.|  
|\<TargetName >|Especifica o destino identificado pelo nome do host ou endereço IP.|  
|/?|Exibe a ajuda no prompt de comando.|  

## <a name="remarks"></a>Comentários  
-   Esta ferramenta de diagnóstico determina o caminho tomado para um destino enviando eco ICMP mensagens de solicitação com diferentes valores de vida (TTL) para o destino. Cada roteador no caminho é necessário para diminuir o TTL em um pacote IP pelo menos 1 antes de encaminhá-lo. Na verdade, o TTL é um contador de máximo de links. Quando a TTL em um pacote chega a 0, o roteador deve retornar uma mensagem de excedeu o tempo ICMP no computador de origem. tracert determina o caminho, enviando o eco de primeira mensagem de solicitação com um TTL de 1 e aumentando a TTL em 1 em cada transmissão subsequente, até que o destino responda ou o número máximo de saltos é atingida. O número máximo de saltos é 30 por padrão e pode ser especificado usando o **/h** parâmetro. O caminho é determinado examinando as mensagens ICMP tempo excedido retornadas por roteadores intermediários e o eco de mensagem de resposta retornada pelo destino. No entanto, alguns roteadores não retornam mensagens de tempo excedido para pacotes com valores de TTL expiradas e são invisile ao comando tracert. Nesse caso, uma linha de asteriscos (*) é exibida para esse salto.  
-   Para rastrear um caminho e fornecer latência de rede e perda de pacotes para cada roteador e link no caminho, use o **pathping** comando.  
-   Esse comando está disponível somente se o protocolo de Internet Protocol (TCP/IP) é instalado como um componente nas propriedades de um adaptador de rede em conexões de rede.  

## <a name="BKMK_Examples"></a>Exemplos  
Para rastrear o caminho para o host chamado corp7. microsoft.com, digite:  
```  
tracert corp7.microsoft.com  
```  
Para rastrear o caminho para o host chamado corp7. microsoft.com e evitar a resolução de cada endereço IP para seu nome, digite:  
```  
tracert /d corp7.microsoft.com  
```  
Para rastrear o caminho para o host chamado corp7. microsoft.com e usar a origem flexível rota 10.12.0.1/10.29.3.1/10.1.44.1, digite:  
```  
tracert /j 10.12.0.1 10.29.3.1 10.1.44.1 corp7.microsoft.com  
```  
## <a name="additional-references"></a>Referências adicionais  
-   [Chave de sintaxe de linha de comando](command-line-syntax-key.md)  
