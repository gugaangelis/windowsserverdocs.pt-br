---
title: Instalar a função de servidor de controlador de rede usando o Gerenciador do servidor
description: Este tópico fornece instruções sobre como instalar a função de servidor de controlador de rede usando o Gerenciador do servidor no Windows Server 2016.
manager: brianlic
ms.prod: windows-server-threshold
ms.technology: networking-sdn
ms.tgt_pltfrm: na
ms.topic: get-started-article
ms.assetid: 3a6e4352-ff62-4290-b8a4-5c83740070fc
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 15cb1ef3bad7038cc97784504807b44b4920def6
ms.sourcegitcommit: 19d9da87d87c9eefbca7a3443d2b1df486b0b010
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/28/2018
---
# <a name="install-the-network-controller-server-role-using-server-manager"></a>Instalar a função de servidor de controlador de rede usando o Gerenciador do servidor

>Aplica-se a: Windows Server (anual por canal), Windows Server 2016

Este tópico fornece instruções sobre como instalar a função de servidor de controlador de rede usando o Gerenciador do servidor.

>[!IMPORTANT]
>Não implante a função de servidor de controlador de rede em hosts físicos. Para implantar o controlador de rede, você deve instalar a função de servidor de controlador de rede em uma máquina virtual de Hyper-V \(VM\) que é instalada em um host do Hyper-V. Depois que você instalou o controlador de rede em VMs em três diferentes hosts de Hyper\-V, você deve habilitar os hosts Hyper\-V para Software de rede definidos \(SDN\) adicionando os hosts de controlador de rede usando o comando do Windows PowerShell **nova NetworkControllerServer**. Fazendo isso, você está ativando o balanceador de carga de Software SDN à função. Para obter mais informações, consulte [nova NetworkControllerServer](https://technet.microsoft.com/itpro/powershell/windows/network-controller/new-networkcontrollerserver).
  
Depois de instalar o controlador de rede, você deve usar comandos do Windows PowerShell de configurações adicionais do controlador de rede. Para obter mais informações, consulte [implantar o controlador de rede usando o Windows PowerShell](../../deploy/Deploy-Network-Controller-using-Windows-PowerShell.md).  
  
### <a name="to-install-network-controller"></a>Para instalar o controlador de rede  
  
1.  No Gerenciador do servidor, clique em **gerenciar**e clique em **adicionar funções e recursos**. Abre o Assistente de adição de funções e recursos. Clique em **próxima**.  
  
2.  Em **selecione tipo de instalação**, mantenha a configuração padrão e clique em **próxima**.  
  
3.  Em **Selecionar servidor de destino**, escolha o servidor onde deseja instalar o controlador de rede e, em seguida, clique em **próxima**.  
  
4.  Em **selecionar funções de servidor**, na **funções**, clique em **rede controlador**.  
  
    ![Função de servidor de controlador de rede](../../../media/Install-the-Network-Controller-server-role-using-Server-Manager/netc_install_07.jpg)  
  
5.  O **adicionar recursos que são necessários para o controlador de rede** caixa de diálogo é aberta. Clique em **adicionar recursos**.  
  
    ![Adicionar recursos para o controlador de rede](../../../media/Install-the-Network-Controller-server-role-using-Server-Manager/netc_install_06.jpg)  
  
6.  Em **selecionar funções de servidor**, clique em **próxima**.  
  
    ![Clique em Avançar](../../../media/Install-the-Network-Controller-server-role-using-Server-Manager/netc_install_07.jpg)  
  
7.  Em **selecionar recursos**, clique em **próxima**.  
  
8.  Em **rede controlador** clique **próxima**.  
  
9. Em **confirmar seleções de instalação**, revise suas escolhas. Instalação do controlador de rede requer que você reinicie o computador depois que o assistente é executado. Por isso, clique em **reiniciar o servidor de destino automaticamente, se necessário**. O **assistente Adicionar funções e recursos** caixa de diálogo é aberta. Clique em **Sim**.  
  
    ![Adicionar Assistente funções e recursos](../../../media/Install-the-Network-Controller-server-role-using-Server-Manager/netc_install_11.jpg)  
  
10. Em **confirmar seleções de instalação**, clique em **instalar**.  
  
11. A função de servidor de rede controlador instala no servidor de destino e, em seguida, o servidor for reiniciado.  
  
12. Depois que o computador for reiniciado, faça logon no computador e verifique se a instalação do controlador de rede exibindo o Gerenciador do servidor.  
  
    ![Gerenciador do servidor](../../../media/Install-the-Network-Controller-server-role-using-Server-Manager/nc_013.jpg)  
  
## <a name="see-also"></a>Consulte também  
[Controlador de rede](Network-Controller.md)  
  


