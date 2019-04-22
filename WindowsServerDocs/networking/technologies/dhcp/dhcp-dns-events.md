---
title: Eventos de registro em log de DHCP para registros de DNS do registros
description: Este tópico fornece informações sobre o servidor DHCP de registro em log eventos no Windows Server 2016.
manager: brianlic
ms.prod: windows-server-threshold
ms.technology: networking-dhcp
ms.topic: get-started-article
ms.assetid: beb8c188-6fcf-4520-8825-d17f8ee9fb04
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 7f4ce217b19cfd8a63bff1ae504362d4fc24fcd0
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59816897"
---
# <a name="dhcp-logging-events-for-dns-registrations"></a>Eventos de log de DHCP para registros de DNS

>Aplica-se a: Windows Server (canal semestral), Windows Server 2016

Logs de eventos do servidor DHCP agora fornecem informações detalhadas sobre falhas de registro DNS.

>[!NOTE]
>Em muitos casos, o motivo para falhas de registro DNS por servidores DHCP é que um DNS reverso\-zona de pesquisa é configurada incorretamente ou não configurada em todos os.

Os seguintes novos eventos DHCP ajudar a identificar facilmente quando registros de DNS estão falhando por causa de um configurado incorretamente ou ausentes DNS inversa\-zona de pesquisa.

|ID|Evento|Valor|
|-----|--------------------|--------------------------------------------------------|
|20317|DHCPv4.ForwardRecordDNSFailure|Processo de registro de encaminhamento para o endereço IPv4 %1 e FQDN %2 falhou com erro %3. Isso ocorre provavelmente porque a zona de pesquisa direta para este registro não existe no servidor DNS.|
|20318|DHCPv4.ForwardRecordDNSTimeout|Processo de registro de encaminhamento para o endereço IPv4 %1 e FQDN %2 falhou com erro %3.|
|20319|DHCPv4.PTRRecordDNSFailure|O processo de registro PTR para o endereço IPv4 %1 e FQDN %2 falhou com erro %3. Isso ocorre provavelmente porque a zona de pesquisa inversa para esse registro não existe no servidor DNS.|
|20320|DHCPv4.PTRRecordDNSTimeout|O processo de registro PTR para o endereço IPv4 %1 e FQDN %2 falhou com erro %3.|
|20321|DHCPv6.ForwardRecordDNSFailure|Processo de registro de encaminhamento para o endereço IPv6 %1 e FQDN %2 falhou com erro %3. Isso ocorre provavelmente porque a zona de pesquisa direta para este registro não existe no servidor DNS.|
|20322|DHCPv6.ForwardRecordDNSTimeout|Processo de registro de encaminhamento para o endereço IPv6 %1 e FQDN %2 falhou com erro %3.|
|20323|DHCPv6.PTRRecordDNSFailure|O processo de registro PTR para o endereço IPv6 %1 e FQDN %2 falhou com erro %3. Isso ocorre provavelmente porque a zona de pesquisa inversa para esse registro não existe no servidor DNS.|
|20324|DHCPv6.PTRRecordDNSTimeout|O processo de registro PTR para o endereço IPv6 %1 e FQDN %2 falhou com erro %3.|
|20325|DHCPv4.ForwardRecordDNSError|O processo de registro PTR para o endereço IPv4 %1 e FQDN %2 falhou com erro %3 \(%4\).|
|20326|DHCPv6.ForwardRecordDNSError|Processo de registro de encaminhamento para o endereço IPv6 %1 e FQDN %2 falhou com erro %3 \(%4\)|
|20327|DHCPv6.PTRRecordDNSError|O processo de registro PTR para o endereço IPv6 %1 e FQDN %2 falhou com erro %3 \(%4\).|

