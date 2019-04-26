---
title: Balanceamento de Carga de Rede
description: Neste tópico, nós fornecemos uma visão geral do balanceamento de carga de rede \(NLB\) recurso no Windows Server 2016. Você pode usar o NLB para gerenciar dois ou mais servidores como um único cluster virtual. NLB melhora a disponibilidade e escalabilidade de aplicativos de servidor de Internet, como aqueles usados na web, FTP, firewall, proxy, rede virtual privada \(VPN\)e a outro missão\-servidores críticos.
manager: dougkim
ms.prod: windows-server-threshold
ms.technology: networking-nlb
ms.topic: article
ms.assetid: 244a4b48-06e5-4796-8750-a50e4f88ac72
ms.author: pashort
author: shortpatti
ms.date: 09/13/2018
ms.openlocfilehash: d0cf1e1d6b1681a0f18908b08cd17572159e0462
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59881747"
---
# <a name="network-load-balancing"></a>Balanceamento de Carga de Rede

>Aplica-se a: Windows Server (canal semestral), Windows Server 2016

Neste tópico, nós fornecemos uma visão geral do balanceamento de carga de rede \(NLB\) recurso no Windows Server 2016. Você pode usar o NLB para gerenciar dois ou mais servidores como um único cluster virtual. NLB melhora a disponibilidade e escalabilidade de aplicativos de servidor de Internet, como aqueles usados na web, FTP, firewall, proxy, rede virtual privada \(VPN\)e a outro missão\-servidores críticos.  

>[!NOTE]
>Windows Server 2016 inclui um novo balanceador de carga de Software inspirada no Azure \(SLB\) como um componente da rede definida pelo Software \(SDN\) infraestrutura. Use SLB, em vez de NLB se você estiver usando a SDN, usando cargas de trabalho não Windows, precisa de conversão de endereços de rede de saída \(NAT\), ou precisar de camada 3 \(L3\) ou balanceamento de carga não-com base em TCP. Você pode continuar a usar o NLB com o Windows Server 2016 para implantações não SDN. Para obter mais informações sobre o SLB, consulte [Software SLB balanceamento de carga () para SDN](../sdn/technologies/network-function-virtualization/Software-Load-Balancing--SLB--for-SDN.md).

O balanceamento de carga de rede \(NLB\) recurso distribui o tráfego entre vários servidores usando o TCP\/protocolo de rede IP. Combinando dois ou mais computadores que executam aplicativos em um único cluster virtual, o NLB proporciona confiabilidade e desempenho para servidores web e outro missão\-servidores críticos.  
  
Os servidores em um cluster NLB se chamam *hosts*, e cada host executa uma cópia separada dos aplicativos de servidor. O NLB distribui as solicitações de entrada dos clientes pelos hosts do cluster. É possível configurar a carga que será tratada por cada host. Você também pode adicionar hosts dinamicamente ao cluster para tratar aumentos de carga. Além disso, o NLB pode direcionar todo o tráfego para um único host designado, chamado de *host padrão*.  
  
O NLB permite que todos os computadores no cluster sejam abordados pelo mesmo conjunto de endereços IP, e mantém um conjunto de endereços IP exclusivos e dedicados para cada host. Para carga\-com balanceamento de aplicativos, quando um host falha ou fica offline, a carga é automaticamente redistribuída entre os computadores que ainda estão operando. Quando estiver pronto, o computador offline poderá reintegrar-se ao cluster de forma transparente e retomar sua parcela da carga de trabalho, o que permite que os outros computadores do cluster lidem com menos tráfego.  
  
## <a name="practical-applications"></a>Aplicações práticas  
O NLB é útil para garantir sem monitoração de estado ou aplicativos, como servidores web executando serviços de informações da Internet \(IIS\), estão disponíveis com inatividade mínima e que eles sejam dimensionáveis \(adicionando mais servidores como a carga aumenta\). As seções a seguir descrevem como o NLB suporta alta disponibilidade, escalabilidade e gerenciamento dos servidores em cluster que executam esses aplicativos.  
  
### <a name="high-availability"></a>Alta disponibilidade  
Um sistema altamente disponível oferece, de forma confiável, um nível aceitável de serviço com tempo de inatividade mínimo. Para fornecer alta disponibilidade, o NLB inclui criado\-nos recursos que podem automaticamente:  
  
-   Detectar um host de cluster que falha ou fica offline, e depois recuperar.  
  
-   Equilibrar a carga da rede quando hosts são adicionados ou removidos.  
  
-   Recuperar e redistribuir carga de trabalho no prazo de dez segundos.  
  
### <a name="scalability"></a>Escalabilidade  
Escalabilidade é a medida que determina como um computador, serviço ou aplicativo pode atender melhor às necessidades crescentes de desempenho. No caso de clusters NLB , a escalabilidade é a capacidade de adicionar paulatinamente um ou mais sistemas a um cluster existente quando a carga total sobre o cluster excede seus recursos. Para dar suporte à escalabilidade, o NLB pode fazer o seguinte:  
  
-   Equilibrar solicitações de carga no cluster NLB para TCP individual\/serviços IP.  
  
-   Dar suporte para até 32 computadores em um único cluster.  
  
-   Equilibrar as várias solicitações de carga \(do mesmo cliente ou de vários clientes\) em vários hosts no cluster.  
  
-   Adicionar hosts ao cluster do NLB à medida que a carga aumenta, sem fazer o cluster falhar.  
  
-   Remover os hosts do cluster quando a carga diminui.  
  
-   Permitir alto desempenho e baixa sobrecarga através de implementação com pipelining integral. O pipelining permite que as solicitações sejam enviadas ao cluster NLB sem aguardar resposta da solicitação enviada anteriormente.  
  
### <a name="manageability"></a>Capacidade de gerenciamento  
Para dar suporte à escalabilidade, o NLB pode fazer o seguinte:  
  
-   Gerenciar e configurar vários clusters NLB e os hosts de cluster de um único computador usando o Gerenciador NLB ou o [os Cmdlets de balanceamento de carga de rede (NLB) no Windows PowerShell](https://technet.microsoft.com/library/hh801274.aspx).
  
-   Especificar o comportamento do balanceamento de carga para uma única porta ou para um grupo de portas IP usando regras de gerenciamento de portas.  
  
-   Definir regras de porta diferentes para cada site. Se você usar o mesmo conjunto de carga\-servidores com balanceamento de vários aplicativos ou sites, as regras de porta baseiam-se no endereço IP virtual de destino \(usando clusters virtuais\).  

-   Direcionar todas as solicitações de cliente para um único host com o uso opcional de único\-hospedar as regras. O NLB roteia solicitações de clientes para um host específico que esteja executando aplicativos específicos.  

-   Bloquear o acesso indesejado à rede para determinadas portas IP.  

-   Habilitar o protocolo IGMP \(IGMP\) suporte nos hosts de cluster para controlar inundações porta do comutador \(onde os pacotes de rede de entrada são enviados para todas as portas no comutador\) ao operar em modo de multicast.  

-   Iniciar, parar e controlar as ações NLB remotamente usando comandos do Windows PowerShell ou scripts.  

-   Exibir o log de eventos do Windows para verificar eventos do NLB. O NLB registra todas as ações e alterações de cluster no log de eventos.  

## <a name="important-functionality"></a>Funcionalidade importante  
 
O NLB é instalado como um componente de padrão driver de rede do Windows Server. Suas operações são transparentes para o TCP\/pilha da rede IP. A figura a seguir mostra a relação entre NLB e outros componentes de software em uma configuração típica.  
  
![O balanceamento de carga de rede e outros componentes de software](../media/NLB/nlb.jpg)  
  
A seguir é os principais recursos do NLB.  
  
- Não requer alterações de hardware para funcionar.  
  
- Oferece Ferramentas de Balanceamento de Carga de Rede para configurar e gerenciar vários clusters e todos os hosts do cluster em um único computador remoto ou local.  
  
- Permite que os clientes acessem o cluster usando um só nome lógico de Internet e endereço IP virtual, o que é conhecido como endereço IP do cluster \(mantém os nomes individuais de cada computador\). O NLB permite vários endereços IP virtuais para servidores multihomed.  
  
> [!NOTE]  
> Quando você implanta VMs como clusters virtuais, o NLB não requer servidores sejam multihomed para ter vários endereços IP virtuais.  
  
- Permite que o NLB seja vinculado a vários adaptadores de rede, o que permite configurar vários clusters independentes em cada host. O suporte a vários adaptadores de rede difere dos clusters virtuais, pois os clusters virtuais permitem configurar vários clusters em um único adaptador de rede.  
  
- Não requer modificações para aplicativos de servidor para que eles possam ser executados em um cluster NLB.  
  
- Pode ser configurado para adicionar automaticamente um host ao cluster se esse host de cluster falhar e voltar ao modo online posteriormente. O host adicionado pode começar a manipular os pedidos de novos servidores clientes.  
  
-   Permite que você coloque computadores offline para manutenção preventiva sem perturbar as operações de cluster nos outros hosts.  
  
## <a name="hardware-requirements"></a>Requisitos de hardware  
A seguir está os requisitos de hardware para executar um cluster NLB.  
  
-   Todos os hosts do cluster devem residir na mesma sub-rede.  
  
-   Não há restrição ao número de adaptadores de rede em cada host, e hosts diferentes podem ter um número diferente de adaptadores.  
  
-   Dentro de cada cluster, todos os adaptadores de rede devem ser multicast ou unicast. O NLB não dá suporte a ambientes mistos unicast e multicast em um único cluster.  
  
-   Se você usar o modo unicast, o adaptador de rede que é usado para lidar com o cliente\-à\-tráfego de cluster deve apoiar a mudança de seu controle de acesso de mídia \(MAC\) endereço.  
  
## <a name="software-requirements"></a>Requisitos de software  
A seguir está os requisitos de software para executar um cluster NLB.  
  
-   Apenas o protocolo TCP\/IP pode ser usado no adaptador para o qual o NLB seja habilitado em cada host. Não adicione todos os outros protocolos \(por exemplo, IPX\) para este adaptador.  
  
-   Os endereços IP dos servidores no cluster devem ser estáticos.  
  
> [!NOTE]  
> NLB não dá suporte a Dynamic Host Configuration Protocol \(DHCP\). O NLB desabilita o DHCP em cada interface que ele configura.  
  
## <a name="installation-information"></a>Informações sobre a instalação  
Você pode instalar o NLB usando o Gerenciador do servidor ou os comandos do Windows PowerShell para NLB.

Opcionalmente, você pode instalar as Ferramentas de Balanceamento de Carga de Rede para gerenciar um cluster NLB local ou remoto. As ferramentas incluem o Gerenciador de balanceamento de carga de rede e os comandos do PowerShell do Windows NLB.

### <a name="installation-with-server-manager"></a>Instalação com o Gerenciador do servidor

No Gerenciador do servidor, você pode usar o assistente Adicionar funções e recursos para adicionar o **balanceamento de carga de rede** recurso. Quando você concluir o assistente, o NLB é instalado e você não precisará reiniciar o computador.


### <a name="installation-with-windows-powershell"></a>Instalação com o Windows PowerShell  

Para instalar o NLB usando o Windows PowerShell, execute o seguinte comando em um prompt elevado do Windows PowerShell no computador onde você deseja instalar o NLB.

    
    Install-WindowsFeature NLB -IncludeManagementTools
    
Após a instalação for concluída, nenhuma reinicialização do computador é necessária.

Para obter mais informações, consulte [Install-WindowsFeature](https://docs.microsoft.com/powershell/module/servermanager/install-windowsfeature?view=win10-ps).

### <a name="network-load-balancing-manager"></a>Gerenciador de balanceamento de carga de rede
Para abrir o Gerenciador de Balanceamento de Carga de Rede em um Gerenciador do Servidor, clique em **Ferramentas** e depois clique em **Gerenciador de Balanceamento de Carga de Rede**.
  
## <a name="additional-resources"></a>Recursos adicionais  
A tabela a seguir fornece links para informações adicionais sobre o recurso NLB.  
  
|Tipo de conteúdo|Referências|  
|----------------|--------------|  
|Implantação|[Guia de implantação de balanceamento de carga de rede](https://technet.microsoft.com/library/cc754833(WS.10).aspx) &#124; [configurando com serviços de Terminal de balanceamento de carga de rede](https://technet.microsoft.com/library/cc771300(v=WS.10).aspx)|  
|Operações|[Gerenciar Clusters de balanceamento de carga de rede](https://technet.microsoft.com/library/cc753954(WS.10).aspx) &#124; [definindo parâmetros de balanceamento de carga de rede](https://technet.microsoft.com/library/cc731619(WS.10).aspx) &#124; [controlando Hosts em Clusters de balanceamento de carga de rede](https://technet.microsoft.com/library/cc770870(WS.10).aspx)|  
|Solução de problemas|[Solução de problemas de Clusters de balanceamento de carga de rede](https://technet.microsoft.com/library/cc732592(WS.10).aspx) &#124; [erros e eventos de Cluster NLB](https://technet.microsoft.com/library/cc731678(WS.10).aspx)|
|Ferramentas e configurações|[Cmdlets do PowerShell do Windows de balanceamento de carga de rede](https://go.microsoft.com/fwlink/p/?LinkId=238123)|
|Recursos da comunidade|[Alta disponibilidade \(Clustering\) fórum](https://go.microsoft.com/fwlink/p/?LinkId=230641)
