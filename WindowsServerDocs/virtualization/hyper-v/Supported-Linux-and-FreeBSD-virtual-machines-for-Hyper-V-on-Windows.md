---
title: Máquinas virtuais Linux e FreeBSD com suporte para Hyper-V no Windows
description: Lista os serviços e recursos de integração do Linux incluídos em cada versão
ms.prod: windows-server-threshold
ms.service: na
manager: dongill
ms.technology: compute-hyper-v
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 990ff94a-30fb-434b-b4a2-3804a5245ba6
author: shirgall
ms.author: kathydav
ms.date: 10/03/2016
ms.openlocfilehash: 593068f4fc2015c7f8f94bfe49c5a11c23cb6599
ms.sourcegitcommit: 1bc3c229e9688ac741838005ec4b88e8f9533e8a
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/18/2019
ms.locfileid: "68314978"
---
# <a name="supported-linux-and-freebsd-virtual-machines-for-hyper-v-on-windows"></a>Máquinas virtuais Linux e FreeBSD com suporte para Hyper-V no Windows

>Aplica-se a: Windows Server 2019, Windows Server 2016, Hyper-V Server 2016, Windows Server 2012 R2, Hyper-V Server 2012 R2, Windows Server 2012, Hyper-V Server 2012, Windows Server 2008 R2, Windows 10, Windows 8.1, Windows 8, Windows 7,1, Windows 7

O Hyper-V dá suporte a dispositivos emulados e Hyper-V específicos para máquinas virtuais Linux e FreeBSD. Ao executar com dispositivos emulados, nenhum software adicional é necessário para ser instalado. No entanto, os dispositivos emulados não fornecem alto desempenho e não podem aproveitar a infra-estrutura avançada de gerenciamento de máquinas virtuais oferecida pela tecnologia Hyper-V. Para fazer uso completo de todos os benefícios que o Hyper-V fornece, é melhor usar dispositivos específicos do Hyper-V para Linux e FreeBSD. A coleção de drivers que são necessários para executar dispositivos específicos do Hyper-V é conhecida como Linux Integration Services (LIS) ou FreeBSD Integration Services (BIS).

LIS foi adicionado ao kernel do Linux e é atualizado para novas versões. Mas as distribuições do Linux baseadas em kernels mais antigos podem não ter os aprimoramentos ou correções mais recentes. A Microsoft fornece um download que contém drivers LIS instaláveis para algumas instalações do Linux com base nesses kernels mais antigos. Como os fornecedores de distribuição incluem versões do Linux Integration Services, é melhor instalar a versão mais recente para download do LIS, se aplicável, para sua instalação.

Para outras distribuições de distribuição do Linux, as alterações do LIS são regularmente integradas ao kernel do sistema operacional e aos aplicativos, portanto, nenhum download ou instalação separado é necessário.

Para versões mais antigas do FreeBSD (antes de 10,0), a Microsoft fornece portas que contêm os drivers BIS instaláveis e os daemons correspondentes para máquinas virtuais FreeBSD. Para versões mais recentes do FreeBSD, o BIS é integrado ao sistema operacional FreeBSD, e nenhum download ou instalação separado é necessário, exceto para download de portas KVP que é necessário para o FreeBSD 10,0.

> [!TIP]
> - Baixe o [Windows Server 2019](https://www.microsoft.com/en-us/evalcenter/evaluate-windows-server-2019) no centro de avaliação.

O objetivo desse conteúdo é fornecer informações que ajudem a facilitar sua implantação do Linux ou FreeBSD no Hyper-V. Os detalhes específicos incluem:

* Distribuições do Linux ou versões do FreeBSD que exigem o download e a instalação de drivers LIS ou BIS.

* Distribuições do Linux ou versões do FreeBSD que contêm drivers LIS ou BIS internos.

* Mapas de distribuição de recursos que indicam os recursos nas principais distribuições do Linux ou versões FreeBSD.

* Problemas conhecidos e soluções alternativas para cada distribuição ou versão.

* Descrição do recurso para cada recurso LIS ou BIS.

**Deseja fazer uma sugestão sobre os recursos e a funcionalidade?** Há algo que poderíamos fazer melhor? Você pode usar o site de [voz de usuário do Windows Server](https://windowsserver.uservoice.com/forums/295062-linux-support) para sugerir novos recursos e funcionalidades para máquinas virtuais Linux e FreeBSD no Hyper-V e para ver o que outras pessoas estão dizendo.

## <a name="in-this-section"></a>Nesta seção

* [Máquinas virtuais CentOS e Red Hat Enterprise Linux com suporte no Hyper-V](Supported-CentOS-and-Red-Hat-Enterprise-Linux-virtual-machines-on-Hyper-V.md)

* [Máquinas virtuais do Debian com suporte no Hyper-V](Supported-Debian-virtual-machines-on-Hyper-V.md)

* [Máquinas virtuais Oracle Linux com suporte no Hyper-V](Supported-Oracle-Linux-virtual-machines-on-Hyper-V.md)

* [Máquinas virtuais SUSE com suporte no Hyper-V](Supported-SUSE-virtual-machines-on-Hyper-V.md)

* [Máquinas virtuais Ubuntu com suporte no Hyper-V](Supported-Ubuntu-virtual-machines-on-Hyper-V.md)

* [Máquinas virtuais FreeBSD com suporte no Hyper-V](Supported-FreeBSD-virtual-machines-on-Hyper-V.md)

* [Descrições de recursos para máquinas virtuais Linux e FreeBSD no Hyper-V](Feature-Descriptions-for-Linux-and-FreeBSD-virtual-machines-on-Hyper-V.md)

* [Práticas recomendadas para executar o Linux no Hyper-V](Best-Practices-for-running-Linux-on-Hyper-V.md)

* [Práticas recomendadas para executar o FreeBSD no Hyper-V](Best-practices-for-running-FreeBSD-on-Hyper-V.md)
