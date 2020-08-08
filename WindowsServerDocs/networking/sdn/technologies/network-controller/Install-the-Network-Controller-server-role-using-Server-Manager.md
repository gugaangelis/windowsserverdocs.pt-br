---
title: Instalar a função de servidor do controlador de rede usando Gerenciador do Servidor
description: Este tópico fornece instruções sobre como instalar a função de servidor do controlador de rede usando Gerenciador do Servidor no Windows Server 2016.
manager: grcusanz
ms.topic: get-started-article
ms.assetid: 3a6e4352-ff62-4290-b8a4-5c83740070fc
ms.author: anpaul
author: AnirbanPaul
ms.openlocfilehash: c5736656fc61f303b360b33ca29de9319a299435
ms.sourcegitcommit: dfa48f77b751dbc34409aced628eb2f17c912f08
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/07/2020
ms.locfileid: "87969563"
---
# <a name="install-the-network-controller-server-role-using-server-manager"></a>Instalar a função de servidor do controlador de rede usando Gerenciador do Servidor

>Aplica-se a: Windows Server (Canal Semestral), Windows Server 2016

Este tópico fornece instruções sobre como instalar a função de servidor do controlador de rede usando Gerenciador do Servidor.

>[!IMPORTANT]
>Não implante a função de servidor do controlador de rede em hosts físicos. Para implantar o controlador de rede, você deve instalar a função de servidor do controlador de rede em uma VM de máquina virtual do Hyper-V \( \) instalada em um host do Hyper-v. Depois de ter instalado o controlador de rede em VMs em três \- hosts Hyper-v diferentes, você deve habilitar os \- hosts Hyper v para o Sdn de rede definido pelo software \( \) adicionando os hosts ao controlador de rede usando o comando **New-NetworkControllerServer**do Windows PowerShell. Ao fazer isso, você está permitindo que o software SDN Load Balancer funcione. Para obter mais informações, consulte [New-NetworkControllerServer](https://technet.microsoft.com/itpro/powershell/windows/network-controller/new-networkcontrollerserver).

Depois de instalar o controlador de rede, você deve usar comandos do Windows PowerShell para configuração adicional do controlador de rede. Para obter mais informações, consulte [implantar o controlador de rede usando o Windows PowerShell](../../deploy/Deploy-Network-Controller-using-Windows-PowerShell.md).

### <a name="to-install-network-controller"></a>Para instalar o controlador de rede

1.  No Gerenciador do Servidor, clique em **Gerenciar** e depois em **Adicionar Funções e Recursos**. O assistente Adicionar funções e recursos é aberto. Clique em **Próximo**.

2.  Em **Selecionar tipo de instalação**, mantenha a configuração padrão e clique em **Avançar**.

3.  Em **selecionar servidor de destino**, escolha o servidor no qual você deseja instalar o controlador de rede e clique em **Avançar**.

4.  Em **selecionar funções de servidor**, em **funções**, clique em **controlador de rede**.

    ![Função de servidor do controlador de rede](../../../media/Install-the-Network-Controller-server-role-using-Server-Manager/netc_install_07.jpg)

5.  A caixa de diálogo **Adicionar recursos necessários para o controlador de rede** é aberta. Clique em **Adicionar Recursos**.

    ![Adicionar recursos para o controlador de rede](../../../media/Install-the-Network-Controller-server-role-using-Server-Manager/netc_install_06.jpg)

6.  Em **selecionar funções de servidor**, clique em **Avançar**.

    ![Clique em Avançar](../../../media/Install-the-Network-Controller-server-role-using-Server-Manager/netc_install_07.jpg)

7.  Em **selecionar recursos**, clique em **Avançar**.

8.  Em **controlador de rede** , clique em **Avançar**.

9. Em **confirmar seleções de instalação**, examine suas escolhas. A instalação do controlador de rede exige que você reinicie o computador após a execução do assistente. Por isso, clique em **reiniciar o servidor de destino automaticamente, se necessário**. A caixa de diálogo **Assistente de adição de funções e recursos** é aberta. Clique em **Sim**.

    ![Assistente de Adição de Funções e Recursos](../../../media/Install-the-Network-Controller-server-role-using-Server-Manager/netc_install_11.jpg)

10. Em **Confirmar seleções de instalação**, clique em **Instalar**.

11. A função de servidor do controlador de rede é instalada no servidor de destino e, em seguida, o servidor é reiniciado.

12. Depois que o computador for reiniciado, faça logon no computador e verifique a instalação do controlador de rede exibindo Gerenciador do Servidor.

    ![Gerenciador do Servidor](../../../media/Install-the-Network-Controller-server-role-using-Server-Manager/nc_013.jpg)

## <a name="see-also"></a>Consulte Também
[Controlador de rede](Network-Controller.md)



