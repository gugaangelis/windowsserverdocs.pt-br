---
title: Habilitar vRSS em um adaptador de rede virtual
description: Neste tópico, você aprende a habilitar o vRSS no Windows Server usando o Device Manager ou o Windows PowerShell.
ms.prod: windows-server
ms.technology: networking
ms.topic: article
ms.assetid: cb48315c-0204-4927-aa24-64f6789c2e20
manager: dougkim
ms.localizationpriority: medium
ms.date: 09/05/2018
ms.author: lizross
author: eross-msft
ms.openlocfilehash: e4be9060da4a738e3ad8e4976d037f3a05467da3
ms.sourcegitcommit: da7b9bce1eba369bcd156639276f6899714e279f
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/26/2020
ms.locfileid: "80315369"
---
# <a name="enable-vrss-on-a-virtual-network-adapter"></a>Habilitar vRSS em um adaptador de rede virtual

>Aplicável a: Windows Server (canal semestral), Windows Server 2016

O\) virtual RSS \(vRSS requer Fila de Máquina Virtual \(suporte à VMQ\) do adaptador físico. Se a VMQ estiver desabilitada ou não for suportada, o dimensionamento virtual no lado do recebimento será desabilitado. 

Para obter mais informações, consulte [planejar o uso de vRSS](vrss-plan.md).

## <a name="enable-vrss-on-a-vm"></a>Habilitar vRSS em uma VM
 
Use os procedimentos a seguir para habilitar o vRSS usando o Windows PowerShell ou o Device Manager.

-   Gerenciador de Dispositivos
-   Windows PowerShell
  
### <a name="device-manager"></a>Gerenciador de Dispositivos

Você pode usar este procedimento para habilitar o vRSS usando Device Manager.

>[!NOTE]
>A primeira etapa neste procedimento é específica para VMs que executam o Windows 10 ou o Windows Server 2016. Se sua VM estiver executando um sistema operacional diferente, você poderá abrir Device Manager abrindo primeiro o painel de controle e, em seguida, localizando e abrindo Device Manager.
  
1.  Na barra de tarefas da VM, em **tipo aqui para pesquisar**, digite **dispositivo**. 

2.  Nos resultados da pesquisa, clique em **Device Manager**.

3.  Em Device Manager, clique para expandir **adaptadores de rede**. 

4.  Clique com o botão direito do mouse no adaptador de rede que você deseja configurar e clique em **Propriedades**.<p>A caixa de diálogo **Propriedades** do adaptador de rede é aberta.

5.  Em **Propriedades**do adaptador de rede, clique na guia **avançado** . 

6.  Em **Propriedade**, role para baixo e clique em **escala de recebimento**. 

7.  Verifique se a seleção em **valor** está **habilitada**. 

8.  Clique em **OK**.
  
> [!NOTE]
> Na guia **avançado** , alguns adaptadores de rede também exibem o número de filas de RSS com suporte pelo adaptador.

---

### <a name="windows-powershell"></a>Windows PowerShell

Use o procedimento a seguir para habilitar o vRSS usando o Windows PowerShell.

1. Na máquina virtual, abra o **Windows PowerShell**.

2. Digite o comando a seguir, assegurando que você substitua o valor de *AdapterName* para o parâmetro **-Name** pelo nome do adaptador de rede que você deseja configurar e pressione Enter. 
  
   ```PowerShell
   Enable-NetAdapterRSS -Name "AdapterName"
   ```

   >[!TIP]
   >Como alternativa, você pode usar o comando a seguir para habilitar o vRSS.
   >```PowerShell
   >Set-NetAdapterRSS -Name "AdapterName" -Enabled $True  
   >```

Para obter mais informações, consulte [comandos do Windows PowerShell para RSS e vRSS](vrss-wps.md).

---