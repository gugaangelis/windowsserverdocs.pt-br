---
title: Conexão com Máquina Virtual do Hyper-V
description: Descreve a Conexão de Máquina Virtual, que fornece acesso remoto a uma máquina virtual. Inclui detalhes sobre como executar tarefas comuns, como enviar Ctrl-Alt-Delete para a máquina virtual.
ms.topic: article
ms.assetid: deae35b9-7647-42b8-b6bf-45645a44c9c4
ms.author: benarm
author: BenjaminArmstrong
ms.date: 10/04/2016
ms.openlocfilehash: 80e87fcfa38f441491985ba7bb58b25c7e4cc165
ms.sourcegitcommit: dd1fbb5d7e71ba8cd1b5bfaf38e3123bca115572
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/17/2020
ms.locfileid: "90745961"
---
# <a name="hyper-v-virtual-machine-connection"></a>Conexão com Máquina Virtual do Hyper-V

>Aplica-se a: Windows Server 2016, Windows 10, Windows 8.1, Windows Server 2012 R2, Windows Server 2012, Windows 8

A Conexão de Máquina Virtual \(VMConnect\) é uma ferramenta usada para se conectar a uma máquina virtual para que você possa instalar ou interagir com o sistema operacional convidado em uma máquina virtual. Algumas das tarefas que você pode executar usando a VMConnect incluem:

-   Iniciar e desativar uma máquina virtual

-   Conectar-se a uma unidade flash USB ou a um \(arquivo .iso\) de imagem de DVD

-   Criar um ponto de verificação

-   modificar as configurações de uma máquina virtual

## <a name="tips-for-using-vmconnect"></a>Dicas para usar o VMConnect
Você pode encontrar as seguintes informações úteis para usar o VMConnect:

|Para fazer isto…|Faça isto…|
|---------------|------------|
|Enviar cliques do mouse ou entrada de teclado para a máquina virtual|Clique em qualquer lugar na janela da máquina virtual. O ponteiro do mouse pode aparecer como um ponto pequeno quando você se conectar a uma máquina virtual em execução.|
|Retornar cliques do mouse ou entrada de teclado para o computador físico|Pressione CTRL\+ALT\+seta para a ESQUERDA e, em seguida, mova o ponteiro do mouse para fora da janela da máquina virtual. Essa combinação de teclas de liberação do mouse pode ser alterada nas configurações do Hyper\-V no Gerenciador do Hyper\-V.|
|Enviar CTRL\+ALT\+exclui a combinação de teclas para uma máquina virtual|Selecione **Ação** > **Ctrl\+Alt\+Delete** ou use a combinação de teclas CTRL\+ALT\+END.|
|Alternar de um modo de janela para um modo de tela\-inteira|Selecione **Exibir** > **Modo de Tela Inteira**. Para voltar para o modo de janela, pressione CTRL\+ALT\+BREAK.|
|Criar um ponto de verificação para capturar o estado atual do computador para solução de problemas|Selecione **Ação** > **Ponto de Verificação** ou use a combinação de teclas CTRL\+N.|
|Alterar as configurações da máquina virtual|Selecione **Arquivo** > **Configurações**.|
|Conecte-se a um \(arquivo .iso\) de imagem de DVD ou a \(arquivo .vfd\) de um disquete virtual|Selecione **Mídia**.<p>Não há suporte para disquetes virtuais para máquinas virtuais de geração 2. Para obter mais informações, confira [Devo criar uma máquina virtual de geração 1 ou 2 no Hyper-V?](../plan/Should-I-create-a-generation-1-or-2-virtual-machine-in-Hyper-V.md).|
|Usar os recursos locais de um host na máquina virtual do Hyper\-V, como uma unidade flash USB|Ative o modo de sessão avançado no host Hyper-V, use o VMConnect para se conectar à máquina virtual e, antes de se conectar, escolha o recurso local que deseja usar. Para obter as etapas específicas, confira [Usar recursos locais na máquina virtual do Hyper\-V com o VMConnect](Use-local-resources-on-Hyper-V-virtual-machine-with-VMConnect.md).|
|Alterar configurações de VMConnect salvas para uma máquina virtual|Execute o seguinte comando do Windows PowerShell ou o prompt de comando:<p>`VMConnect.exe <ServerName> <VMName> /edit`|
|Impedir que um usuário do VMConnect assuma o controle da sessão VMConnect de outro usuário|[Ativar o modo de sessão avançada no host do Hyper-V](Use-local-resources-on-Hyper-V-virtual-machine-with-VMConnect.md#turn-on-enhanced-session-mode-on-a-hyper-v-host).<p>Não ter o modo de sessão avançado ativado pode representar um risco de segurança e privacidade. Se um usuário estiver conectado e tiver feito logon em uma máquina virtual por meio do VMConnect e outro usuário autorizado conectar-se à mesma máquina virtual, a sessão será usada pelo segundo usuário e o primeiro usuário perderá a sessão. O segundo usuário poderá ver a área de trabalho, os documentos e os aplicativos do primeiro usuário.|
|Gerenciar serviços de integração ou componentes que permitem que a VM se comunique com o host Hyper-V| Em hosts Hyper-V que executam o Windows 10 ou o Windows Server 2016, você não pode gerenciar os serviços de integração com o VMConnect. Confira estes tópicos: <br />- [Ligar/desligar Integration Services do host do Hyper-V](../manage/manage-hyper-v-integration-services.md) <br />- [Ligar/desligar Integration Services de uma máquina virtual do Windows](../manage/manage-hyper-v-integration-services.md#start-and-stop-an-integration-service-from-a-windows-guest)<br />- [Ligar/desligar Integration Services de uma máquina virtual do Linux](../manage/manage-hyper-v-integration-services.md#start-and-stop-an-integration-service-from-a-linux-guest) <br />- [Manter os serviços de integração atualizados para a máquina virtual](../manage/manage-hyper-v-integration-services.md#keep-integration-services-up-to-date)  <br />Para hosts que executam o Windows Server 2012 ou o Windows Server 2012 R2, confira [Integration Services](/previous-versions/windows/it-pro/windows-server-2012-R2-and-2012/dn798297(v=ws.11)).|
|Redimensionar a janela do VMConnect|Você pode alterar o tamanho da janela do VMConnect para máquinas virtuais de geração 2 que executam um sistema operacional Windows. Para fazer isso, talvez seja necessário ativar o modo de sessão avançada no host Hyper-V. Para obter mais informações, confira [Ativar o modo de sessão avançada no host do Hyper-V](Use-local-resources-on-Hyper-V-virtual-machine-with-VMConnect.md#turn-on-enhanced-session-mode-on-a-hyper-v-host). Para máquinas virtuais que executam o Ubuntu, confira [Como alterar a resolução de tela do Ubuntu em uma VM do Hyper-V](/archive/blogs/virtual_pc_guy/changing-ubuntu-screen-resolution-in-a-hyper-v-vm).|


## <a name="keyboard-shortcuts"></a>Atalhos de teclado
Por padrão, os cliques com o mouse e a digitação com o teclado são enviados para a máquina virtual. Portanto, talvez seja necessário pressionar CTRL + ALT + seta para a ESQUERDA antes de usar as teclas de atalho a seguir.

|Combinação de teclas|Descrição|
|-------------------|---------------|
|CTRL\+ALT\+seta para a ESQUERDA|Liberação do mouse|
|CTRL\+ALT\+END|Equivalente a CTRL\+ALT\+DELETE na máquina virtual|
|CTRL\+ALT\+BREAK|Alternar do modo de tela\-inteira de volta para o modo de janela|
|CTRL\+O|Abre as configurações para a máquina virtual|
|CTRL\+S|Inicia a máquina virtual|
|CTRL\+N|Criar um ponto de verificação|
|CTRL\+E|Reverter para um ponto de verificação|
|CTRL\+C|Realizar uma captura de tela|

## <a name="see-also"></a>Consulte Também
-   [Usar recursos locais na máquina virtual do Hyper-V com VMConnect](Use-local-resources-on-Hyper-V-virtual-machine-with-VMConnect.md)
-   [Hyper-V no Windows Server 2016](../Hyper-V-on-Windows-Server.md)
-   [Hyper-V no Windows 10](/virtualization/hyper-v-on-windows/)