---
title: Sistemas operacionais convidados do Windows com suporte para o Hyper-V no Windows Server
description: Lista os sistemas operacionais Windows com suporte para uso como um convidado em uma máquina virtual. Também fornece links para artigos semelhantes para versões anteriores do Hyper-V.
manager: dongill
ms.topic: article
ms.assetid: 06b35897-2192-48b7-8c2d-125c520b0786
author: lizap
ms.author: elizapo
ms.date: 01/08/2019
ms.openlocfilehash: a3c1f82d1c1f12c51a36a8285d28ccb9402f2817
ms.sourcegitcommit: dfa48f77b751dbc34409aced628eb2f17c912f08
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/07/2020
ms.locfileid: "87963702"
---
# <a name="supported-windows-guest-operating-systems-for-hyper-v-on-windows-server"></a>Sistemas operacionais convidados do Windows com suporte para o Hyper-V no Windows Server

>Aplica-se a: Windows Server 2016, Windows Server 2019

O Hyper-V dá suporte a várias versões de distribuições do Windows Server, Windows e Linux para serem executadas em máquinas virtuais, como sistemas operacionais convidados. Este artigo aborda os sistemas operacionais Windows Server e Windows Guest com suporte. Para distribuições Linux e FreeBSD, consulte [máquinas virtuais Linux e FreeBSD com suporte para o Hyper-V no Windows](Supported-Linux-and-FreeBSD-virtual-machines-for-Hyper-V-on-Windows.md).

Alguns sistemas operacionais têm o Integration Services integrado. Outros exigem que você instale ou atualize o Integration Services como uma etapa separada depois de configurar o sistema operacional na máquina virtual. Para obter mais informações, consulte as seções abaixo e [Integration Services](https://docs.microsoft.com/virtualization/hyper-v-on-windows/reference/integration-services).

## <a name="supported-windows-server-guest-operating-systems"></a>Sistemas operacionais convidados com suporte do Windows Server

A seguir estão as versões do Windows Server com suporte como sistemas operacionais convidados para o Hyper-V no Windows Server 2016 e no Windows Server 2019.

|Sistema operacional convidado (servidor)|Número máximo de processadores virtuais|Integration Services|Observações|
|-------------------------------------|----------------------------------------|------------------------|---------|
|Windows Server, versão 1909 |240 para geração 2;<br>64 para geração 1|Interno|Maior que 240 o suporte ao processador virtual requer o Windows Server, versão 1903 ou sistemas operacionais convidados posteriores.|
|Windows Server, versão 1903 |240 para geração 2;<br>64 para geração 1|Interno||
|Windows Server, versão 1809 |240 para geração 2;<br>64 para geração 1|Interno||
|Windows Server 2019 |240 para geração 2;<br>64 para geração 1|Interno||
|Windows Server, versão 1803 |240 para geração 2;<br>64 para geração 1|Interno||
|Windows Server 2016 |240 para geração 2;<br>64 para geração 1|Interno||
|Windows Server 2012 R2 |64|Interno||
|Windows Server 2012 |64|Interno||
|Windows Server 2008 R2 com Service Pack 1 (SP1)|64|Instale todas as atualizações críticas do Windows depois de configurar o sistema operacional convidado.|Edições Standard, Enterprise, Datacenter e Web.|
|Windows Server 2008 com Service Pack 2 (SP2)|8|Instale todas as atualizações críticas do Windows depois de configurar o sistema operacional convidado.|Edições Standard, Enterprise, Datacenter e Web (32 bits e 64 bits).|

## <a name="supported-windows-client-guest-operating-systems"></a>Sistemas operacionais convidados com suporte do Windows Client

A seguir estão as versões do cliente Windows com suporte como sistemas operacionais convidados para o Hyper-V no Windows Server 2016 e no Windows Server 2019.

|Sistema operacional convidado (cliente)|Número máximo de processadores virtuais|Integration Services|Observações|
|-------------------------------------|----------------------------------------|------------------------|---------|
|Windows 10|32|Interno||
|Windows 8.1|32|Interno||
|Windows 7 com Service Pack 1 (SP1)|4|Atualize os serviços de integração depois de configurar o sistema operacional convidado.|Edições Ultimate, Enterprise e Professional (32 bits e 64 bits).|

## <a name="guest-operating-system-support-on-other-versions-of-windows"></a>Suporte do sistema operacional convidado em outras versões do Windows

A tabela a seguir fornece links para informações sobre os sistemas operacionais convidados com suporte para o Hyper-V em outras versões do Windows.

|Sistema operacional do host|Tópico|
|-------------------------|---------|
|Windows 10|[Sistemas operacionais convidados com suporte para o Hyper-V cliente no Windows 10](https://docs.microsoft.com/virtualization/hyper-v-on-windows/about/supported-guest-os)|
|Windows Server 2012 R2 e Windows 8.1|-   [Sistemas operacionais convidados do Windows com suporte para o Hyper-V no Windows Server 2012 R2 e Windows 8.1](/previous-versions/windows/it-pro/windows-server-2012-R2-and-2012/dn792027(v=ws.11))<br />-   [Máquinas virtuais Linux e FreeBSD no Hyper-V](Supported-Linux-and-FreeBSD-virtual-machines-for-Hyper-V-on-Windows.md)|
|Windows Server 2012 e Windows 8|[Sistemas operacionais convidados do Windows com suporte para Hyper-V no Windows Server 2012 e Windows 8](/previous-versions/windows/it-pro/windows-server-2012-R2-and-2012/dn792028(v=ws.11))|
|Windows Server 2008 e Windows Server 2008 R2|[Sobre máquinas virtuais e sistemas operacionais convidados](/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/cc794868(v=ws.10))|

## <a name="how-microsoft-provides-support-for-guest-operating-systems"></a>Como a Microsoft oferece suporte para sistemas operacionais convidados

A Microsoft dá suporte aos sistemas operacionais convidados da seguinte forma:

-   Os problemas encontrados em sistemas operacionais da Microsoft e em serviços de integração são tratados pelo suporte da Microsoft.

-   Para problemas encontrados em outros sistemas operacionais que tenham sido certificados pelo fornecedor do sistema operacional para execução em Hyper-V, o suporte é prestado pelo fornecedor.

-   Para problemas encontrados em outros sistemas operacionais, a Microsoft envia o problema à comunidade de suporte de vários fornecedores, a [TSANet](https://www.tsanet.org/).

## <a name="additional-references"></a>Referências adicionais

-   [Máquinas Virtuais do Linux e FreeBSD no Hyper-V](Supported-Linux-and-FreeBSD-virtual-machines-for-Hyper-V-on-Windows.md)

-   [Sistemas operacionais convidados com suporte para o Hyper-V cliente no Windows 10](https://docs.microsoft.com/virtualization/hyper-v-on-windows/about/supported-guest-os)




