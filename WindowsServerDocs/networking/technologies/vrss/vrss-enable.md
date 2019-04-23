---
title: Habilite o vRSS em um adaptador de rede Virtual
description: Neste tópico, você aprenderá como habilitar o vRSS no Windows Server usando o Gerenciador de dispositivos ou o Windows PowerShell.
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
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59882677"
---
# <a name="enable-vrss-on-a-virtual-network-adapter"></a>Habilite o vRSS em um adaptador de rede Virtual

>Aplica-se a: Windows Server (canal semestral), Windows Server 2016

RSS virtual \(vRSS\) requer a fila de máquina Virtual \(VMQ\) e suporte do adaptador físico. Se a VMQ está desabilitada ou não há suporte para Virtual RSS está desabilitado. 

Para obter mais informações, consulte [planejar o uso de vRSS](vrss-plan.md).

## <a name="enable-vrss-on-a-vm"></a>Habilite o vRSS em uma máquina virtual
 
Use os procedimentos a seguir para habilitar o vRSS por meio do Windows PowerShell ou Gerenciador de dispositivos.

-   Gerenciador de Dispositivos
-   Windows PowerShell
  
### <a name="device-manager"></a>Gerenciador de Dispositivos

Você pode usar este procedimento para habilitar o vRSS usando o Gerenciador de dispositivos.

>[!NOTE]
>A primeira etapa neste procedimento é específica para VMs que executam o Windows 10 ou Windows Server 2016. Se sua VM estiver executando um sistema operacional diferente, você pode abrir o Gerenciador de dispositivo primeiro abrir o painel de controle, localizando e abrindo o Gerenciador de dispositivos.
  
1.  Na barra de tarefas VM, na **digite para pesquisar**, digite **dispositivo**. 

2.  Nos resultados da pesquisa, clique em **Gerenciador de dispositivos**.

3.  No Gerenciador de dispositivos, clique para expandir **adaptadores de rede**. 

4.  O adaptador de rede que você deseja configurar e, em seguida, clique com o botão direito **propriedades**.<p>O adaptador de rede **propriedades** caixa de diálogo é aberta.

5.  No adaptador de rede **propriedades**, clique no **avançado** guia. 

6.  Na **propriedade**, role para baixo e clique em **RSS**. 

7.  Certifique-se de que a seleção no **valor** é **habilitado**. 

8.  Clique em **OK**.
  
> [!NOTE]
> Sobre o **avançado** guia, alguns adaptadores de rede também exibem o número de filas RSS que têm suporte pelo adaptador.

---

### <a name="windows-powershell"></a>Windows PowerShell

Use o procedimento a seguir para habilitar o vRSS usando o Windows PowerShell.

1. Na máquina virtual, abra **Windows PowerShell**.

2. Digite o seguinte comando, garantindo que você substitui o *AdapterName* valor para o **-nome** parâmetro com o nome do adaptador de rede que você deseja configurar e, em seguida, pressione ENTER. 
  
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