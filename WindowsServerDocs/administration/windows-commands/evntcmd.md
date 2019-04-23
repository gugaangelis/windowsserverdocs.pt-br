---
title: Evntcmd
description: 'Tópico de comandos do Windows para * * *- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: c1aabb74-76e7-4304-95a6-50ad87e92fd9
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: a149e78170f2849512dcfc0a0a82f9eed979abe2
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59885977"
---
# <a name="evntcmd"></a>Evntcmd

>Aplica-se a: Windows Server (canal semestral), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Configura a conversão de eventos em desvios, destinos de interceptação ou ambos com base nas informações em um arquivo de configuração.   
## <a name="syntax"></a>Sintaxe  
```  
evntcmd [/s <computerName>] [/v <verbosityLevel>] [/n] <FileName>  
```  
### <a name="parameters"></a>Parâmetros  
|Parâmetro|Descrição|  
|-------|--------|  
|/s <computerName>|Especifica o nome, o computador no qual você deseja configurar a conversão de eventos em desvios, destinos de interceptação ou ambos. Se você não especificar um computador, a configuração ocorre no computador local.|  
|/v <verbosityLevel>|Especifica quais tipos de status de mensagens são exibidos como interceptações e destinos de interceptações são configurados. Esse parâmetro deve ser um inteiro entre 0 e 10. Se você especificar 10, todos os tipos de mensagens são exibidos, incluindo o rastreamento de mensagens e avisos sobre se a configuração de interceptação foi bem-sucedido. Se você especificar 0, nenhuma mensagem será exibida.|  
|/n|Especifica que o serviço SNMP não deve ser reiniciado se este computador receber alterações de configuração de interceptação.|  
|<FileName>|Especifica o nome, o arquivo de configuração que contém informações sobre a conversão de eventos em desvios e destinos de interceptação que você deseja configurar.|  
|/?|Exibe a ajuda no prompt de comando.|  
## <a name="remarks"></a>Comentários  
-   Se você quiser configurar interceptações, mas não os destinos das, você pode criar um arquivo de configuração válido usando o evento para o conversor de desvio, que é um utilitário gráfico. Se você tiver o serviço SNMP instalado, você pode iniciar o conversor de interceptação de evento em digitando **evntwin** em um prompt de comando. Depois de definir os desvios desejados, clique em Exportar para criar um arquivo adequado para uso com **evntcmd**. Você pode usar o evento para o conversor de desvio para criar facilmente um arquivo de configuração e, em seguida, use o arquivo de configuração com **evntcmd** no prompt de comando para configurar rapidamente os desvios em vários computadores.  
-   A sintaxe para a configuração de uma interceptação é da seguinte maneira:  
    **#pragma add***<EventLogFile> <EventSource> <EventID> [<Count> [<Period>]]*  
    -   O texto **#pragma** deve aparecer no início de cada entrada no arquivo.  
    -   O parâmetro **adicionar** Especifica que você deseja adicionar um evento para configuração de desvio.  
    -   Os parâmetros *Arquivo_de_log_de_eventos*, *EventSource*, e *EventID* são necessários. O parâmetro *Arquivo_de_log_de_eventos* Especifica o arquivo no qual o evento é registrado. O parâmetro *EventSource* Especifica o aplicativo que gera o evento. O *EventID* parâmetro especifica o número exclusivo que identifica cada evento. Para saber quais valores correspondem a determinados eventos, iniciar eventos para o conversor de desvio, digitando **evntwin** em um prompt de comando. Clique em **personalizado**e, em seguida, clique em **editar**. Sob **origens do evento**, procure nas pastas até localizar o evento que você deseja configurar, clique nele e, em seguida, clique em **adicionar**. Informações sobre a origem do evento, o arquivo de log de eventos e a ID do evento aparecem sob **de origem, faça logon**, e **interceptar a ID específica**, respectivamente.  
    -   O *contagem* parâmetro é opcional e especifica quantas vezes o evento deve ocorrer antes do envio uma mensagem de interceptação. Se você não usar o *contagem* parâmetro, a mensagem de interceptação será enviada depois que o evento ocorre uma vez.  
    -   O *período* parâmetro é opcional, mas requer que você usar o *contagem* parâmetro. O *período* parâmetro especifica um período de tempo (em segundos) durante o qual o evento deve ocorrer o número de vezes especificado com o parâmetro de contagem antes de uma mensagem de interceptação é enviada. Se você não usar o *período* parâmetro, uma mensagem de interceptação é enviada depois que o evento ocorre o número de vezes especificado com o *contagem* parâmetro, não importa quanto tempo decorrido entre ocorrências.  
-   A sintaxe para remover uma interceptação é da seguinte maneira:  
    **#pragma delete * * *<EventLogFile> <EventSource> <EventID>*  
    -   O texto **#pragma** deve aparecer no início de cada entrada no arquivo.  
    -   O parâmetro *excluir* Especifica que você deseja remover um evento para configuração de desvio.  
    -   Os parâmetros *Arquivo_de_log_de_eventos*, *EventSource*, e *EventID* são necessários. O parâmetro *Arquivo_de_log_de_eventos* Especifica o log no qual o evento é registrado. O parâmetro *EventSource* Especifica o aplicativo que gera o evento. O *EventID* parâmetro especifica o número exclusivo que identifica cada evento.  
-   A sintaxe para configurar um destino de interceptação é da seguinte maneira:  
    **#pragma add_TRAP_DEST***<CommunityName> <HostID>*  
    -   O texto **#pragma** deve aparecer no início de cada entrada no arquivo.  
    -   O parâmetro **add_TRAP_DEST** Especifica que você deseja que as mensagens de interceptação a serem enviados para um host específico em uma comunidade.  
    -   O parâmetro *Nome_da_comunidade* Especifica o nome, a comunidade no qual interceptação as mensagens são enviadas.  
    -   O parâmetro *HostID* Especifica, por nome ou endereço IP, o host ao qual você deseja que as mensagens de interceptação a serem enviados.  
-   A sintaxe para remover um destino de interceptação é da seguinte maneira:  
    **#pragma delete_TRAP_DEST***<CommunityName> <HostID>*  
    -   O texto **#pragma** deve aparecer no início de cada entrada no arquivo.  
    -   O parâmetro *delete_TRAP_DEST* Especifica que você não deseja que as mensagens de interceptação a serem enviados para um host específico em uma comunidade.  
    -   O parâmetro *Nome_da_comunidade* Especifica o nome, a comunidade no qual interceptação as mensagens são enviadas.  
    -   O parâmetro *HostID* Especifica, por nome ou endereço IP, o host ao qual você deseja não mensagens de interceptação a serem enviados.  
## <a name="BKMK_Examples"></a>Exemplos  
Os exemplos a seguir ilustram as entradas no arquivo de configuração para o **evntcmd** comando. Eles não são projetados para ser digitados no prompt de comando.  
Para enviar uma mensagem de desvio se o serviço de Log de eventos for reiniciado, digite:  
```  
#pragma add System "Eventlog" 2147489653  
```  
Para enviar uma mensagem de desvio se o serviço de Log de eventos seja reiniciado duas vezes em três minutos, digite:  
```  
#pragma add System "Eventlog" 2147489653 2 180  
```  
Para interromper o envio de uma mensagem de interceptação sempre que o serviço de Log de eventos for reiniciado, digite:  
```  
#pragma delete System "Eventlog" 2147489653  
```  
Para enviar mensagens de desvio dentro da comunidade chamada Public para o host com o endereço IP 192.168.100.100, digite:  
```  
#pragma add_TRAP_DEST public 192.168.100.100  
```  
Para enviar mensagens de desvio dentro da comunidade chamada Private para o host chamado Host1, digite:  
```  
#pragma add_TRAP_DEST private Host1  
```  
Para interromper o envio de mensagens de desvio dentro da comunidade chamada Private no mesmo computador em que você está configurando destinos de interceptação, digite:  
```  
#pragma delete_TRAP_DEST private localhost  
```  
## <a name="additional-references"></a>Referências adicionais  
-   [Chave de sintaxe de linha de comando](command-line-syntax-key.md)  
