---
title: Sistemas de operacionais de convidados Windows com suporte para Hyper-V no Windows Server
description: Lista os sistemas operacionais do Windows com suporte para uso como um convidado em uma máquina virtual. Fornece também links para artigos semelhantes para versões anteriores do Hyper-V.
ms.prod: windows-server-threshold
ms.service: na
manager: dongill
ms.technology: compute-hyper-v
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 06b35897-2192-48b7-8c2d-125c520b0786
author: lizap
ms.author: elizapo
ms.date: 01/08/2019
ms.openlocfilehash: 5f0e91f3202f09d340154b49408c56752a9de577
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59874207"
---
# <a name="supported-windows-guest-operating-systems-for-hyper-v-on-windows-server"></a>Sistemas de operacionais de convidados Windows com suporte para Hyper-V no Windows Server

>Aplica-se a: Windows Server 2016, Windows Server 2019

Hyper-V dá suporte a várias versões do Windows Server, Windows e Linux distribuições para execução em máquinas virtuais, como sistemas operacionais convidados. Este artigo aborda o Windows Server com suporte e sistemas operacionais convidados do Windows. Para distribuições do Linux e FreeBSD, consulte [máquinas virtuais de suporte para Linux e FreeBSD para Hyper-V no Windows](Supported-Linux-and-FreeBSD-virtual-machines-for-Hyper-V-on-Windows.md).  
    
Alguns sistemas operacionais têm os serviços de integração internos. Outros requerem que você instala ou atualizar o integration services como uma etapa separada depois de configurar o sistema operacional na máquina virtual. Para obter mais informações, consulte as seções a seguir e [Integration Services](https://docs.microsoft.com/virtualization/hyper-v-on-windows/reference/integration-services).  
  
## <a name="supported-windows-server-guest-operating-systems"></a>Sistemas de operacionais de convidados do Windows Server com suporte  

A seguir estão as versões do Windows Server que têm suporte como sistemas operacionais convidados para Hyper-V no Windows Server 2016 e Windows Server 2019. 
  
|Sistema operacional convidado (servidor)|Número máximo de processadores virtuais|Serviços de Integração|Observações|  
|-------------------------------------|----------------------------------------|------------------------|---------|  
|Windows Server 2019 |240 para geração 2.<br>64 para a geração 1|Internos|| 
|Windows Server 2016 |240 para geração 2.<br>64 para a geração 1|Internos|| 
|Windows Server 2012 R2 |64|Internos||  
|Windows Server 2012 |64|Internos||  
|Windows Server 2008 R2 com Service Pack 1 (SP1)|64|Instale todas as atualizações críticas do Windows depois de configurar o sistema operacional convidado.|Edições Standard, Enterprise, Datacenter e Web.|
|Windows Server 2008 com Service Pack 2 (SP2)|8|Instale todas as atualizações críticas do Windows depois de configurar o sistema operacional convidado.|Edições Standard, Enterprise, Datacenter e Web (32 bits e 64 bits).|  
  
## <a name="supported-windows-client-guest-operating-systems"></a>Sistemas de operacionais de convidados Windows client  

A seguir estão as versões do cliente do Windows que têm suporte como sistemas operacionais convidados para Hyper-V no Windows Server 2016 e Windows Server 2019.
  
|Sistema operacional convidado (cliente)|Número máximo de processadores virtuais|Serviços de Integração|Observações|  
|-------------------------------------|----------------------------------------|------------------------|---------|  
|Windows 10|32|Internos||  
|Windows 8.1|32|Internos||  
|Windows 7 com Service Pack 1 (SP1)|4|Atualize os serviços de integração depois de configurar o sistema operacional convidado.|Edições Ultimate, Enterprise e Professional (32 bits e 64 bits).|  
  
## <a name="guest-operating-system-support-on-other-versions-of-windows"></a>Suporte do sistema operacional convidado em outras versões do Windows  

A tabela a seguir fornece links para informações sobre sistemas operacionais de convidados com suporte para Hyper-V em outras versões do Windows.  
  
|Sistema operacional do host|Tópico|  
|-------------------------|---------|  
|Windows 10|[Sistemas operacionais convidados com suporte para Hyper-V cliente no Windows 10](https://docs.microsoft.com/virtualization/hyper-v-on-windows/about/supported-guest-os)|  
|Windows Server 2012 R2 e Windows 8.1|-   [Sistemas operacionais convidados do Windows com suporte para Hyper-V no Windows Server 2012 R2 e Windows 8.1](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2012-R2-and-2012/dn792027(v=ws.11))<br />-   [Linux e máquinas de virtuais FreeBSD no Hyper-V](Supported-Linux-and-FreeBSD-virtual-machines-for-Hyper-V-on-Windows.md)|  
|Windows Server 2012 e Windows 8|[Sistemas operacionais convidados do Windows com suporte para Hyper-V no Windows Server 2012 e Windows 8](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2012-R2-and-2012/dn792028(v=ws.11))|  
|Windows Server 2008 e Windows Server 2008 R2|[Sobre máquinas virtuais e sistemas operacionais convidados](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/cc794868(v=ws.10))|  
  
## <a name="how-microsoft-provides-support-for-guest-operating-systems"></a>Como a Microsoft fornece suporte para sistemas operacionais convidados  

A Microsoft dá suporte aos sistemas operacionais convidados da seguinte forma:  
  
-   Os problemas encontrados em sistemas operacionais da Microsoft e em serviços de integração são tratados pelo suporte da Microsoft.  
  
-   Para problemas encontrados em outros sistemas operacionais que tenham sido certificados pelo fornecedor do sistema operacional para execução em Hyper-V, o suporte é prestado pelo fornecedor.  
  
-   Para problemas encontrados em outros sistemas operacionais, a Microsoft envia o problema à comunidade de suporte de vários fornecedores, a [TSANet](https://www.tsanet.org/).  
  
## <a name="see-also"></a>Consulte também  
  
-   [Linux e máquinas de virtuais FreeBSD no Hyper-V](Supported-Linux-and-FreeBSD-virtual-machines-for-Hyper-V-on-Windows.md)  
  
-   [Sistemas operacionais convidados com suporte para Hyper-V cliente no Windows 10](https://docs.microsoft.com/virtualization/hyper-v-on-windows/about/supported-guest-os)  
  



