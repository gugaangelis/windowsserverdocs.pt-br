---
title: Balanceamento de carga de rede
description: Este tópico fornece uma visão geral do recurso balanceamento de carga de rede (NLB) no Windows Server 2016 e inclui links para orientações adicionais sobre como criar, configurar e gerenciar clusters NLB.
manager: brianlic
ms.prod: windows-server-threshold
ms.technology: networking-nlb
ms.topic: article
ms.assetid: 244a4b48-06e5-4796-8750-a50e4f88ac72
ms.author: pashort
author: shortpatti
ms.openlocfilehash: b9fd39381316a8bcd06328e7aa75492ed99bc7f1
ms.sourcegitcommit: 19d9da87d87c9eefbca7a3443d2b1df486b0b010
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/28/2018
---
# <a name="network-load-balancing"></a>Balanceamento de carga de rede

>Aplica-se a: Windows Server (anual por canal), Windows Server 2016

Este tópico fornece uma visão geral do recurso \(NLB\) balanceamento de carga de rede no Windows Server 2016 e inclui links para orientações adicionais sobre como criar, configurar e gerenciar clusters NLB.

Você pode usar NLB para gerenciar servidores de dois ou mais como um único cluster virtual. NLB aprimora a disponibilidade e escalabilidade dos aplicativos de servidor de Internet, como aqueles usados na web, FTP, firewall, proxy, virtuais privadas \(VPN\) e outros servidores mission\ críticos de rede.   

>[!NOTE]
>Windows Server 2016 inclui um novo \(SLB\) Software balanceador inspirado Azure como um componente da infraestrutura \(SDN\) Software de rede definidos. Use SLB em vez de NLB se você estiver usando SDN, são usando cargas de trabalho não são do Windows, precisa de conversão de endereços de rede de saída \(NAT\), ou precisa \(L3\) Layer 3 ou não-TCP com base em balanceamento de carga. Você pode continuar a usar o NLB com o Windows Server 2016 para implantações de não SDN. Para saber mais sobre SLB, consulte [Software carga balanceamento (SLB) para SDN](../sdn/technologies/network-function-virtualization/Software-Load-Balancing--SLB--for-SDN.md).

O recurso de balanceamento de carga de rede \(NLB\) distribui o tráfego entre vários servidores usando o protocolo de rede TCP\/IP. Combinando dois ou mais computadores que executam aplicativos em um único cluster virtual, o NLB fornece confiabilidade e desempenho para servidores web e outros servidores mission\ crítico.  
  
Os servidores em um cluster NLB são chamados *hosts*, e cada host executa uma cópia separada dos aplicativos do servidor. NLB distribui solicitações de cliente em todos os hosts no cluster. Você pode configurar a carga deve ser tratado por cada host. Você também pode adicionar hosts dinamicamente ao cluster para lidar com o aumento na carga. NLB também pode direcionar todo o tráfego para um único host designado, que é chamado a *host padrão*.  
  
NLB permite que todos os computadores do cluster devem ser atendidas pelo mesmo conjunto de endereços IP e mantém um conjunto de endereços IP exclusivos e dedicados para cada host. Para aplicativos equilibrado load\, quando um host falha ou fica offline, a carga é automaticamente redistribuída entre os computadores que ainda estão funcionando. Quando estiver pronto, o computador offline pode reingressar no cluster transparente e retomar sua parcela da carga de trabalho, que permite que os outros computadores do cluster para lidar com menos tráfego.  
  
## <a name="BKMK_APP"></a>Aplicativos práticos  
NLB é útil para assegurar que os aplicativos sem estado, como servidores web que executam o Internet Information Services \(IIS\), estão disponíveis com tempo ocioso mínimo e que estão escalonáveis \ (adicionando servidores adicionais como o increases\ de carga). As seções a seguir descrevem como NLB dá suporte a capacidade de gerenciamento de cluster servidores que executam esses aplicativos, alta disponibilidade e escalabilidade.  
  
### <a name="high-availability"></a>Alta disponibilidade  
Um sistema de alta disponibilidade confiável fornece um nível de serviço com tempo ocioso mínimo aceitável. Para proporcionar alta disponibilidade, NLB inclui built\ recursos que podem executar automaticamente:  
  
-   Detectar um host de cluster que falha ou fica offline e, em seguida, recuperar.  
  
-   Equilibre a carga de rede quando hosts são adicionados ou removidos.  
  
-   Recuperar e redistribuir a carga de trabalho até dez segundos.  
  
### <a name="scalability"></a>Escalabilidade  
Escalabilidade é a medida de quanto um computador, serviço ou aplicativo pode crescer para atender às necessidades crescentes de desempenho. Para clusters NLB escalabilidade é a capacidade de adicionar um ou mais sistemas incrementais a um cluster existente quando a carga total do cluster excede seus recursos. Para dar suporte a escalabilidade, você pode fazer o seguinte com NLB:  
  
-   Equilibre solicitações de carga no cluster NLB para serviços TCP\/IP individuais.  
  
-   Suporte a até 32 computadores em um único cluster.  
  
-   Equilibrar várias solicitações de carregamento de servidor \ (do mesmo cliente ou de vários clients\) em vários hosts no cluster.  
  
-   Adicione hosts ao cluster NLB como a carga aumenta, sem causar falha no cluster.  
  
-   Remova os hosts do cluster quando diminui a carga.  
  
-   Ative alto desempenho e baixa sobrecarga por meio de uma implementação pipelining integral. Pipelining permite que as solicitações sejam enviadas ao cluster NLB sem aguardar uma resposta a uma solicitação anterior.  
  
### <a name="manageability"></a>Capacidade de gerenciamento  
Para dar suporte a capacidade de gerenciamento, você pode fazer o seguinte com NLB:  
  
-   Gerenciar e configurar vários clusters NLB e os hosts de cluster de um único computador, usando o Gerenciador NLB ou o [Cmdlets de balanceamento de carga de rede (NLB) no Windows PowerShell](https://technet.microsoft.com/library/hh801274.aspx).
  
-   Especifica o comportamento de uma única porta IP ou grupo de portas balanceamento usando as regras de gerenciamento de porta.  
  
-   Defina regras de porta diferente para cada site. Se você usar o mesmo conjunto de servidores equilibrado load\ para vários aplicativos ou sites, regras de porta são com base no endereço IP virtual de destino \(using virtual clusters\).  

-   Direcioná-todas as solicitações de cliente para um único host usando opcional e regras de single\-host. NLB roteia solicitações de cliente para um host específico que esteja executando aplicativos específicos.  

-   Bloquear o acesso de rede indesejadas para determinadas portas IP.  

-   Habilitar o suporte do protocolo IGMP \(IGMP\) nos hosts de cluster para controlar a saturação de porta do switch \ (onde pacotes de rede de entrada são enviadas para todas as portas o switch\) ao operar no modo de multicast.  

-   Iniciar, parar e controlar as ações de NLB remotamente usando comandos do Windows PowerShell ou scripts.  

-   Exiba o Log de eventos do Windows para verificar os eventos NLB. NLB registra em log todas as ações e alterações de cluster no log de eventos.  

## <a name="important-functionality"></a>Funcionalidade importante  
 
NLB é instalado como um componente de padrão driver de rede do Windows Server. Suas operações são transparentes para a pilha de rede TCP\/IP. A figura a seguir mostra a relação entre NLB e outros componentes de software em uma configuração típica.  
  
![Balanceamento de carga de rede e outros componentes de software](../media/NLB/nlb.jpg)  
  
A seguir estão os principais recursos do NLB.  
  
- Não requer nenhuma alteração de hardware para executar.  
  
- Fornece ferramentas de balanceamento de carga de rede para configurar e gerenciar vários clusters e todos os hosts de um único computador local ou remoto.  
  
- Permite que os clientes acessem o cluster usando um único nome lógico de Internet e o endereço IP virtual, que é conhecido como endereço IP do cluster \ (mantém os nomes individuais para cada computer\). NLB permite vários endereços IP virtuais para servidores de hospedagem múltipla.  
  
> [!NOTE]  
> Quando você implanta VMs como clusters virtuais, NLB não exige servidores seja de hospedagem múltipla com vários endereços IP virtuais.  
  
- Permite NLB a ser vinculado a vários adaptadores de rede, que permite configurar vários clusters independentes em cada host. Suporte para vários adaptadores de rede é diferente de clusters virtuais, clusters virtuais permitem que você configurar vários clusters em um único adaptador de rede.  
  
- Não exige nenhuma modificação para aplicativos de servidor para que eles podem executar em um cluster NLB.  
  
- Pode ser configurado para adicionar automaticamente um host ao cluster se esse host de cluster falha e subsequentemente é colocado online. O host adicionado pode começar a lidar com novas solicitações de servidor de clientes.  
  
-   Permite que você tire computadores offline para manutenção preventiva sem afetar as operações de cluster nos outros hosts.  
  
## <a name="BKMK_HARD"></a>Requisitos de hardware  
A seguir é os requisitos de hardware para executar um cluster NLB.  
  
-   Todos os hosts no cluster devem residir na mesma sub-rede.  
  
-   Não há nenhuma restrição no número de adaptadores de rede em cada host e hosts diferentes podem ter um número diferente de adaptadores.  
  
-   Dentro de cada cluster, todos os adaptadores de rede devem ser multicast ou unicast. NLB não oferece suporte a um ambiente misto de multicast e unicast em um único cluster.  
  
-   Se você usar o modo unicast, o adaptador de rede é usado para manipular o tráfego client\-to\-cluster deve ter suporte alterando seu endereço de \(MAC\) de controle de acesso de mídia.  
  
## <a name="BKMK_SOFT"></a>Requisitos de software  
A seguir é os requisitos de software para executar um cluster NLB.  
  
-   TCP\/IP somente pode ser usado no adaptador para o qual NLB está habilitado em cada host. Não adicione quaisquer outros protocolos \ (por exemplo, IPX\) a esse adaptador.  
  
-   Os endereços IP dos servidores no cluster devem ser estáticos.  
  
> [!NOTE]  
> NLB não oferece suporte a \(DHCP\) Dynamic Host Configuration Protocol. NLB desabilita o DHCP em cada interface que configura o.  
  
## <a name="BKMK_INSTALL"></a>Informações de instalação  
Você pode instalar NLB usando o Gerenciador do servidor ou os comandos do Windows PowerShell para NLB.

Opcionalmente, você pode instalar as ferramentas de balanceamento de carga de rede para gerenciar um cluster NLB local ou remoto. As ferramentas incluem os comandos de NLB Windows PowerShell e Gerenciador de balanceamento de carga de rede.

### <a name="installation-with-server-manager"></a>Instalação com o Gerenciador do servidor

No Gerenciador do servidor, você pode usar a adição de funções e recursos do Assistente para adicionar o **balanceamento de carga de rede** recurso. Quando você concluir o assistente, NLB é instalado, e você não precisa reiniciar o computador.


### <a name="installation-with-windows-powershell"></a>Instalação com o Windows PowerShell  

Para instalar o NLB usando o Windows PowerShell, execute o seguinte comando em um prompt do Windows PowerShell com privilégios elevados no computador onde deseja instalar o NLB.

    
    Install-WindowsFeature NLB -IncludeManagementTools
    
Após a instalação estiver concluída, nenhuma reinicialização do computador é necessária.

Para obter mais informações, consulte [Install-WindowsFeature](https://technet.microsoft.com/library/jj205467.aspx).

### <a name="network-load-balancing-manager"></a>Gerenciador de balanceamento de carga de rede
Para abrir o Gerenciador de balanceamento de carga de rede no Gerenciador do servidor, clique em **ferramentas**e clique em **Gerenciador de balanceamento de carga de rede**.
  
## <a name="BKMK_LINKS"></a>Recursos adicionais  
A tabela a seguir fornece links para informações adicionais sobre o recurso NLB.  
  
|Tipo de conteúdo|Referências|  
|----------------|--------------|  
|Implantação|[Guia de implantação de balanceamento de carga de rede](https://technet.microsoft.com/library/cc754833(WS.10).aspx) & #124; [Configurando com serviços de Terminal de balanceamento de carga de rede](https://technet.microsoft.com/library/cc771300(v=WS.10).aspx)|  
|Operações|[Gerenciando Clusters de balanceamento de carga de rede](https://technet.microsoft.com/library/cc753954(WS.10).aspx) & #124; [Definir parâmetros de balanceamento de carga de rede](https://technet.microsoft.com/library/cc731619(WS.10).aspx) & #124; [Controlando Hosts em Clusters de balanceamento de carga de rede](https://technet.microsoft.com/library/cc770870(WS.10).aspx)|  
|Solução de problemas|[Solução de problemas de Clusters de balanceamento de carga de rede](https://technet.microsoft.com/library/cc732592(WS.10).aspx) & #124; [Erros e eventos do Cluster NLB](https://technet.microsoft.com/library/cc731678(WS.10).aspx)|
|Ferramentas e configurações|[Cmdlets do PowerShell de Windows de balanceamento de carga de rede](https://go.microsoft.com/fwlink/p/?LinkId=238123)|
|Recursos da comunidade|[Fórum de \(Clustering\) alta disponibilidade](https://go.microsoft.com/fwlink/p/?LinkId=230641)