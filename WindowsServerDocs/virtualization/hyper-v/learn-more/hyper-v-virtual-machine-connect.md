---
title: Conexão de máquina virtual do Hyper-V
description: Descreve a conexão de máquina virtual, que fornece acesso remoto a uma máquina virtual. Inclui detalhes sobre como executar tarefas comuns, como enviar Ctrl-Alt-Delete para a máquina virtual.
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
manager: dongill
ms.technology: compute-hyper-v
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: deae35b9-7647-42b8-b6bf-45645a44c9c4
author: KBDAzure
ms.author: kathydav
ms.date: 10/04/2016
ms.openlocfilehash: fba83d22d9e5d9f31a5809781aa04943cc4cd3af
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71364146"
---
# <a name="hyper-v-virtual-machine-connection"></a>Conexão de máquina virtual do Hyper-V

>Aplica-se a: Windows Server 2016, Windows 10, Windows 8.1, Windows Server 2012 R2, Windows Server 2012, Windows 8

A VMConnect \(\) de conexão de máquina virtual é uma ferramenta que você usa para se conectar a uma máquina virtual para que você possa instalar ou interagir com o sistema operacional convidado em uma máquina virtual. Algumas das tarefas que você pode executar usando VMConnect incluem o seguinte:  
  
-   Iniciar e desligar uma máquina virtual  
  
-   Conectar-se a um \(arquivo\) . ISO da imagem de DVD ou a uma unidade flash USB  
  
-   Criar um ponto de verificação  
  
-   modificar as configurações de uma máquina virtual  
    
## <a name="tips-for-using-vmconnect"></a>Dicas para usar o VMConnect  
Você pode encontrar as seguintes informações úteis para usar o VMConnect:  
  
|Para fazer isso...|Faça isso...|  
|---------------|------------|  
|Enviar cliques do mouse ou entrada de teclado para a máquina virtual|Clique em qualquer lugar na janela da máquina virtual. O ponteiro do mouse pode aparecer como um ponto pequeno quando você se conectar a uma máquina virtual em execução.|  
|Retornar cliques do mouse ou entrada de teclado para o computador físico|Pressione CTRL\+Alt\+seta para a esquerda e mova o ponteiro do mouse para fora da janela da máquina virtual. Essa combinação de teclas de liberação do mouse pode ser alterada\-nas configurações do Hyper\--v no Gerenciador do Hyper v.|  
|Enviar combinação\+de\+teclas Ctrl Alt Delete para uma máquina virtual|Selecione **ação** > \+\+**CtrlALTDELETE\+ou use a combinação de teclas Ctrl Alt End.\+**|  
|Alternar de um modo de janela para um\-modo de tela inteira|Selecione **Exibir** > **modo de tela inteira**. Para voltar para o modo de janela, pressione\+CTRL\+Alt Break.|  
|Criar um ponto de verificação para capturar o estado atual do computador para solução de problemas|Selecione > o ponto de**verificação** de ação ou use\+a combinação de teclas Ctrl N.|  
|Alterar as configurações da máquina virtual|Selecione**configurações**de **arquivo** > .|  
|Conecte-se a um \(arquivo\) . ISO de imagem de DVD \(ou a um arquivo de disquete virtual. VFD\)|Selecione **mídia**.<br /><br />Não há suporte para disquetes virtuais para máquinas virtuais de geração 2. Para obter mais informações, consulte [devo criar uma máquina virtual de geração 1 ou 2 no Hyper-V?](../plan/Should-I-create-a-generation-1-or-2-virtual-machine-in-Hyper-V.md).|  
|Usar os recursos locais de um host em\-uma máquina virtual Hyper-V como uma unidade flash USB|Ative o modo de sessão avançada no host Hyper-V, use VMConnect para se conectar à máquina virtual e, antes de se conectar, escolha o recurso local que você deseja usar. Para obter as etapas específicas, consulte [usar recursos locais na\-máquina virtual Hyper V com VMConnect](Use-local-resources-on-Hyper-V-virtual-machine-with-VMConnect.md).|  
|Alterar configurações de VMConnect salvas para uma máquina virtual|Execute o seguinte comando no Windows PowerShell ou no prompt de comando:<br /><br />`VMConnect.exe <ServerName> <VMName> \/edit`|  
|Impedir que um usuário VMConnect assuma a sessão VMConnect de outro usuário|[Ative o modo de sessão avançado no host Hyper-V](Use-local-resources-on-Hyper-V-virtual-machine-with-VMConnect.md#turn-on-enhanced-session-mode-on-a-hyper-v-host).<br /><br />Não ter o modo de sessão avançado ativado pode representar um risco de segurança e privacidade. Se um usuário estiver conectado e conectado a uma máquina virtual por meio do VMConnect e outro usuário autorizado se conectar à mesma máquina virtual, a sessão será usada pelo segundo usuário e o primeiro usuário perderá a sessão. O segundo usuário poderá exibir a área de trabalho, os documentos e os aplicativos do primeiro usuário.|
|Gerenciar serviços de integração ou componentes que permitem que a VM se comunique com o host Hyper-V| Em hosts Hyper-V que executam o Windows 10 ou o Windows Server 2016, você não pode gerenciar os serviços de integração com o VMConnect. Consulte estes tópicos: <br />- [Ativar/desativar o Integration Services do host do Hyper-V](https://msdn.microsoft.com/virtualization/hyperv_on_windows/user_guide/managing_ics) <br />- [Ativar/desativar o Integration Services de uma máquina virtual do Windows](https://msdn.microsoft.com/virtualization/hyperv_on_windows/user_guide/managing_ics#manage-integration-services-from-guest-os-windows)<br />- [Ativar/desativar o Integration Services de uma máquina virtual do Linux](https://msdn.microsoft.com/virtualization/hyperv_on_windows/user_guide/managing_ics#manage-integration-services-from-guest-os-linux) <br />- [Manter os serviços de integração atualizados para a máquina virtual](https://msdn.microsoft.com/virtualization/hyperv_on_windows/user_guide/managing_ics#integration-service-maintenance)  <br />Para hosts que executam o Windows Server 2012 ou o Windows Server 2012 R2, consulte [Integration Services](https://technet.microsoft.com/library/dn798297(v=ws.11).aspx).|
|Redimensionar a janela VMConnect|Você pode alterar o tamanho da janela VMConnect para máquinas virtuais de geração 2 que executam um sistema operacional Windows. Para fazer isso, talvez seja necessário ativar o modo de sessão avançado no host Hyper-V. Para obter mais informações, consulte [ativar o modo de sessão avançada no host Hyper-V](Use-local-resources-on-Hyper-V-virtual-machine-with-VMConnect.md#turn-on-enhanced-session-mode-on-a-hyper-v-host). Para máquinas virtuais que executam o Ubuntu, consulte [alterando a resolução da tela do Ubuntu em uma VM do Hyper-V](https://blogs.msdn.microsoft.com/virtual_pc_guy/2014/09/19/changing-ubuntu-screen-resolution-in-a-hyper-v-vm/).|


## <a name="keyboard-shortcuts"></a>Atalhos de teclado  
Por padrão, os cliques de entrada e de mouse do teclado são enviados para a máquina virtual. Portanto, talvez seja necessário pressionar CTRL + ALT + seta para a esquerda antes de usar as seguintes teclas de atalho. 

|Combinação de teclas|Descrição|  
|-------------------|---------------|  
|CTRL\+ALT\+seta para a esquerda|Versão do mouse|  
|CTRL\+ALT\+END|Equivalente a CTRL\+Alt\+Delete na máquina virtual|  
|CTRL\+ALT\+BREAK|Alternar do modo\-de tela inteira de volta para o modo de janela|  
|CTRL\+O|Abre as configurações para a máquina virtual|  
|CTRL\+S|Inicia a máquina virtual|  
|CTRL\+N|Criar um ponto de verificação|  
|CTRL\+E|Reverter para um ponto de verificação|  
|CTRL\+C|Fazer uma captura de tela|  

## <a name="see-also"></a>Consulte também  
-   [Usar recursos locais na máquina virtual do Hyper-V com VMConnect](Use-local-resources-on-Hyper-V-virtual-machine-with-VMConnect.md)  
-   [Hyper-V no Windows Server 2016](../Hyper-V-on-Windows-Server.md)  
-   [Hyper-V no Windows 10](https://msdn.microsoft.com/virtualization/hyperv_on_windows/windows_welcome)  
  
  
