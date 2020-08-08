---
ms.assetid: 0f2a7f7b-aca8-4e5d-ad67-4258e88bc52f
title: Novidades no armazenamento no Windows Server
ms.author: jgerend
manager: dongill
ms.topic: article
author: jasongerend
ms.date: 05/29/2019
ms.openlocfilehash: 3b1a3fa120e27439c1ec55f68521b813588d330b
ms.sourcegitcommit: dfa48f77b751dbc34409aced628eb2f17c912f08
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/07/2020
ms.locfileid: "87965943"
---
# <a name="whats-new-in-storage-in-windows-server"></a>O que há de novo no armazenamento no Windows Server

>Aplica-se a: Windows Server 2019, Windows Server 2016, Windows Server (canal semestral)

Este tópico explica as funcionalidades novas e alteradas no armazenamento nas versões de canal semestral do Windows Server 2019, do Windows Server 2016 e do Windows Server.

## <a name="whats-new-in-storage-in-windows-server-version-1903"></a>O que há de novo no armazenamento no Windows Server, versão 1903

Esta versão do Windows Server adiciona as seguintes alterações e tecnologias.

### <a name="storage-migration-service-now-migrates-local-accounts-clusters-and-linux-servers"></a>Agora, o serviço de migração de armazenamento migra contas locais, clusters e servidores Linux

O Serviço de Migração de Armazenamento facilita a migração dos servidores para uma versão mais recente do Windows Server. Ele fornece uma ferramenta gráfica que faz o inventário de dados nos servidores e, em seguida, transfere os dados e a configuração para servidores mais recentes — tudo sem aplicativos ou usuários precisando mudar tudo.

Ao usar essa versão do Windows Server para orquestrar as migrações, adicionamos as seguintes capacidades:

- Migrar usuários e grupos locais para o novo servidor
- Migrar o armazenamento dos clusters de failover
- Migrar o armazenamento de um servidor Linux que usa o Samba
- Sincronização mais fácil de compartilhamentos integrados no Azure usando a Sincronização de Arquivos do Azure
- Migrar para novas redes, como o Azure

Para obter mais informações sobre o serviço de migração de armazenamento, confira [Visão geral do Serviço de Migração de Armazenamento](storage-migration-service/overview.md).

### <a name="system-insights-disk-anomaly-detection"></a>Detecção de anomalias de disco de insights do sistema

Os [insights do sistema](../manage/system-insights/overview.md) são um recurso de análise preditiva que analisa localmente os dados do sistema do Windows Server e fornece insights sobre o funcionamento do servidor. Eles vêm com vários recursos internos, mas adicionamos a capacidade de instalar funcionalidades por meio do Windows Admin Center, começando com a detecção de anomalias de disco.

A detecção de anomalias de disco é uma nova funcionalidade que destaca quando discos estão se comportando *de forma diferente* da normal. Embora diferente não necessariamente uma seja uma coisa ruim, ver esses momentos anormais pode ser útil ao solucionar problemas em seus sistemas.

Essa funcionalidade também está disponível para servidores que executam o Windows Server 2019.

### <a name="windows-admin-center-enhancements"></a>Aprimoramentos do Windows Admin Center

Uma nova versão do Windows Admin Center foi lançada, adicionando novas funcionalidades ao Windows Server. Para obter informações sobre os recursos mais recentes, confira [Windows Admin Center](../manage/windows-admin-center/overview.md).

## <a name="whats-new-in-storage-in-windows-server-2019-and-windows-server-version-1809"></a>O que há de novo no armazenamento no Windows Server 2019 e no Windows Server, versão 1809

Esta versão do Windows Server adiciona as seguintes alterações e tecnologias.

### <a name="manage-storage-with-windows-admin-center"></a>Gerenciar o armazenamento com o centro de administração do Windows

O [centro de administração do Windows](../manage/windows-admin-center/overview.md) é um novo aplicativo baseado em navegador, implantado localmente para gerenciar servidores, clusters, infraestrutura hiperconvergente com espaços de armazenamento diretos e computadores com Windows 10. Ele não tem nenhum custo adicional além do Windows e está pronto para uso em produção.

Para ser justo, o centro de administração do Windows é um download separado que é executado no Windows Server 2019 e em outras versões do Windows, mas é novo e não queremos que você perca...

### <a name="storage-migration-service"></a>Serviço de Migração de Armazenamento

O Serviço de Migração de Armazenamento é uma nova tecnologia que facilita a migração dos servidores para uma versão mais recente do Windows Server. Ele fornece uma ferramenta gráfica que faz o inventário dos dados em servidores, transfere os dados e a configuração para servidores mais recentes e, em seguida, opcionalmente, move as identidades dos servidores antigos para os novos servidores para que os aplicativos e usuários não precisem alterar nada. Para obter mais informações, consulte [Serviço de Migração de Armazenamento](storage-migration-service/overview.md).

### <a name="storage-spaces-direct-windows-server-2019-only"></a><a id="storage-spaces-direct"></a>Espaços de Armazenamento Diretos (somente Windows Server 2019)

Há uma série de aprimoramentos para Espaços de Armazenamento Diretos no Windows Server 2019 (Espaços de Armazenamento Diretos não está incluído no Windows Server, canal semestral):

- **Eliminação de duplicação e compactação para volumes do ReFS**

    Armazene até dez vezes mais dados no mesmo volume com eliminação de duplicação e compactação para o sistema de arquivos ReFS. (É [apenas um clique](https://www.youtube.com/watch?v=PRibTacyKko&feature=youtu.be) para ativar com o centro de administração do Windows.) O repositório de partes de tamanho variável com compactação opcional maximiza as taxas de economia, enquanto a arquitetura de pós-processamento multi-threaded mantém o impacto do desempenho mínimo. Dá suporte a volumes de até 64 TB e eliminará a duplicação dos primeiros 4 TB de cada arquivo.

- **Suporte nativo para memória persistente**

    Desbloquear desempenho incomparável com suporte nativo de espaços de armazenamento diretos para módulos de memória persistente, incluindo Intel® Optane™ DC PM e NVDIMM-N. Use memória persistente como cache para acelerar o conjunto de trabalho ativo ou capacidade para garantir a baixa latência consistente na ordem microssegundos. Gerencie a memória persistente assim como faria com qualquer outra unidade no PowerShell ou o Windows Admin Center.

- **Resiliência aninhada para a infraestrutura hiperconvergente de dois nós na borda**

    Sobreviver a duas falhas de hardware em uma vez com a resiliência de um software novo opção inspirada RAID 5 + 1. Com resiliência aninhada, um cluster de espaços de armazenamento diretos de dois nós pode fornecer armazenamento continuamente acessível para aplicativos e máquinas virtuais, mesmo se um nó de servidor falhar e uma unidade falha no nó do servidor.

- **Clusters de dois servidores usando um pen drive como testemunha**

    Use uma unidade flash USB de baixo custo conectada ao seu roteador para atuar como uma testemunha em clusters de dois servidores. Se um servidor ficar inativo e, em seguida, fazer backup, o cluster da unidade USB saberá qual servidor tem os dados mais atualizados. Para obter mais informações, consulte o [blog armazenamento na Microsoft](https://techcommunity.microsoft.com/t5/storage-at-microsoft/bg-p/FileCAB).

- **Windows Admin Center**

    Gerenciar e monitorar espaços de armazenamento diretos com o novo [painel especificamente](../manage/windows-admin-center/use/manage-hyper-converged.md) e experiência no Windows Admin Center. Criar, abrir, expanda ou excluir volumes com apenas alguns cliques. Monitore o desempenho como latência de IOPS e de E/S do cluster geral até o SSD ou HDD individual. Disponível sem custo adicional para Windows Server 2016 e Windows Server 2019.

- **Histórico de desempenho**

    Obtenha visibilidade sem esforço para utilização de recursos e desempenho com [histórico interno](storage-spaces/performance-history.md). Mais de 50 contadores essenciais abrangendo computação, memória, rede e armazenamento automaticamente sejam coletadas e armazenadas no cluster por até um ano. O melhor de tudo: não há nada a ser instalado, configurado ou iniciado – ele simplesmente funciona. Visualizar no Windows Admin Center ou consulta e processe no PowerShell.

- **Dimensionar até 4 PB por cluster**

    Obter vários petabytes escala – muito bem para mídia, casos de uso de backup e arquivamento. No Windows Server 2019, espaços de armazenamento diretos dá suporte a até 4 petabytes (PB) = 4.000 terabytes de capacidade bruta por pool de armazenamento. Relacionados diretrizes de capacidade aumentam bem: por exemplo, você pode criar duas vezes mais volumes (64 em vez de 32), cada duas vezes maior que antes (64 TB em vez de 32 TB). Junte vários clusters em um [conjunto de clusters](storage-spaces/cluster-sets.md) para uma escala ainda maior em um namespace de armazenamento. Para obter mais informações, consulte o [blog armazenamento na Microsoft](https://techcommunity.microsoft.com/t5/storage-at-microsoft/bg-p/FileCAB).

- **A paridade acelerada por espelho é duas vezes mais rápida**

    Com paridade acelerada por espelho, você pode criar volumes espaços de armazenamento diretos que são parte espelho e parte paridade, como misturando RAID-1 e RAID-5/6 para obter o melhor dos dois. (É [mais fácil do que você imagina](https://www.youtube.com/watch?v=R72QHudqWpE) no centro de administração do Windows.) No Windows Server 2019, o desempenho da paridade acelerada por espelhamento é mais do que dobrar em relação ao Windows Server 2016 graças às otimizações.

- **Detecção de exceções de latência da unidade**

    Identifique facilmente unidades com latência anormal com monitoramento proativo e detecção de exceção interna, inspirado pela abordagem de longo prazo e com êxito da Microsoft Azure. Seja a latência média ou algo mais sutil, como a latência de 99 º percentil que se destaca, unidades lentas são rotuladas automaticamente no PowerShell e no centro de administração do Windows com o status ' latência anormal '.

- **Delimitar manualmente a alocação de volumes para aumentar a tolerância a falhas**

    Isso permite que os administradores delimitem manualmente a alocação de volumes em Espaços de Armazenamento Diretos. Isso pode aumentar significativamente a tolerância a falhas sob determinadas condições, mas impõe algumas considerações e complexidade de gerenciamento adicionais. Para obter mais informações, consulte [delimitar a alocação de volumes](storage-spaces/delimit-volume-allocation.md).

### <a name="storage-replica"></a><a name="storage-replica2019"></a>Réplica de Armazenamento

Há várias melhorias na [réplica de armazenamento](storage-replica/storage-replica-overview.md) nesta versão:

#### <a name="storage-replica-in-windows-server-standard-edition"></a>Réplica de armazenamento no Windows Server, Standard Edition

Agora você pode usar a réplica de armazenamento com o Windows Server, Standard Edition, além do Datacenter Edition. A réplica de armazenamento em execução no Windows Server, Standard Edition, tem as seguintes limitações:

- A réplica de armazenamento replica um único volume em vez de um número ilimitado de volumes.
- Os volumes podem ter um tamanho de até 2 TB em vez de um tamanho ilimitado.

#### <a name="storage-replica-log-performance-improvements"></a>Melhorias de desempenho do log da Réplica de Armazenamento

Também fizemos melhorias no modo como o log da réplica de armazenamento rastreia a replicação, melhorando a taxa de transferência e a latência da replicação, especialmente em todo o armazenamento flash, bem como Espaços de Armazenamento Diretos clusters que são replicados entre si.

Para obter o aumento de desempenho, todos os membros do grupo de replicação devem executar o Windows Server 2019.

#### <a name="test-failover"></a>Failover de Teste

Agora você pode montar temporariamente um instantâneo do armazenamento replicado em um servidor de destino para fins de teste ou backup. Para obter mais informações, consulte [Perguntas Frequentes sobre a Réplica de Armazenamento](https://aka.ms/srfaq).

#### <a name="windows-admin-center-support"></a>Suporte para o Windows Admin Center

O suporte para o gerenciamento gráfico da replicação agora está disponível no centro de administração do Windows por meio da ferramenta de Gerenciador do Servidor. Isso inclui a replicação de servidor para servidor, cluster para cluster, bem como a replicação de cluster de ampliação.

#### <a name="miscellaneous-improvements"></a>Aprimoramentos diversos

A réplica de armazenamento também contém os seguintes aprimoramentos:

-   Altera comportamentos assíncronos do cluster de ampliação para que ocorram failovers automáticos agora
-   Várias correções de bugs

### <a name="smb"></a>SMB

- **Remoção de autenticação de convidado e SMB1**: o Windows Server não instala mais o cliente e o servidor do SMB1 por padrão. Além disso, a capacidade de autenticar como um convidado no SMB2 e posterior está desativada por padrão. Para obter mais informações, confira [o SMBv1 não é instalado por padrão no Windows 10 versão 1709 e no Windows Server versão 1709](https://support.microsoft.com/help/4034314/smbv1-is-not-installed-by-default-in-windows-10-rs3-and-windows-server).

- **Compatibilidade e segurança de SMB2/SMB3**: foram adicionadas mais opções de compatibilidade e segurança do aplicativo, incluindo a capacidade de desabilitar os bloqueios em SMB2+ para aplicativos herdados, bem como exigir assinatura ou criptografia de um cliente para cada conexão. Para obter mais informações, confira a ajuda do módulo SMBShare do PowerShell.

### <a name="data-deduplication"></a>Eliminação de duplicação de dados

- **A eliminação de duplicação de dados agora dá suporte a ReFS**: você não precisa mais escolher entre as vantagens de um sistema de arquivos moderno com o ReFS e a eliminação de duplicação de dados: agora, você pode habilitar a eliminação de duplicação de dados, na qual você pode habilitar o ReFS. Aumente a eficiência do armazenamento em mais de 95% com o ReFS.
- **API de DataPort para entrada/saída otimizada para volumes com eliminação de duplicação**: os desenvolvedores agora podem aproveitar o conhecimento que a eliminação de duplicação de dados tem sobre como armazenar dados de modo eficaz para mover os dados entre volumes, servidores e clusters de forma eficiente.

### <a name="file-server-resource-manager"></a>File Server Resource Manager

O Windows Server 2019 inclui a capacidade de impedir que o serviço do Gerenciador de recursos de servidor de arquivos crie um diário de alterações (também conhecido como um diário de USN) em todos os volumes quando o serviço é iniciado. Isso pode economizar espaço em cada volume, mas desabilita a classificação de arquivos em tempo real. Para obter mais informações, consulte [Visão geral do Gerenciador de Recursos de Servidor de Arquivos](fsrm/fsrm-overview.md).

## <a name="whats-new-in-storage-in-windows-server-version-1803"></a>O que há de novo no armazenamento no Windows Server, versão 1803

### <a name="file-server-resource-manager"></a>File Server Resource Manager

O Windows Server, versão 1803 inclui a capacidade de impedir que o serviço do Gerenciador de recursos de servidor de arquivos crie um diário de alterações (também conhecido como um diário de USN) em todos os volumes quando o serviço é iniciado. Isso pode economizar espaço em cada volume, mas desabilita a classificação de arquivos em tempo real. Para obter mais informações, consulte [Visão geral do Gerenciador de Recursos de Servidor de Arquivos](fsrm/fsrm-overview.md).

## <a name="whats-new-in-storage-in-windows-server-version-1709"></a>O que há de novo no armazenamento no Windows Server, versão 1709

O Windows Server, versão 1709, é a primeira versão do Windows Server no canal semestral. O canal semestral é um benefício do Software Assurance e tem suporte total na produção por 18 meses, com uma nova versão a cada seis meses.

Para obter mais informações, consulte a [Visão geral do canal semestral do Windows Server](../get-started-19/servicing-channels-19.md).

### <a name="storage-replica"></a>Réplica de Armazenamento

A proteção de recuperação de desastre adicionada pela réplica de armazenamento agora está expandida para incluir:

- **Failover de teste**: a opção para montar o armazenamento de destino agora é possível por meio do recurso de failover de teste. Você pode montar um instantâneo do armazenamento replicado em nós de destino temporariamente para fins de teste ou backup. Para obter mais informações, consulte [Perguntas Frequentes sobre a Réplica de Armazenamento](https://aka.ms/srfaq).
- **Suporte do centro de administração do Windows**: o suporte ao gerenciamento gráfico da replicação agora está disponível no centro de administração do Windows por meio da ferramenta de Gerenciador do servidor. Isso inclui a replicação de servidor para servidor, cluster para cluster, bem como a replicação de cluster de ampliação.

A réplica de armazenamento também contém os seguintes aprimoramentos:

-   Altera comportamentos assíncronos do cluster de ampliação para que ocorram failovers automáticos agora
-   Várias correções de bugs

### <a name="smb"></a>SMB

- **SMB1 e remoção de autenticação de convidado**: o Windows Server versão 1709 não instala mais o cliente SMB1 e o servidor por padrão. Além disso, a capacidade de autenticar como um convidado no SMB2 e posterior está desativada por padrão. Para obter mais informações, confira [o SMBv1 não é instalado por padrão no Windows 10 versão 1709 e no Windows Server versão 1709](https://support.microsoft.com/help/4034314/smbv1-is-not-installed-by-default-in-windows-10-rs3-and-windows-server).

- **Compatibilidade e segurança de SMB2/SMB3**: foram adicionadas mais opções de compatibilidade e segurança do aplicativo, incluindo a capacidade de desabilitar os bloqueios em SMB2+ para aplicativos herdados, bem como exigir assinatura ou criptografia de um cliente para cada conexão. Para obter mais informações, confira a ajuda do módulo SMBShare do PowerShell.

### <a name="data-deduplication"></a>Eliminação de duplicação de dados

- **A eliminação de duplicação de dados agora dá suporte a ReFS**: você não precisa mais escolher entre as vantagens de um sistema de arquivos moderno com o ReFS e a eliminação de duplicação de dados: agora, você pode habilitar a eliminação de duplicação de dados, na qual você pode habilitar o ReFS. Aumente a eficiência do armazenamento em mais de 95% com o ReFS.
- **API de DataPort para entrada/saída otimizada para volumes com eliminação de duplicação**: os desenvolvedores agora podem aproveitar o conhecimento que a eliminação de duplicação de dados tem sobre como armazenar dados de modo eficaz para mover os dados entre volumes, servidores e clusters de forma eficiente.

## <a name="whats-new-in-storage-in-windows-server-2016"></a>Novidades no armazenamento no Windows Server 2016

### <a name="storage-spaces-direct"></a><a name="s2d"></a>Espaços de Armazenamento Direct
Os Espaços de Armazenamento Direto habilitam a criação de armazenamento altamente disponível e escalonável usando servidores com armazenamento local. Ele simplifica a implantação e o gerenciamento de sistemas de armazenamento definidos por software e desbloqueia o uso de novas classes de dispositivos de disco, como dispositivos de disco SATA SSD e NVMe, que anteriormente não eram possíveis com Espaços de Armazenamento clusterizados com discos compartilhados.

**Qual é o valor agregado desta alteração?**
Os Espaços de Armazenamento Direto habilitam provedores de serviços e empresas a usar servidores padrão do setor com armazenamento local para criar armazenamento altamente disponível e escalonável definido por software. O uso de servidores com armazenamento local diminui a complexidade, aumenta a escalabilidade e habilita o uso de dispositivos de armazenamento que não eram possíveis antes, como discos de estado sólido SATA para reduzir os custos de armazenamento flash ou discos de estado sólido NVMe para melhorar o desempenho.

Os Espaços de Armazenamento Direto eliminam a necessidade de malha SAS compartilhada, simplificando a implantação e a configuração. Em vez disso, eles usam a rede como uma malha de armazenamento, aproveitando SMB3 e SMB Direct (RDMA) para o armazenamento eficiente de alta velocidade e baixa latência da CPU. Para escalar horizontalmente, basta adicionar mais servidores para aumentar a capacidade de armazenamento e o desempenho de e/s para obter mais informações, consulte o [espaços de armazenamento diretos no Windows Server 2016](storage-spaces/storage-spaces-direct-overview.md).

**O que passou a funcionar de maneira diferente?**
Esse recurso é novo no Windows Server 2016.

### <a name="storage-replica"></a><a name="storage-replica"></a>Réplica de Armazenamento

A Réplica de Armazenamento habilita a replicação síncrona em nível de bloco independente de armazenamento entre servidores ou clusters para recuperação de desastre, bem como a expansão de um cluster de failover entre sites. A replicação síncrona habilita o espelhamento de dados em locais físicos com volumes consistentes com falha para garantir perda zero de dados no nível do sistema de arquivos. A replicação assíncrona permite a extensão de site além das dimensões metropolitanas com a possibilidade de perda de dados.

**Qual é o valor agregado desta alteração?**
A Replicação de Armazenamento o habilita a fazer o seguinte:

* Fornecer uma solução de recuperação de desastre de um único fornecedor para interrupções planejadas e não planejadas de cargas de trabalho críticas.
* Usar o transporte SMB3 com desempenho, escalabilidade e confiabilidade comprovados.
* Alongar clusters de failover do Windows para distâncias metropolitanas.
* Usar o software da Microsoft de ponta a ponta para armazenamento e clustering, como Hyper-V, Réplica de Armazenamento, Espaços de Armazenamento, Cluster, Servidor de Arquivos de Escalabilidade Horizontal, SMB3, Eliminação de Duplicação e ReFS/NTFS.
* Ajudar a reduzir o custo e a complexidade da seguinte maneira:
    * É independente de hardware, sem a necessidade de uma configuração de armazenamento específica, como DAS ou SAN.
    * Permite armazenamento de mercadorias e tecnologias de rede.
    * Conta com facilidade de gerenciamento gráfico para nós individuais e clusters por meio do Gerenciador de Cluster de Failover.
    * Inclui opções de script abrangentes em grande escala por meio do Windows PowerShell.
* Ajudar a reduzir o tempo de inatividade e aumentar a confiabilidade e a produtividade intrínsecas ao Windows.
* Fornecer capacidade de suporte, métricas de desempenho e recursos de diagnóstico.

Para saber mais, consulte [Novidades na Réplica de Armazenamento no Windows Server 2016](storage-replica/storage-replica-overview.md).

**O que passou a funcionar de maneira diferente?**
Esse recurso é novo no Windows Server 2016.

### <a name="storage-quality-of-service"></a><a name="storage-qos"></a>Qualidade de serviço do armazenamento
Agora você pode usar QoS (qualidade do serviço de armazenamento) para monitorar centralmente e de ponta a ponta o desempenho do armazenamento e criar políticas usando o clusters do Hyper-V e CSV no Windows Server 2016.

**Qual é o valor agregado desta alteração?**
Agora você pode criar políticas de QoS de armazenamento em um cluster CSV e atribuí-las a um ou mais discos virtuais em máquinas virtuais Hyper-V. O desempenho de armazenamento é reajustado automaticamente para atender às políticas à medida que as cargas de trabalho e de armazenamento flutuam.

* Cada política pode especificar uma reserva (mínima) e um limite (máximo) a serem aplicados a uma coleção de fluxos de dados, como um disco rígido virtual, uma única máquina virtual ou um grupo de máquinas virtuais, um serviço ou um locatário.
* Usando o Windows PowerShell ou o WMI, você pode executar as seguintes tarefas:
    * Crie políticas em um cluster CSV.
    * Enumere as políticas disponíveis em um cluster CSV.
    * Atribua uma política a um disco rígido virtual de uma máquina virtual Hyper-V.
    * Monitorar o desempenho de cada fluxo e o status na política.
* Se vários discos rígidos virtuais compartilham a mesma política, o desempenho é distribuído uniformemente para atender à demanda de acordo com as configurações mínimas e máximas da política. Portanto, uma política pode ser usada para gerenciar um disco rígido virtual, uma máquina virtual, várias máquinas virtuais que compõem um serviço ou todas máquinas virtuais pertencentes a um locatário.

**O que passou a funcionar de maneira diferente?**
Esse recurso é novo no Windows Server 2016. Não era possível realizar o gerenciamento de reservas mínimas, o monitoramento de fluxos de todos os discos virtuais no cluster por meio de um único comando e o gerenciamento centralizado baseado em política nas versões anteriores do Windows Server.

Para obter mais informações, consulte [qualidade de serviço de armazenamento](storage-qos/storage-qos-overview.md)

### <a name="data-deduplication"></a><a name="dedup"></a>Eliminação de duplicação de dados
| Funcionalidade | Novo ou atualizado | Descrição |
|---------------|----------------|-------------|
| [Suporte a volumes grandes](data-deduplication/whats-new.md#large-volume-support) | Atualizado | Antes do Windows Server 2016, os volumes tinham que ser dimensionados especificamente para a variação esperada e tamanhos de volume acima de 10 TB não eram bons candidatos para eliminação de duplicação. No Windows Server 2016, a Eliminação de Duplicação de Dados dá suporte a tamanhos de volume de **até 64 TB**. |
| [Suporte para arquivos grandes](data-deduplication/whats-new.md#large-file-support) | Atualizado | Antes do Windows Server 2016, arquivos com tamanho próximo a 1 TB não eram bons candidatos para eliminação de duplicação. No Windows Server 2016, arquivos com **até 1 TB** têm suporte completo. |
| [Suporte ao Nano Server](data-deduplication/whats-new.md#nano-server-support) | Novo | A Eliminação de Duplicação de Dados está disponível e tem suporte completo na nova opção de implantação do Nano Server para Windows Server 2016. |
| [Suporte de backup simplificado](data-deduplication/whats-new.md#simple-backup-support) | Novo | No Windows Server 2012 R2, Aplicativos de Backup Virtualizado, como o [Data Protection Manager](/previous-versions/system-center/system-center-2012-R2/hh758173(v=sc.12)), tinham suporte por meio de uma série de etapas de configuração manual. No Windows Server 2016, um novo "Backup" de tipo de uso padrão foi adicionado para implantação perfeita da Eliminação de Duplicação de Dados para aplicativos de backup virtualizado. |
| [Suporte a atualizações do sistema operacional de cluster sem interrupção](data-deduplication/whats-new.md#cluster-upgrade-support) | Novo | A Eliminação de Duplicação de Dados dá suporte completo ao novo recurso [Atualização sem Interrupção do SO de Cluster](..//failover-clustering/cluster-operating-system-rolling-upgrade.md) do Windows Server 2016. |

### <a name="smb-hardening-improvements-for-sysvol-and-netlogon-connections"></a><a name="smb-hardening-improvements"></a>Melhorias em proteção de SMB para conexões SYSVOL e NETLOGON
No Windows 10 e no Windows Server 2016, conexões de cliente para os compartilhamentos padrão SYSVOL e NETLOGON do Active Directory Domain Services em controladores de domínio agora exigem assinatura SMB e autenticação mútua (como Kerberos).

**Qual é o valor agregado desta alteração?**
Essa alteração reduz a probabilidade de ataques intermediários.

**O que passou a funcionar de maneira diferente?**
Se a assinatura SMB e a autenticação mútua não estiverem disponíveis, um computador com Windows 10 ou Windows Server 2016 não processará scripts e a Política de Grupo baseada em domínio.

> [!NOTE]
> Os valores do Registro para essas configurações não estão presentes por padrão, mas as regras de proteção ainda se aplicam até serem substituídas pela Política de Grupo ou outros valores do Registro.

Para saber mais sobre esses aprimoramentos de segurança (também conhecidos como Proteção UNC), consulte o artigo da Base de Dados de Conhecimento [3000483](https://support.microsoft.com/kb/3000483) e [MS15-011 & MS15-014: Política de Grupo de Proteção](https://msrc-blog.microsoft.com/2015/02/10/ms15-011-ms15-014-hardening-group-policy).

### <a name="work-folders"></a>Pastas de trabalho
Notificação de alteração aprimorada quando o servidor de pastas de trabalho está executando o Windows Server 2016 e o cliente de pastas de trabalho é o Windows 10.

**Qual é o valor agregado desta alteração?**<br>
Para o Windows Server 2012 R2, quando as alterações do arquivo são sincronizadas com o servidor de pastas de trabalho, os clientes não são notificados sobre a alteração e aguardam até 10 minutos para receber a atualização.  Ao usar o Windows Server 2016, o servidor de pastas de trabalho notifica imediatamente os clientes do Windows 10 e as alterações de arquivo são sincronizadas imediatamente.

**O que passou a funcionar de maneira diferente?**<br>
Esse recurso é novo no Windows Server 2016. Isso requer um servidor de pastas de trabalho do Windows Server 2016, e o cliente deve ser o Windows 10.

Se você estiver usando um cliente mais antigo ou o servidor de Pastas de Trabalho for o Windows Server 2012 R2, o cliente continuará sondando alterações a cada 10 minutos.

### <a name="refs"></a>ReFS
A próxima iteração do ReFS dá suporte a implantações de armazenamento em grande escala com cargas de trabalho diversificadas, o que proporciona confiabilidade, resiliência e escalabilidade para os dados.

**Qual é o valor agregado desta alteração?**<br>
O ReFS apresenta as seguintes melhorias:

* O ReFS implementa a nova funcionalidade de camadas de armazenamento, ajudando a oferecer um desempenho mais rápido e mais capacidade de armazenamento. Essa nova funcionalidade permite:
    * Vários tipos de resiliência no mesmo disco virtual (usando espelhamento na camada desempenho e paridade na camada de capacidade, por exemplo).
    * Define a maior capacidade de resposta para conjuntos de trabalho erráticos.
* A introdução da clonagem do bloco melhora muito o desempenho de operações de VM, como operações de mesclagem do ponto de verificação .vhdx.
* A nova ferramenta de verificação ReFS permite a recuperação de armazenamento vazado e ajuda na recuperação de dados de danos críticos.

**O que passou a funcionar de maneira diferente?**<br>
Esses recursos são novos no Windows Server 2016.

## <a name="additional-references"></a>Referências adicionais
* [Novidades no Windows Server 2016](../get-started/whats-new-in-windows-server-2016.md)
