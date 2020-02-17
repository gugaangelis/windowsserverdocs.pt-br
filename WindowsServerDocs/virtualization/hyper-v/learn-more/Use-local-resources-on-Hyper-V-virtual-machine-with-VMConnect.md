---
title: Use local resources on Hyper-V virtual machine with VMConnect
description: Descreve os requisitos para usar os recursos locais com o VMConnect
ms.prod: windows-server
ms.service: na
manager: dongill
ms.technology: compute-hyper-v
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 18eface5-7518-4c6b-9282-93e2e3e87492
author: KBDAzure
ms.author: kathyDav
ms.date: 12/06/2016
ms.openlocfilehash: 70bf72ec2277679820d985c9f78f10a4ea6e04df
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71392892"
---
# <a name="use-local-resources-on-hyper-v-virtual-machine-with-vmconnect"></a>Use local resources on Hyper-V virtual machine with VMConnect

>Aplica-se a: Windows 10, Windows 8.1, Windows Server 2016, Windows Server 2012 R2

A Conexão de Máquina Virtual (VMConnect) permite que você use os recursos locais de um computador em uma máquina virtual, como uma unidade flash USB removível ou uma impressora. O modo de sessão avançado também permite redimensionar a janela VMConnect. Este artigo mostra como configurar o host e, em seguida, conceder à máquina virtual acesso a um recurso local.

O modo de sessão avançada e o tipo texto da área de transferência estão disponíveis somente para máquinas virtuais que executam sistemas operacionais Windows recentes. \(Confira [Requisitos para usar os recursos locais](#requirements-for-using-local-resources) abaixo.\) 

Para máquinas virtuais que executam o Ubuntu, confira [Como alterar a resolução de tela do Ubuntu em uma VM do Hyper-V](https://blogs.msdn.microsoft.com/virtual_pc_guy/2014/09/19/changing-ubuntu-screen-resolution-in-a-hyper-v-vm/). 
  
## <a name="turn-on-enhanced-session-mode-on-a-hyper-v-host"></a>Ativar o modo de sessão avançado em um host Hyper-V  
Se o host Hyper-V executa o Windows 10 ou o Windows 8.1, o modo de sessão avançado está ativado por padrão, assim, você pode ignorar esta etapa e ir para a próxima seção. Porém, se o host executar o Windows Server 2016 ou o Windows Server 2012 R2, faça isso primeiro. 
  
Ativar o modo de sessão avançada:

1.  Conecte-se ao computador que hospeda a máquina virtual.  
  
2.  No Gerenciador do Hyper-V, selecione o nome do computador do host.  
  
    ![Captura de tela que mostra um nome do computador host listado em Gerenciador do Hyper-V no painel esquerdo.](media/Hyper-V-HyperVManager-HostNameSelected.png)  
  
3.  Selecione **Configurações do Hyper-V**.  
  
    ![Captura de tela que mostra a opção Configurações do Hyper-V em Ações no painel direito.](media/HyperV-ActionsHyperVSettings.png)  
  
4.  Em **Servidor**, selecione **Política do modo de sessão avançado**.  
  
    ![Captura de tela que mostra a opção de Política de modo de sessão avançada na seção Segurança.](media/Hyper-V-Settings-ServerEnhancedSessionModePolicy.png)  
  
5.  Selecione a caixa de seleção **Permitir modo de sessão avançado** .  
  
    ![Captura de tela que mostra a caixa de seleção Permitir modo de sessão avançada para Política de modo de sessão avançada.](media/Hyper-V-Settings-EnhancedSessionModePolicyCheckBox.png)  
  
6.  Em **Usuário**, selecione **Modo de sessão avançado**.  
  
    ![Captura de tela que mostra a opção de Modo de sessão avançada na seção Usuário. ](media/Hyper-V-Settings-UserEnhancedSessionMode.png)  
  
7.  Selecione a caixa de seleção **Permitir modo de sessão avançado** .  
  
8.  Clique em **Ok**.  
  
## <a name="choose-a-local-resource"></a>Escolher um recurso local

Os recursos locais incluem impressoras, a área de transferência e uma unidade local no computador em que você está executando o VMConnect. Para mais detalhes, confira [Requisitos para usar os recursos locais](#requirements-for-using-local-resources) abaixo.  
  
Para escolher um recurso local:
  
1.  Abra o VMConnect.  
  
2.  Selecione a máquina virtual à qual deseja se conectar.  
  
3.  Clique em **Mostrar opções**.  
  
    ![Captura de tela que chama Mostrar opções na parte inferior esquerda da caixa de diálogo.](media/HyperV-VMConnect-DisplayConfig.png)  
  
4.  Selecione **Recursos locais**.  
  
    ![Captura de tela que chama a guia Recursos locais.](media/HyperV-VMConnect-DisplayConfig-LocalResources.png)  
  
5.  Clique em **Mais**.  
  
    ![Captura de tela que chama o botão Mais.](media/HyperV-VMConnect-DisplayConfig-LocalResourcesMore.png)  
  
6.  Selecione a unidade que deseja usar na máquina virtual e clique em **Ok**.  
  
    ![Captura de tela que mostra os recursos locais e as unidades que você pode selecionar.](media/HyperV-VMConnect-Settings-LocalResourcesDrives.png)  
  
7.  Selecione **Salvar minhas configurações para conexões futuras com esta máquina virtual**.  
  
    ![Captura de tela que chama a caixa de seleção para selecionar esta opção.](media/HyperV-VMConnect-SaveSettings.png)  
  
8.  Clique em **Conectar**.  
  
## <a name="edit-vmconnect-settings"></a>Editar as configurações do VMConnect

Você pode editar facilmente as configurações de conexão do VMConnect executando o seguinte comando no Windows PowerShell ou no prompt de comando:  
  
`VMConnect.exe <ServerName> <VMName> /edit`  
  
## <a name="requirements-for-using-local-resources"></a>Requisitos para usar os recursos locais

Para poder usar os recursos locais de um computador em uma máquina virtual:  
  
-   O host Hyper-V deve ter as configurações **Política do modo de sessão avançada** e **Modo de sessão avançada** ativadas.  
  
-   O computador no qual você usa o VMConnect deve executar o Windows 10, o Windows 8.1, o Windows Server 2016 ou o Windows Server 2012 R2.  
  
-   A máquina virtual deve ter Serviços de Área de Trabalho Remota habilitados e executar o Windows 10, o Windows 8.1, o Windows Server 2016 ou o Windows Server 2012 R2 como o sistema operacional convidado.  
  
Se o computador que executa o VMConnect e a máquina virtual cumprir os requisitos, você poderá usar qualquer um dos seguintes recursos locais se eles estiverem disponíveis:  
  
-   Configuração de vídeo  
  
-   Áudio
  
-   Impressoras  
  
-   Áreas de transferência para copiar e colar  
  
-   Cartões inteligentes  
  
-   Dispositivos USB  
  
-   Unidades  
  
-   Dispositivos plug and play com suporte  
  
## <a name="why-use-a-computers-local-resources"></a>Por que usar os recursos locais de um computador?
Você pode desejar usar os recursos locais de um computador para:  
  
-   Solucionar problemas de uma máquina virtual sem uma conexão de rede com a máquina virtual.  
  
-   Copiar e colar arquivos de e para a máquina virtual da mesma maneira que copia e cola usando uma RDP (Conexão de Área de Trabalho Remota).  
  
-   Entrar na máquina virtual usando um cartão inteligente.  
  
-   Imprimir de uma máquina virtual para uma impressora local.  
  
-   Testar e solucionar problemas de aplicativos de desenvolvedor que exigem USB e redirecionamento de som sem usar RDP.  
  
## <a name="see-also"></a>Consulte Também  
[Conectar-se a uma Máquina Virtual](https://technet.microsoft.com/library/cc742407.aspx)  
[Devo criar uma máquina virtual de geração 1 ou 2 no Hyper-V?](../plan/Should-I-create-a-generation-1-or-2-virtual-machine-in-Hyper-V.md)



