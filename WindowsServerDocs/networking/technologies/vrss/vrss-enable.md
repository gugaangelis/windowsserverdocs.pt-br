---
title: Habilitar vRSS em um adaptador de rede Virtual
description: Neste tópico, você aprenderá como habilitar vRSS no Windows Server usando o Gerenciador de dispositivos ou do Windows PowerShell.
ms.prod: windows-server-threshold
ms.technology: networking
ms.topic: article
ms.assetid: cb48315c-0204-4927-aa24-64f6789c2e20
manager: dougkim
ms.localizationpriority: medium
ms.date: 09/05/2018
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 19e8011fb98b84c20e8237792664551d2362d589
ms.sourcegitcommit: e84e328c13a701e8039b16a4824a6e58a6e59b0b
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/22/2018
ms.locfileid: "4133412"
---
# Habilitar vRSS em um adaptador de rede Virtual

>Aplicável a: Windows Server (canal semestral), Windows Server 2016

Virtual RSS \(vRSS\) requires Virtual Machine Queue \(VMQ\) support from the physical adapter. Se VMQ estiver desabilitada ou não há suporte para dimensionamento Virtual Receive-side está desabilitada. 

Para obter mais informações, consulte a [Planejar o uso de vRSS](vrss-plan.md).

## Habilitar vRSS em uma VM
 
Use os procedimentos a seguir para habilitar vRSS usando o Windows PowerShell ou Gerenciador de dispositivos.

-   Gerenciador de Dispositivos
-   Windows PowerShell
  
### Gerenciador de Dispositivos

Você pode usar este procedimento para habilitar vRSS usando o Gerenciador de dispositivos.

>[!NOTE]
>A primeira etapa neste procedimento é específica para VMs que executam o Windows 10 ou Windows Server 2016. Se sua VM estiver executando um sistema operacional diferente, você pode abrir o Gerenciador de dispositivos primeiro abrindo o painel de controle, localizar e abrir o Gerenciador de dispositivos.
  
1.  Na barra de tarefas VM, **Digite aqui para pesquisar**, digite o **dispositivo**. 

2.  Nos resultados da pesquisa, clique em **Gerenciador de dispositivos**.

3.  No Gerenciador de dispositivos, clique para expandir **os adaptadores de rede**. 

4.  Clique com botão direito o adaptador de rede que você deseja configurar e, em seguida, clique em **Propriedades**.<p>Abre a caixa de diálogo de **Propriedades** do adaptador de rede.

5.  No adaptador de rede **Propriedades**, clique na guia **Avançado** . 

6.  Na **propriedade**, role para baixo e clique em **dimensionamento do lado do recebimento**. 

7.  Certifique-se de que a seleção no **valor** é **habilitado**. 

8.  Clique em **OK**.
  
> [!NOTE]
> On the **Advanced** tab, some network adapters also display the number of RSS queues that are supported by the adapter.

---

### Windows PowerShell

Use o procedimento a seguir para habilitar vRSS usando o Windows PowerShell.

1. Na máquina virtual, abra o **Windows PowerShell**.

2. Digite o seguinte comando, garantindo que você substitua o valor *AdapterName* para o **-nome** parâmetro com o nome do adaptador de rede que você deseja configurar e, em seguida, pressione ENTER. 
  
   ```PowerShell
   Enable-NetAdapterRSS -Name "AdapterName"
   ```

   >[!TIP]
   >Como alternativa, você pode usar o comando a seguir para habilitar vRSS.
   >```PowerShell
   >Set-NetAdapterRSS -Name "AdapterName" -Enabled $True  
   >```

For more information, see [Windows PowerShell Commands for RSS and vRSS](vrss-wps.md).

---