---
title: Novidades no DHCP
description: Este tópico fornece uma visão geral dos novos recursos do protocolo DHCP no Windows Server 2016.
manager: brianlic
ms.topic: get-started-article
ms.assetid: c6f36998-5b64-45d1-b1f0-0f0d6604dbe3
ms.author: lizross
author: eross-msft
ms.openlocfilehash: 9b0b23b6058655baf599ed66877228b39eb807f9
ms.sourcegitcommit: 68444968565667f86ee0586ed4c43da4ab24aaed
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/07/2020
ms.locfileid: "87993928"
---
# <a name="whats-new-in-dhcp"></a>Novidades no DHCP

>Aplica-se a: Windows Server (Canal Semestral), Windows Server 2016

Este tópico descreve a funcionalidade DHCP (protocolo de configuração dinâmica de hosts) que é nova ou alterada no Windows Server 2016.

O DHCP é um padrão IETF (Internet Engineering Task Force), projetado para reduzir a carga administrativa e a complexidade da configuração de hosts em uma \- rede baseada em TCP/IP, como uma intranet privada. Usando o serviço de servidor DHCP, o processo de configuração de TCP/IP em clientes DHCP é automático.

As seções a seguir fornecem informações sobre novos recursos e alterações na funcionalidade do DHCP.

## <a name="new-dhcp-client-side-features-in-the-windows-10-may-2020-update"></a>Novos recursos do lado do cliente DHCP no Windows 10 podem ser atualizados 2020

O cliente DHCP no Windows 10 foi atualizado na atualização de 10 de maio de 2020 (também conhecida como Windows 10, versão 2004). Quando você estiver executando um cliente Windows e se conectar à Internet por meio de um telefone Android conectado, a conexão deverá ser marcada como "limitada". Anteriormente, as conexões foram marcadas como ilimitadas. Observe que nem todos os telefones com compartilhamento de Internet do Android serão detectados como limitados, e algumas outras redes também poderão aparecer como limitadas.

Além disso, o nome tradicional do fornecedor do cliente foi atualizado para alguns dispositivos baseados no Windows. Esse valor costumava ser simplesmente MSFT 5,0. Alguns dispositivos agora aparecerão como MSFT 5,0 XBOX.

## <a name="dhcp-subnet-selection-options"></a>Opções de seleção de sub-rede DHCP

O DHCP agora dá suporte à opção 82 \( sub-opção 5 \) . Você pode usar essa opção para permitir que clientes proxy DHCP e agentes de retransmissão solicitem um endereço IP para uma sub-rede específica.


Se você estiver usando um agente de retransmissão DHCP configurado com a opção de DHCP 82, \- a subopção 5, o agente de retransmissão poderá solicitar uma concessão de endereço IP para clientes DHCP de um intervalo de endereços IP específico.

Para obter mais informações, consulte [Opções de seleção de sub-rede do DHCP](dhcp-subnet-options.md).

## <a name="new-logging-events-for-dns-registration-failures-by-the-dhcp-server"></a>Novos eventos de log para falhas de registro de DNS pelo servidor DHCP

O DHCP agora inclui eventos de registro em log para circunstâncias em que os registros de registro DNS do servidor DHCP falham no servidor DNS.

Para obter mais informações, consulte [eventos de log do DHCP para registros de registro DNS](dhcp-dns-events.md).

## <a name="dhcp-nap-is-not-supported-in-windows-server-2016"></a>Não há suporte para NAP do DHCP no Windows Server 2016

A NAP de proteção de acesso à rede \( \) foi preterida no windows Server 2012 R2 e no windows Server 2016 a função de servidor DHCP não oferece mais suporte a NAP. Para obter mais informações, consulte [recursos removidos ou preteridos no Windows Server 2012 R2](/previous-versions/windows/it-pro/windows-server-2012-R2-and-2012/dn303411(v=ws.11)).

O suporte a NAP foi introduzido na função de servidor DHCP com o Windows Server 2008 e tem suporte em sistemas operacionais de cliente e servidor do Windows antes do Windows 10 e do Windows Server 2016. A tabela a seguir resume o suporte para NAP no Windows Server.

|Sistema operacional|Suporte a NAP|
|--------------------|---------------|
| Windows Server 2008 |Com suporte|
| Windows Server 2008 R2 |Com suporte|
| Windows Server 2012 |Com suporte|
| Windows Server 2012 R2 |Com suporte|
| Windows Server 2016|Sem suporte|

Em uma implantação de NAP, um servidor DHCP que executa um sistema operacional que dá suporte a NAP pode funcionar como um ponto de imposição de NAP para o método de imposição de DHCP NAP. Para obter mais informações sobre o DHCP em NAP, consulte [lista de verificação: Implementando um design de imposição de DHCP](/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/dd314186(v=ws.10)).

No Windows Server 2016, os servidores DHCP não impõem políticas NAP e os escopos DHCP não podem ser \- habilitados para NAP. Os computadores cliente DHCP que também são clientes NAP enviam uma declaração de soh de integridade \( \) com a solicitação DHCP. Se o servidor DHCP estiver executando o Windows Server 2016, essas solicitações serão processadas como se nenhuma SoH estiver presente. O servidor DHCP concede uma concessão DHCP normal ao cliente.

Se servidores que executam o Windows Server 2016 são proxies RADIUS que encaminham solicitações de autenticação para um NPS de servidor de diretivas de rede \( \) que dá suporte a NAP, esses clientes NAP são avaliados pelo NPS como sem \- capacidade de NAP e o processamento de NAP falha.

## <a name="additional-references"></a>Referências adicionais

-   [Protocolo DHCP](./dhcp-top.md)