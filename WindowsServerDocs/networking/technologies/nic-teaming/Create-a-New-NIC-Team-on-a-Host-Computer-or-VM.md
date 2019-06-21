---
title: Criar um novo agrupamento NIC em uma VM ou computador host
description: Neste tópico, você cria um novo agrupamento NIC em um computador host ou em uma máquina virtual do Hyper-V (VM) executando o Windows Server 2016.
manager: dougkim
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: networking-nict
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: a4caaa86-5799-4580-8775-03ee213784a3
ms.author: pashort
author: shortpatti
ms.date: 09/13/2018
ms.openlocfilehash: 5380cb2007bab1a296e0facc12885d47c6afc708
ms.sourcegitcommit: afb0602767de64a76aaf9ce6a60d2f0e78efb78b
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/20/2019
ms.locfileid: "67282093"
---
# <a name="create-a-new-nic-team-on-a-host-computer-or-vm"></a>Criar um novo agrupamento NIC em uma VM ou computador host

>Aplica-se a: Windows Server (canal semestral), Windows Server 2016

Neste tópico, você cria um novo agrupamento NIC em um computador host ou em uma máquina virtual do Hyper-V (VM) executando o Windows Server 2016.  

## <a name="network-configuration-requirements"></a>Requisitos de configuração de rede  
Antes de criar um novo agrupamento NIC, você deve implantar um host do Hyper-V com dois adaptadores de rede que se conectam aos comutadores físicos diferentes. Você também deve configurar os adaptadores de rede com endereços IP que são do mesmo intervalo de endereços IP.  

O comutador físico, comutador Virtual Hyper-V, rede local (LAN) e requisitos de agrupamento NIC para a criação de um agrupamento NIC em uma VM são:  

-   O computador que executa o Hyper-V deve ter dois ou mais adaptadores de rede.  

-   Se conectando os adaptadores de rede a vários comutadores físicos, os comutadores físicos devem estar na mesma sub-rede de camada 2.  

-   Você deve usar o Gerenciador do Hyper-V ou o Windows PowerShell para criar dois Switches de Virtual do Hyper-V externo, cada uma conectada a um adaptador de rede física diferentes.  

-   A máquina virtual deve se conectar a ambos os comutadores virtuais externos que você cria.  

-   O agrupamento NIC, no Windows Server 2016, dá suporte a equipes com dois membros em máquinas virtuais. Você pode criar equipes maiores, mas não há suporte.

-   Se você estiver configurando um agrupamento NIC em uma VM, você deve selecionar um **modo de agrupamento** dos _independente de comutador_ e um **modo de balanceamento de carga** de _deHashdeendereço_. 

## <a name="step-1-configure-the-physical-and-virtual-network"></a>Etapa 1. Configurar a rede física e virtual  
Neste procedimento, criar comutadores virtuais de Hyper-V externo dois, se conectar a uma VM para as opções e, em seguida, configurar as conexões de VM com as opções.  

### <a name="prerequisites"></a>Pré-requisitos

Você deve estar associado ao **administradores**, ou equivalente.  

### <a name="procedure"></a>Procedimento

1.  No host Hyper-V, abra o Gerenciador do Hyper-V e, em ações, clique em **Gerenciador de comutador Virtual**.  

   ![Gerenciador de comutador virtual](../../media/Create-a-New-NIC-Team-in-a-VM/nict_hv.jpg)  

2.  No Gerenciador de comutador Virtual, certifique-se **externos** está selecionado e, em seguida, clique em **criar comutador Virtual**.  

   ![Criar comutador Virtual](../../media/Create-a-New-NIC-Team-in-a-VM/nict_hv_02.jpg)  

3.  Em Propriedades do comutador Virtual, digite uma **nome** para alternar de virtual e adicione **notas** conforme necessário.  

4.  Na **tipo de Conexão**, na **rede externa**, selecione o adaptador de rede física ao qual você deseja anexar o comutador virtual.  

5.  Configurar propriedades do comutador adicionais para sua implantação e, em seguida, clique em **Okey**.  

6.  Crie um comutador virtual externo de segundo, repetindo as etapas anteriores. Conecte-se a segunda opção externa a um adaptador de rede diferente.  

7.  No Gerenciador do Hyper-V, sob **máquinas virtuais**, a VM que você deseja configurar e, em seguida, clique com o botão direito **configurações**.  

   A VM **configurações** caixa de diálogo é aberta.

8.  Certifique-se de que a VM não foi iniciada. Se ele for iniciado, execute um desligamento antes de configurar a VM.  

8.  Na **Hardware**, clique em **adaptador de rede**.  

   ![Adaptador de rede](../../media/Create-a-New-NIC-Team-in-a-VM/nict_hvs_01.jpg)  

9. Na **adaptador de rede** propriedades, selecione um dos comutadores virtuais que você criou nas etapas anteriores e, em seguida, clique em **aplicar**.  

    ![Selecione um comutador virtual](../../media/Create-a-New-NIC-Team-in-a-VM/nict_hvs_02.jpg)  

10. Na **Hardware**, clique para expandir o sinal de adição (+) próximo a **adaptador de rede**. 

11. Clique em **recursos avançados** para habilitar o agrupamento NIC, usando a interface gráfica do usuário. 

    >[!TIP]
    >Você também pode habilitar o agrupamento NIC com um comando do Windows PowerShell: 
    >
    >```PowerShell
    >Set-VMNetworkAdapter -VMName <VMname> -AllowTeaming On
    >```

    a. Selecione **dinâmico** para o endereço MAC. 

    b. Clique para marcar **rede protegido**. 

    c. Clique para marcar **habilitar este adaptador de rede ser parte de uma equipe no sistema operacional convidado**. 

    d. Clique em **OK**.  

    ![Adicionar um adaptador de rede a uma equipe](../../media/Create-a-New-NIC-Team-in-a-VM/nict_hvs_05.jpg)  

13. Para adicionar um segundo adaptador de rede, no Gerenciador do Hyper-V, na **máquinas virtuais**, a mesma VM com o botão direito e, em seguida, clique em **configurações**.  

    A VM **configurações** caixa de diálogo é aberta.

14. Na **Adicionar Hardware**, clique em **adaptador de rede**e, em seguida, clique em **adicionar**.  

   ![Adicionar um adaptador de rede](../../media/Create-a-New-NIC-Team-in-a-VM/nict_hvs_06.jpg)  

15. Na **adaptador de rede** propriedades, selecione o segundo comutador virtual que você criou nas etapas anteriores e, em seguida, clique em **aplicar**.  

   ![Aplicar um comutador virtual](../../media/Create-a-New-NIC-Team-in-a-VM/nict_hvs_07.jpg)  

16. Na **Hardware**, clique para expandir o sinal de adição (+) próximo a **adaptador de rede**. 

17. Clique em **recursos avançados**, role para baixo até **agrupamento NIC**e clique para selecionar **habilitar este adaptador de rede ser parte de uma equipe no sistema operacional convidado**. 

18. Clique em **OK**.  

_**Parabéns!** _  Você configurou a rede física e virtual.  Agora você pode prosseguir para a criação de um novo agrupamento NIC.  

## <a name="step-2-create-a-new-nic-team"></a>Etapa 2. Criar um novo agrupamento NIC

Quando você cria um novo agrupamento NIC, você deve configurar as propriedades de agrupamento NIC:  

-   Nome da equipe  

-   Adaptadores de membro  

-   Modo de agrupamento  

-   Modo de balanceamento de carga  

-   Adaptador em espera  

Também opcionalmente, você pode configurar a interface primária de equipe e configurar um número de LAN (VLAN) virtual.  

Para obter mais detalhes sobre essas configurações, consulte [configurações de agrupamento NIC](nic-teaming-settings.md).

### <a name="prerequisites"></a>Pré-requisitos

Você deve estar associado ao **administradores**, ou equivalente.  

### <a name="procedure"></a>Procedimento

1. No Gerenciador do Servidor, clique em **Servidor Local**.  

2. No **propriedades** painel, na primeira coluna, localize **agrupamento NIC**e, em seguida, clique no **desabilitado** link.  

   O **agrupamento NIC** caixa de diálogo é aberta.

   ![Caixa de diálogo Agrupamento NIC](../../media/Create-a-New-NIC-Team/nict_02_nicteaming.jpg)

3. Na **adaptadores e Interfaces**, selecione um ou mais adaptadores de rede que você deseja adicionar a uma equipe NIC.  

4. Clique em **tarefas**e clique em **adicionar a nova equipe**.  

   O **nova equipe** caixa de diálogo é aberta e exibe os membros da equipe e adaptadores de rede.

5. Na **nome da equipe**, digite um nome para o novo agrupamento NIC e, em seguida, clique em **propriedades adicionais**.  

6. Na **propriedades adicionais**, selecione valores para:

   - **Modo de agrupamento**. As opções para o modo de agrupamento são **comutador independente** e **dependentes de comutador**. Inclui o modo dependentes de comutador **agrupamento estático** e **Link Aggregation Control Protocol (LACP)** . 

     - **Independente do comutador.** [!INCLUDE [switch-independent-shortdesc-include](../../includes/switch-independent-shortdesc-include.md)]

     - **Dependente do comutador.** [!INCLUDE [switch-dependent-shortdesc-include](../../includes/switch-dependent-shortdesc-include.md)] 


       |                                              |                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                      |
       |----------------------------------------------|--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
       |              **Agrupamento estático**              |                                                                                                                                              Exige que você configure manualmente o comutador e o host para identificar quais links formam o agrupamento. Como esta é uma solução configurada estaticamente, não há nenhum protocolo adicional para ajudar o comutador e no host para identificar incorretamente conectados cabos ou outros erros que poderiam causar falhas executar no grupo. Normalmente, comutadores de classe de servidor dão suporte para esse modo.                                                                                                                                              |
       | **Link Aggregation Control Protocol (LACP)** | Ao contrário de um agrupamento estático, o modo de agrupamento LACP dinamicamente identifica links que estão conectados entre o host e o comutador. Essa conexão dinâmica permite a criação automática de uma equipe e, em teoria, mas raramente na prática, a expansão e redução de uma equipe simplesmente por transmissão ou recebimento de pacotes LACP da entidade de par. Todos os comutadores de classe de servidor dão suporte a LACP e exigem o operador de rede habilitar administrativamente LACP na porta do comutador. Quando você configura um modo de agrupamento de LACP, sempre agrupamento NIC opera em modo de Active Directory do LACP, com um temporizador curto.  Nenhuma opção está atualmente disponível para modificar o temporizador ou alterar o modo LACP. |

       ---

   - **Modo de balanceamento de carga**. As opções de balanceamento de carga de modo de distribuição são **Hash de endereço**, **porta Hyper-V**, e **dinâmico**.

     - **Hash de endereço.** [!INCLUDE [address-hash-shortdesc-include](../../includes/address-hash-shortdesc-include.md)]

     - **Porta do Hyper-V.** [!INCLUDE [hyper-v-port-shortdesc-include](../../includes/hyper-v-port-shortdesc-include.md)]

     - **Dynamic.** [!INCLUDE [dynamic-shortdesc-include](../../includes/dynamic-shortdesc-include.md)]

   - **Adaptador espera**. As opções para o adaptador em espera serão **nenhum (todos os adaptadores do Active Directory)** ou a seleção de um adaptador de rede específico na equipe NIC que atua como um adaptador de modo de espera.

   > [!TIP]  
   > Se você estiver configurando um agrupamento NIC em uma máquina virtual (VM), você deve selecionar um **modo de agrupamento** dos _independente de comutador_ e um **modo de balanceamento de carga** de  _Hash de endereço_.  

7. Se você quiser configurar o nome da interface primárias da equipe ou atribuir um número VLAN para o agrupamento NIC, clique no link à direita da vírgula **interface de equipe primária**.  

    O **nova interface de equipe** caixa de diálogo é aberta.

    ![Nova interface de equipe](../../media/Create-a-New-NIC-Team/nict_newteaminterface.jpg)

8. Dependendo dos seus requisitos, siga um destes procedimentos:  

   -   Forneça um nome de interface tNIC.  

   -   Configurar a associação à VLAN: clique em **VLAN específica** e digite as informações de VLAN. Por exemplo, se você deseja adicionar esta equipe de NIC para a VLAN de contabilidade número 44, 44 contabilização de tipo - VLAN.   

9. Clique em **OK**.  

_**Parabéns!** _  Você criou um novo agrupamento NIC em uma VM ou computador host.

## <a name="related-topics"></a>Tópicos relacionados

- [Agrupamento NIC](NIC-Teaming.md): Neste tópico, nós fornecemos a você uma visão geral do agrupamento de placa de Interface de rede (NIC) no Windows Server 2016. Agrupamento NIC permite que você agrupe entre um e 32 adaptadores de rede Ethernet física em um ou mais adaptadores de rede virtual baseada em software. Esses adaptadores de rede virtual oferecem desempenho rápido e tolerância a falhas no caso de uma falha do adaptador de rede.   

- [Gerenciamento e uso do endereço MAC de agrupamento NIC](NIC-Teaming-MAC-Address-Use-and-Management.md): Quando você configura uma equipe NIC com a opção de modo independente e hash de endereço ou distribuição de carga dinâmico, a equipe usa que o acesso à mídia (MAC) de endereço do membro da equipe de NIC primário no tráfego de saída de controle. O membro da equipe de NIC primário é um adaptador de rede selecionado pelo sistema operacional do conjunto inicial de membros da equipe.

- [Configurações de agrupamento NIC](nic-teaming-settings.md): Neste tópico, podemos lhe dar uma visão geral das propriedades de agrupamento NIC, como agrupamento e modos de balanceamento de carga. Nós também fornecemos a você detalhes sobre a configuração do adaptador em espera e a propriedade de interface de equipe principal. Se você tiver pelo menos dois adaptadores de rede em um agrupamento NIC, você não precisa designar um adaptador de modo de espera para tolerância a falhas.

- [Solução de problemas de agrupamento NIC](Troubleshooting-NIC-Teaming.md): Neste tópico, discutiremos maneiras de solucionar problemas de agrupamento NIC, como hardware, firmas de comutador físico e desabilitando ou habilitando adaptadores de rede usando o Windows PowerShell. 

---