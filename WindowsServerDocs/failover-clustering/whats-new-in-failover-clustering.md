---
ms.assetid: 350aa5a3-5938-4921-93dc-289660f26bad
title: "Quais são as novidades no cluster de Failover no Windows Server"
ms.prod: windows-server-threshold
ms.technology: storage-failover-clustering
ms.topic: get-started-article
manager: dongill
author: JasonGerend
ms.author: jgerend
ms.date: 10/11/2016
ms.openlocfilehash: a4330f62095e13f2f4736f15924ed31fb4893e7a
ms.sourcegitcommit: 583355400f6b0d880dc0ac6bc06f0efb50d674f7
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 10/17/2017
---
# <a name="whats-new-in-failover-clustering-in-windows-server-2016"></a>Quais são as novidades no cluster de Failover no Windows Server 2016
> Aplica-se a: Windows Server (anual por canal), o Windows Server 2016

Este tópico explica as funcionalidades novas e alteradas no cluster de Failover no Windows Server 2016.  

## <a name="BKMK_RollingUpgrade"></a>Atualização do sistema operacional sem interrupção do cluster  
Um novo recurso no cluster de Failover, Cluster lançando a atualização do sistema operacional, permite que um administrador para atualizar o sistema operacional de nós de cluster do Windows Server 2012 R2 para o Windows Server 2016 sem interromper o Hyper-V ou as cargas de trabalho do servidor de arquivos Scale-Out. Usando esse recurso, as penalidades de tempo de inatividade contra contratos de nível de serviço (SLA) podem ser evitadas.  

**O valor adicione essa alteração?**  

Atualizando um cluster Hyper-V ou servidor de arquivos de Scale-Out do Windows Server 2012 R2 para o Windows Server 2016 não requer tempo de inatividade. O cluster continuarão a funcionar em um nível do Windows Server 2012 R2 até que todos os nós de cluster estiverem executando o Windows Server 2016. O nível funcional do cluster for atualizado para o Windows Server 2016 usando o Windows PowerShell cmdlt `Update-ClusterFunctionalLevel`.  

> [!WARNING]  
> -   Depois de atualizar o nível funcional do cluster, você não poderá voltar para um nível funcional do Windows Server 2012 R2 cluster.  
> -   Até que o `Update-ClusterFunctionalLevel`cmdlet é executado, o processo é reversível e nós do Windows Server 2012 R2 podem ser adicionados e Windows Server 2016 nós podem ser removidas.  

**O que funciona de modo diferente?**  

Um cluster de failover Hyper-V ou servidor de arquivos de Scale-Out facilmente agora pode ser atualizada sem qualquer tempo de inatividade ou precisar criar um novo cluster conosco que estão executando o sistema operacional Windows Server 2016. Migrando clusters ao Windows Server 2012 R2 envolvidos tirando offline no cluster existente e reinstalando o novo sistema operacional para cada nós e, em seguida, trazendo o cluster on-line. O processo antigo era complicado e necessário tempo de inatividade. No entanto, no Windows Server 2016, o cluster não precisa entrar no modo offline a qualquer momento.  

Os sistemas operacionais de cluster para a atualização em fases são para cada nó em um cluster:  
-   O nó é pausado e drenagem de todas as máquinas virtuais que estão em execução.  
-   As máquinas virtuais (ou outra carga de trabalho do cluster) é migrada para outro nó no cluster cluster.The são migradas para outro nó no cluster.  
-   O sistema operacional existente é removido e uma instalação limpa do sistema operacional Windows Server 2016 no nó é realizada.  
-   O nó que executam o sistema operacional Windows Server 2016 é adicionado de volta para o cluster.  
-   Neste ponto, o cluster deve estar em execução no modo misto, porque os nós de cluster estão executando o Windows Server 2012 R2 ou Windows Server 2016.  
-   O nível funcional do cluster permaneça no Windows Server 2012 R2. Nesse nível funcional, novos recursos no Windows Server 2016 que afetam a compatibilidade com versões anteriores do sistema operacional não estará disponíveis.  
-   Eventualmente, todos os nós forem atualizados para o Windows Server 2016.  
-   Nível funcional do cluster for alterada para o Windows Server 2016 usando o cmdlet do Windows PowerShell `Update-ClusterFunctionalLevel`. Neste ponto, você pode tirar proveito dos recursos do Windows Server 2016.  

Para obter mais informações, consulte [Cluster lançando a atualização do sistema operacional ](cluster-operating-system-rolling-upgrade.md).  

## <a name="BKMK_SR"></a>Réplica de armazenamento  
Armazenamento réplica é um novo recurso que permite independentes do tipo de armazenamento, o nível de bloco, síncrona replicação entre servidores ou clusters para recuperação de desastres, bem como alongamento de um cluster de failover entre sites. Replicação síncrona permite o espelhamento de dados em locais físicos com volumes de falha consistente para garantir a perda de dados no nível do sistema de arquivos. Replicação assíncrona permite que a extensão do site além metropolitanas intervalos com a possibilidade de perda de dados.  

**O valor adicione essa alteração?**  

Armazenamento réplica permite que você faça o seguinte:  

-   Fornecer uma única solução de recuperação de desastres do fornecedor para planejadas e falhas no fornecimento de cargas de trabalho crítico missão.  

-   Use para SMB3 transporte com o desempenho, escalabilidade e confiabilidade comprovada.  

-   Amplie clusters de failover do Windows para distâncias metropolitanas.  

-   Use o software da Microsoft ponta a ponta para armazenamento e clusters, como Hyper-V, réplica de armazenamento, espaços de armazenamento, Cluster, servidor de arquivos Scale-Out, para SMB3, duplicação de dados e ReFS/NTFS.  

-   Ajuda a reduzir o custo e a complexidade da seguinte maneira:  

    -   É independente de hardware, sem a necessidade de uma configuração de armazenamento específico como DAS ou SAN.  

    -   Permite a mercadoria tecnologias de rede e armazenamento.  

    -   Facilidade de recursos de gerenciamento de gráfico para nós individuais e clusters pelo Gerenciador de Cluster de Failover.  

    -   Inclui opções de script abrangentes em grande escala através do Windows PowerShell.  

-   Ajuda a reduzir o tempo de inatividade e aumentar a confiabilidade e a produtividade intrínseca ao Windows.  

-   Fornece suporte, as métricas de desempenho e recursos de diagnósticos.  

Para obter mais informações, consulte o [réplica de armazenamento no Windows Server 2016](../storage/storage-replica/storage-replica-overview.md).  


## <a name="BKMK_CloudWitness"></a>Testemunha de nuvem  
Testemunha na nuvem é um novo tipo de testemunha de quorum de Cluster de Failover no Windows Server 2016 que aproveita o Microsoft Azure como o ponto de arbitragem. Testemunha na nuvem, como qualquer outra testemunha quorum, obtém uma votação e pode participar de cálculos de quorum. Você pode configurar testemunha de nuvem como testemunha quorum usando a configurar um Assistente de Quorum do Cluster.  

**O valor adicione essa alteração?**  

Usando a nuvem testemunha como um Cluster de Failover testemunha quorum fornece as seguintes vantagens:  

-   Aproveita o Microsoft Azure e elimina a necessidade de um terceiro datacenter separado.  

-   Usa o padrão disponíveis publicamente Blob de armazenamento do Microsoft Azure que elimina a sobrecarga de manutenção adicionais de VMs hospedado em uma nuvem pública.  

-   Mesma conta da Microsoft Azure armazenamento pode ser usada para vários clusters (arquivo de um blob por cluster; usado como nome de arquivo do blob de id exclusivo do cluster).  

-   Fornece um custo contínuo muito baixo para a conta de armazenamento (muito pequenos dados gravados por arquivo de blob, arquivo de blob atualizado somente uma vez quando muda o estado de nós de cluster).  

Para obter mais informações, consulte [implantar uma nuvem testemunha para um Cluster de Failover ](deploy-cloud-witness.md).  

**O que funciona de modo diferente?**  

Essa funcionalidade é nova no Windows Server 2016.  

## <a name="BKMK_VMs"></a>Máquina virtual resiliência  
**Calcular resiliência** Windows Server 2016 inclui máquinas virtuais maior resiliência de computação para ajudar a reduzir problemas de comunicação dentro do cluster no cluster de computação da seguinte maneira: 

-   **Opções de resiliência disponíveis para máquinas virtuais:** agora você pode configurar opções de resiliência de máquina virtual que definem o comportamento das máquinas virtuais durante falhas transitórias:  

    -   **Nível de resiliência:** ajuda a definir como são manipuladas as falhas transitórias.  

    -   **Período de resiliência:** ajuda a definir quanto tempo todas as máquinas virtuais podem ser executados isolado.  

-   **Quarentena de nós não íntegros:** nós não íntegros estão em quarentena e não têm permissão para ingressar no cluster. Isso impede que nós falta de sincronismo afetar negativamente o cluster geral e outros nós.  

Para mais informações máquina virtual computação resiliência fluxo de trabalho e o nó de quarentena configurações que controlam como seu nó é colocado em quarentena ou isolamento, consulte [resiliência de computação de máquina Virtual no Windows Server 2016](http://blogs.msdn.com/b/clustering/archive/2015/06/03/10619308.aspx).  

**Armazenamento resiliência** no Windows Server 2016, máquinas virtuais são mais resilientes a falhas de armazenamento temporário. A máquina virtual aprimorado resiliência ajuda a preservar estados de sessão de máquina virtual de locatário caso ocorra uma interrupção de armazenamento. Isso é alcançado pela resposta rápida e inteligente máquina virtual para problemas de infraestrutura de armazenamento.  

Quando uma máquina virtual se desconectar da é armazenamento subjacente, ele pausa e aguarda armazenamento recuperar. Enquanto em pausa, a máquina virtual mantém o contexto de aplicativos que estão em execução. Quando a conexão da máquina virtual para o armazenamento é restaurado, a máquina virtual retorna ao seu estado em execução. Como resultado, o estado da sessão da máquina de locatário é mantido em recuperação.  

No Windows Server 2016, resiliência de armazenamento de máquina virtual é otimizado para clusters de convidado e reconhecimento muito.  

## <a name="BKMK_Diagnostics"></a>Melhorias de diagnósticos no cluster de Failover  
Para ajudar a diagnosticar problemas com clusters de failover, Windows Server 2016 inclui o seguinte:  

-   Várias melhorias para arquivos de log de cluster (por exemplo, o log de informações de fuso horário e DiagnosticVerbose) que faz com que é mais fácil solucionar problemas de cluster de failover. Para obter mais informações, consulte [Windows Server 2016 Cluster solução de problemas de melhorias de Failover - Cluster Log](http://blogs.msdn.com/b/clustering/archive/2015/05/15/10614930.aspx).  

-   Um despejo de memória de um novo tipo de **despejo de memória ativa**, que filtra a maioria das páginas de memória alocada para máquinas virtuais e, portanto, faz o memory.dmp muito menor e mais fáceis de salvar ou copiar.  Para obter mais informações, consulte [Windows Server 2016 Cluster solução de problemas de melhorias de Failover - despejar Active](http://blogs.msdn.com/b/clustering/archive/2015/05/18/10615526.aspx).  

## <a name="BKMK_SiteAware"></a>Clusters de Failover reconhecimento de sites  
Windows Server 2016 inclui site - clusters de failover ciente de que permitem que nós de grupo nos clusters esticadas com base na sua localização física (site). Reconhecimento de sites do cluster aprimora operações principais durante o ciclo de vida do cluster, como o comportamento de failover, políticas de posicionamento, pulsação entre os nós e comportamento de quorum. Para obter mais informações, consulte [reconhecimento de Site Clusters de Failover no Windows Server 2016](http://blogs.msdn.com/b/clustering/archive/2015/08/19/10636304.aspx).  

## <a name="BKMK_multidomainclusters"></a>Grupo de trabalho e vários domínios clusters  
No Windows Server 2012 R2 e versões anteriores, só é possível criar um cluster entre nós de membro que tenha ingressados no mesmo domínio.   Windows Server 2016 essas barreiras e apresenta a capacidade de criar um Cluster de Failover sem dependências do Active Directory. Agora você pode criar clusters de failover nas seguintes configurações:  

-   **Clusters de domínio único.** Clusters com todos os nós que tenha ingressados no mesmo domínio.  

-   **Clusters de vários domínios.** Clusters conosco que são membros de domínios diferentes.  

-   **Clusters de grupo de trabalho.** Clusters de nós que são servidores membro / grupo de trabalho (não do domínio associado).  

Para obter mais informações, consulte [clusters de grupo de trabalho e vários domínios no Windows Server 2016](http://blogs.msdn.com/b/clustering/archive/2015/08/17/10635825.aspx)  
## <a name="BKMK_VMLoadBalancing"></a>Balanceamento de carga de máquina virtual  
Balanceamento de carga de máquina virtual é um novo recurso no cluster de Failover que facilita o balanceamento de carga perfeita de máquinas virtuais em todos os nós em um cluster. Nós excesso comprometidos são identificados com base na máquina virtual memória e a utilização da CPU no nó. Máquinas virtuais são movidas (live migrados) de um nó excesso comprometido para nós com largura de banda disponível (se aplicável). Agressividade do balanceamento pode ser ajustada para garantir a utilização e o desempenho ideal de cluster.  Balanceamento de carga é habilitada por padrão no Windows Server 2016 Technical Preview. No entanto, a balanceamento de carga é desativado quando a otimização dinâmica SCVMM for ativada.  

## <a name="BKMK_VMStartOrder"></a>Ordem de inicialização de máquina virtual  
Ordem de começar a máquina virtual é um novo recurso no cluster de Failover que apresenta iniciar ordem coordenação para máquinas virtuais (e todos os grupos) em um cluster. Máquinas virtuais agora podem ser agrupadas em camadas e iniciar ordem dependências podem ser criadas entre diferentes níveis. Isso garante que as máquinas virtuais mais importantes (como controladores de domínio ou utilitário máquinas virtuais) são iniciadas primeiro. Máquinas virtuais não são iniciadas até que as máquinas virtuais que eles têm uma dependência também são iniciadas.  

## <a name="BKMK_SMBMultiChannel"></a>Vários canais de SMB simplificada e redes de Cluster Multi-NIC  
Redes de Cluster de failover não estão mais limitados a uma única NIC por sub-rede / de rede. Com vários canais de SMB simplificado e Multi-NIC Cluster redes, a configuração de rede é automática e cada placa de rede na sub-rede pode ser usada para o tráfego de cluster e a carga de trabalho. Essa melhoria permite que os clientes maximizar a taxa de transferência de rede para outras cargas de trabalho SMB, instância de Cluster de Failover do SQL Server e Hyper-V.  

Para obter mais informações, consulte [simplificado SMB multicanal e redes de Cluster Multi-NIC](smb-multichannel.md).

## <a name="see-also"></a>Consulte também  
* [Armazenamento](../storage/storage.md)  
* [O que há de novo no armazenamento no Windows Server 2016](../storage/whats-new-in-storage.md)  
