---
title: Use local resources on Hyper-V virtual machine with VMConnect
description: Descreve os requisitos para usar recursos locais com o VMConnect
ms.prod: windows-server-threshold
ms.service: na
manager: dongill
ms.technology: compute-hyper-v
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 18eface5-7518-4c6b-9282-93e2e3e87492
author: KBDAzure
ms.author: kathyDav
ms.date: 12/06/2016
ms.openlocfilehash: a7e465313c68ee793715aba045cc56a2ca5fd1de
ms.sourcegitcommit: 8ba2c4de3bafa487a46c13c40e4a488bf95b6c33
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 05/25/2019
ms.locfileid: "66222847"
---
# <a name="use-local-resources-on-hyper-v-virtual-machine-with-vmconnect"></a>Use local resources on Hyper-V virtual machine with VMConnect

>Aplica-se a: Windows 10, Windows 8.1, Windows Server 2016, Windows Server 2012 R2

Conexão de máquina virtual (VMConnect) permite o uso de recursos um computador local em uma máquina virtual, como um removível unidade flash USB ou uma impressora. Modo de sessão avançado também permite que você redimensione a janela VMConnect. Este artigo mostra como configurar o host e, em seguida, conceder o acesso de máquina virtual a um recurso local.

Modo de sessão avançado e digite o texto da área de transferência estão disponíveis apenas para máquinas virtuais que executam os sistemas operacionais Windows recentes. \(Ver [requisitos para usar recursos locais](#requirements-for-using-local-resources), abaixo.\) 

Para máquinas virtuais que executam o Ubuntu, consulte [alteração de resolução de tela de Ubuntu em uma VM do Hyper-V](https://blogs.msdn.microsoft.com/virtual_pc_guy/2014/09/19/changing-ubuntu-screen-resolution-in-a-hyper-v-vm/). 
  
## <a name="turn-on-enhanced-session-mode-on-a-hyper-v-host"></a>Ativar o modo de sessão avançado em um host Hyper-V  
Se o host do Hyper-V executa o Windows 10 ou Windows 8.1, modo de sessão Avançado está ativado por padrão, portanto, você pode ignorar essa etapa e vá para a próxima seção. Mas, se o host executa o Windows Server 2016 ou Windows Server 2012 R2, faça isso primeiro. 
  
Ative o modo de sessão avançado:

1.  Conecte-se ao computador que hospeda a máquina virtual.  
  
2.  No Gerenciador do Hyper-V, selecione o nome do computador do host.  
  
    ![Captura de tela que mostra um host do nome do computador listado no Gerenciador do Hyper-V no painel esquerdo.](media/Hyper-V-HyperVManager-HostNameSelected.png)  
  
3.  Selecione **Configurações do Hyper-V**.  
  
    ![Captura de tela que mostra a opção de configurações do Hyper-V em ações no painel direito.](media/HyperV-ActionsHyperVSettings.png)  
  
4.  Em **Servidor**, selecione **Política do modo de sessão avançado**.  
  
    ![Captura de tela que mostra a opção de política do modo de sessão Avançado na seção de segurança.](media/Hyper-V-Settings-ServerEnhancedSessionModePolicy.png)  
  
5.  Selecione a caixa de seleção **Permitir modo de sessão avançado** .  
  
    ![Captura de tela que mostra a permitir aprimorada a caixa de seleção de modo de sessão para a política do modo de sessão avançado.](media/Hyper-V-Settings-EnhancedSessionModePolicyCheckBox.png)  
  
6.  Em **Usuário**, selecione **Modo de sessão avançado**.  
  
    ![Captura de tela que mostra a opção de modo de sessão avançado sob a seção do usuário. ](media/Hyper-V-Settings-UserEnhancedSessionMode.png)  
  
7.  Selecione a caixa de seleção **Permitir modo de sessão avançado** .  
  
8.  Clique em **Ok**.  
  
## <a name="choose-a-local-resource"></a>Escolha um recurso local

Recursos locais incluem impressoras, a área de transferência e uma unidade local no computador em que você está executando o VMConnect. Para obter mais detalhes, consulte [requisitos para usar recursos locais](#requirements-for-using-local-resources), abaixo.  
  
Para escolher um recurso local:
  
1.  Abra o VMConnect.  
  
2.  Selecione a máquina virtual à qual deseja se conectar.  
  
3.  Clique em **Mostrar opções**.  
  
    ![Captura de tela que chama as opções de mostrar no canto inferior esquerdo da caixa de diálogo.](media/HyperV-VMConnect-DisplayConfig.png)  
  
4.  Selecione **Recursos locais**.  
  
    ![Captura de tela que chama a guia de recursos locais.](media/HyperV-VMConnect-DisplayConfig-LocalResources.png)  
  
5.  Clique em **Mais**.  
  
    ![Captura de tela que chama o botão mais.](media/HyperV-VMConnect-DisplayConfig-LocalResourcesMore.png)  
  
6.  Selecione a unidade que deseja usar na máquina virtual e clique em **Ok**.  
  
    ![Captura de tela que mostra as unidades que você pode selecionar e recursos locais.](media/HyperV-VMConnect-Settings-LocalResourcesDrives.png)  
  
7.  Selecione **Salvar minhas configurações para conexões futuras com esta máquina virtual**.  
  
    ![Captura de tela que chama a caixa de seleção para selecionar essa opção.](media/HyperV-VMConnect-SaveSettings.png)  
  
8.  Clique em **Conectar**.  
  
## <a name="edit-vmconnect-settings"></a>Editar as configurações do VMConnect

Você pode editar facilmente as configurações de conexão do VMConnect executando o seguinte comando no Windows PowerShell ou no prompt de comando:  
  
`VMConnect.exe <ServerName> <VMName> /edit`  
  
## <a name="requirements-for-using-local-resources"></a>Requisitos para usar recursos locais

Para poder usar os recursos de locais de um computador em uma máquina virtual:  
  
-   O host do Hyper-V deve ter **política do modo de sessão avançado** e **modo de sessão avançado** configurações ativadas.  
  
-   O computador em que você use o VMConnect deve executar o Windows 10, Windows 8.1, Windows Server 2016 ou Windows Server 2012 R2.  
  
-   A máquina virtual deve ter os serviços de área de trabalho remota habilitados e executar o Windows 10, Windows 8.1, Windows Server 2016 ou Windows Server 2012 R2 como o sistema operacional convidado.  
  
Se o computador que executa o VMConnect e a máquina virtual ambos atender aos requisitos, você pode usar qualquer um dos seguintes recursos locais se elas estão disponíveis:  
  
-   Configuração de vídeo  
  
-   Áudio
  
-   Impressoras  
  
-   Áreas de transferência para copiar e colar  
  
-   Cartões inteligentes  
  
-   Dispositivos USB  
  
-   Unidades  
  
-   Dispositivos plug and play com suporte  
  
## <a name="why-use-a-computers-local-resources"></a>Por que usar recursos um computador local?
Você pode desejar usar os recursos de um computador local para:  
  
-   Solucionar problemas de uma máquina virtual sem uma conexão de rede com a máquina virtual.  
  
-   Copiar e colar arquivos de e para a máquina virtual da mesma maneira que copia e cola usando uma RDP (Conexão de Área de Trabalho Remota).  
  
-   Entrar na máquina virtual usando um cartão inteligente.  
  
-   Imprimir a partir de uma máquina virtual para uma impressora local.  
  
-   Testar e solucionar problemas de aplicativos de desenvolvedor que exigem USB e redirecionamento de som sem usar RDP.  
  
## <a name="see-also"></a>Consulte também  
[Conectar-se a uma máquina Virtual](https://technet.microsoft.com/library/cc742407.aspx)  
[Deve criar uma máquina virtual de geração 1 ou 2 no Hyper-V?](../plan/Should-I-create-a-generation-1-or-2-virtual-machine-in-Hyper-V.md)



