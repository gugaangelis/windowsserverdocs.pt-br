---
title: Resolver problemas de vRSS
description: Resolva problemas de vRSS se você não vir vRSS carregar balanceamento tráfego para os LPs de VM.
ms.prod: windows-server-threshold
ms.technology: networking
ms.topic: article
ms.assetid: ''
ms.localizationpriority: medium
manager: dougkim
ms.author: pashort
author: shortpatti
ms.date: 09/04/2018
ms.openlocfilehash: a2d6eb43149361b4270565b63fc99f483f364f74
ms.sourcegitcommit: 515b4fd5c40fcbae0e12a2c30090384533972353
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 11/29/2018
ms.locfileid: "8232515"
---
## Resolver problemas de vRSS

Se você concluiu todas as etapas de preparação e você ainda não vê vRSS carregar balanceamento tráfego para os LPs de VM, há diferentes possíveis problemas.

1. Antes de você executou as etapas de preparação, vRSS foi desativada - e agora deve ser habilitado. Você pode executar **Conjunto VMNetworkAdapter** para habilitar vRSS para a VM.

   ```PowerShell
   Set-VMNetworkAdapter <VMname> -VrssEnabled $TRUE
   Set-VMNetworkAdapter -ManagementOS -VrssEnabled $TRUE
   ```

2. RSS was disabled in the VM or on the host vNIC. Windows Server 2016 enables RSS by default; someone might have disabled it. 

   - Ativado = **True**

   **Exiba as configurações atuais:** 

   Execute o seguinte cmdlet do PowerShell no VM\ (para vRSS em um VM\) ou no host \ (para vRSS\ de vNIC do host).

   ```PowerShell
   Get-NetAdapterRss
   ```

   **Habilite o recurso:** 

   Para alterar o valor de False para True, execute o seguinte cmdlet do PowerShell.

   ```PowerShell
   Enable-NetAdapterRss *
   ```
   
   Another system-wide way to configure RSS is using netsh. Uso 
   
    ```cmd
   netsh int tcp show global
   ```
   
   to make sure that RSS isn't disabled globally. E habilitá-lo se necessário. Essa configuração não é tocada por *-NetAdapterRSS.

3. Se você encontrar que VMMQ não está habilitado depois de configurar vRSS, verifique se as seguintes configurações em cada adaptador conectado ao comutador virtual:

   - VmmqEnabled = **False**
   - VmmqEnabledRequested = **True**

   ![vmmq habilitado](../../media/vmmq-enabled.png)

   **Exiba as configurações atuais:** 

   ```PowerShell
   Get-NetAdapterAdvancedProperty -Name NICName -DisplayName 'Virtual Switch RSS'
   ```

   **Habilite o recurso:** 

   ```PowerShell
   Set-NetAdapterAdvancedProperty -Name NICName -DisplayName 'Virtual Switch RSS' -DisplayValue Enabled”
   ```
 
4. _(Windows Server 2019)_ Você não pode habilitar VMMQ (VmmqEnabled = False) ao definir **VrssQueueSchedulingMode** para **dinâmico**. O VrssQueueSchedulingMode não muda para dinâmico depois VMMQ está habilitado.<p>O **VrssQueueSchedulingMode** de **dinâmico** requer suporte de driver quando VMMQ está habilitado.  VMMQ é um descarregamento do posicionamento do pacote em processadores lógicos e como tal, requer suporte de driver para aproveitar o algoritmo dinâmico.  Instale o fornecedor NIC driver e firmware que oferece suporte a VMMQ dinâmico.



---
