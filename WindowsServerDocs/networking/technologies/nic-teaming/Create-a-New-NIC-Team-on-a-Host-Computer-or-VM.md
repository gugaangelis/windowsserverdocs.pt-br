---
title: Criar uma nova equipe de NIC em um computador host ou VM
description: Neste tópico, você cria uma nova equipe NIC em um computador host ou em uma VM (máquina virtual) do Hyper-V que executa o Windows Server 2016.
manager: dougkim
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: networking-nict
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: a4caaa86-5799-4580-8775-03ee213784a3
ms.author: pashort
author: shortpatti
ms.date: 09/13/2018
ms.openlocfilehash: f1e7e27100d801d226adf79e078d8b16ddbcd308
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71401928"
---
# <a name="create-a-new-nic-team-on-a-host-computer-or-vm"></a>Criar uma nova equipe de NIC em um computador host ou VM

>Aplica-se a: Windows Server (Canal Semestral), Windows Server 2016

Neste tópico, você cria uma nova equipe NIC em um computador host ou em uma VM (máquina virtual) do Hyper-V que executa o Windows Server 2016.  

## <a name="network-configuration-requirements"></a>Requisitos de configuração de rede  
Antes de criar uma nova equipe NIC, você deve implantar um host Hyper-V com dois adaptadores de rede que se conectam a diferentes comutadores físicos. Você também deve configurar os adaptadores de rede com endereços IP que são do mesmo intervalo de endereços IP.  

O comutador físico, o comutador virtual do Hyper-V, a LAN (rede local) e os requisitos de agrupamento NIC para criar uma equipe NIC em uma VM são:  

-   O computador que executa o Hyper-V deve ter dois ou mais adaptadores de rede.  

-   Se conectar os adaptadores de rede a vários comutadores físicos, os comutadores físicos deverão estar na mesma sub-rede de camada 2.  

-   Você deve usar o Gerenciador do Hyper-V ou o Windows PowerShell para criar dois comutadores virtuais Hyper-V externos, cada um conectado a um adaptador de rede física diferente.  

-   A VM deve se conectar a ambos os comutadores virtuais externos que você criar.  

-   O agrupamento NIC, no Windows Server 2016, dá suporte a equipes com dois membros em VMs. Você pode criar equipes maiores, mas não há suporte.

-   Se você estiver configurando uma equipe NIC em uma VM, deverá selecionar um **modo** de agrupamento _independente de comutador_ e um **modo de balanceamento de carga** de hash de _endereço_. 

## <a name="step-1-configure-the-physical-and-virtual-network"></a>Etapa 1. Configurar a rede física e virtual  
Neste procedimento, você cria dois comutadores virtuais Hyper-V externos, conecta uma VM aos comutadores e, em seguida, configura as conexões de VM com os comutadores.  

### <a name="prerequisites"></a>Pré-requisitos

Você deve ter a associação em **Administradores**ou equivalente.  

### <a name="procedure"></a>Procedimento

1.  No host Hyper-V, abra o Gerenciador do Hyper-V e, em ações, clique em **Gerenciador de comutador virtual**.  

   ![Gerenciador de comutador virtual](../../media/Create-a-New-NIC-Team-in-a-VM/nict_hv.jpg)  

2.  No Gerenciador de comutador virtual, verifique se **externo** está selecionado e, em seguida, clique em **criar comutador virtual**.  

   ![Criar comutador virtual](../../media/Create-a-New-NIC-Team-in-a-VM/nict_hv_02.jpg)  

3.  Em Propriedades do comutador virtual, digite um **nome** para o comutador virtual e adicione **anotações** conforme necessário.  

4.  Em **tipo de conexão**, em **rede externa**, selecione o adaptador de rede física ao qual você deseja anexar o comutador virtual.  

5.  Configure as propriedades de comutador adicionais para sua implantação e clique em **OK**.  

6.  Crie um segundo comutador virtual externo repetindo as etapas anteriores. Conecte o segundo comutador externo a um adaptador de rede diferente.  

7.  No Gerenciador do Hyper-V, em **máquinas virtuais**, clique com o botão direito do mouse na VM que você deseja configurar e clique em **configurações**.  

   A caixa de diálogo **configurações** da VM é aberta.

8.  Verifique se a VM não foi iniciada. Se ele for iniciado, execute um desligamento antes de configurar a VM.  

8.  Em **hardware**, clique em **adaptador de rede**.  

   ![Adaptador de rede](../../media/Create-a-New-NIC-Team-in-a-VM/nict_hvs_01.jpg)  

9. Em Propriedades do **adaptador de rede** , selecione um dos comutadores virtuais que você criou nas etapas anteriores e clique em **aplicar**.  

    ![Selecionar um comutador virtual](../../media/Create-a-New-NIC-Team-in-a-VM/nict_hvs_02.jpg)  

10. Em **hardware**, clique para expandir o sinal de mais (+) ao lado de **adaptador de rede**. 

11. Clique em **recursos avançados** para habilitar o agrupamento NIC usando a interface gráfica do usuário. 

    >[!TIP]
    >Você também pode habilitar o agrupamento NIC com um comando do Windows PowerShell: 
    >
    >```PowerShell
    >Set-VMNetworkAdapter -VMName <VMname> -AllowTeaming On
    >```

    a. Selecione **dinâmico** para endereço Mac. 

    b. Clique para selecionar **rede protegida**. 

    c. Clique para selecionar **habilitar este adaptador de rede para fazer parte de uma equipe no sistema operacional convidado**. 

    d. Clique em **OK**.  

    ![Adicionar um adaptador de rede a uma equipe](../../media/Create-a-New-NIC-Team-in-a-VM/nict_hvs_05.jpg)  

13. Para adicionar um segundo adaptador de rede, no Gerenciador do Hyper-V, em **máquinas virtuais**, clique com o botão direito do mouse na mesma VM e clique em **configurações**.  

    A caixa de diálogo **configurações** da VM é aberta.

14. Em **adicionar hardware**, clique em **adaptador de rede**e, em seguida, clique em **Adicionar**.  

   ![Adicionar um adaptador de rede](../../media/Create-a-New-NIC-Team-in-a-VM/nict_hvs_06.jpg)  

15. Em Propriedades do **adaptador de rede** , selecione o segundo comutador virtual que você criou nas etapas anteriores e clique em **aplicar**.  

   ![Aplicar um comutador virtual](../../media/Create-a-New-NIC-Team-in-a-VM/nict_hvs_07.jpg)  

16. Em **hardware**, clique para expandir o sinal de mais (+) ao lado de **adaptador de rede**. 

17. Clique em **recursos avançados**, role para baixo até **agrupamento NIC**e clique para selecionar **habilitar este adaptador de rede para fazer parte de uma equipe no sistema operacional convidado**. 

18. Clique em **OK**.  

_**Congratula!**_  Você configurou a rede física e virtual.  Agora você pode continuar criando uma nova equipe NIC.  

## <a name="step-2-create-a-new-nic-team"></a>Etapa 2. Criar uma nova equipe de NIC

Ao criar uma nova equipe NIC, você deve configurar as propriedades da equipe NIC:  

-   Nome da equipe  

-   Adaptadores de membro  

-   Modo de agrupamento  

-   Modo de balanceamento de carga  

-   Adaptador em espera  

Opcionalmente, você também pode configurar a interface da equipe primária e configurar um número de rede virtual (VLAN).  

Para obter mais detalhes sobre essas configurações, consulte [configurações de agrupamento NIC](nic-teaming-settings.md).

### <a name="prerequisites"></a>Pré-requisitos

Você deve ter a associação em **Administradores**ou equivalente.  

### <a name="procedure"></a>Procedimento

1. No Gerenciador do Servidor, clique em **Servidor Local**.  

2. No painel **Propriedades** , na primeira coluna, localize **agrupamento NIC**e, em seguida, clique no link **desabilitado** .  

   A caixa de diálogo **agrupamento NIC** é aberta.

   ![Caixa de diálogo Agrupamento NIC](../../media/Create-a-New-NIC-Team/nict_02_nicteaming.jpg)

3. Em **adaptadores e interfaces**, selecione um ou mais adaptadores de rede que você deseja adicionar a uma equipe NIC.  

4. Clique em **tarefas**e clique em **Adicionar à nova equipe**.  

   A caixa de diálogo **nova equipe** é aberta e exibe os adaptadores de rede e os membros da equipe.

5. Em **nome da equipe**, digite um nome para a nova equipe NIC e clique em **Propriedades adicionais**.  

6. Em **Propriedades adicionais**, selecione valores para:

   - **Modo de agrupamento**. As opções para o modo de agrupamento são **independentes de comutação** e **comutadores dependentes**. O modo dependente de comutador inclui **agrupamento estático** e **LACP (protocolo de controle de agregação de link)** . 

     - **Alternar independente.** [!INCLUDE [switch-independent-shortdesc-include](../../includes/switch-independent-shortdesc-include.md)]

     - **Alternar dependente.** [!INCLUDE [switch-dependent-shortdesc-include](../../includes/switch-dependent-shortdesc-include.md)] 


       |                                              |                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                      |
       |----------------------------------------------|--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
       |              **Agrupamento estático**              |                                                                                                                                              Exige que você configure manualmente o comutador e o host para identificar quais links formam a equipe. Como essa é uma solução configurada estaticamente, não há nenhum protocolo adicional para ajudar o comutador e o host a identificar cabos conectados incorretamente ou outros erros que poderiam fazer com que a equipe não funcione. Normalmente, comutadores de classe de servidor dão suporte para esse modo.                                                                                                                                              |
       | **Protocolo de controle de agregação de link (LACP)** | Diferentemente do agrupamento estático, o modo de agrupamento do LACP identifica dinamicamente os links que estão conectados entre o host e o comutador. Essa conexão dinâmica permite a criação automática de uma equipe e, teoricamente, mas raramente na prática, a expansão e a redução de uma equipe simplesmente pela transmissão ou recebimento de pacotes de LACP da entidade de mesmo nível. Todas as opções de classe de servidor dão suporte a LACP e todas exigem que o operador de rede habilite o LACP administrativamente na porta do comutador. Quando você configura um modo de agrupamento do LACP, o agrupamento NIC sempre opera no modo ativo do LACP com um temporizador curto.  Nenhuma opção está disponível atualmente para modificar o temporizador ou alterar o modo LACP. |

       ---

   - **Modo de balanceamento de carga**. As opções para o modo de distribuição de balanceamento de carga são o **hash de endereço**, **a porta do Hyper-V**e o **dinâmico**.

     - **Hash de endereço.** [!INCLUDE [address-hash-shortdesc-include](../../includes/address-hash-shortdesc-include.md)]

     - **Porta Hyper-V.** [!INCLUDE [hyper-v-port-shortdesc-include](../../includes/hyper-v-port-shortdesc-include.md)]

     - **Dinâmico.** [!INCLUDE [dynamic-shortdesc-include](../../includes/dynamic-shortdesc-include.md)]

   - **Adaptador em espera**. As opções para o adaptador em espera são **nenhuma (todos os adaptadores ativos)** ou sua seleção de um adaptador de rede específico na equipe NIC que atua como um adaptador em espera.

   > [!TIP]  
   > Se você estiver configurando uma equipe NIC em uma VM (máquina virtual), deverá selecionar um **modo** de agrupamento _independente de comutador_ e um **modo de balanceamento de carga** de hash de _endereço_.  

7. Se você quiser configurar o nome da interface de equipe principal ou atribuir um número de VLAN à equipe NIC, clique no link à direita da **interface da equipe principal**.  

    A caixa de diálogo **nova interface de equipe** é aberta.

    ![Nova interface de equipe](../../media/Create-a-New-NIC-Team/nict_newteaminterface.jpg)

8. Dependendo de seus requisitos, siga um destes procedimentos:  

   -   Forneça um nome de interface tNIC.  

   -   Configurar Associação de VLAN: clique em **VLAN específica** e digite as informações de VLAN. Por exemplo, se você quiser adicionar essa equipe NIC ao número de VLAN de contabilidade 44, digite contabilidade 44-VLAN.   

9. Clique em **OK**.  

_**Congratula!**_  Você criou uma nova equipe NIC em um computador host ou VM.

## <a name="related-topics"></a>Tópicos relacionados

- [Agrupamento NIC](NIC-Teaming.md): Neste tópico, fornecemos uma visão geral do agrupamento NIC (placa de interface de rede) no Windows Server 2016. O agrupamento NIC permite que você agrupe entre um e 32 adaptadores de rede Ethernet físicos em um ou mais adaptadores de rede virtual baseados em software. Esses adaptadores de rede virtual oferecem desempenho rápido e tolerância a falhas no caso de uma falha do adaptador de rede.   

- [Uso e gerenciamento de endereço MAC de agrupamento NIC](NIC-Teaming-MAC-Address-Use-and-Management.md): Quando você configura uma equipe NIC com o modo independente de comutador e endereço hash ou distribuição dinâmica de carga, a equipe usa o endereço MAC (controle de acesso à mídia) do membro da equipe NIC primário no tráfego de saída. O membro da equipe NIC primário é um adaptador de rede selecionado pelo sistema operacional do conjunto inicial de membros da equipe.

- [Configurações de agrupamento NIC](nic-teaming-settings.md): Neste tópico, fornecemos uma visão geral das propriedades da equipe da NIC, como os modos de agrupamento e balanceamento de carga. Também fornecemos detalhes sobre a configuração do adaptador em espera e a propriedade da interface da equipe principal. Se você tiver pelo menos dois adaptadores de rede em uma equipe NIC, não será necessário designar um adaptador em espera para tolerância a falhas.

- [Solução de problemas de agrupamento NIC](Troubleshooting-NIC-Teaming.md): Neste tópico, discutiremos maneiras de solucionar problemas de agrupamento de NIC, como hardware, comutador físico de valores e desabilitar ou habilitar adaptadores de rede usando o Windows PowerShell. 

---