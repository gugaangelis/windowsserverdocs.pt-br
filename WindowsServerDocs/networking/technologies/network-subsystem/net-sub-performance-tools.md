---
title: Ferramentas de desempenho para cargas de trabalho de rede
description: Este tópico faz parte do guia de ajuste de desempenho do subsistema de rede para o Windows Server 2016.
ms.prod: windows-server-threshold
ms.technology: networking
ms.topic: article
ms.assetid: c7789781-87e8-464e-981b-af887d01badd
manager: dougkim
ms.author: pashort
author: shortpatti
ms.date: 07/16/2018
ms.openlocfilehash: e71c5f34041145907c30b279dc91a94c03c2abed
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59824927"
---
# <a name="performance-tools-for-network-workloads"></a>Ferramentas de desempenho para cargas de trabalho de rede

>Aplica-se a: Windows Server (canal semestral), Windows Server 2016

Você pode usar este tópico para saber mais sobre as ferramentas de desempenho.

Este tópico contém seções sobre o cliente para a ferramenta de tráfego do servidor, o tamanho de janela TCP/IP e Microsoft Server Performance Advisor.

##  <a name="bkmk_tuning"></a> Cliente para a ferramenta de tráfego do servidor

O cliente para o tráfego do servidor \(ctsTraffic\) ferramenta oferece a capacidade de criar e verificar o tráfego de rede.

Para obter mais informações e baixar a ferramenta, consulte [ctsTraffic (tráfego de cliente-servidor)](https://github.com/Microsoft/ctsTraffic).
  
##  <a name="bkmk_size"></a> Tamanho da janela de TCP/IP

Para adaptadores de 1 GB, as configurações mostradas na tabela anterior devem fornecer boa taxa de transferência porque NTttcp define o tamanho da janela TCP padrão como 64K através de uma opção específica do processador lógico \(SO_RCVBUF\) para a conexão. Isso fornece um bom desempenho em uma rede de baixa latência.  

Por outro lado, para redes de alta latência ou adaptadores de 10 GB, o valor de tamanho de janela TCP padrão para o NTttcp resulta em desempenho abaixo do ideal. Em ambos os casos, você deve ajustar o tamanho da janela TCP para permitir que o produto do atraso da largura de banda maior.  

Você pode definir estaticamente o tamanho da janela TCP com um valor grande usando o **-rb** opção. Esta opção desabilita o ajuste automático da janela TCP, e é recomendável usá-lo somente se o usuário compreende plenamente a alteração resultante no comportamento de TCP/IP. Por padrão, o tamanho da janela TCP é definido em um valor suficiente e ajusta somente sob carga pesada ou em links de alta latência.  

##  <a name="bkmk_advisor"></a> Microsoft Server Performance Advisor

Microsoft Server Performance Advisor \(SPA\) ajuda os administradores de TI coletam métricas para identificar, comparar e diagnosticar possíveis problemas de desempenho em um Windows Server 2016, o Windows Server 2012 R2, o Windows Server 2012, o Windows Server 2008 R2 ou implantação do Windows Server 2008. 

SPA gera gráficos e relatórios de diagnóstico abrangentes e fornece recomendações para ajudar você a analisem problemas e desenvolvem ações corretivas.  
  
 Para obter mais informações e baixar o Supervisor, consulte [Microsoft Server Performance Advisor](https://msdn.microsoft.com/library/windows/hardware/dn481522.aspx) no Centro de desenvolvimento de Hardware do Windows.

Para obter links para todos os tópicos neste guia, consulte [ajuste de desempenho do subsistema de rede](net-sub-performance-top.md).