---
title: Criar um comutador virtual para máquinas virtuais Hyper-V
description: Fornece instruções sobre como criar um comutador virtual usando o Gerenciador do Hyper-V ou o Windows PowerShell
ms.prod: windows-server-threshold
ms.service: na
manager: dongill
ms.technology: compute-hyper-v
ms.tgt_pltfrm: na
ms.topic: get-started-article
ms.assetid: fdc8063c-47ce-4448-b445-d7ff9894dc17
author: KBDAzure
ms.author: kathydav
ms.date: 10/04/2016
ms.openlocfilehash: e7c43a1b9173d347a3b6d6e1f8bd9127c62bd081
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59880217"
---
# <a name="create-a-virtual-switch-for-hyper-v-virtual-machines"></a>Criar um comutador virtual para máquinas virtuais Hyper-V

>Aplica-se a: Windows 10, Windows Server 2016, Microsoft Hyper-V Server 2016, Windows Server 2019, Microsoft Hyper-V Server 2019
  
Um comutador virtual permite que as máquinas virtuais criadas em hosts do Hyper-V para se comunicar com outros computadores. Quando você instala a função Hyper-V no Windows Server, você pode criar um comutador virtual. Para criar comutadores virtuais adicionais, use o Gerenciador do Hyper-V ou o Windows PowerShell. Para saber mais sobre comutadores virtuais, consulte [comutador Virtual Hyper-V](../../hyper-v-virtual-switch/Hyper-V-Virtual-Switch.md).  
  
Rede de máquina virtual pode ser um assunto complexo. E há vários recursos novos do comutador virtual que você talvez queira usar, como [SET Switch Embedded Teaming ()](../../hyper-v-virtual-switch/RDMA-and-Switch-Embedded-Teaming.md#bkmk_sswitchembedded). Mas a rede básica é muito fácil de fazer. Este tópico aborda apenas o suficiente para que você possa criar em rede máquinas de virtuais no Hyper-V. Para saber mais sobre como você pode configurar sua infraestrutura de rede, examine os [rede](../../../networking/Networking.md) documentação.   
  
## <a name="BKMK_HyperVMan"></a>Criar um comutador virtual usando o Gerenciador do Hyper-V  
  
1.  Abra o Gerenciador do Hyper-V, selecione o nome do computador host do Hyper-V.  
  
2.  Selecione **ação** > **Gerenciador de comutador Virtual**.  
  
    ![Captura de tela que mostra a opção de menu Ação > Gerenciador de comutador Virtual](../media/Hyper-V-Action-VSwitchManager.png)  
  
3.  Escolha o tipo de comutador virtual que você deseja.  
  
    |Tipo de conexão|Descrição|  
    |-------------------|---------------|  
    |Externo|Fornece acesso de máquinas virtuais a uma rede física para se comunicar com servidores e clientes em uma rede externa. Permite que máquinas virtuais no mesmo servidor Hyper-V para se comunicar entre si.|  
    |Interna|Permite a comunicação entre máquinas virtuais no mesmo servidor Hyper-V e entre as máquinas virtuais e o sistema operacional de host de gerenciamento.|  
    |Private|Só permite a comunicação entre máquinas virtuais no mesmo servidor Hyper-V. Uma rede privada é isolada de todo o tráfego de rede externo no servidor Hyper-V. Esse tipo de rede é útil quando você deve criar um ambiente isolado de rede, como um domínio de teste isolada.|  
  
4.  Selecione **criar comutador Virtual**.  
  
5.  Adicione um nome para o comutador virtual.  
  
6.  Se você selecionar externo, escolha o adaptador de rede (NIC) que você deseja usar e quaisquer outras opções descritas na tabela a seguir.  
  
    ![Captura de tela que mostra as opções de rede externa](../media/Hyper-V-NewVSwitch-ExternalOptions.png)  
  
    |Nome da configuração|Descrição|  
    |----------------|---------------|  
    |Permitir que o sistema de operacional de gerenciamento compartilhe esse adaptador de rede|Selecione esta opção se você deseja permitir que o host do Hyper-V compartilhar o uso do comutador virtual e NIC ou NIC da equipe com a máquina virtual. Com esse recurso habilitado, o host pode usar qualquer uma das configurações que você configurar para o comutador virtual, como configurações de qualidade de serviço (QoS), as configurações de segurança ou outros recursos do comutador virtual Hyper-V.|  
    |Habilitar a virtualização de e/s de raiz única (SR-IOV)|Selecione esta opção somente se você quiser permitir o tráfego de máquina virtual para ignorar o comutador da máquina virtual e ir diretamente para a NIC física. Para obter mais informações, consulte [virtualização de e/s de raiz única](https://technet.microsoft.com/library/dn641211.aspx#Sec4) na referência de complementar do cartaz: Rede do Hyper-V.|  
  
7.  Se você quiser isolar o tráfego de rede do sistema de operacional do host de gerenciamento do Hyper-V ou outras máquinas virtuais que compartilham o mesmo comutador virtual, selecione **habilitar identificação de LAN virtual para o sistema operacional de gerenciamento**. Você pode alterar a ID de VLAN para qualquer número ou deixe o padrão. Isso é o número de identificação de LAN virtual que o sistema operacional de gerenciamento usará para todas as comunicações de rede através deste comutador virtual.  
  
    ![Captura de tela que mostra as opções de ID de VLAN](../media/Hyper-V-NewSwitch-VLAN.png)  
  
8.  Clique em **OK**.  
  
9. Clique em **Sim**.  
  
    ![Captura de tela que mostra a mensagem "As alterações pendentes podem atrapalhar a conectividade de rede"](../media/Hyper-V-NewVSwitch-DisruptNetwork.png)  
  
## <a name="BKMK_WPS"></a>Criar um comutador virtual usando o Windows PowerShell  
  
1.  Na área de trabalho do Windows, clique no botão Iniciar e digite qualquer parte do nome **Windows PowerShell**.  
  
2.  Windows PowerShell com o botão direito e selecione **executar como administrador**.  
  
3.  Encontrar adaptadores de rede existente executando o [Get-NetAdapter](https://technet.microsoft.com/library/jj130867.aspx) cmdlet. Anote o nome do adaptador de rede que você deseja usar para o comutador virtual.  
  
    ```  
    Get-NetAdapter  
    ```  
  
4.  Criar um comutador virtual usando o [New-VMSwitch](https://technet.microsoft.com/library/hh848455.aspx) cmdlet. Por exemplo, para criar um comutador virtual externo, chamado ExternalSwitch, usando o adaptador de rede ethernet e com **permitir que o sistema operacional de gerenciamento compartilhe esse adaptador de rede** ativado, execute o comando a seguir.  
  
    ```  
    New-VMSwitch -name ExternalSwitch  -NetAdapterName Ethernet -AllowManagementOS $true  
    ```  
  
    Para criar um comutador interno, execute o comando a seguir.  
  
    ```  
    New-VMSwitch -name InternalSwitch -SwitchType Internal  
    ```  
  
    Para criar um comutador privado, execute o comando a seguir.  
  
    ```  
    New-VMSwitch -name PrivateSwitch -SwitchType Private  
    ```  
  
Para scripts mais avançados do Windows PowerShell que abrangem recursos novos ou aprimorados de comutador virtual no Windows Server 2016, consulte [remotos acesso direto à memória e agrupamento incorporado alternar](../../hyper-v-virtual-switch/RDMA-and-Switch-Embedded-Teaming.md).  

  
## <a name="next-step"></a>Próximas etapas  
[Criar uma máquina virtual no Hyper-V](Create-a-virtual-machine-in-Hyper-V.md)  
  


