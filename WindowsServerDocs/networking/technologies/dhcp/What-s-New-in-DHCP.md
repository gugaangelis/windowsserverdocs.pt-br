---
title: Novidades no DHCP
description: Este tópico fornece uma visão geral dos novos recursos do protocolo DHCP no Windows Server 2016.
manager: brianlic
ms.prod: windows-server
ms.technology: networking-dhcp
ms.topic: get-started-article
ms.assetid: c6f36998-5b64-45d1-b1f0-0f0d6604dbe3
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 8032b7c8e78170d57b0367775672577d9fd900e2
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71355444"
---
# <a name="whats-new-in-dhcp"></a>Novidades no DHCP

>Aplicável a: Windows Server (canal semestral), Windows Server 2016

Este tópico descreve a funcionalidade DHCP (protocolo de configuração dinâmica de hosts) que é nova ou alterada no Windows Server 2016.
  
O DHCP é um padrão IETF (Internet Engineering Task Force) que foi projetado para reduzir a carga administrativa e a complexidade da configuração de hosts em uma rede baseada em\-TCP/IP, como uma intranet privada. Usando o serviço de servidor DHCP, o processo de configuração de TCP/IP em clientes DHCP é automático.

As seções a seguir fornecem informações sobre novos recursos e alterações na funcionalidade do DHCP.

## <a name="dhcp-subnet-selection-options"></a>Opções de seleção de sub-rede DHCP

O DHCP agora dá suporte às opções 118 e 82 \(a subopção 5\). Você pode usar essas opções para permitir que clientes proxy DHCP e agentes de retransmissão solicitem um endereço IP para uma sub-rede específica e de um intervalo de endereços IP e escopo específicos.


Se você estiver usando um agente de retransmissão DHCP configurado com a opção de DHCP 82, sub\-opção 5, o agente de retransmissão poderá solicitar uma concessão de endereço IP para clientes DHCP de um intervalo de endereços IP específico.

Para obter mais informações, consulte [Opções de seleção de sub-rede do DHCP](dhcp-subnet-options.md).

## <a name="new-logging-events-for-dns-registration-failures-by-the-dhcp-server"></a>Novos eventos de log para falhas de registro de DNS pelo servidor DHCP

O DHCP agora inclui eventos de registro em log para circunstâncias em que os registros de registro DNS do servidor DHCP falham no servidor DNS.

Para obter mais informações, consulte [eventos de log do DHCP para registros de registro DNS](dhcp-dns-events.md).

## <a name="dhcp-nap-is-not-supported-in-windows-server-2016"></a>Não há suporte para NAP do DHCP no Windows Server 2016

A proteção de acesso à rede \(NAP\) foi preterida no Windows Server 2012 R2 e no Windows Server 2016 a função de servidor DHCP não oferece mais suporte a NAP. Para obter mais informações, consulte [recursos removidos ou preteridos no Windows Server 2012 R2](https://technet.microsoft.com/library/dn303411.aspx).  
  
O suporte a NAP foi introduzido na função de servidor DHCP com o Windows Server 2008 e tem suporte em sistemas operacionais de cliente e servidor do Windows antes do Windows 10 e do Windows Server 2016. A tabela a seguir resume o suporte para NAP no Windows Server.  
  
|Sistema operacional|Suporte a NAP|  
|--------------------|---------------|  
| Windows Server 2008 |Com suporte|  
| Windows Server 2008 R2 |Com suporte|  
| Windows Server 2012 |Com suporte|  
| Windows Server 2012 R2 |Com suporte|  
| Windows Server 2016|Sem suporte|  
  
Em uma implantação de NAP, um servidor DHCP que executa um sistema operacional que dá suporte a NAP pode funcionar como um ponto de imposição de NAP para o método de imposição de DHCP NAP. Para obter mais informações sobre o DHCP em NAP, consulte [lista de verificação: Implementando um design de imposição de DHCP](https://technet.microsoft.com/library/dd314186.aspx).  
  
No Windows Server 2016, os servidores DHCP não impõem políticas NAP e os escopos DHCP não podem ser NAP\-habilitados. Os computadores cliente DHCP que também são clientes NAP enviam uma declaração de integridade \(SoH\) com a solicitação DHCP. Se o servidor DHCP estiver executando o Windows Server 2016, essas solicitações serão processadas como se nenhuma SoH estiver presente. O servidor DHCP concede uma concessão DHCP normal ao cliente. 

Se servidores que executam o Windows Server 2016 são proxies RADIUS que encaminham solicitações de autenticação para um servidor de diretivas de rede \(\) de NPS com suporte a NAP, esses clientes NAP são avaliados pelo NPS como não NAP\-compatíveis e o processamento de NAP falha.
  
## <a name="see-also"></a>Consulte também  
  
-   [Protocolo DHCP](Dynamic-Host-Configuration-Protocol--DHCP-.md)  
  

