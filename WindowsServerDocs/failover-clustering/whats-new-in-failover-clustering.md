---
ms.assetid: 350aa5a3-5938-4921-93dc-289660f26bad
title: O que há de novo no Clustering de Failover do Windows Server
ms.prod: windows-server-threshold
ms.technology: storage-failover-clustering
ms.topic: get-started-article
manager: dongill
author: JasonGerend
ms.author: jgerend
ms.date: 10/18/2018
ms.openlocfilehash: 3c0792347aaa70fe80d346cc51cbc44b73c42f39
ms.sourcegitcommit: ed27ddbe316d543b7865bc10590b238290a2a1ad
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 05/09/2019
ms.locfileid: "65476016"
---
# <a name="whats-new-in-failover-clustering"></a>Novidades no Clustering de Failover

> Aplica-se a: Windows Server 2019, Windows Server 2016

Este tópico explica as funcionalidades novas e alteradas no Clustering do Failover para o Windows Server 2019 e Windows Server 2016.

## <a name="whats-new-in-windows-server-2019"></a>Novidades no Windows Server 2019

- **Conjuntos de cluster**

    Conjuntos de cluster permitem que você aumente o número de servidores em uma solução única definida pelo software SDDC (datacenter), além dos limites atuais de um cluster. Isso é feito através do agrupamento de vários clusters em um conjunto de cluster – um agrupamento flexível de vários clusters de failover: computação, armazenamento e hiperconvergentes.
    Com conjuntos de cluster, você pode mover máquinas virtuais online (migração ao vivo) entre clusters dentro do cluster definido.

    Para obter mais informações, consulte [conjuntos de Cluster](../storage/storage-spaces/cluster-sets.md).

- **Clusters com suporte do Azure**

    Clusters de failover agora automaticamente detectam quando estiver em execução em máquinas virtuais de IaaS do Azure e otimizar a configuração para fornecer um failover proativo e registro em log de eventos de manutenção planejada do Azure para atingir os mais altos níveis de disponibilidade. Implantação também é simples, removendo a necessidade de configurar o balanceador de carga com o nome de rede dinâmico para o nome do cluster.

- **Migração de cluster entre domínios**

    Clusters de failover pode agora mover dinamicamente de um domínio do Active Directory para outro, simplificando a consolidação de domínio e permitindo que os clusters sejam criados por parceiros de hardware e Unidos ao domínio do cliente mais tarde.
- **Testemunha USB**

    Agora você pode usar uma unidade USB simple anexada a um comutador de rede como uma testemunha de quorum para um cluster de determinar. Isso estende a testemunha de compartilhamento de arquivo para dar suporte a qualquer dispositivo em conformidade SMB2.

- **Aprimoramentos da infraestrutura de cluster**

    Agora, o cache do CSV está habilitado por padrão para melhorar o desempenho da máquina virtual. MSDTC agora oferece suporte a Volumes Compartilhados do Cluster, para permitir a implantação de cargas de trabalho MSDTC em espaços de armazenamento diretos, como com o SQL Server. Lógica aprimorada para detectar nós particionados com autorrecuperação para retornar nós à associação de cluster. Detecção de rota de rede do cluster e autorrecuperação avançadas.

- **Atualização com suporte de cluster dá suporte a espaços de armazenamento diretos**

    A Atualização com Suporte a Cluster (CAU) agora está integrada e tem suporte aos Espaços de Armazenamento Diretos, validando e garantindo que a ressincronização de dados seja concluída em cada nó. A atualização com suporte do cluster inspeciona as atualizações de forma inteligente reiniciem somente se for necessário. Isso permite que as reinicializações de orquestração de todos os servidores no cluster para a manutenção planejada.

- **Aprimoramentos de testemunha de compartilhamento de arquivo** habilitamos o uso de uma testemunha de compartilhamento de arquivos nos seguintes cenários: 
    - Acesso à Internet ausente ou muito ruim devido a um local remoto, evitando o uso de uma testemunha de nuvem. 
    - Falta de unidades compartilhadas para uma testemunha de disco. Isso pode ser uma configuração hiperconvergente espaços de armazenamento diretos, um SQL Server sempre em grupos AG (disponibilidade), ou um * Exchange banco de dados do grupo de disponibilidade (DAG), nenhuma delas usar discos compartilhados. 
    - Falta de uma conexão do controlador de domínio devido a do cluster que está sendo por trás de uma rede de Perímetro. 
    - Um cluster de grupo de trabalho ou domínio cruzado para o qual existe não é nenhum objeto de nome de cluster (CNO) do Active Directory. Saiba mais sobre esses aprimoramentos na seguinte postagem no servidor de & gerenciamento Blogs: Testemunha de compartilhamento de arquivos do Cluster de failover e do DFS.
    
    Vamos agora também bloquear explicitamente o uso de um compartilhamento de Namespaces do DFS como um local. Adicionar uma testemunha de compartilhamento de arquivo para um DFS compartilhamento pode causar problemas de estabilidade do cluster e essa configuração nunca tem sido suportada. Adicionamos a lógica para detectar se um compartilhamento usa Namespaces do DFS, e se os Namespaces do DFS for detectado, o Gerenciador de Cluster de Failover bloqueia a criação da testemunha e exibe uma mensagem de erro sobre não tem suporte.
- **Proteção do cluster**

    Comunicação dentro do cluster sobre Server Message Block (SMB) para Volumes Compartilhados do Cluster e espaços de armazenamento diretos agora aproveita os certificados para fornecer a plataforma mais segura. Isso permite que os Clusters de Failover operar sem nenhuma dependência do NTLM e habilitar linhas de base de segurança.
- **Cluster de failover não usa mais a autenticação NTLM**

    Clusters de failover não for mais usar a autenticação NTLM. Em vez disso, autenticação Kerberos e baseada em certificado será utilizado exclusivamente. Não há nenhuma alteração é necessária pelo usuário ou ferramentas de implantação, para aproveitar esse aprimoramento de segurança. Ele também permite que os clusters de failover ser implantado em ambientes em que o NTLM foi desabilitada. 


## <a name="whats-new-in-windows-server-2016"></a>O que há de novo no Windows Server 2016

### <a name="BKMK_RollingUpgrade"></a>Atualização sem interrupção do sistema operacional do cluster

Atualização sem interrupção do cluster sistema operacional permite que um administrador atualizar o sistema operacional de nós do cluster do Windows Server 2012 R2 para uma versão mais recente, sem interromper o Hyper-V ou as cargas de trabalho do servidor de arquivos de escalabilidade horizontal. Usando esse recurso, as penalidades de tempo de inatividade em SLAs (Contratos de Nível de Serviço) podem ser evitadas. 

**Qual é o valor agregado desta alteração?**  

Atualizando um cluster do Hyper-V ou servidor de arquivos de escalabilidade horizontal do Windows Server 2012 R2 para o Windows Server 2016 não requer tempo de inatividade. O cluster continuará a funcionar em um nível do Windows Server 2012 R2, até que todos os nós no cluster estão executando o Windows Server 2016. O nível funcional do cluster é atualizado para o Windows Server 2016 usando o cmdlet do Windows PowerShell `Update-ClusterFunctionalLevel`. 

> [!WARNING]  
> -   Depois de atualizar o nível funcional do cluster, você não poderá voltar para um nível funcional de cluster do Windows Server 2012 R2. 
> -   Até que o `Update-ClusterFunctionalLevel` cmdlet é executado, o processo é reversível e nós do Windows Server 2012 R2 podem ser adicionados e nós do Windows Server 2016 podem ser removidos. 

**O que passou a funcionar de maneira diferente?**  

Um cluster de failover do Hyper-V ou servidor de arquivos de escalabilidade horizontal podem agora ser facilmente atualizado sem qualquer tempo de inatividade ou precisa para criar um novo cluster conosco que estão executando o sistema operacional Windows Server 2016. Migrando clusters para o Windows Server 2012 R2 envolvidos colocar o cluster existente offline e reinstalar o novo sistema operacional para cada nós e, em seguida, colocar o cluster novamente online. O processo antigo era complicado e necessário tempo de inatividade. No entanto, no Windows Server 2016, o cluster não precisa ficar offline a qualquer momento. 

Os sistemas operacionais de cluster para a atualização em fases são as seguintes para cada nó em um cluster:  
-   O nó está em pausa e esgotado de todas as máquinas virtuais que estão sendo executadas nele. 
-   As máquinas virtuais (ou outra carga de trabalho do cluster) é migrada para outro nó no cluster. 
-   O sistema operacional existente é removido e uma instalação limpa do sistema operacional Windows Server 2016 no nó é executada. 
-   O nó que executa o sistema operacional Windows Server 2016 é adicionado novamente ao cluster. 
-   Neste ponto, o cluster deve estar em execução no modo misto, porque os nós de cluster estão executando o Windows Server 2012 R2 ou Windows Server 2016. 
-   O nível funcional do cluster permanece no Windows Server 2012 R2. Nesse nível funcional, novos recursos no Windows Server 2016 que afetam a compatibilidade com versões anteriores do sistema operacional estará disponíveis. 
-   Eventualmente, todos os nós são atualizados para o Windows Server 2016. 
-   Nível funcional do cluster for alterada para o Windows Server 2016 usando o cmdlet do Windows PowerShell `Update-ClusterFunctionalLevel`. Neste ponto, você pode aproveitar os recursos do Windows Server 2016. 

Para obter mais informações, consulte [atualização sem interrupção do Cluster de sistema operacional](cluster-operating-system-rolling-upgrade.md). 

### <a name="BKMK_SR"></a>Réplica de armazenamento  
Réplica de armazenamento é um novo recurso que permite independente de armazenamento, a replicação síncrona em nível de bloco entre servidores ou clusters para recuperação de desastres, bem como expansão de um cluster de failover entre sites. A replicação síncrona habilita o espelhamento de dados em locais físicos com volumes consistentes com falha para garantir perda zero de dados no nível do sistema de arquivos. A replicação assíncrona permite a extensão de site além das dimensões metropolitanas com a possibilidade de perda de dados. 

**Qual é o valor agregado desta alteração?**  

A réplica de armazenamento permite que você faça o seguinte:  

-   Fornecer uma solução de recuperação de desastre de um único fornecedor para interrupções planejadas e não planejadas de cargas de trabalho críticas. 

-   Usar o transporte SMB3 com desempenho, escalabilidade e confiabilidade comprovados. 

-   Alongar clusters de failover do Windows para distâncias metropolitanas. 

-   Use o software da Microsoft ponta a ponta para armazenamento e clustering, como o Hyper-V, réplica de armazenamento, espaços de armazenamento, Cluster, servidor de arquivos de escalabilidade horizontal, SMB3, eliminação de duplicação de dados e ReFS/NTFS. 

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

Testemunha de quorum usando testemunha de nuvem como um Cluster de Failover oferece as seguintes vantagens:  

-   Utiliza o Microsoft Azure e elimina a necessidade de um datacenter separado de terceiro. 

-   Usa o padrão disponível publicamente Blob de armazenamento do Microsoft Azure que elimina a sobrecarga adicional de manutenção de máquinas virtuais hospedadas em uma nuvem pública. 

-   Mesma conta de armazenamento do Microsoft Azure pode ser usada para vários clusters (arquivo de um blob por cluster; usado como nome de arquivo de blob de id exclusiva do cluster). 

-   Fornece um custo muito baixo de contínuo para a conta de armazenamento (muito pequena de dados gravados por arquivo de blob, arquivo de blob atualizado apenas uma vez quando muda o estado de nós de cluster). 

Para obter mais informações, consulte [implantar uma nuvem testemunha para um Cluster de Failover](deploy-cloud-witness.md). 

**O que passou a funcionar de maneira diferente?**  

Esse recurso é novo no Windows Server 2016. 

### <a name="BKMK_VMs"></a>Resiliência de máquina virtual  
**Resiliência de computação** Windows Server 2016 inclui máquinas de virtuais maior resiliência de computação para ajudar a reduzir problemas de comunicação dentro do cluster no cluster de cálculo da seguinte maneira: 

-   **Opções de resiliência disponíveis para máquinas virtuais:**  Agora você pode configurar as opções de resiliência de máquina virtual que definem o comportamento das máquinas virtuais durante falhas transitórias:  

    -   **Nível de resiliência:** Ajuda a definir como as falhas transitórias são tratadas. 

    -   **Período de resiliência:**  Ajuda a definir quanto todas as máquinas virtuais podem ser executados isolado. 

-   **Quarentena de nós não íntegros:** Nós não íntegros são colocados em quarentena e não têm permissão para ingressar no cluster. Isso impede que nós oscilação afetando negativamente o cluster geral e outros nós. 

Para obter mais informações sobre máquina virtual computação resiliência fluxo de trabalho e o nó de quarentena configurações que controlam como o nó é colocado em quarentena ou de isolamento, consulte [resiliência de máquina Virtual de computação no Windows Server 2016](http://blogs.msdn.com/b/clustering/archive/2015/06/03/10619308.aspx). 

**Resiliência de armazenamento** no Windows Server 2016, as máquinas virtuais são mais resilientes a falhas transitórias de armazenamento. A resiliência de maior de máquina virtual ajuda a preservar a estados de sessão de máquina virtual de locatário caso haja uma interrupção de armazenamento. Isso é feito pela resposta de máquina virtual inteligente e rápido para problemas de infraestrutura de armazenamento. 

Quando uma máquina virtual se desconecta de seu armazenamento subjacente, ele faz uma pausa e aguarda o armazenamento recuperar. Enquanto estiver em pausa, a máquina virtual mantém o contexto de aplicativos que são executados nele. Quando a conexão da máquina virtual em seu armazenamento é restaurado, a máquina virtual retorna ao seu estado de execução. Como resultado, o estado de sessão do computador do locatário é mantido na recuperação. 

No Windows Server 2016, resiliência de máquina virtual de armazenamento está ciente e otimizada para clusters convidados muito. 

### <a name="BKMK_Diagnostics"></a>Melhorias de diagnóstico no cluster de Failover  
Para ajudar a diagnosticar problemas com clusters de failover, o Windows Server 2016 inclui o seguinte:  

-   Várias melhorias para arquivos de log de cluster (como informações de fuso horário e DiagnosticVerbose log) que faz é mais fácil solucionar problemas de clustering de failover. Para obter mais informações, consulte [Windows Server 2016 Failover Cluster de solução de problemas aprimoramentos - Cluster Log](http://blogs.msdn.com/b/clustering/archive/2015/05/15/10614930.aspx). 

-   Um despejo de um novo tipo de **despejo de memória ativa**, que filtra a maioria das páginas de memória alocados para máquinas virtuais e, portanto, faz o DMP muito menor e mais fácil de salvar ou copiar. Para obter mais informações, consulte [Windows Server 2016 Failover Cluster de solução de problemas aprimoramentos - despejo Active Directory](http://blogs.msdn.com/b/clustering/archive/2015/05/18/10615526.aspx). 

### <a name="BKMK_SiteAware"></a>Clusters de Failover de reconhecimento de site  
Windows Server 2016 inclui site - com suporte a clusters de failover permitem que nós de grupo em clusters estendidos com base em sua localização física (site). Reconhecimento de sites do cluster aprimora as operações de chave durante o ciclo de vida do cluster como comportamento de failover, políticas de posicionamento, pulsação entre os nós e o comportamento de quorum. Para obter mais informações, consulte [reconhecimento de Site Clusters de Failover no Windows Server 2016](http://blogs.msdn.com/b/clustering/archive/2015/08/19/10636304.aspx). 

### <a name="BKMK_multidomainclusters"></a>Clusters de grupo de trabalho e vários domínios  
No Windows Server 2012 R2 e versões anteriores, um cluster só pode ser criado entre nós do membro associados ao mesmo domínio. O Windows Server 2016 quebra essas barreiras e apresenta a capacidade de criar um Cluster de failover sem dependências do Active Directory. Agora você pode criar clusters de failover nas seguintes configurações:  

-   **Clusters de domínio único.** Clusters com todos os nós que ingressaram no mesmo domínio. 

-   **Clusters de vários domínios.** Clusters conosco que são membros de domínios diferentes. 

-   **Clusters de grupo de trabalho.** Clusters conosco que são servidores de membro / grupo de trabalho (não ingressado no domínio). 

Para obter mais informações, consulte [clusters de grupo de trabalho e vários domínios no Windows Server 2016](http://blogs.msdn.com/b/clustering/archive/2015/08/17/10635825.aspx)  
### <a name="BKMK_VMLoadBalancing"></a>Balanceamento de carga da máquina virtual  
Balanceamento de carga de máquina virtual é um novo recurso no cluster de Failover que facilita o balanceamento de carga uniforme de máquinas virtuais em todos os nós em um cluster. Excesso de nós são identificados com base em máquina virtual memória e utilização da CPU no nó. Máquinas virtuais são movidas (migrado ao vivo) de um nó de excesso de comprometimento para nós com largura de banda disponível (se aplicável). A agressividade do balanceamento de pode ser ajustada para garantir a utilização e desempenho de cluster ideal. Balanceamento de carga é habilitada por padrão no Windows Server 2016 Technical Preview. No entanto, o balanceamento de carga é desabilitado quando a otimização dinâmica do SCVMM está habilitada. 

### <a name="BKMK_VMStartOrder"></a>Ordem de inicialização da máquina virtual  
A ordem de início de máquina virtual é um novo recurso no cluster de Failover que apresenta a orquestração de ordem de início para máquinas virtuais (e todos os grupos) em um cluster. Máquinas virtuais agora podem ser agrupadas em camadas e dependências de ordem de início podem ser criadas entre camadas diferentes. Isso garante que as máquinas virtuais mais importantes (como máquinas de virtuais de controladores de domínio ou do utilitário) são iniciadas primeiro. As máquinas virtuais não são iniciadas até que as máquinas virtuais que eles têm uma dependência em também são iniciadas. 

### <a name="BKMK_SMBMultiChannel"></a> SMB Multichannel simplificado e redes de Cluster com várias NICs  
Redes de Cluster de failover não estão mais limitados a uma única NIC por sub-rede / rede. Com simplificado SMB Multichannel e redes de Cluster com várias NICs, configuração de rede é automática e todas as NICs na sub-rede podem ser usada para tráfego de cluster e a carga de trabalho. Esse aprimoramento permite que os clientes a maximizar a taxa de transferência de rede para Hyper-V, a instância de Cluster de Failover do SQL Server e outras cargas de trabalho SMB. 

Para obter mais informações, consulte [simplificado SMB Multichannel e redes de Cluster com várias NICs](smb-multichannel.md).

## <a name="see-also"></a>Consulte também  
* [Armazenamento](../storage/storage.md)  
* [O que há de novo no armazenamento no Windows Server 2016](../storage/whats-new-in-storage.md)  
