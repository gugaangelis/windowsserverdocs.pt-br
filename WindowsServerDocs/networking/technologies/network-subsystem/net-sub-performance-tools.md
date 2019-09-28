---
title: Ferramentas de desempenho para cargas de trabalho de rede
description: Este tópico faz parte do guia de ajuste de desempenho do subsistema de rede para o Windows Server 2016.
ms.prod: windows-server
ms.technology: networking
ms.topic: article
ms.assetid: c7789781-87e8-464e-981b-af887d01badd
manager: dougkim
ms.author: pashort
author: shortpatti
ms.date: 07/16/2018
ms.openlocfilehash: 09e775bfe956d67adbd70cf4ce3f9461e1c37cf5
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71405512"
---
# <a name="performance-tools-for-network-workloads"></a>Ferramentas de desempenho para cargas de trabalho de rede

>Aplica-se a: Windows Server (Canal Semestral), Windows Server 2016

Você pode usar este tópico para saber mais sobre as ferramentas de desempenho.

Este tópico contém seções sobre o cliente para a ferramenta de tráfego de servidor, o tamanho da janela TCP/IP e o Microsoft Server performance Advisor.

##  <a name="bkmk_tuning"></a>Ferramenta de tráfego de cliente para servidor

A ferramenta cliente para servidor de tráfego \(ctsTraffic @ no__t-1 fornece a capacidade de criar e verificar o tráfego de rede.

Para obter mais informações e baixar a ferramenta, consulte [ctsTraffic (tráfego de cliente para servidor)](https://github.com/Microsoft/ctsTraffic).
  
##  <a name="bkmk_size"></a>Tamanho da janela TCP/IP

Para adaptadores de 1 GB, as configurações mostradas na tabela anterior devem fornecer boa taxa de transferência, pois NTttcp define o tamanho da janela TCP padrão como 64 K por meio de uma opção de processador lógico específica \(SO_RCVBUF @ no__t-1 para a conexão. Isso fornece um bom desempenho em uma rede de baixa latência.  

Por outro lado, para redes de alta latência ou para adaptadores de 10 GB, o valor padrão de tamanho de janela TCP para NTttcp gera um desempenho inferior ao ideal. Em ambos os casos, você deve ajustar o tamanho da janela TCP para permitir o produto de atraso de largura de banda maior.  

Você pode definir estaticamente o tamanho da janela TCP para um valor grande usando a opção **-RB** . Essa opção desabilita o ajuste automático da janela TCP e é recomendável usá-la somente se o usuário entender totalmente a alteração resultante no comportamento do TCP/IP. Por padrão, o tamanho da janela TCP é definido com um valor suficiente e ajusta somente sob carga pesada ou por links de alta latência.  

##  <a name="bkmk_advisor"></a>Supervisor de desempenho de servidor da Microsoft

O Microsoft Server performance Advisor \(SPA @ no__t-1 ajuda os administradores de ti a coletar métricas para identificar, comparar e diagnosticar possíveis problemas de desempenho em um Windows Server 2016, Windows Server 2012 R2, Windows Server 2012, Windows Server 2008 R2 ou Implantação do Windows Server 2008. 

O SPA gera gráficos e relatórios de diagnóstico abrangentes, além de fornecer recomendações para ajudá-lo a analisar rapidamente os problemas e desenvolver ações corretivas.  
  
 Para obter mais informações e baixar o Advisor, consulte [Microsoft Server performance Advisor](https://msdn.microsoft.com/library/windows/hardware/dn481522.aspx) no centro de desenvolvimento de hardware do Windows.

Para obter links para todos os tópicos deste guia, consulte [ajuste de desempenho do subsistema de rede](net-sub-performance-top.md).