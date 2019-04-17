---
title: Ferramentas de desempenho para cargas de trabalho de rede
description: Este tópico faz parte do guia ajuste de desempenho do subsistema de rede para Windows Server 2016.
ms.prod: windows-server-threshold
ms.technology: networking
ms.topic: article
ms.assetid: c7789781-87e8-464e-981b-af887d01badd
manager: brianlic
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 81c31871b3dfa4644690fe074ae15eaaa680d7f2
ms.sourcegitcommit: 19d9da87d87c9eefbca7a3443d2b1df486b0b010
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/28/2018
---
# <a name="performance-tools-for-network-workloads"></a>Ferramentas de desempenho para cargas de trabalho de rede

>Aplica-se a: Windows Server (anual por canal), Windows Server 2016

Você pode usar este tópico para saber mais sobre ferramentas de desempenho.

Este tópico contém as seções sobre o cliente ferramenta tráfego de servidor, tamanho da janela TCP/IP e Supervisor de desempenho de servidor Microsoft.

##  <a name="bkmk_tuning"></a>Cliente à ferramenta de tráfego de servidor

O cliente à ferramenta do servidor tráfego \(ctsTraffic\) fornece a capacidade de criar e verificar o tráfego de rede.

Para obter mais informações e baixar a ferramenta, consulte [ctsTraffic (cliente-servidor tráfego)](http://ctstraffic.codeplex.com/).
  
##  <a name="bkmk_size"></a>Tamanho da janela de TCP/IP

Para adaptadores de 1 GB, as configurações mostradas na tabela anterior devem fornecer throughput boa porque NTttcp define o tamanho da janela padrão TCP para 64 K por meio de um processador de lógico específico opção \(SO_RCVBUF\) para a conexão. Isso proporciona um bom desempenho em uma rede de baixa latência.  

Por outro lado, para redes de alta latência ou para adaptadores de 10 GB, o valor de tamanho de janela TCP padrão para NTttcp produz menor do que o desempenho ideal. Em ambos os casos, você deve ajustar o tamanho da janela TCP para permitir o produto de atraso de largura de banda maior.  

Você pode definir estaticamente o tamanho da janela TCP com um valor grande usando o **-rb** opção. Essa opção desativa o ajuste automático da janela TCP, e é recomendável usá-lo somente se o usuário entende a alteração resultante no comportamento de TCP/IP. Por padrão, o tamanho da janela TCP for definido como um valor suficiente e ajusta somente com excesso de carga ou ao longo de links de alta latência.  

##  <a name="bkmk_advisor"></a>Supervisor de desempenho de servidor da Microsoft

Supervisor de desempenho de servidor Microsoft \(SPA\) ajuda os administradores de TI coletar métricas para identificar, comparar e diagnosticar problemas de desempenho potenciais em Windows Server 2016, Windows Server 2012 R2, Windows Server 2012, Windows Server 2008 R2 ou implantação do Windows Server 2008. 

SPA gera gráficos e relatórios de diagnósticos abrangentes e fornece recomendações para ajudá-lo rapidamente analisem problemas e desenvolvem ações corretivas.  
  
 Para obter mais informações e para baixar o Supervisor, consulte [Supervisor de desempenho de servidor Microsoft](https://msdn.microsoft.com/library/windows/hardware/dn481522.aspx) no Centro de desenvolvimento de Hardware do Windows.

Para obter links para todos os tópicos neste guia, consulte [ajuste de desempenho do subsistema de rede](net-sub-performance-top.md).