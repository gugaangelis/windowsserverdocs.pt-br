---
title: O que há de novo no DHCP
description: Este tópico fornece uma visão geral dos novos recursos para DHCP Dynamic Host Configuration Protocol () no Windows Server 2016.
manager: brianlic
ms.prod: windows-server-threshold
ms.technology: networking-dhcp
ms.topic: get-started-article
ms.assetid: c6f36998-5b64-45d1-b1f0-0f0d6604dbe3
ms.author: pashort
author: shortpatti
ms.openlocfilehash: f67e5dfd8aefcf408a41f6fc919665419f0ff1c9
ms.sourcegitcommit: 19d9da87d87c9eefbca7a3443d2b1df486b0b010
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/28/2018
---
# <a name="whats-new-in-dhcp"></a>O que há de novo no DHCP

>Aplica-se a: Windows Server (anual por canal), Windows Server 2016

Este tópico descreve a funcionalidade de Dynamic Host Configuration Protocol (DHCP) que é novo ou alterado no Windows Server 2016.
  
DHCP é um padrão de Internet Engineering Task Force (IETF) que foi projetado para reduzir a sobrecarga administrativa e a complexidade de configurar hosts em uma rede baseada em TCP/IP\, como uma intranet privada. Usando o serviço de servidor DHCP, o processo de configuração TCP/IP nos clientes DHCP é automático.

As seções a seguir fornecem informações sobre novos recursos e alterações na funcionalidade de DHCP.

## <a name="dhcp-subnet-selection-options"></a>Opções de seleção de sub-rede DHCP

DHCP agora dá suporte a opções 118 e 82 \(sub-option 5\). Você pode usar essas opções para permitir que os clientes de proxy DHCP e agentes de retransmissão solicitar um endereço IP para uma sub-rede específica e de um intervalo de endereços IP específico e escopo.

Se você estiver usando um cliente de proxy DHCP está configurado com a opção de DHCP 118, como um servidor \(VPN\) de rede virtual privada que está executando o Windows Server 2016 e a função de servidor de acesso remoto, o servidor VPN pode solicitar uma concessão de endereço IP para clientes VPN de um intervalo de endereços IP específico.

Se você estiver usando um agente de retransmissão DHCP está configurado com a opção de DHCP 82, opção de sub\ 5, o agente de retransmissão pode solicitar uma concessão de endereço IP para clientes DHCP de um intervalo de endereços IP específico.

Para obter mais informações, consulte [opções de seleção de sub-rede DHCP](dhcp-subnet-options.md).

## <a name="new-logging-events-for-dns-registration-failures-by-the-dhcp-server"></a>Novos eventos de log de falhas de registro DNS pelo servidor DHCP

DHCP agora inclui a registrar eventos para circunstâncias no qual DHCP registros do servidor DNS falharem no servidor DNS.

Para obter mais informações, consulte [DHCP registro em log eventos registros de registro DNS](dhcp-dns-events.md).

## <a name="dhcp-nap-is-not-supported-in-windows-server-2016"></a>NAP DHCP não tem suporte no Windows Server 2016

Rede proteção de acesso \(NAP\) foi preterida no Windows Server 2012 R2 e no Windows Server 2016 a função de servidor DHCP não dá mais suporte NAP. Para obter mais informações, consulte [recursos removido ou preteridas no Windows Server 2012 R2](https://technet.microsoft.com/library/dn303411.aspx).  
  
Suporte NAP foi introduzido para a função de servidor DHCP no Windows Server 2008 e é compatível com o Windows sistemas operacionais cliente e servidor antes do Windows 10 e Windows Server 2016. A tabela a seguir resume suporte NAP no Windows Server.  
  
|Sistema Operacional|Suporte NAP|  
|--------------------|---------------|  
| Windows Server 2008 |Com suporte|  
| Windows Server 2008 R2 |Com suporte|  
| Windows Server 2012 |Com suporte|  
| Windows Server 2012 R2 |Com suporte|  
| Windows Server 2016|Sem suporte|  
  
Em uma implantação NAP, um servidor DHCP com um sistema operacional que oferece suporte NAP pode funcionar como um ponto de imposição NAP para o método de imposição NAP DHCP. Para obter mais informações sobre o DHCP no NAP, consulte [lista de verificação: Implementando um Design de imposição DHCP](https://technet.microsoft.com/library/dd314186.aspx).  
  
No Windows Server 2016, servidores DHCP não impor políticas NAP e escopos DHCP não podem ser habilitado para NAP\. Computadores cliente DHCP que também são clientes NAP enviem uma declaração de integridade \(SoH\) com a solicitação DHCP. Se o servidor DHCP estiver executando o Windows Server 2016, essas solicitações são processadas como se nenhuma SoH está presente. O servidor DHCP concede uma concessão DHCP normal para o cliente. 

Se os servidores que executam o Windows Server 2016 proxies RADIUS que encaminhe solicitações de autenticação para um servidor de política de rede \(NPS\) que dá suporte NAP, esses clientes NAP são avaliados pelo NPS como não oferecem suporte NAP\ e NAP houver falha no processamento.
  
## <a name="see-also"></a>Consulte também  
  
-   [Protocolo DHCP (protocolo DHCP)](Dynamic-Host-Configuration-Protocol--DHCP-.md)  
  

