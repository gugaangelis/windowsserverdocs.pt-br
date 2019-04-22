---
title: Resolver problemas de vRSS
description: Resolva problemas de vRSS se você não vir o vRSS carga do tráfego de balanceamento para a VM LPs.
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
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59824027"
---
## <a name="resolve-vrss-issues"></a>Resolver problemas de vRSS

Se você concluiu todas as etapas de preparação e você ainda não vir o vRSS carga do tráfego de balanceamento para a VM LPs, há possíveis problemas diferentes.

1. Antes de você executar as etapas de preparação, o vRSS foi desabilitado - e agora deve ser habilitado. Você pode executar **Set-VMNetworkAdapter** para habilitar o vRSS para a VM.

   ```PowerShell
   Set-VMNetworkAdapter <VMname> -VrssEnabled $TRUE
   Set-VMNetworkAdapter -ManagementOS -VrssEnabled $TRUE
   ```

2. RSS foi desabilitada na VM ou na vNIC do host. Windows Server 2016 permite RSS por padrão. alguém talvez tenha sido desativada. 

   - Habilitado = **True**

   **Exiba as configurações atuais:** 

   Execute o seguinte cmdlet do PowerShell na VM\(para o vRSS em uma VM\) ou no host \(para o host vNIC vRSS\).

   ```PowerShell
   Get-NetAdapterRss
   ```

   **Habilite o recurso:** 

   Para alterar o valor de False para True, execute o seguinte cmdlet do PowerShell.

   ```PowerShell
   Enable-NetAdapterRss *
   ```
   
   Outra maneira de todo o sistema para configurar o RSS é usar o netsh. Uso 
   
    ```cmd
   netsh int tcp show global
   ```
   
   para garantir que esse RSS não está desabilitado globalmente. E habilitá-lo se necessário. Essa configuração não é tocada pelo *-NetAdapterRSS.

3. Se você encontrar que VMMQ não é habilitado depois de configurar o vRSS, verifique se as configurações a seguir em cada adaptador conectado ao comutador virtual:

   - VmmqEnabled = **False**
   - VmmqEnabledRequested = **True**

   ![vmmq-enabled](../../media/vmmq-enabled.png)

   **Exiba as configurações atuais:** 

   ```PowerShell
   Get-NetAdapterAdvancedProperty -Name NICName -DisplayName 'Virtual Switch RSS'
   ```

   **Habilite o recurso:** 

   ```PowerShell
   Set-NetAdapterAdvancedProperty -Name NICName -DisplayName 'Virtual Switch RSS' -DisplayValue Enabled”
   ```
 
4. _(Windows Server 2019)_  Não é possível habilitar VMMQ (VmmqEnabled = False) durante a configuração **VrssQueueSchedulingMode** à **dinâmico**. O VrssQueueSchedulingMode não for alterado para dinâmico, quando VMMQ está habilitada.<p>O **VrssQueueSchedulingMode** dos **dinâmico** requer suporte de driver quando VMMQ está habilitado.  VMMQ é um descarregamento do posicionamento de pacote em processadores lógicos e como tal, requer suporte de driver para aproveitar o algoritmo dinâmico.  Instale o driver e firmware que oferece suporte a VMMQ dinâmico do fornecedor da NIC.



---
