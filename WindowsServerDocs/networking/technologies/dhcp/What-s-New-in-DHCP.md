---
title: Novidades no DHCP
description: Este tópico fornece uma visão geral dos novos recursos de configuração de protocolo DHCP (Dynamic Host) no Windows Server 2016.
manager: brianlic
ms.prod: windows-server-threshold
ms.technology: networking-dhcp
ms.topic: get-started-article
ms.assetid: c6f36998-5b64-45d1-b1f0-0f0d6604dbe3
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 73cc5134f7af5063c912ad578fa7d660b3194aa1
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59840227"
---
# <a name="whats-new-in-dhcp"></a>Novidades no DHCP

>Aplica-se a: Windows Server (canal semestral), Windows Server 2016

Este tópico descreve a funcionalidade de protocolo de configuração de Host dinâmico (DHCP) que é nova ou alterada no Windows Server 2016.
  
DHCP é um padrão de Internet Engineering Task Force (IETF) foi projetado para reduzir a carga administrativa e a complexidade da configuração de hosts em um TCP/IP\-com base em rede, como uma intranet privada. Usando o serviço de servidor DHCP, o processo de configuração de TCP/IP em clientes DHCP é automático.

As seções a seguir fornecem informações sobre novos recursos e alterações na funcionalidade do DHCP.

## <a name="dhcp-subnet-selection-options"></a>Opções de seleção de sub-rede do DHCP

Agora, o DHCP dá suporte a opções 118 e 82 \(opção inferior a 5\). Você pode usar essas opções para permitir que os clientes de proxy DHCP e agentes de retransmissão solicitar um endereço IP para uma sub-rede específica e de um intervalo de endereços IP específico e um escopo.


Se você estiver usando um agente de retransmissão DHCP que é configurado com a opção DHCP 82, sub\-opção 5, o agente de retransmissão pode solicitar uma concessão de endereço IP para clientes DHCP de um intervalo de endereço IP específico.

Para obter mais informações, consulte [opções de seleção de subrede DHCP](dhcp-subnet-options.md).

## <a name="new-logging-events-for-dns-registration-failures-by-the-dhcp-server"></a>Novos eventos de registro em log para falhas de registro DNS pelo servidor DHCP

O DHCP agora inclui log de eventos para circunstâncias em que DHCP registros de registro de DNS do servidor falharem no servidor DNS.

Para obter mais informações, consulte [eventos de log de DHCP para registros de DNS do registro](dhcp-dns-events.md).

## <a name="dhcp-nap-is-not-supported-in-windows-server-2016"></a>Não há suporte para NAP de DHCP no Windows Server 2016

Proteção de acesso de rede \(NAP\) foi preterido no Windows Server 2012 R2 e no Windows Server 2016, a função de servidor DHCP não oferece suporte a NAP. Para obter mais informações, consulte [recursos removidos ou preteridos no Windows Server 2012 R2](https://technet.microsoft.com/library/dn303411.aspx).  
  
Suporte a NAP foi introduzido para a função de servidor DHCP com o Windows Server 2008 e é suportado em sistemas de operacionais de cliente e servidor do Windows em antes do Windows 10 e Windows Server 2016. A tabela a seguir resume o suporte para NAP no Windows Server.  
  
|Sistema operacional|Suporte a NAP|  
|--------------------|---------------|  
| Windows Server 2008 |Com suporte|  
| Windows Server 2008 R2 |Com suporte|  
| Windows Server 2012 |Com suporte|  
| Windows Server 2012 R2 |Com suporte|  
| Windows Server 2016|Sem suporte|  
  
Em uma implantação de NAP, um servidor DHCP executando um sistema operacional que dá suporte a NAP pode funcionar como um ponto de imposição de NAP para o método de imposição de NAP para DHCP. Para obter mais informações sobre o DHCP na NAP, consulte [lista de verificação: Implementando um Design de imposição de DHCP](https://technet.microsoft.com/library/dd314186.aspx).  
  
No Windows Server 2016, os servidores DHCP não impõem diretivas de NAP e escopos DHCP não podem ser NAP\-habilitado. Computadores cliente DHCP que também são clientes NAP enviarem uma declaração de integridade \(SoH\) com a solicitação DHCP. Se o servidor DHCP estiver executando o Windows Server 2016, essas solicitações são processadas como se nenhum SoH está presente. O servidor DHCP concede uma concessão de DHCP normal para o cliente. 

Se os servidores que executam o Windows Server 2016 são proxies RADIUS que encaminha as solicitações de autenticação para um servidor de diretivas de rede \(NPS\) que dá suporte a NAP, esses clientes NAP são avaliadas pelo NPS como a NAP não\-capaz, e Houver falha no processamento de NAP.
  
## <a name="see-also"></a>Consulte também  
  
-   [Dynamic Host Configuration Protocol (DHCP)](Dynamic-Host-Configuration-Protocol--DHCP-.md)  
  

