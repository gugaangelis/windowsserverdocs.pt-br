---
title: Eventos de log de DHCP para registros de registro DNS
description: Este tópico fornece informações sobre eventos de log de servidor DHCP no Windows Server 2016.
manager: brianlic
ms.prod: windows-server
ms.technology: networking-dhcp
ms.topic: get-started-article
ms.assetid: beb8c188-6fcf-4520-8825-d17f8ee9fb04
ms.author: lizross
author: eross-msft
ms.openlocfilehash: 5d167666e632aa1a8d92de71feafc9014b66e7ce
ms.sourcegitcommit: da7b9bce1eba369bcd156639276f6899714e279f
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/26/2020
ms.locfileid: "80312604"
---
# <a name="dhcp-logging-events-for-dns-registrations"></a>Eventos de log de DHCP para registros de DNS

>Aplicável a: Windows Server (canal semestral), Windows Server 2016

Os logs de eventos do servidor DHCP agora fornecem informações detalhadas sobre falhas de registro de DNS.

>[!NOTE]
>Em muitos casos, o motivo para falhas de registro de registro de DNS por servidores DHCP é que uma zona de pesquisa de\-inversa de DNS está configurada incorretamente ou não está configurada.

Os novos eventos de DHCP a seguir ajudam você a identificar facilmente quando os registros de DNS estão falhando devido a uma zona de pesquisa in\-vertida de DNS incorretamente configurada ou ausente.

|ID|{1&gt;Evento&lt;1}|{1&gt;Valor&lt;1}|
|-----|--------------------|--------------------------------------------------------|
|20317|DHCPv4. ForwardRecordDNSFailure|O registro de registro de encaminhamento para o endereço IPv4% 1 e FQDN %2 falhou com o erro %3. Isso provavelmente ocorre porque a zona de pesquisa direta deste registro não existe no servidor DNS.|
|20318|DHCPv4. ForwardRecordDNSTimeout|O registro de registro de encaminhamento para o endereço IPv4% 1 e FQDN %2 falhou com o erro %3.|
|20319|DHCPv4. PTRRecordDNSFailure|Falha no registro de registro PTR para o endereço IPv4% 1 e FQDN %2 com o erro %3. Isso provavelmente ocorre porque a zona de pesquisa inversa para esse registro não existe no servidor DNS.|
|20320|DHCPv4. PTRRecordDNSTimeout|Falha no registro de registro PTR para o endereço IPv4% 1 e FQDN %2 com o erro %3.|
|20321|DHCPv6. ForwardRecordDNSFailure|O registro de registro de encaminhamento para o endereço IPv6% 1 e FQDN %2 falhou com o erro %3. Isso provavelmente ocorre porque a zona de pesquisa direta deste registro não existe no servidor DNS.|
|20322|DHCPv6. ForwardRecordDNSTimeout|O registro de registro de encaminhamento para o endereço IPv6% 1 e FQDN %2 falhou com o erro %3.|
|20323|DHCPv6. PTRRecordDNSFailure|Falha no registro de registro PTR para o endereço IPv6% 1 e FQDN %2 com o erro %3. Isso provavelmente ocorre porque a zona de pesquisa inversa para esse registro não existe no servidor DNS.|
|20324|DHCPv6. PTRRecordDNSTimeout|Falha no registro de registro PTR para o endereço IPv6% 1 e FQDN %2 com o erro %3.|
|20325|DHCPv4. ForwardRecordDNSError|Falha no registro de registro PTR para o endereço IPv4% 1 e FQDN %2 com o erro %3 \(%4\).|
|20326|DHCPv6. ForwardRecordDNSError|O registro de registro de encaminhamento para o endereço IPv6% 1 e FQDN %2 falhou com o erro %3 \(%4\)|
|20327|DHCPv6. PTRRecordDNSError|Falha no registro de registro PTR para o endereço IPv6% 1 e FQDN %2 com o erro %3 \(%4\).|

