---
title: Ferramentas de desempenho para cargas de trabalho de rede
description: Este tópico faz parte do guia de ajuste de desempenho do subsistema de rede para o Windows Server 2016.
ms.topic: article
ms.assetid: c7789781-87e8-464e-981b-af887d01badd
manager: dcscontentpm
ms.author: v-tea
author: Teresa-Motiv
ms.date: 07/16/2018
ms.openlocfilehash: 4344357db92a65725a7bcdc749966d3889d20695
ms.sourcegitcommit: 68444968565667f86ee0586ed4c43da4ab24aaed
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/07/2020
ms.locfileid: "87995084"
---
# <a name="performance-tools-for-network-workloads"></a>Ferramentas de desempenho para cargas de trabalho de rede

>Aplica-se a: Windows Server (Canal Semestral), Windows Server 2016

Você pode usar este tópico para saber mais sobre as ferramentas de desempenho.

Este tópico contém seções sobre o cliente para a ferramenta de tráfego de servidor, o tamanho da janela TCP/IP e o Microsoft Server performance Advisor.

##  <a name="client-to-server-traffic-tool"></a><a name="bkmk_tuning"></a>Ferramenta de tráfego de cliente para servidor

A ferramenta cliente para servidor \( ctsTraffic de tráfego \) fornece a capacidade de criar e verificar o tráfego de rede.

Para obter mais informações e baixar a ferramenta, consulte [ctsTraffic (tráfego de cliente para servidor)](https://github.com/Microsoft/ctsTraffic).

##  <a name="tcpip-window-size"></a><a name="bkmk_size"></a>Tamanho da janela TCP/IP

Para adaptadores de 1 GB, as configurações mostradas na tabela anterior devem fornecer uma boa taxa de transferência, pois NTttcp define o tamanho padrão da janela TCP como 64 K por meio de uma opção de processador lógico específica \( SO_RCVBUF \) para a conexão. Isso fornece um bom desempenho em uma rede de baixa latência.

Por outro lado, para redes de alta latência ou para adaptadores de 10 GB, o valor padrão de tamanho de janela TCP para NTttcp gera um desempenho inferior ao ideal. Em ambos os casos, você deve ajustar o tamanho da janela TCP para permitir o produto de atraso de largura de banda maior.

Você pode definir estaticamente o tamanho da janela TCP para um valor grande usando a opção **-RB** . Essa opção desabilita o ajuste automático da janela TCP e é recomendável usá-la somente se o usuário entender totalmente a alteração resultante no comportamento do TCP/IP. Por padrão, o tamanho da janela TCP é definido com um valor suficiente e ajusta somente sob carga pesada ou por links de alta latência.

##  <a name="microsoft-server-performance-advisor"></a><a name="bkmk_advisor"></a>Supervisor de desempenho de servidor da Microsoft

O Microsoft Server performance Advisor \( Spa \) ajuda os administradores de ti a coletar métricas para identificar, comparar e diagnosticar possíveis problemas de desempenho em uma implantação do windows Server 2016, do windows Server 2012 R2, do windows Server 2012, do windows Server 2008 R2 ou do windows Server 2008.

O SPA gera gráficos e relatórios de diagnóstico abrangentes, além de fornecer recomendações para ajudá-lo a analisar rapidamente os problemas e desenvolver ações corretivas.

 Para obter mais informações e baixar o Advisor, consulte [Microsoft Server performance Advisor](/previous-versions/dn481522(v=vs.85)) no centro de desenvolvimento de hardware do Windows.

Para obter links para todos os tópicos deste guia, consulte [ajuste de desempenho do subsistema de rede](net-sub-performance-top.md).