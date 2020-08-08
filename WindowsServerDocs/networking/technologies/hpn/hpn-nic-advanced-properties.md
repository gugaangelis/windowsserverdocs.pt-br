---
title: As propriedades avançadas de NIC
description: Você pode gerenciar NICs e todos os recursos por meio do Windows PowerShell ou do painel de controle de rede.
ms.topic: article
ms.assetid: 0cafb1cc-5798-42f5-89b6-3ffe7ac024ba
manager: dougkim
ms.author: lizross
author: eross-msft
ms.date: 09/20/2018
ms.openlocfilehash: ce9c4049ab701d647701029f41d2570b7fc8cd03
ms.sourcegitcommit: 68444968565667f86ee0586ed4c43da4ab24aaed
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/07/2020
ms.locfileid: "87997661"
---
# <a name="nic-advanced-properties"></a>As propriedades avançadas de NIC

Você pode gerenciar NICs e todos os recursos por meio do Windows PowerShell usando o cmdlet do [netadapter](/powershell/module/netadapter/?view=win10-ps&viewFallbackFrom=winserverr2-ps) .  Você também pode gerenciar NICs e todos os recursos usando o painel de controle de rede (ncpa.cpl).

1. No **Windows PowerShell**, execute o `Get‑NetAdapterAdvancedProperties` cmdlet em relação a dois Make/modelo diferentes de NICs.

   ![M1 Get-NetAdapterAdvancedProperty](../../media/network-offload-and-optimization/Get-NetAdapterAdvancedProperty-m1.png)

   ![Get-NetAdapterAdvancedProperty C1](../../media/network-offload-and-optimization/Get-NetAdapterAdvancedProperty-c1.png)

   Há semelhanças e diferenças nessas duas listas de propriedades avançadas de NIC.

2. No **painel de controle de rede** (ncpa.cpl), faça o seguinte:

   a. Clique com o botão direito do mouse na NIC.

   ![Caixa de diálogo conexões de rede](../../media/network-offload-and-optimization/network-connections-dialog.png)

   b. Na caixa de diálogo Propriedades, clique em **Configurar**.

    ![Propriedades C1](../../media/network-offload-and-optimization/c1-properties.png)

   c. Clique na guia **avançado** para exibir as propriedades avançadas.<p>Os itens nessa lista se relacionam aos itens na `Get-NetAdapterAdvancedProperties` saída.

   ![Propriedades do adaptador de rede do Chelsio](../../media/network-offload-and-optimization/chelsio-network-adapter-properties.png)

---