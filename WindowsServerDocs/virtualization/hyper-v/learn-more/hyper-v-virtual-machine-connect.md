---
title: Conexão de máquina Virtual Hyper-V
description: Descreve a Conexão de máquina Virtual, que fornece acesso remoto a uma máquina virtual. Inclui detalhes sobre como realizar tarefas comuns, como enviar Ctrl-Alt-Delete para a máquina virtual.
ms.custom: na
ms.prod: windows-server-threshold
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
ms.openlocfilehash: a3c0fd18ded0621c550546a2f0108b573cc67767
ms.sourcegitcommit: 8ba2c4de3bafa487a46c13c40e4a488bf95b6c33
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 05/25/2019
ms.locfileid: "66222509"
---
# <a name="hyper-v-virtual-machine-connection"></a>Conexão de máquina Virtual Hyper-V

>Aplica-se a: Windows Server 2016, Windows 10, Windows 8.1, Windows Server 2012 R2, Windows Server 2012, Windows 8

Conexão de máquina virtual \(VMConnect\) é uma ferramenta que você pode usar para se conectar a uma máquina virtual para que você possa instalar ou interagir com o sistema operacional convidado em uma máquina virtual. Algumas das tarefas que você pode executar usando o VMConnect incluem o seguinte:  
  
-   Iniciar e desligar uma máquina virtual  
  
-   Conectar-se a uma imagem de DVD \(arquivo. ISO\) ou uma unidade flash USB  
  
-   Criar um ponto de verificação  
  
-   modificar as configurações de uma máquina virtual  
    
## <a name="tips-for-using-vmconnect"></a>Dicas para usar o VMConnect  
Você pode achar as seguintes informações úteis para usar o VMConnect:  
  
|Para fazer isso...|Fazer isso...|  
|---------------|------------|  
|Enviar cliques do mouse ou entrada do teclado para a máquina virtual|Clique em qualquer lugar na janela da máquina virtual. O ponteiro do mouse pode aparecer como um pequeno ponto ao se conectar a uma máquina virtual em execução.|  
|Retornar os cliques do mouse ou teclado de entrada para o computador físico|Pressione CTRL\+ALT\+esquerda da seta e em seguida, mova o ponteiro do mouse fora da janela da máquina virtual. Essa combinação de teclas de liberação do mouse pode ser alterada no Hyper\-configurações de V no Hyper\-Manager V.|  
|Enviar CTRL\+ALT\+combinação de teclas DELETE em uma máquina virtual|Selecione **ação** > **Ctrl\+Alt\+excluir** ou usar a combinação de teclas CTRL\+ALT\+final.|  
|Alternar de um modo de janela para uma completa\-modo de tela|Selecione **modo de exibição** > **modo de tela inteira**. Para alternar para o modo de janela, pressione CTRL\+ALT\+INTERROMPER.|  
|Criar um ponto de verificação para capturar o estado atual da máquina para solução de problemas|Selecione **ação** > **ponto de verificação** ou usar a combinação de teclas CTRL\+N.|  
|Alterar as configurações da máquina virtual|Selecione **arquivo** > **configurações**.|  
|Conectar-se a uma imagem de DVD \(arquivo. ISO\) ou em um disquete virtual \(. vfd arquivo\)|Selecione **mídia**.<br /><br />Disquetes virtuais não têm suporte para máquinas virtuais de geração 2. Para obter mais informações, consulte [devo criar uma máquina virtual de geração 1 ou 2 no Hyper-V?](../plan/Should-I-create-a-generation-1-or-2-virtual-machine-in-Hyper-V.md).|  
|Usar recursos de local de um host Hyper\-V máquina de virtual como uma unidade flash USB|Ativar o modo de sessão avançado no host Hyper-V, use o VMConnect para se conectar à máquina virtual e antes de se conectar, escolha o recurso local que você deseja usar. Para obter as etapas específicas, consulte [usar os recursos locais Hyper\-máquina virtual V com VMConnect](Use-local-resources-on-Hyper-V-virtual-machine-with-VMConnect.md).|  
|Alteração salva as configurações do VMConnect para uma máquina virtual|Execute o seguinte comando no prompt de comando ou no Windows PowerShell:<br /><br />`VMConnect.exe <ServerName> <VMName> \/edit`|  
|Impedir que um usuário do VMConnect assumindo a outra sessão do usuário VMConnect|[Ativar o modo de sessão avançado no host Hyper-V](Use-local-resources-on-Hyper-V-virtual-machine-with-VMConnect.md#turn-on-enhanced-session-mode-on-a-hyper-v-host).<br /><br />Não que aprimoram a ativado de modo de sessão pode representar um risco de segurança e privacidade. Se um usuário está conectado e conectado a uma máquina virtual por meio do VMConnect e outro usuário autorizado se conecta à mesma máquina virtual, a sessão será levada a tecla TAB o segundo usuário e o primeiro usuário perderá a sessão. O segundo usuário será capaz de exibir a área de trabalho, documentos e aplicativos o usuário primeiro.|
|Gerenciar serviços de integração ou componentes que permitem que a VM se comunique com o host do Hyper-V| Em hosts do Hyper-V que executam o Windows 10 ou Windows Server 2016, você não pode gerenciar serviços de integração com o VMConnect. Consulte estes tópicos: <br />- [Ativar / desativar os serviços de integração do host do Hyper-V](https://msdn.microsoft.com/virtualization/hyperv_on_windows/user_guide/managing_ics) <br />- [Ativar / desativar os serviços de integração de uma máquina virtual do Windows](https://msdn.microsoft.com/virtualization/hyperv_on_windows/user_guide/managing_ics#manage-integration-services-from-guest-os-windows)<br />- [Ativar / desativar os serviços de integração de uma máquina virtual Linux](https://msdn.microsoft.com/virtualization/hyperv_on_windows/user_guide/managing_ics#manage-integration-services-from-guest-os-linux) <br />- [Manter os serviços de integração atualizados para a máquina virtual](https://msdn.microsoft.com/virtualization/hyperv_on_windows/user_guide/managing_ics#integration-service-maintenance)  <br />Para hosts que executam o Windows Server 2012 ou Windows Server 2012 R2, consulte [Integration Services](https://technet.microsoft.com/library/dn798297(v=ws.11).aspx).|
|Redimensionar a janela VMConnect|Você pode alterar o tamanho da janela VMConnect para máquinas virtuais de geração 2 que executam um sistema de operacional Windows. Para fazer isso, você precisa ativar o modo de sessão avançado no host Hyper-V. Para obter mais informações, consulte [ativar o modo de sessão avançado no host Hyper-V](Use-local-resources-on-Hyper-V-virtual-machine-with-VMConnect.md#turn-on-enhanced-session-mode-on-a-hyper-v-host). Para máquinas virtuais que executam o Ubuntu, consulte [alteração de resolução de tela de Ubuntu em uma VM do Hyper-V](https://blogs.msdn.microsoft.com/virtual_pc_guy/2014/09/19/changing-ubuntu-screen-resolution-in-a-hyper-v-vm/).|


## <a name="keyboard-shortcuts"></a>Atalhos de teclado  
Por padrão, os cliques de mouse e entrada de teclado são enviados para a máquina virtual. Portanto, talvez seja necessário pressionar CTRL + ALT + esquerda seta antes de usar as teclas de atalho a seguir. 

|Combinação de teclas|Descrição|  
|-------------------|---------------|  
|CTRL\+ALT\+seta para a esquerda|Liberação do mouse|  
|CTRL\+ALT\+END|Equivalentes de CTRL\+ALT\+excluir na máquina virtual|  
|CTRL\+ALT\+BREAK|Mudar de completo\-modo de tela de volta para o modo de janela|  
|CTRL\+O|Abre as configurações para a máquina virtual|  
|CTRL\+S|Inicia a máquina virtual|  
|CTRL\+N|Criar um ponto de verificação|  
|CTRL\+E|Reverter para um ponto de verificação|  
|CTRL\+C|Fazer uma captura de tela|  

## <a name="see-also"></a>Consulte também  
-   [Usar os recursos locais na máquina virtual de Hyper-V com VMConnect](Use-local-resources-on-Hyper-V-virtual-machine-with-VMConnect.md)  
-   [Hyper-V no Windows Server 2016](../Hyper-V-on-Windows-Server.md)  
-   [Hyper-V no Windows 10](https://msdn.microsoft.com/virtualization/hyperv_on_windows/windows_welcome)  
  
  
