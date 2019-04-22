---
title: As propriedades avançadas de NIC
description: Você pode gerenciar as NICs e todos os recursos por meio do Windows PowerShell ou o painel de controle de rede.
ms.prod: windows-server-threshold
ms.technology: networking
ms.topic: article
ms.assetid: 0cafb1cc-5798-42f5-89b6-3ffe7ac024ba
manager: dougkim
ms.author: pashort
author: shortpatti
ms.date: 09/20/2018
ms.openlocfilehash: d1a5fb57bf71fd981e001cfd9ac595ab5bc3cfc5
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59819897"
---
# <a name="nic-advanced-properties"></a>As propriedades avançadas de NIC

Você pode gerenciar as NICs e todos os recursos por meio do Windows PowerShell usando o [NetAdapter](https://docs.microsoft.com/powershell/module/netadapter/?view=win10-ps&viewFallbackFrom=winserverr2-ps) cmdlet.  Você também pode gerenciar as NICs e todos os recursos usando o painel de controle de rede (ncpa. cpl). 

1. Na **Windows PowerShell**, execute o `Get‑NetAdapterAdvancedProperties` cmdlet em relação a dois diferentes marca/modelo de NICs.

   ![Get-NetAdapterAdvancedProperty m1](../../media/network-offload-and-optimization/Get-NetAdapterAdvancedProperty-m1.png)

   ![Get-NetAdapterAdvancedProperty c1](../../media/network-offload-and-optimization/Get-NetAdapterAdvancedProperty-c1.png)

   Há semelhanças e diferenças em como esses dois NIC avançada propriedades lista.

2. No **painel de controle de rede** (ncpa. cpl), faça o seguinte:

   a. e o botão direito do mouse a NIC.

   ![Caixa de diálogo de conexões de rede](../../media/network-offload-and-optimization/network-connections-dialog.png)

   b. Na caixa de diálogo Propriedades, clique em **configurar**.

    ![Propriedades de C1](../../media/network-offload-and-optimization/c1-properties.png)

   c. Clique o **avançado** guia para exibir as propriedades avançadas.<p>Os itens nessa lista se correlaciona com os itens a `Get-NetAdapterAdvancedProperties` saída.

   ![Propriedades do adaptador de rede Chelsio](../../media/network-offload-and-optimization/chelsio-network-adapter-properties.png)

---