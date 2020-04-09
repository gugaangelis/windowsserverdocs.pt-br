---
title: Instalar a função de servidor do controlador de rede usando Gerenciador do Servidor
description: Este tópico fornece instruções sobre como instalar a função de servidor do controlador de rede usando Gerenciador do Servidor no Windows Server 2016.
manager: grcusanz
ms.prod: windows-server
ms.technology: networking-sdn
ms.topic: get-started-article
ms.assetid: 3a6e4352-ff62-4290-b8a4-5c83740070fc
ms.author: anpaul
author: AnirbanPaul
ms.openlocfilehash: a93d737b80e41fbc4401105a9f72426c3a73fa7d
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80859669"
---
# <a name="install-the-network-controller-server-role-using-server-manager"></a>Instalar a função de servidor do controlador de rede usando Gerenciador do Servidor

>Aplicável a: Windows Server (canal semestral), Windows Server 2016

Este tópico fornece instruções sobre como instalar a função de servidor do controlador de rede usando Gerenciador do Servidor.

>[!IMPORTANT]
>Não implante a função de servidor do controlador de rede em hosts físicos. Para implantar o controlador de rede, você deve instalar a função de servidor do controlador de rede em uma máquina virtual do Hyper-V \(\) da VM que está instalada em um host do Hyper-V. Depois de instalar o controlador de rede em VMs em três hosts Hyper\-V diferentes, você deve habilitar os hosts do Hyper\-V para a rede definida pelo software \(SDN\) adicionando os hosts ao controlador de rede usando o comando **New-NetworkControllerServer**do Windows PowerShell. Ao fazer isso, você está permitindo que o software SDN Load Balancer funcione. Para obter mais informações, consulte [New-NetworkControllerServer](https://technet.microsoft.com/itpro/powershell/windows/network-controller/new-networkcontrollerserver).
  
Depois de instalar o controlador de rede, você deve usar comandos do Windows PowerShell para configuração adicional do controlador de rede. Para obter mais informações, consulte [implantar o controlador de rede usando o Windows PowerShell](../../deploy/Deploy-Network-Controller-using-Windows-PowerShell.md).  
  
### <a name="to-install-network-controller"></a>Para instalar o controlador de rede  
  
1.  No Gerenciador do Servidor, clique em **Gerenciar**e depois em **Adicionar Funções e Recursos**. O assistente Adicionar funções e recursos é aberto. Clique em **Avançar**.  
  
2.  Em **Selecionar tipo de instalação**, mantenha a configuração padrão e clique em **Avançar**.  
  
3.  Em **selecionar servidor de destino**, escolha o servidor no qual você deseja instalar o controlador de rede e clique em **Avançar**.  
  
4.  Em **selecionar funções de servidor**, em **funções**, clique em **controlador de rede**.  
  
    ![Função de servidor do controlador de rede](../../../media/Install-the-Network-Controller-server-role-using-Server-Manager/netc_install_07.jpg)  
  
5.  A caixa de diálogo **Adicionar recursos necessários para o controlador de rede** é aberta. Clique em **Adicionar Recursos**.  
  
    ![Adicionar recursos para o controlador de rede](../../../media/Install-the-Network-Controller-server-role-using-Server-Manager/netc_install_06.jpg)  
  
6.  Em **selecionar funções de servidor**, clique em **Avançar**.  
  
    ![Clique em Avançar.](../../../media/Install-the-Network-Controller-server-role-using-Server-Manager/netc_install_07.jpg)  
  
7.  Em **selecionar recursos**, clique em **Avançar**.  
  
8.  Em **controlador de rede** , clique em **Avançar**.  
  
9. Em **confirmar seleções de instalação**, examine suas escolhas. A instalação do controlador de rede exige que você reinicie o computador após a execução do assistente. Por isso, clique em **reiniciar o servidor de destino automaticamente, se necessário**. A caixa de diálogo **Assistente de adição de funções e recursos** é aberta. Clique em **Sim**.  
  
    ![Assistente para Adicionar Funções e Recursos](../../../media/Install-the-Network-Controller-server-role-using-Server-Manager/netc_install_11.jpg)  
  
10. Em **Confirmar seleções de instalação**, clique em **Instalar**.  
  
11. A função de servidor do controlador de rede é instalada no servidor de destino e, em seguida, o servidor é reiniciado.  
  
12. Depois que o computador for reiniciado, faça logon no computador e verifique a instalação do controlador de rede exibindo Gerenciador do Servidor.  
  
    ![Gerenciador do Servidor](../../../media/Install-the-Network-Controller-server-role-using-Server-Manager/nc_013.jpg)  
  
## <a name="see-also"></a>Consulte também  
[Controlador de rede](Network-Controller.md)  
  


