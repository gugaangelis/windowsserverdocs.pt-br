---
title: Instalar a função de servidor do controlador de rede usando o Gerenciador do servidor
description: Este tópico fornece instruções sobre como instalar a função de servidor do controlador de rede usando o Gerenciador do servidor no Windows Server 2016.
manager: brianlic
ms.prod: windows-server-threshold
ms.technology: networking-sdn
ms.tgt_pltfrm: na
ms.topic: get-started-article
ms.assetid: 3a6e4352-ff62-4290-b8a4-5c83740070fc
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 699e2ca5c4ec33099d0ad948523b6f587ad118e4
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59859057"
---
# <a name="install-the-network-controller-server-role-using-server-manager"></a>Instalar a função de servidor do controlador de rede usando o Gerenciador do servidor

>Aplica-se a: Windows Server (canal semestral), Windows Server 2016

Este tópico fornece instruções sobre como instalar a função de servidor do controlador de rede usando o Gerenciador do servidor.

>[!IMPORTANT]
>Não implante a função de servidor do controlador de rede em hosts físicos. Para implantar o controlador de rede, você deve instalar a função de servidor do controlador de rede em uma máquina virtual de Hyper-V \(VM\) que é instalado em um host do Hyper-V. Depois que você instalou o controlador de rede em máquinas virtuais no Hyper diferentes três\-hosts V, você deve habilitar o Hyper\-hosts V para a rede definida pelo Software \(SDN\) adicionando hosts usando o controlador de rede o comando do Windows PowerShell **New-NetworkControllerServer**. Ao fazer isso, você habilitará o balanceador de carga de Software SDN à função. Para obter mais informações, consulte [New-NetworkControllerServer](https://technet.microsoft.com/itpro/powershell/windows/network-controller/new-networkcontrollerserver).
  
Depois de instalar o controlador de rede, você deve usar comandos do Windows PowerShell para a configuração do controlador de rede adicional. Para obter mais informações, consulte [implantar o controlador de rede usando o Windows PowerShell](../../deploy/Deploy-Network-Controller-using-Windows-PowerShell.md).  
  
### <a name="to-install-network-controller"></a>Para instalar o controlador de rede  
  
1.  No Gerenciador do Servidor, clique em **Gerenciar**e depois em **Adicionar Funções e Recursos**. Abre o assistente Adicionar funções e recursos. Clique em **Avançar**.  
  
2.  Na **Selecionar tipo de instalação**, mantenha a configuração padrão e clique em **próxima**.  
  
3.  Na **Selecionar servidor de destino**, escolha o servidor onde você deseja instalar o controlador de rede e, em seguida, clique em **próxima**.  
  
4.  Na **selecionar funções do servidor**, na **funções**, clique em **controlador de rede**.  
  
    ![Função de servidor de controlador de rede](../../../media/Install-the-Network-Controller-server-role-using-Server-Manager/netc_install_07.jpg)  
  
5.  O **adicionar recursos que são necessários para o controlador de rede** caixa de diálogo é aberta. Clique em **adicionar recursos**.  
  
    ![Adicionar recursos para o controlador de rede](../../../media/Install-the-Network-Controller-server-role-using-Server-Manager/netc_install_06.jpg)  
  
6.  Na **selecionar funções do servidor**, clique em **próxima**.  
  
    ![Clique em Avançar](../../../media/Install-the-Network-Controller-server-role-using-Server-Manager/netc_install_07.jpg)  
  
7.  Na **selecionar recursos**, clique em **próxima**.  
  
8.  Na **controlador de rede** clique em **próxima**.  
  
9. Na **confirmar seleções de instalação**, reveja suas escolhas. Instalação do controlador de rede requer que você reiniciar o computador depois que o assistente é executado. Por isso, clique em **reiniciar o servidor de destino automaticamente se necessário**. O **assistente Adicionar funções e recursos** caixa de diálogo é aberta. Clique em **Sim**.  
  
    ![Assistente de Adição de Funções e Recursos](../../../media/Install-the-Network-Controller-server-role-using-Server-Manager/netc_install_11.jpg)  
  
10. Em **Confirmar seleções de instalação**, clique em **Instalar**.  
  
11. A função de servidor do controlador de rede instalado no servidor de destino e, em seguida, o servidor for reiniciado.  
  
12. Depois que o computador for reiniciado, faça logon computador e verifique se a instalação do controlador de rede, exibindo o Gerenciador do servidor.  
  
    ![Gerenciador do Servidor](../../../media/Install-the-Network-Controller-server-role-using-Server-Manager/nc_013.jpg)  
  
## <a name="see-also"></a>Consulte também  
[Controlador de rede](Network-Controller.md)  
  


