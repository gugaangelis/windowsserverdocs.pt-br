---
title: Criar uma opção virtual para máquinas virtuais Hyper-V
description: Fornece instruções sobre como criar um comutador virtual usando o Gerenciador do Hyper-V ou o Windows PowerShell
manager: dongill
ms.topic: get-started-article
ms.assetid: fdc8063c-47ce-4448-b445-d7ff9894dc17
author: kbdazure
ms.author: kathydav
ms.date: 10/04/2016
ms.openlocfilehash: b9de14f9c3f66f6d8c8b532e8f4a83192b3e824b
ms.sourcegitcommit: 68444968565667f86ee0586ed4c43da4ab24aaed
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/07/2020
ms.locfileid: "87996628"
---
# <a name="create-a-virtual-switch-for-hyper-v-virtual-machines"></a>Criar uma opção virtual para máquinas virtuais Hyper-V

>Aplica-se a: Windows 10, Windows Server 2016, Microsoft Hyper-V Server 2016, Windows Server 2019, Microsoft Hyper-V Server 2019

Um comutador virtual permite que máquinas virtuais criadas em hosts Hyper-V se comuniquem com outros computadores. Você pode criar um comutador virtual ao instalar pela primeira vez a função Hyper-V no Windows Server. Para criar outros comutadores virtuais, use o Gerenciador do Hyper-V ou o Windows PowerShell. Para saber mais sobre os comutadores virtuais, consulte [comutador virtual do Hyper-V](../../hyper-v-virtual-switch/Hyper-V-Virtual-Switch.md).

A rede de máquinas virtuais pode ser um assunto complexo. E há vários novos recursos de comutador virtual que talvez você queira usar como o [conjunto de agrupamentos incorporados (Set)](../../hyper-v-virtual-switch/RDMA-and-Switch-Embedded-Teaming.md#switch-embedded-teaming-set). Mas a rede básica é razoavelmente fácil de fazer. Este tópico aborda apenas o suficiente para que você possa criar máquinas virtuais em rede no Hyper-V. Para saber mais sobre como você pode configurar sua infraestrutura de rede, examine a documentação de [rede](../../../networking/index.yml) .

## <a name="create-a-virtual-switch-by-using-hyper-v-manager"></a>Criar um comutador virtual usando o Gerenciador do Hyper-V

1.  Abra o Gerenciador do Hyper-V, selecione o nome do computador host do Hyper-V.

2.  Selecione **ação**  >  **Gerenciador de comutador virtual**.

    ![Captura de tela que mostra a ação de opção de menu > Gerenciador de comutador virtual](../media/Hyper-V-Action-VSwitchManager.png)

3.  Escolha o tipo de comutador virtual desejado.

    |Tipo de conexão|Descrição|
    |-------------------|---------------|
    |Externo|Dá acesso às máquinas virtuais a uma rede física para se comunicar com servidores e clientes em uma rede externa. Permite que as máquinas virtuais no mesmo servidor Hyper-V se comuniquem entre si.|
    |Interna|Permite a comunicação entre máquinas virtuais no mesmo servidor Hyper-V e entre as máquinas virtuais e o sistema operacional do host de gerenciamento.|
    |Privado|Permite apenas a comunicação entre as máquinas virtuais no mesmo servidor Hyper-V. Uma rede privada é isolada de todo o tráfego de rede externo no servidor Hyper-V. Esse tipo de rede é útil quando você deve criar um ambiente de rede isolado, como um domínio de teste isolado.|

4.  Selecione **criar comutador virtual**.

5.  Adicione um nome para o comutador virtual.

6.  Se você selecionar externo, escolha o adaptador de rede (NIC) que você deseja usar e quaisquer outras opções descritas na tabela a seguir.

    ![Captura de tela que mostra as opções de rede externa](../media/Hyper-V-NewVSwitch-ExternalOptions.png)

    |Nome da configuração|Descrição|
    |----------------|---------------|
    |Permitir que o sistema de operacional de gerenciamento compartilhe esse adaptador de rede|Selecione esta opção se desejar permitir que o host Hyper-V Compartilhe o uso do comutador virtual e NIC ou equipe NIC com a máquina virtual. Com isso habilitado, o host pode usar qualquer uma das configurações definidas para o comutador virtual, como configurações de QoS (qualidade de serviço), configurações de segurança ou outros recursos do comutador virtual Hyper-V.|
    |Habilitar SR-IOV (virtualização de e/s de raiz única)|Selecione esta opção somente se você quiser permitir que o tráfego da máquina virtual ignore o comutador da máquina virtual e vá diretamente para a NIC física. Para obter mais informações, consulte [virtualização de e/s de raiz única](/previous-versions/windows/it-pro/windows-server-2012-R2-and-2012/dn641211(v=ws.11)#Sec4) na referência do pôster Companion: rede Hyper-V.|

7.  Se você quiser isolar o tráfego de rede do sistema operacional do host Hyper-V de gerenciamento ou outras máquinas virtuais que compartilham o mesmo comutador virtual, selecione **habilitar identificação de LAN virtual para o sistema operacional de gerenciamento**. Você pode alterar a ID de VLAN para qualquer número ou deixar o padrão. Esse é o número de identificação de LAN virtual que o sistema operacional de gerenciamento usará para toda a comunicação de rede por meio desse comutador virtual.

    ![Captura de tela que mostra as opções de ID de VLAN](../media/Hyper-V-NewSwitch-VLAN.png)

8.  Clique em **OK**.

9. Clique em **Sim**.

    ![Captura de tela que mostra a mensagem "as alterações pendentes podem interromper a conectividade de rede"](../media/Hyper-V-NewVSwitch-DisruptNetwork.png)

## <a name="create-a-virtual-switch-by-using-windows-powershell"></a>Criar um comutador virtual usando o Windows PowerShell

1.  Na área de trabalho do Windows, clique no botão Iniciar e digite qualquer parte do nome **Windows PowerShell**.

2.  Clique com o botão direito do mouse em Windows PowerShell e selecione **Executar como administrador**.

3.  Encontre adaptadores de rede existentes executando o cmdlet [Get-netadapter](https://technet.microsoft.com/library/jj130867.aspx) . Anote o nome do adaptador de rede que você deseja usar para o comutador virtual.

    ```
    Get-NetAdapter
    ```

4.  Crie um comutador virtual usando o cmdlet [New-VMSwitch](/powershell/module/hyper-v/new-vmswitch?view=win10-ps) . Por exemplo, para criar um comutador virtual externo chamado ExternalSwitch, usando o adaptador de rede Ethernet e com **permitir que o sistema operacional de gerenciamento compartilhe esse adaptador de rede** ativado, execute o comando a seguir.

    ```
    New-VMSwitch -name ExternalSwitch  -NetAdapterName Ethernet -AllowManagementOS $true
    ```

    Para criar um comutador interno, execute o comando a seguir.

    ```
    New-VMSwitch -name InternalSwitch -SwitchType Internal
    ```

    Para criar um comutador particular, execute o comando a seguir.

    ```
    New-VMSwitch -name PrivateSwitch -SwitchType Private
    ```

Para obter scripts mais avançados do Windows PowerShell que abrangem recursos de comutador virtual aprimorados ou novos no Windows Server 2016, consulte [acesso remoto direto à memória e agrupamento integrado de comutador](../../hyper-v-virtual-switch/RDMA-and-Switch-Embedded-Teaming.md).


## <a name="next-step"></a>Próxima etapa
[Criar uma máquina virtual com o Hyper-V](Create-a-virtual-machine-in-Hyper-V.md)