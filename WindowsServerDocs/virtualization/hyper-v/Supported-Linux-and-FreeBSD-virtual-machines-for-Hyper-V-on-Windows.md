---
title: Linux e FreeBSD máquinas virtuais com suporte para Hyper-V no Windows
description: Lista os serviços de integração do Linux e os recursos incluídos em cada versão
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
ms.openlocfilehash: 9df495bdc67b06a675fec050fb4c2960337ce8ca
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59832897"
---
# <a name="supported-linux-and-freebsd-virtual-machines-for-hyper-v-on-windows"></a>Linux e FreeBSD máquinas virtuais com suporte para Hyper-V no Windows

>Aplica-se a: Windows Server 2016, Hyper-V Server 2016, Windows Server 2012 R2, Hyper-V Server 2012 R2, Windows Server 2012, do Hyper-V Server 2012, Windows Server 2008 R2, Windows 10, Windows 8.1, Windows 8, Windows 7.1, Windows 7

Hyper-V dá suporte a dispositivos emulados e específico do Hyper-V para máquinas virtuais Linux e FreeBSD. Ao executar com dispositivos emulados, nenhum software adicional é necessário para ser instalado. No entanto, dispositivos emulados não fornecem alto desempenho e não é possível aproveitar a infraestrutura de gerenciamento de máquina avançado que oferece a tecnologia Hyper-V. Para fazer uso integral de todas as vantagens do Hyper-V, é melhor usar dispositivos específico do Hyper-V para Linux e FreeBSD. A coleção de drivers que são necessários para executar a dispositivos específicos do Hyper-V são conhecidos como serviços de integração do Linux (LIS) ou serviços de integração do FreeBSD (BIS).

LIS foi adicionado ao kernel do Linux e é atualizado para novas versões. Mas, com base nos kernels mais antigos de distribuições do Linux podem não ter as mais recentes aprimoramentos ou correções. A Microsoft fornece um download que contém os drivers LIS instaláveis para algumas instalações do Linux com base nesses kernels mais antigos. Como os fornecedores de distribuição incluem versões dos serviços de integração do Linux, é melhor instalar a versão mais recente disponível para download do LIS, se aplicável, para a instalação.

Para outras distribuições de Linux LIS alterações regularmente são integradas ao kernel do sistema operacional e os aplicativos para que nenhuma instalação ou download separado é necessária.

Para versões mais antigas do FreeBSD (antes 10.0), a Microsoft fornece as portas que contêm os drivers instaláveis do BIS e daemons correspondentes para máquinas virtuais FreeBSD. Para versões mais recentes do FreeBSD, BIS interna do sistema operacional FreeBSD e nenhuma instalação ou download separado é exigida, exceto um download de portas KVP é necessária para FreeBSD 10.0.

> [!TIP]
> - Baixe [Windows Server 2016](https://www.microsoft.com/evalcenter/evaluate-windows-server-2016) do Centro de avaliação.
> - Baixe [Microsoft Hyper-V Server 2016](https://www.microsoft.com/evalcenter/evaluate-hyper-v-server-2016) do Centro de avaliação.

O objetivo deste conteúdo é fornecer informações que ajuda a facilitar a implantação do Linux ou FreeBSD no Hyper-V. Detalhes específicos incluem:

* Distribuições do Linux ou versões do FreeBSD que exigem o download e instalação de drivers LIS ou BIS.

* Distribuições do Linux ou versões do FreeBSD que contêm os drivers LIS ou BIS internos.

* Mapas de distribuição de recurso que indicam os recursos principais distribuições do Linux ou versões do FreeBSD.

* Problemas conhecidos e soluções alternativas para cada distribuição ou versão.

* Descrição do recurso para cada recurso LIS ou BIS.

**Deseja fazer uma sugestão sobre recursos e funcionalidades?** Há algo que podíamos fazer melhor? Você pode usar o [Windows Server User Voice](https://windowsserver.uservoice.com/forums/295062-linux-support) site sugerir novos recursos e funcionalidades para Linux e máquinas de virtuais FreeBSD no Hyper-V e ver o que outras pessoas estão dizendo.

## <a name="in-this-section"></a>Nesta seção

* [Suporte para CentOS e Red Hat Enterprise Linux máquinas virtuais do Hyper-V](Supported-CentOS-and-Red-Hat-Enterprise-Linux-virtual-machines-on-Hyper-V.md)

* [Suporte para máquinas virtuais do Debian no Hyper-V](Supported-Debian-virtual-machines-on-Hyper-V.md)

* [Máquinas de virtuais do Oracle Linux com suporte no Hyper-V](Supported-Oracle-Linux-virtual-machines-on-Hyper-V.md)

* [De máquinas virtuais SUSE com suporte no Hyper-V](Supported-SUSE-virtual-machines-on-Hyper-V.md)

* [Máquinas virtuais do Ubuntu com suporte no Hyper-V](Supported-Ubuntu-virtual-machines-on-Hyper-V.md)

* [Máquinas de virtuais FreeBSD com suporte no Hyper-V](Supported-FreeBSD-virtual-machines-on-Hyper-V.md)

* [Descrições de recursos para máquinas virtuais de Linux e FreeBSD no Hyper-V](Feature-Descriptions-for-Linux-and-FreeBSD-virtual-machines-on-Hyper-V.md)

* [Práticas recomendadas para executar o Linux no Hyper-V](Best-Practices-for-running-Linux-on-Hyper-V.md)

* [Práticas recomendadas para executar o FreeBSD no Hyper-V](Best-practices-for-running-FreeBSD-on-Hyper-V.md)
