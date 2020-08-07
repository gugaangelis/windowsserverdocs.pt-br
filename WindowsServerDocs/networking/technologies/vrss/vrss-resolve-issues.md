---
title: Resolver problemas de vRSS
description: Resolva os problemas de vRSS se você não vir o tráfego de balanceamento de carga vRSS para a VM LPs.
ms.topic: article
ms.localizationpriority: medium
manager: dougkim
ms.author: lizross
author: eross-msft
ms.date: 09/04/2018
ms.openlocfilehash: a7f0a3b190232cafb68e3a39104c357972831441
ms.sourcegitcommit: dfa48f77b751dbc34409aced628eb2f17c912f08
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/07/2020
ms.locfileid: "87951946"
---
# <a name="resolve-vrss-issues"></a>Resolver problemas de vRSS

Se você tiver concluído todas as etapas de preparação e ainda não vir o tráfego de balanceamento de carga vRSS para a VM LPs, haverá possíveis problemas diferentes.

1. Antes de executar as etapas de preparação, o vRSS foi desabilitado-e agora deve ser habilitado. Você pode executar **set-VMNetworkAdapter** para habilitar o vRSS para a VM.

   ```PowerShell
   Set-VMNetworkAdapter <VMname> -VrssEnabled $TRUE
   Set-VMNetworkAdapter -ManagementOS -VrssEnabled $TRUE
   ```

2. O RSS foi desabilitado na VM ou no host vNIC. O Windows Server 2016 habilita o RSS por padrão; Alguém pode tê-lo desabilitado.

   - Habilitado = **verdadeiro**

   **Exibir as configurações atuais:**

   Execute o seguinte cmdlet do PowerShell na VM \( para vrss em uma VM \) ou no host \( para o vrss vNIC do host \) .

   ```PowerShell
   Get-NetAdapterRss
   ```

   **Habilite o recurso:**

   Para alterar o valor de false para true, execute o seguinte cmdlet do PowerShell.

   ```PowerShell
   Enable-NetAdapterRss *
   ```

   Outra maneira em todo o sistema de configurar o RSS é usar o netsh. Use

    ```cmd
   netsh int tcp show global
   ```

   para garantir que o RSS não seja desabilitado globalmente. E habilitá-lo se necessário. Essa configuração não é coberta por *-NetAdapterRSS.

3. Se você achar que o VMMQ não está habilitado depois de configurar o vRSS, verifique as seguintes configurações em cada adaptador anexado ao comutador virtual:

   - VmmqEnabled = **false**
   - VmmqEnabledRequested = **true**

   ![habilitado para vmmq](../../media/vmmq-enabled.png)

   **Exibir as configurações atuais:**

   ```PowerShell
   Get-NetAdapterAdvancedProperty -Name NICName -DisplayName 'Virtual Switch RSS'
   ```

   **Habilite o recurso:**

   ```PowerShell
   Set-NetAdapterAdvancedProperty -Name NICName -DisplayName 'Virtual Switch RSS' -DisplayValue Enabled”
   ```

4. _(Windows Server 2019)_ Não é possível habilitar VMMQ (VmmqEnabled = false) ao definir **VrssQueueSchedulingMode** como **dinâmico**. O VrssQueueSchedulingMode não é alterado para dinâmico quando VMMQ está habilitado.<p>O **VrssQueueSchedulingMode** de **dinâmico** requer suporte de driver quando o VMMQ está habilitado.  VMMQ é um descarregamento do posicionamento de pacotes em processadores lógicos e, como tal, requer suporte de driver para aproveitar o algoritmo dinâmico.  Instale o driver e o firmware do fornecedor da NIC que dá suporte a VMMQ dinâmicos.



---
