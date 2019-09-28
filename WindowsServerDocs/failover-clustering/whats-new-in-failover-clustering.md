---
ms.assetid: 350aa5a3-5938-4921-93dc-289660f26bad
title: O que há de novo no clustering de failover no Windows Server
ms.prod: windows-server
ms.technology: storage-failover-clustering
ms.topic: get-started-article
manager: dongill
author: JasonGerend
ms.author: jgerend
ms.date: 10/18/2018
ms.openlocfilehash: 26417f0fdbe2c4c8c374b3a1b8955c6297865397
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71360827"
---
# <a name="whats-new-in-failover-clustering"></a>Novidades no Clustering de Failover

> Aplica-se a: Windows Server 2019, Windows Server 2016

Este tópico explica as funcionalidades novas e alteradas no clustering de failover do Windows Server 2019 e do Windows Server 2016.

## <a name="whats-new-in-windows-server-2019"></a>Novidades no Windows Server 2019

- **Conjuntos de cluster**

    Os conjuntos de clusters permitem aumentar o número de servidores em uma única solução SDDC (Datacenter) definida pelo software além dos limites atuais de um cluster. Isso é feito agrupando-se vários clusters em um conjunto de clusters, um agrupamento livremente acoplado de vários clusters de failover: computação, armazenamento e hiperconvergente.
    Com conjuntos de clusters, você pode mover máquinas virtuais online (migrações ao vivo) entre clusters no conjunto de clusters.

    Para obter mais informações, consulte [conjuntos de clusters](../storage/storage-spaces/cluster-sets.md).

- **Clusters com suporte ao Azure**

    Os clusters de failover agora detectam automaticamente quando estão em execução em máquinas virtuais IaaS do Azure e otimizam a configuração para fornecer failover proativo e registro em log de eventos de manutenção planejada do Azure para atingir os níveis mais altos de disponibilidade. A implantação também é simplificada com a remoção da necessidade de configurar o balanceador de carga com o nome de rede dinâmico para o nome do cluster.

- **Migração de cluster entre domínios**

    Os clusters de failover agora podem ser movidos dinamicamente de um domínio Active Directory para outro, simplificando a consolidação de domínio e permitindo que os clusters sejam criados por parceiros de hardware e ingressados no domínio do cliente posteriormente.
- **Testemunha USB**

    Agora você pode usar uma unidade USB simples anexada a um comutador de rede como uma testemunha para determinar o quorum para um cluster. Isso estende a testemunha de compartilhamento de arquivos para dar suporte a qualquer dispositivo compatível com SMB2.

- **Melhorias de infraestrutura de cluster**

    O cache CSV agora é habilitado por padrão para aumentar o desempenho da máquina virtual. MSDTC agora oferece suporte a Volumes Compartilhados do Cluster, para permitir a implantação de cargas de trabalho MSDTC em espaços de armazenamento diretos, como com o SQL Server. Lógica aprimorada para detectar nós particionados com autorrecuperação para retornar nós à associação de cluster. Detecção de rota de rede do cluster e autorrecuperação avançadas.

- **A atualização com suporte a cluster é compatível com os Espaços de Armazenamento Diretos**

    A Atualização com Suporte a Cluster (CAU) agora está integrada e tem suporte aos Espaços de Armazenamento Diretos, validando e garantindo que a ressincronização de dados seja concluída em cada nó. A atualização com reconhecimento de cluster inspeciona as atualizações para reiniciar de modo inteligente apenas se necessário. Isso permite que a orquestração reinicie de todos os servidores no cluster para manutenção planejada.

- **Aprimoramentos de testemunha de compartilhamento de arquivos** Habilitamos o uso de uma testemunha de compartilhamento de arquivos nos seguintes cenários: 
  - Acesso à Internet ausente ou extremamente fraco devido a um local remoto, impedindo o uso de uma testemunha de nuvem. 
  - Falta de unidades compartilhadas para uma testemunha de disco. Isso pode ser uma Espaços de Armazenamento Diretos configuração de hiperconvergente, uma SQL Server Always On AG (grupos de disponibilidade) ou um * DAG (grupo de disponibilidade de banco de dados do Exchange), nenhum deles usa discos compartilhados. 
  - Falta de uma conexão de controlador de domínio devido ao cluster estar atrás de uma DMZ. 
  - Um grupo de trabalho ou cluster de domínio cruzado para o qual não há Active Directory CNO (objeto de nome de cluster). Saiba mais sobre esses aprimoramentos na postagem a seguir nos Blogs de gerenciamento de & de servidor: Testemunha de compartilhamento de arquivos de cluster de failover e DFS.
    
    Agora, também bloqueamos explicitamente o uso de um compartilhamento de namespaces DFS como um local. A adição de uma testemunha de compartilhamento de arquivos a um compartilhamento DFS pode causar problemas de estabilidade para o cluster, e essa configuração nunca foi suportada. Adicionamos lógica para detectar se um compartilhamento usa namespaces DFS e se os namespaces do DFS são detectados, Gerenciador de Cluster de Failover bloqueia a criação da testemunha e exibe uma mensagem de erro sobre não ter suporte.
- **Proteção de cluster**

    Comunicação dentro do cluster sobre Server Message Block (SMB) para Volumes Compartilhados do Cluster e espaços de armazenamento diretos agora aproveita os certificados para fornecer a plataforma mais segura. Isso permite que os Clusters de Failover operar sem nenhuma dependência do NTLM e habilitar linhas de base de segurança.
- **O Cluster de Failover não usa mais a autenticação NTLM**

    Os clusters de failover não usam mais a autenticação NTLM. Em vez disso, o Kerberos e a autenticação baseada em certificado são usados exclusivamente. Não há nenhuma alteração exigida pelo usuário ou por ferramentas de implantação para aproveitar esse aprimoramento de segurança. Ele também permite que os clusters de failover sejam implantados em ambientes em que o NTLM foi desabilitado. 


## <a name="whats-new-in-windows-server-2016"></a>O que há de novo no Windows Server 2016

### <a name="BKMK_RollingUpgrade"></a>Atualização sem interrupção do sistema operacional do cluster

A atualização sem interrupção do sistema operacional do cluster permite que um administrador atualize o sistema operacional dos nós do cluster do Windows Server 2012 R2 para uma versão mais recente sem interromper o Hyper-V ou as cargas de trabalho de Servidor de Arquivos de Escalabilidade Horizontal. Usando esse recurso, as penalidades de tempo de inatividade em SLAs (Contratos de Nível de Serviço) podem ser evitadas. 

**Qual é o valor agregado desta alteração?**  

A atualização de um cluster Hyper-V ou Servidor de Arquivos de Escalabilidade Horizontal do Windows Server 2012 R2 para o Windows Server 2016 não requer mais tempo de inatividade. O cluster continuará a funcionar em um nível do Windows Server 2012 R2 até que todos os nós no cluster estejam executando o Windows Server 2016. O nível funcional do cluster é atualizado para o Windows Server 2016 usando o cmdlt do Windows PowerShell `Update-ClusterFunctionalLevel`. 

> [!WARNING]  
> -   Depois de atualizar o nível funcional do cluster, você não pode voltar para um nível funcional de cluster do Windows Server 2012 R2. 
> -   Até que o cmdlet `Update-ClusterFunctionalLevel` seja executado, o processo é reversível e os nós do Windows Server 2012 R2 podem ser adicionados e os nós do Windows Server 2016 podem ser removidos. 

**O que passou a funcionar de maneira diferente?**  

Um cluster de failover do Hyper-V ou Servidor de Arquivos de Escalabilidade Horizontal agora pode ser facilmente atualizado sem qualquer tempo de inatividade ou necessidade de criar um novo cluster com nós que estejam executando o sistema operacional Windows Server 2016. Migrar clusters para o Windows Server 2012 R2 envolvido colocando o cluster existente offline e reinstalando o novo sistema operacional para cada nó e, em seguida, colocando o cluster novamente online. O processo antigo era trabalhoso e exigia tempo de inatividade. No entanto, no Windows Server 2016, o cluster não precisa ficar offline em nenhum momento. 

Os sistemas operacionais de cluster para a atualização em fases são os seguintes para cada nó em um cluster:  
-   O nó é pausado e drenado de todas as máquinas virtuais em execução nela. 
-   As máquinas virtuais (ou outras cargas de trabalho de cluster) são migradas para outro nó no cluster. 
-   O sistema operacional existente é removido e uma instalação limpa do sistema operacional Windows Server 2016 no nó é executada. 
-   O nó que executa o sistema operacional Windows Server 2016 é adicionado de volta ao cluster. 
-   Neste ponto, o cluster deve estar sendo executado no modo misto, pois os nós de cluster estão executando o Windows Server 2012 R2 ou o Windows Server 2016. 
-   O nível funcional do cluster permanece no Windows Server 2012 R2. Nesse nível funcional, os novos recursos do Windows Server 2016 que afetam a compatibilidade com as versões anteriores do sistema operacional ficarão indisponíveis. 
-   Eventualmente, todos os nós são atualizados para o Windows Server 2016. 
-   O nível funcional do cluster é então alterado para Windows Server 2016 usando o cmdlet do Windows PowerShell `Update-ClusterFunctionalLevel`. Neste ponto, você pode aproveitar os recursos do Windows Server 2016. 

Para obter mais informações, consulte [atualização sem interrupção do sistema operacional do cluster](cluster-operating-system-rolling-upgrade.md). 

### <a name="BKMK_SR"></a>Réplica de armazenamento  
A réplica de armazenamento é um novo recurso que permite a replicação síncrona de nível de bloco e independente de armazenamento entre servidores ou clusters para recuperação de desastre, bem como o alargamento de um cluster de failover entre sites. A replicação síncrona habilita o espelhamento de dados em locais físicos com volumes consistentes com falha para garantir perda zero de dados no nível do sistema de arquivos. A replicação assíncrona permite a extensão de site além das dimensões metropolitanas com a possibilidade de perda de dados. 

**Qual é o valor agregado desta alteração?**  

A réplica de armazenamento permite que você faça o seguinte:  

-   Fornecer uma solução de recuperação de desastre de um único fornecedor para interrupções planejadas e não planejadas de cargas de trabalho críticas. 

-   Usar o transporte SMB3 com desempenho, escalabilidade e confiabilidade comprovados. 

-   Alongar clusters de failover do Windows para distâncias metropolitanas. 

-   Use o software da Microsoft de ponta a ponta para armazenamento e clustering, como Hyper-V, réplica de armazenamento, espaços de armazenamento, cluster, Servidor de Arquivos de Escalabilidade Horizontal, SMB3, eliminação de duplicação de dados e ReFS/NTFS. 

-   Ajudar a reduzir o custo e a complexidade da seguinte maneira:  

    -   É independente de hardware, sem a necessidade de uma configuração de armazenamento específica, como DAS ou SAN. 

    -   Permite armazenamento de mercadorias e tecnologias de rede. 

    -   Conta com facilidade de gerenciamento gráfico para nós individuais e clusters por meio do Gerenciador de Cluster de Failover. 

    -   Inclui opções de script abrangentes em grande escala por meio do Windows PowerShell. 

-   Ajudar a reduzir o tempo de inatividade e aumentar a confiabilidade e a produtividade intrínsecas ao Windows. 

-   Fornecer capacidade de suporte, métricas de desempenho e recursos de diagnóstico. 

Para saber mais, consulte [Novidades na Réplica de Armazenamento no Windows Server 2016](../storage/storage-replica/storage-replica-overview.md). 


### <a name="BKMK_CloudWitness"></a>Testemunha de nuvem  
Testemunha de nuvem é um novo tipo de testemunha de quorum de Cluster de Failover no Windows Server 2016 que utiliza o Microsoft Azure como o ponto de arbitragem. A Testemunha de nuvem, como qualquer outra testemunha de quorum, obtém um voto e pode participar dos cálculos de quorum. Você pode configurar a testemunha de nuvem como uma testemunha de quorum usando o Configurar um Assistente para Quorum do Cluster. 

**Qual é o valor agregado desta alteração?**  

Usar a testemunha de nuvem como uma testemunha de quorum de cluster de failover oferece as seguintes vantagens:  

-   Aproveita Microsoft Azure e elimina a necessidade de um datacenter separado. 

-   Usa o armazenamento de BLOBs disponível publicamente Microsoft Azure, o que elimina a sobrecarga de manutenção extra das VMs hospedadas em uma nuvem pública. 

-   A mesma conta de Armazenamento do Microsoft Azure pode ser usada para vários clusters (um arquivo de blob por cluster; ID exclusiva do cluster usada como nome do arquivo de BLOB). 

-   Fornece um custo de andamento muito baixo para a conta de armazenamento (dados muito pequenos gravados por arquivo de BLOB, arquivo de blob atualizado apenas uma vez quando o estado dos nós de cluster é alterado). 

Para obter mais informações, consulte [implantar uma testemunha em nuvem para um cluster de failover](deploy-cloud-witness.md). 

**O que passou a funcionar de maneira diferente?**  

Esse recurso é novo no Windows Server 2016. 

### <a name="BKMK_VMs"></a>Resiliência da máquina virtual  
**Resiliência de computação** O Windows Server 2016 inclui maior resiliência de computação de máquinas virtuais para ajudar a reduzir problemas de comunicação dentro do cluster em seu cluster de computação da seguinte maneira: 

-   **Opções de resiliência disponíveis para máquinas virtuais:**  Agora você pode configurar opções de resiliência de máquina virtual que definem o comportamento das máquinas virtuais durante falhas transitórias:  

    -   **Nível de resiliência:** Ajuda a definir como as falhas transitórias são tratadas. 

    -   **Período de resiliência:**  Ajuda a definir por quanto tempo todas as máquinas virtuais têm permissão para serem executadas isoladas. 

-   **Quarentena de nós não íntegros:** Os nós não íntegros são colocados em quarentena e não têm mais permissão para ingressar no cluster. Isso impede que nós oscilantes afetem negativamente outros nós e o cluster geral. 

Para obter mais informações sobre o fluxo de trabalho de resiliência de computação de máquina virtual e configurações de quarentena de nó que controlam como o nó é colocado em isolamento ou quarentena, consulte [resiliência de computação de máquina virtual no Windows Server 2016](http://blogs.msdn.com/b/clustering/archive/2015/06/03/10619308.aspx). 

**Resiliência de armazenamento** No Windows Server 2016, as máquinas virtuais são mais resilientes a falhas de armazenamento transitórias. A resiliência de máquina virtual aprimorada ajuda a preservar os Estados de sessão de máquina virtual de locatário no caso de uma interrupção de armazenamento. Isso é obtido por uma resposta inteligente e rápida de máquina virtual para problemas de infraestrutura de armazenamento. 

Quando uma máquina virtual se desconecta de seu armazenamento subjacente, ela pausa e aguarda o armazenamento ser recuperado. Enquanto está em pausa, a máquina virtual retém o contexto dos aplicativos que estão sendo executados nele. Quando a conexão da máquina virtual com seu armazenamento é restaurada, a máquina virtual retorna ao seu estado de execução. Como resultado, o estado de sessão da máquina de locatário é retido na recuperação. 

No Windows Server 2016, a resiliência de armazenamento de máquina virtual também está ciente e otimizado para clusters de convidado. 

### <a name="BKMK_Diagnostics"></a>Aprimoramentos de diagnóstico no clustering de failover  
Para ajudar a diagnosticar problemas com clusters de failover, o Windows Server 2016 inclui o seguinte:  

-   Vários aprimoramentos nos arquivos de log de cluster (como informações de fuso horário e log DiagnosticVerbose) facilitam a solução de problemas de clustering de failover. Para obter mais informações, consulte [aprimoramentos de solução de problemas do cluster de failover do Windows Server 2016-log de cluster](http://blogs.msdn.com/b/clustering/archive/2015/05/15/10614930.aspx). 

-   Um novo tipo de despejo de **despejo de memória ativo**, que filtra a maioria das páginas de memória alocadas para máquinas virtuais e, portanto, torna o Memory. dmp muito menor e mais fácil de salvar ou copiar. Para obter mais informações, consulte [aprimoramentos de solução de problemas do cluster de failover do Windows Server 2016-despejo ativo](http://blogs.msdn.com/b/clustering/archive/2015/05/18/10615526.aspx). 

### <a name="BKMK_SiteAware"></a>Clusters de failover com reconhecimento de site  
O Windows Server 2016 inclui clusters de failover com reconhecimento de site que habilitam nós de grupo em clusters ampliados com base em seu local físico (site). O reconhecimento de site de cluster aprimora as principais operações durante o ciclo de vida do cluster, como comportamento de failover, políticas de posicionamento, pulsação entre os nós e o comportamento de quorum. Para obter mais informações, consulte [clusters de failover com reconhecimento de site no Windows Server 2016](http://blogs.msdn.com/b/clustering/archive/2015/08/19/10636304.aspx). 

### <a name="BKMK_multidomainclusters"></a>Clusters de grupo de trabalho e vários domínios  
No Windows Server 2012 R2 e versões anteriores, um cluster só pode ser criado entre nós membro ingressados no mesmo domínio. O Windows Server 2016 quebra essas barreiras e apresenta a capacidade de criar um Cluster de failover sem dependências do Active Directory. Agora você pode criar clusters de failover nas seguintes configurações:  

-   **Clusters de domínio único.** Clusters com todos os nós ingressados no mesmo domínio. 

-   **Clusters de vários domínios.** Clusters com nós que são membros de domínios diferentes. 

-   **Clusters de grupo de trabalho.** Clusters com nós que são servidores membros/grupo de trabalho (não ingressados no domínio). 

Para obter mais informações, consulte [clusters de vários domínios e grupos de trabalho no Windows Server 2016](http://blogs.msdn.com/b/clustering/archive/2015/08/17/10635825.aspx)  
### <a name="BKMK_VMLoadBalancing"></a>Balanceamento de carga de máquina virtual  
O balanceamento de carga de máquina virtual é um novo recurso no clustering de failover que facilita o balanceamento de carga contínuo de máquinas virtuais entre os nós em um cluster. Nós de excesso de confirmações são identificados com base na memória da máquina virtual e na utilização da CPU no nó. As máquinas virtuais são então movidas (migradas ao vivo) de um nó com excesso de confirmações para nós com largura de banda disponível (se aplicável). A agressividade do balanceamento pode ser ajustada para garantir o desempenho e a utilização ideais do cluster. O balanceamento de carga é habilitado por padrão no Windows Server 2016 Technical Preview. No entanto, o balanceamento de carga é desabilitado quando a otimização dinâmica do SCVMM está habilitada. 

### <a name="BKMK_VMStartOrder"></a>Ordem de início da máquina virtual  
A ordem de início da máquina virtual é um novo recurso no clustering de failover que introduz a orquestração de ordem de início para máquinas virtuais (e todos os grupos) em um cluster. As máquinas virtuais agora podem ser agrupadas em camadas, e as dependências de ordem de início podem ser criadas entre diferentes camadas. Isso garante que as máquinas virtuais mais importantes (como controladores de domínio ou máquinas virtuais do utilitário) sejam iniciadas primeiro. As máquinas virtuais não são iniciadas até que as máquinas virtuais nas quais elas têm uma dependência também sejam iniciadas. 

### <a name="BKMK_SMBMultiChannel"></a>Redes de cluster de vários canais e multichannels SMB simplificadas  
As redes de cluster de failover não são mais limitadas a uma única NIC por sub-rede/rede. Com redes SMB simplificadas de cluster Multichannel e MultiNIC, a configuração de rede é automática e cada NIC na sub-rede pode ser usada para tráfego de cluster e carga de trabalho. Esse aprimoramento permite que os clientes maximizem a taxa de transferência de rede para o Hyper-V, SQL Server instância de cluster de failover e outras cargas de trabalho SMB. 

Para obter mais informações, consulte [redes de cluster de vários canais e multiplataforma SMB simplificadas](smb-multichannel.md).

## <a name="see-also"></a>Consulte também  
* [Armazenamento](../storage/storage.md)  
* [O que há de novo no armazenamento no Windows Server 2016](../storage/whats-new-in-storage.md)  
