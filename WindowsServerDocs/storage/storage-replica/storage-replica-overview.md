---
title: Visão geral da Réplica de Armazenamento
ms.prod: windows-server
manager: siroy
ms.author: nedpyle
ms.technology: storage-replica
ms.topic: get-started-article
author: nedpyle
ms.date: 4/26/2019
ms.assetid: e9b18e14-e692-458a-a39f-d5b569ae76c5
ms.openlocfilehash: 400af7c4fb5db6e6740b1140688602c55d8ca0a9
ms.sourcegitcommit: 771db070a3a924c8265944e21bf9bd85350dd93c
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/27/2020
ms.locfileid: "85469801"
---
# <a name="storage-replica-overview"></a>Visão geral da Réplica de Armazenamento

>Aplica-se a: Windows Server 2019, Windows Server 2016, Windows Server (canal semestral)

Réplica de Armazenamento é a tecnologia do Windows Server que permite a replicação de volumes entre servidores ou clusters para recuperação de desastres. Ele também permite que você estenda clusters de failover que abrangem dois sites, com todos os nós mantendo a sincronia.

A Réplica de Armazenamento oferece suporte à replicação síncrona e assíncrona:

* A **replicação síncrona** espelha os dados em um site de rede de baixa latência com volumes consistentes com falha para garantir perda zero de dados no nível do sistema de arquivos durante uma falha.
* A **replicação assíncrona** espelha os dados entre locais além dos limites metropolitanos em links de rede com latências maiores, mas sem uma garantia de que os dois locais tenham cópias idênticas de dados no momento da falha.

## <a name="why-use-storage-replica"></a>Por que usar a Réplica de Armazenamento?

A réplica de armazenamento oferece recursos de recuperação de desastre e prontidão no Windows Server. O Windows Server oferece a tranqüilidade de nenhuma perda de dados, com a capacidade de proteger dados de forma síncrona em diferentes racks, andares, prédios, campus, municípios e cidades. Após um desastre, todos os dados existem em outro lugar sem a possibilidade de perda. O mesmo se aplica *antes de* um desastre. A Réplica de Armazenamento oferece a capacidade de alternar as cargas de trabalho para locais seguros antes das catástrofes, quando há um aviso com alguns instantes de antecedência (novamente, sem perda de dados).

A Réplica de Armazenamento permite o uso mais eficiente de vários datacenters. Alongando ou replicando clusters, as cargas de trabalho podem ser executadas em vários datacenters para o acesso a dados mais rápido por usuários e aplicativos com proximidade local, bem como melhor distribuição de carga e uso de recursos de computação. Se um desastre coloca um datacenter offline, você pode mover suas cargas de trabalho típicas para outro local temporariamente.

A Réplica de Armazenamento pode permitir o encerramento de sistemas de replicação de arquivos existentes, como a Replicação do DFS, que eram impostos como soluções de recuperação de desastre de baixo custo. Embora a Replicação do DFS funcione bem em redes com largura de banda extremamente baixa, sua latência é muito alta, sendo geralmente medida em horas ou dias. Isso é causado por seu requisito de fechamento de arquivos e seus limites artificiais para evitar o congestionamento da rede. Com essas características de design, os arquivos mais recentes e mais usados em uma réplica da Replicação do DFS são os que têm menor probabilidade de replicação. A Réplica de Armazenamento opera abaixo do nível de arquivo e não tem essas restrições.

A Réplica de Armazenamento também dá suporte à replicação assíncrona para redes com latência maior e intervalos mais longos. Como ele não é baseado em ponto de verificação e, em vez disso, Replica continuamente, o Delta das alterações tende a ser muito menor do que os produtos baseados em instantâneo. Além disso, a Réplica de Armazenamento opera na camada da partição. Portanto, ela replica todos os instantâneos de VSS criados pelo Windows Server ou o software de backup. Isso permite o uso de instantâneos de dados consistentes com o aplicativo para recuperação pontual, principalmente dados de usuário não estruturados replicados de forma assíncrona.

## <a name="supported-configurations"></a><a name="BKMK_SRSupportedScenarios"></a>Configurações com suporte

Você pode implantar a réplica de armazenamento em um cluster de ampliação, entre cluster para cluster e em configurações de servidor para servidor (consulte as figuras 1-3).

O **Cluster Estendido** permite a configuração de computadores e o armazenamento em um único cluster, em que alguns nós compartilham um conjunto de armazenamento assimétrico e alguns nós compartilham outro e, depois, fazem a replicação de forma síncrona ou assíncrona com reconhecimento de sites. Esse cenário pode utilizar Espaços de Armazenamento com armazenamento SAS compartilhado, LUNs conectadas a SAN e iSCSI. Ele é gerenciado com o PowerShell e a ferramenta gráfica do Gerenciador de Cluster de Failover e permite failover de carga de trabalho automatizado.

![Diagrama que mostra dois nós de cluster em Nova York usando a Réplica de Armazenamento para replicar o armazenamento com dois nós em Nova Jersey](./media/Storage-Replica-Overview/Storage_SR_StretchCluster.png)

**FIGURA 1: replicação de armazenamento em um cluster estendido usando a Réplica de Armazenamento**

**Cluster para Cluster** permite a replicação entre dois clusters separados, em que um cluster é replicado de forma síncrona ou assíncrona com outro cluster. Esse cenário pode utilizar Espaços de Armazenamento Diretos, Espaços de Armazenamento com armazenamento SAS compartilhado, LUNs conectadas a SAN e iSCSI. Ele é gerenciado com o centro de administração do Windows e o PowerShell e requer intervenção manual para failover.

![Diagrama que mostra um cluster em Los Angeles usando a Réplica de Armazenamento para replicar o armazenamento com um cluster diferente em Las Vegas](./media/Storage-Replica-Overview/Storage_SR_ClustertoCluster.png)

**FIGURA 2: replicação de armazenamento de cluster para cluster usando a Réplica de Armazenamento**

**Servidor para servidor** permite a replicação síncrona e assíncrona entre dois servidores autônomos, usando Espaços de Armazenamento com armazenamento SAS compartilhado, LUNs conectadas a SAN e iSCSI e unidades locais. Ele é gerenciado com o centro de administração do Windows e o PowerShell e requer intervenção manual para failover.

![Diagrama que mostra um servidor no Prédio 5 replicando com um servidor no Prédio 9](./media/Storage-Replica-Overview/Storage_SR_ServertoServer.png)

**FIGURA 3: replicação de armazenamento de servidor para servidor usando a Réplica de Armazenamento**

> [!NOTE]
> Você também pode configurar a replicação de servidor para si mesmo usando quatro volumes separados em um computador. No entanto, este guia não abrange esse cenário.

## <a name="storage-replica-features"></a><a name="BKMK_SR2"> </a> Recursos de réplica de armazenamento

* **Zero perda de dados, replicação em nível de bloco**. Com a replicação síncrona, não há possibilidade de perda de dados. Com a replicação em nível de bloco, não há possibilidade de bloqueio de arquivos.

* **Implantação e gerenciamento simples**. A Réplica de Armazenamento tem uma exigência de design para facilidade de uso. A criação de uma parceria de replicação entre dois servidores pode utilizar o centro de administração do Windows. A implantação de clusters estendidos usa o assistente intuitivo na familiar ferramenta Gerenciador de Cluster de Failover.

* **Convidado e host**. Todos os recursos de Réplica de Armazenamento são expostos em implantações virtualizadas com base em host e convidado. Isso significa que os convidados podem replicar seus volumes de dados mesmo se estiverem em execução em plataformas de virtualização não Windows ou em nuvens públicas, desde que o Windows Server seja usado no convidado.

* **Com base em SMB3**. A Réplica de Armazenamento usa a tecnologia e comprovada SMB3, lançada pela primeira vez no Windows Server 2012. Isso significa que todos os recursos avançados do SMB (como suporte direto a SMB e multicanal em placas de rede RDMA InfiniBand, iWARP e RoCE) estão disponíveis para a Réplica de Armazenamento.

* **Segurança**. Diferentemente de muitos produtos de fornecedores, a Réplica de Armazenamento tem a tecnologia de segurança líder do setor integrada a ela. Isso inclui assinatura de pacotes, criptografia de dados completa AES-128-GCM, suporte a aceleração de criptografia Intel AES-NI e prevenção de ataques intermediários de integridade de pré-autenticação. A Réplica de Armazenamento utiliza Kerberos AES256 para toda a autenticação entre nós.

* **Sincronização inicial de alto desempenho**. A Réplica de Armazenamento dá suporte à sincronização inicial propagada, em que um subconjunto de dados já existe em um destino graças a cópias mais antigas, backups ou unidades enviadas. A replicação inicial copia apenas os blocos diferentes, reduzindo potencialmente o tempo de sincronização inicial e impedindo que os dados usem largura de banda limitada. Réplicas de Armazenamento bloqueiam o cálculo de soma de verificação e a agregação significa que o desempenho da sincronização inicial é limitado apenas pela velocidade do armazenamento e da rede.

* **Grupos de consistência**. A classificação de gravação garante que aplicativos como Microsoft SQL Server possam gravar em vários volumes replicados e saber que os dados são gravados no servidor de destino sequencialmente.

* **Delegação de usuário**. Os usuários podem ter permissões delegadas para gerenciar a replicação sem serem membros do grupo interno de Administradores em nós replicados, limitando seu acesso a áreas não relacionadas.

* **Restrição de rede**. A Réplica de Armazenamento pode ser limitada a redes individuais por servidor e por volumes replicados, para fornecer largura de banda de software de gerenciamento, backup e aplicativos.

* **Provisionamento dinâmico**. Há suporte para o provisionamento dinâmico em dispositivos SAN e Espaços de Armazenamento, a fim de fornecer tempos de replicação inicial quase instantâneos em muitas circunstâncias.

A réplica de armazenamento inclui os seguintes recursos:

| Recurso | Detalhes |
| ----------- | ----------- |
| Digite | Baseado em host |
| Síncrona | Yes |
| Assíncrona | Yes |
| Independente de hardware de armazenamento | Yes |
| Unidade de replicação | Volume (partição) |
| Criação de cluster de ampliação do Windows Server | Yes |
| Replicação de servidor para servidor | Yes |
| Replicação de cluster para cluster | Yes |
| Transport | SMB3 |
| Rede | TCP/IP ou RDMA |
| Suporte a restrição de rede | Yes |
| RDMA* | iWARP, InfiniBand, RoCE v2 |
| Requisitos de firewall de porta de rede de replicação | Porta IANA única (TCP 445 ou 5445) |
| Multicaminhos/multicanal | Sim (SMB3) |
| Suporte a Kerberos | Sim (SMB3) |
| Criptografia e assinatura durante a transmissão|Sim (SMB3) |
| Failovers por volume permitidos | Yes |
| Suporte ao armazenamento com provisionamento dinâmico | Yes |
| Interface do usuário de gerenciamento pronta para uso | PowerShell, Gerenciador de Cluster de Failover |

*Pode exigir equipamento e cabeamento de longa distância adicionais.

## <a name="storage-replica-prerequisites"></a><a name="BKMK_SR3"></a>Pré-requisitos de réplica de armazenamento

* Floresta de Active Directory Domain Services.
* Espaços de Armazenamento com JBODs de SAS, Espaços de Armazenamento Diretos, fibre channel de SAN, VHDX compartilhado, iSCSI de destino ou armazenamento SCSI/SAS/SATA local. SSD ou mais rápido recomendado para unidades de log de replicação. A Microsoft recomenda que o armazenamento de log seja mais rápido do que o armazenamento de dados. Volumes de log nunca devem ser usados para outras cargas de trabalho.
* Pelo menos uma conexão de Ethernet/TCP em cada servidor para replicação síncrona, mas, preferencialmente, RDMA.
* Pelo menos 2 GB de RAM e dois núcleos por servidor.
* Uma rede entre servidores com largura de banda suficiente para conter a carga de trabalho de gravação de E/S e uma média de 5 ms ou menos de latência de viagem de ida e volta, para replicação síncrona. A replicação assíncrona não tem uma recomendação de latência.
* Windows Server, Datacenter Edition ou Windows Server, Standard Edition. A réplica de armazenamento em execução no Windows Server, Standard Edition, tem as seguintes limitações:

  * Você deve usar o Windows Server 2019 ou posterior
  * A réplica de armazenamento replica um único volume em vez de um número ilimitado de volumes.
  * Os volumes podem ter um tamanho de até 2 TB em vez de um tamanho ilimitado.

##  <a name="background"></a><a name="BKMK_SR4"> </a> Plano de fundo

Esta seção inclui informações sobre termos de alto nível do setor, replicação síncrona e assíncrona e comportamentos importantes.

### <a name="high-level-industry-terms"></a>Termos do setor de alto nível

RD (Recuperação de Desastre) refere-se a um plano de contingência para recuperação de catástrofes do local para que a empresa continue a operar. RD de dados significa várias cópias dos dados de produção em um local físico separado. Por exemplo, um cluster estendido, em que metade dos nós está em um site e metade está em outro. PD (Preparação para Desastre) refere-se a um plano de contingência para mover cargas de trabalho de forma preventiva para um local diferente antes de um desastre iminente, como um furacão.

SLAs (contratos de nível de serviço) definem a disponibilidade de aplicativos de uma empresa e sua tolerância de tempo de inatividade e perda de dados durante interrupções planejadas e não planejadas. RTO (objetivo de tempo de recuperação) define por quanto tempo a empresa pode tolerar a total falta de acesso aos dados. RPO (objetivo de ponto de recuperação) define a quantidade de dados perdidos com a qual a empresa pode arcar.

### <a name="synchronous-replication"></a>Replicação síncrona

A replicação síncrona garante que o aplicativo grave dados em dois locais ao mesmo tempo antes da conclusão da E/S. Essa replicação é mais adequada para dados críticos, pois exige investimentos em armazenamento e rede, bem como um risco de desempenho de aplicativos degradado.

Quando gravações de aplicativo ocorrem na cópia de dados de origem, o armazenamento de origem não reconhece a E/S imediatamente. Em vez disso, essas alterações de dados são replicadas para a cópia de destino remoto e retornam uma confirmação. Somente então o aplicativo recebe a confirmação de E/S. Isso garante a sincronização constante do local remoto com o local de origem, estendendo de fato o armazenamento de E/S pela rede. Em caso de falha do local de origem, os aplicativos podem fazer failover para o local remoto do site e retomar as operações com a garantia de zero perda de dados.

| Mode | Diagrama | Etapas |
| -------- | ----------- | --------- |
| **Síncrona**<p>Zero Perda de dados<p>RPO | ![Diagrama que mostra como a Réplica de Armazenamento grava dados em replicação síncrona](./media/Storage-Replica-Overview/Storage_SR_SynchronousV2.png) | 1.  O aplicativo grava dados<br />2.  Dados de log são gravados e os dados são replicados para o local remoto<br />3.  Dados de log são gravados no local remoto<br />4.  Confirmação do local remoto<br />5.  Gravação de aplicativo confirmada<p>t e t1: dados liberados para o volume, logs sempre realizam gravação |

### <a name="asynchronous-replication"></a>Replicação assíncrona

Diferentemente, a replicação assíncrona significa que, quando o aplicativo grava dados, eles são replicados para o local remoto sem garantias de confirmação imediata. Esse modo permite um tempo de resposta mais rápido para o aplicativo, bem como uma solução de recuperação de desastre que funciona geograficamente.

Quando o aplicativo grava dados, o mecanismo de replicação captura a gravação e imediatamente informa ao aplicativo. Em seguida, os dados capturados são replicados para o local remoto. O nó remoto processa a cópia dos dados e reconhece lentamente para a cópia de origem. Como o desempenho de replicação não está mais no caminho de E/S do aplicativo, a capacidade de resposta e a distância do local remoto são fatores menos importantes. Haverá risco de perda de dados se a fonte de dados for perdida e a cópia de destino dos dados ainda estiver no buffer sem sair da origem.

Com seu RPO maior que zero, a replicação assíncrona é menos adequada para soluções de alta disponibilidade como Clusters de Failover, pois eles são projetados para operação contínua com redundância e sem perda de dados.

| Mode | Diagrama | Etapas |
| -------- | ----------- | --------- |
| **Assíncrona**<p>Perda de dados quase zero<p>(depende de vários fatores)<p>RPO | ![Diagrama que mostra como a Réplica de Armazenamento grava dados em replicação assíncrona](./media/Storage-Replica-Overview/Storage_SR_AsynchronousV2.png)|1.  O aplicativo grava dados<br />2.  Dados de log gravados<br />3.  Gravação de aplicativo confirmada<br />4.  Dados replicados para o local remoto<br />5.  Dados de log gravados no local remoto<br />6.  Confirmação do local remoto<p>t e t1: dados liberados para o volume, logs sempre realizam gravação |

### <a name="key-evaluation-points-and-behaviors"></a>Principais pontos de avaliação e comportamentos

-   Largura de banda e latência com armazenamento mais rápido. Há limitações físicas em torno de replicação síncrona. Como a Réplica de Armazenamento implementa um mecanismo de filtragem de E/S usando logs e exigindo idas e vindas de rede, a replicação síncrona provavelmente torna as gravações de aplicativos mais lentas. Usando redes de baixa latência, alta largura de banda, bem como subsistemas de disco de alta taxa de transferência para os logs, você minimiza a sobrecarga de desempenho.

-   O volume de destino não está acessível durante a replicação no Windows Server 2016. Quando você configura a replicação, o volume de destino é desmontado, tornando-o inacessível para quaisquer leituras ou gravações realizadas por usuários. Sua letra de driver pode estar visível em interfaces típicas, como o Explorador de Arquivos, mas um aplicativo não pode acessar o volume em si. Tecnologias de replicação em nível de bloco são incompatíveis com a permissão de acesso ao sistema de arquivos montado no destino em um volume. NTFS e ReFS não dão suporte à gravação de dados por usuários no volume enquanto os blocos são alterados abaixo deles.

O cmdlet **Test-failover** introduziu no Windows Server, versão 1709, e também foi incluído no windows Server 2019. Isso agora dá suporte à montagem temporária de um instantâneo de leitura/gravação do volume de destino para backups, testes, etc. Consulte https://aka.ms/srfaq para obter mais informações.

-   A implementação da replicação assíncrona pela Microsoft é diferente da maioria. A maioria das implementações do setor de replicação assíncrona conta com replicação baseada em instantâneos, em que transferências diferenciais periódicas se movem para outro nó e realizam a mesclagem. A replicação assíncrona da Réplica de Armazenamento funciona como a replicação síncrona, com a exceção de que elimina a necessidade de confirmação síncrona serializada do destino. Isso significa que, teoricamente, a Réplica de Armazenamento tem um RPO mais baixo, pois realiza a replicação continuamente. No entanto, isso também significa que depende de garantias de consistência interna do aplicativo, em vez de usar instantâneos para forçar a consistência em arquivos de aplicativos. A Réplica de Armazenamento garante a consistência de falhas em todos os modos de replicação

-   Muitos clientes utilizam a Replicação do DFS como uma solução de recuperação de desastre, embora frequentemente seja impraticável para esse cenário. a Replicação do DFS não pode replicar arquivos abertos e foi projetado para minimizar o uso de largura de banda às custas do desempenho, levando a grandes deltas de ponto de recuperação. A Réplica de Armazenamento pode permitir que você desative a Replicação do DFS para alguns desses tipos de tarefas de recuperação de desastre.

-   A réplica de armazenamento não é uma solução de backup. Alguns ambientes de TI implantam sistemas de replicação como soluções de backup, devido a suas opções de zero perda de dados, em comparação com backups diários. A Réplica de Armazenamento replica todas as alterações para todos os blocos de dados no volume, independentemente do tipo de alteração. Se um usuário excluir todos os dados de um volume, a réplica de armazenamento replicará a exclusão instantaneamente para o outro volume, removendo irrevogávelmente os dados de ambos os servidores. Não use a Réplica de Armazenamento como substituição para uma solução de backup pontual.

-   A Réplica de Armazenamento não é a Réplica do Hyper-V ou Grupos de Disponibilidade AlwaysOn do Microsoft SQL. A Réplica de Armazenamento é um mecanismo de finalidade geral, independente de armazenamento. Por definição, ele não pode personalizar seu comportamento de forma tão ideal quanto a replicação em nível de aplicativo. Isso pode levar a falhas de recurso específicas que o encorajam a implantar ou permanecer com tecnologias de replicação de aplicativos específicas.

> [!NOTE]
> Este documento contém uma lista de [problemas conhecidos](storage-replica-known-issues.md) e comportamentos esperado, bem como uma seção de [perguntas frequentes](storage-replica-frequently-asked-questions.md).

### <a name="storage-replica-terminology"></a>Terminologia de Réplica de Armazenamento
Frequentemente, este guia usa os seguintes termos:

-   A origem é o volume de computador que permite gravações locais e replica a saída. Também conhecida como "principal".

-   O destino é um volume de computador que não permite gravações locais e replica a entrada. Também conhecido como "secundário".

-   Um parceiro de replicação é a relação de sincronização entre um computador de origem e um computador de destino para um ou mais volumes e utiliza um único log.

-   Um grupo de replicação é a organização de volumes e suas configurações de replicação em uma parceria, por servidor. Um grupo pode conter um ou mais volumes.

### <a name="whats-new-for-storage-replica"></a>O que há de novo para a réplica de armazenamento

Para obter uma lista dos novos recursos na réplica de armazenamento no Windows Server 2019, consulte [novidades no armazenamento](../whats-new-in-storage.md#storage-replica2019)

## <a name="additional-references"></a>Referências adicionais

- [Estender a replicação do cluster usando o armazenamento compartilhado](stretch-cluster-replication-using-shared-storage.md)
- [Replicação de armazenamento de servidor para servidor](server-to-server-storage-replication.md)
- [Cluster para replicação de armazenamento de cluster](cluster-to-cluster-storage-replication.md)
- [Réplica de armazenamento: problemas conhecidos](storage-replica-known-issues.md)
- [Réplica de armazenamento: perguntas frequentes](storage-replica-frequently-asked-questions.md)
- [Espaços de Armazenamento Diretos no Windows Server 2016](../storage-spaces/storage-spaces-direct-overview.md)
- [Suporte do Windows IT Pro](https://www.microsoft.com/itpro/windows/support)
