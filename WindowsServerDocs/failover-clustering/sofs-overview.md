---
title: Visão geral do servidor de arquivos de expansão para dados de aplicativos
description: Visão geral do recurso de servidor de arquivos de escalabilidade horizontal para Windows Server 201 R2, Windows Server 2012 e Windows Server 2016.
ms.prod: windows-server-threshold
ms.topic: article
author: JasonGerend
ms.author: jgerend
ms.technology: storage-failover-clustering
ms.date: 04/26/2018
ms.localizationpriority: medium
ms.openlocfilehash: 04e25e9c69062611d9d14c220614f148ac5de770
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59884767"
---
# <a name="scale-out-file-server-for-application-data-overview"></a>Visão geral do servidor de arquivos de expansão para dados de aplicativos

>Aplica-se a: Windows Server 2012 R2, Windows Server 2012, Windows Server 2016

Servidor de Arquivos de Escalabilidade Horizontal é um recurso projetado para fornecer compartilhamentos de arquivos de escalabilidade horizontal que estão continuamente disponíveis para armazenamento de aplicativo para servidores com base em arquivo. Os compartilhamentos de arquivos de expansão proporcionam a capacidade de compartilhar a mesma pasta em múltiplos nós do mesmo cluster. Esse cenário se concentra em como planejar e implantar o Servidor de Arquivos de Escalabilidade Horizontal.

Você pode implantar e configurar um servidor de arquivos clusterizado usando qualquer um dos seguintes métodos:

- **Servidor de arquivos de escalabilidade horizontal para dados de aplicativo** esse recurso de servidor de arquivos em cluster foi introduzido no Windows Server 2012 e permite que você armazene dados de aplicativos de servidor, como arquivos de máquina virtual do Hyper-V, em compartilhamentos de arquivos e obtenha um nível semelhante de confiabilidade, disponibilidade, capacidade de gerenciamento e alto desempenho que você esperaria de uma rede de área de armazenamento. Todos os compartilhamentos de arquivos ficam online em todos os nós simultaneamente. Os compartilhamentos de arquivos associados a esse tipo de servidor de arquivos clusterizado são chamados de compartilhamentos de arquivos de expansão. Às vezes é chamado de ativo-ativo. Esse é o tipo de servidor de arquivos recomendado ao implantar o Hyper-V no protocolo SMB ou o Microsoft SQL Server por SMB.
- **Servidor de arquivos para uso geral** Corresponde à continuação do servidor de arquivos clusterizado que era suportado no Windows Server desde a introdução do cluster de failover. Esse tipo de servidor de arquivos clusterizado e, por conseguinte, todos os compartilhamentos associados a esse servidor, ficam online em um nó de cada vez. Às vezes é chamado de ativo-passivo ou ativo dual. Os compartilhamentos de arquivos associados a esse tipo de servidor de arquivos clusterizado são chamados de compartilhamentos de arquivos clusterizados. Esse é o tipo de servidor de arquivos recomendado ao implantar cenários de trabalhadores de informações.

## <a name="scenario-description"></a>Descrição do cenário

Com os compartilhamentos de arquivos de escalabilidade horizontal você pode compartilhar a mesma pasta em vários nós de um cluster. Por exemplo, se você tiver um cluster de servidor de arquivos de quatro nós que está usando a expansão de bloco de mensagens de servidor (SMB), um computador que executa o Windows Server 2012 R2 ou Windows Server 2012 pode acessar compartilhamentos de arquivos de qualquer um dos quatro nós. Isso é possível com o aproveitamento dos novos recursos e capacidades de cluster de failover do Windows Server na nova versão do protocolo do servidor de arquivos Windows – SMB 3.0. Os administradores dos servidores de arquivos podem oferecer compartilhamentos de arquivos de expansão e serviços de arquivos continuamente disponíveis aos aplicativos para servidores e responder rapidamente à crescente demanda simplesmente colocando mais servidores online. Tudo isso pode ser feito em um ambiente de produção e é completamente transparente ao aplicativo para servidores.

Os principais benefícios fornecidos pelo servidor de arquivos de escalabilidade horizontal incluem:

- **Compartilhamentos de arquivos de ativo-ativo**. Todos os nós de cluster podem aceitar e atender a solicitações do cliente SMB. Ao tornar o conteúdo do compartilhamento de arquivos acessível em todos os nós de cluster simultaneamente, os clusters e clientes SMB 3.0 cooperam para fornecer um failover transparente aos nós de cluster alternativos durante a manutenção planejada e falhas não planejadas com interrupção do serviço.
- **Largura de banda aumentada**. A largura de banda máxima de compartilhamento é a largura de banda total de todos os nós de cluster de servidor de arquivos. Ao contrário das versões anteriores do Windows Server, a largura de banda total não é mais limitada à largura de banda de um único nó de cluster; mas, em vez disso, a capacidade do armazenamento do sistema de suporte define as restrições. Você pode aumentar a largura de banda total adicionando nós.
- **CHKDSK com zero tempo de inatividade**. CHKDSK no Windows Server 2012 é significativamente aprimorado para reduzir drasticamente o tempo que um sistema de arquivos fica offline para reparo. Os CSVs (Volumes Compartilhados Clusterizados) levam isso adiante e eliminam a fase offline. Um CSVFS (Sistema de Arquivos CSV) pode executar o CHKDSK sem causar impacto nos aplicativos com identificadores abertos no sistema de arquivos.
- **Cache de Volume compartilhado clusterizado**. CSVs no Windows Server 2012 introduz o suporte para um cache de leitura, que pode melhorar significativamente o desempenho em determinados cenários, como no Virtual Desktop Infrastructure (VDI).
- **Gerenciamento mais simples**. Com o servidor de arquivos de escalabilidade horizontal, você criará os servidores de arquivos escalável e, em seguida, adicione o necessário CSVs e compartilhamentos de arquivos. Não é mais preciso criar vários servidores de arquivos clusterizados, cada um com discos de cluster separados, e depois desenvolver políticas de posicionamento para garantir a atividade em cada nó de cluster.
- **Rebalanceamento automático de clientes do servidor de arquivos de escalabilidade horizontal**. No Windows Server 2012 R2, o rebalanceamento automático melhora a escalabilidade e capacidade de gerenciamento para servidores de arquivos de escalabilidade horizontal. As conexões de clientes SMB são controladas por compartilhamento de arquivos (em vez de por servidor), e os clientes são então redirecionados para o nó de cluster com o melhor acesso ao volume usado pelo compartilhamento de arquivos. Isso melhora a eficiência, reduzindo o tráfego de redirecionamento entre nós de servidor de arquivos. Os clientes são redirecionados após uma conexão inicial e quando o armazenamento de cluster é reconfigurado.

## <a name="in-this-scenario"></a>Neste cenário

Os tópicos a seguir estão disponíveis para ajudá-lo a implantar um servidor de arquivos de escalabilidade horizontal:

- [Plano para o servidor de arquivos de escalabilidade horizontal](<https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2012-r2-and-2012/jj134258(v%3dws.11)>)

  - [Etapa 1: Planejar o armazenamento no servidor de arquivos de escalabilidade horizontal](<https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2012-r2-and-2012/jj134181%28v%3dws.11%29>)
  - [Etapa 2: Planejar a rede no servidor de arquivos de escalabilidade horizontal](<https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2012-r2-and-2012/jj134253%28v%3dws.11%29>)

- [Implantar servidor de arquivos de escalabilidade horizontal](<https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2012-r2-and-2012/hh831359%28v%3dws.11%29>)

  - [Etapa 1: Instale os pré-requisitos do servidor de arquivos de escalabilidade horizontal](<https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2012-r2-and-2012/hh831478%28v%3dws.11%29>)
  - [Etapa 2: Configurar o servidor de arquivos de escalabilidade horizontal](<https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2012-r2-and-2012/hh831718%28v%3dws.11%29>)
  - [Etapa 3: Configurar o Hyper-V para usar o servidor de arquivos de escalabilidade horizontal](<https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2012-r2-and-2012/hh831463%28v%3dws.11%29>)
  - [Etapa 4: Configurar o Microsoft SQL Server para usar o servidor de arquivos de escalabilidade horizontal](<https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2012-r2-and-2012/hh831815%28v%3dws.11%29>)

## <a name="when-to-use-scale-out-file-server"></a>Quando usar o servidor de arquivos de expansão

Você não deve usar o servidor de arquivos de expansão se a carga de trabalho gerar um número maior de operações de metadados, como abrir arquivos, fechar arquivos, criar novos arquivos ou renomear os arquivos existentes. Um típico operador de informações poderia gerar uma série de operações de metadados. Use um servidor de arquivos de escalabilidade horizontal se estiver interessado na escalabilidade e simplicidade que ele oferece e precisar apenas de tecnologias que sejam compatíveis com o servidor de arquivos de escalabilidade horizontal.

A tabela a seguir lista os recursos do SMB 3.0, os sistemas de arquivos comuns do Windows, tecnologias de gerenciamento de dados de servidor de arquivo e cargas de trabalho comuns. Você pode ver se a tecnologia é compatível com o servidor de arquivos de escalabilidade horizontal, ou se ele requer um servidor de arquivos clusterizado tradicional (também conhecido como um servidor de arquivos para uso geral).

<table>
<thead>
<tr class="header">
<th>Tecnologia</th>
<th>Recurso</th>
<th>Cluster de servidor de arquivos de uso geral</th>
<th>Servidor de Arquivos Escalável</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td>SMB</td>
<td>SMB Continuous Availability</td>
<td>Sim</td>
<td>Sim</td>
</tr>
<tr class="even">
<td>SMB</td>
<td>SMB Multichannel</td>
<td>Sim</td>
<td>Sim</td>
</tr>
<tr class="odd">
<td>SMB</td>
<td>SMB Direct</td>
<td>Sim</td>
<td>Sim</td>
</tr>
<tr class="even">
<td>SMB</td>
<td>Criptografia SMB</td>
<td>Sim</td>
<td>Sim</td>
</tr>
<tr class="odd">
<td>SMB</td>
<td>SMB Transparent failover</td>
<td>Sim (se estiver habilitada a disponibilidade contínua)</td>
<td>Sim</td>
</tr>
<tr class="even">
<td>Sistema de Arquivos</td>
<td>NTFS</td>
<td>Sim</td>
<td>N/D</td>
</tr>
<tr class="odd">
<td>Sistema de Arquivos</td>
<td>Sistema de arquivos resiliente (<a href="https://docs.microsoft.com/windows-server/storage/refs/refs-overview">ReFS</a>)</td>
<td>Recomendado com o armazenamento de espaços diretos</td>
<td>Recomendado com o armazenamento de espaços diretos</td>
</tr>
<tr class="even">
<td>Sistema de Arquivos</td>
<td>Sistema de arquivos CSV (Volume Compartilhado Clusterizado)</td>
<td>N/D</td>
<td>Sim</td>
</tr>
<tr class="odd">
<td>Gerenciamento de arquivos</td>
<td>BranchCache</td>
<td>Sim</td>
<td>Não</td>
</tr>
<tr class="even">
<td>Gerenciamento de arquivos</td>
<td>Eliminação de duplicação de dados (Windows Server 2012)</td>
<td>Sim</td>
<td>Não</td>
</tr>
<tr class="odd">
<td>Gerenciamento de arquivos</td>
<td>Eliminação de duplicação de dados (Windows Server 2012 R2)</td>
<td>Sim</td>
<td>Sim (VDI somente)</td>
</tr>
<tr class="even">
<td>Gerenciamento de arquivos</td>
<td>Raiz de servidor raiz DFSN (Namespace do DFS)</td>
<td>Sim</td>
<td>Não</td>
</tr>
<tr class="odd">
<td>Gerenciamento de arquivos</td>
<td>Servidor de destino da pasta DFSN (Namespace do DFS)</td>
<td>Sim</td>
<td>Sim</td>
</tr>
<tr class="even">
<td>Gerenciamento de arquivos</td>
<td>DFS-R (Replicação do DFS)</td>
<td>Sim</td>
<td>Não</td>
</tr>
<tr class="odd">
<td>Gerenciamento de arquivos</td>
<td>Gerenciador de Recursos de Servidor de Arquivos (telas e cotas)</td>
<td>Sim</td>
<td>Não</td>
</tr>
<tr class="even">
<td>Gerenciamento de arquivos</td>
<td>Infraestrutura de Classificação de Arquivos</td>
<td>Sim</td>
<td>Não</td>
</tr>
<tr class="odd">
<td>Gerenciamento de arquivos</td>
<td>Controle de Acesso Dinâmico (acesso baseado em declarações, CAP)</td>
<td>Sim</td>
<td>Não</td>
</tr>
<tr class="even">
<td>Gerenciamento de arquivos</td>
<td>Redirecionamento de pasta</td>
<td>Sim</td>
<td>Não recomendável*</td>
</tr>
<tr class="odd">
<td>Gerenciamento de arquivos</td>
<td>Arquivos offline (cache do lado do cliente)</td>
<td>Sim</td>
<td>Não recomendável*</td>
</tr>
<tr class="even">
<td>Gerenciamento de arquivos</td>
<td>Perfis de usuário em roaming</td>
<td>Sim</td>
<td>Não recomendável*</td>
</tr>
<tr class="odd">
<td>Gerenciamento de arquivos</td>
<td>Diretórios base</td>
<td>Sim</td>
<td>Não recomendável*</td>
</tr>
<tr class="even">
<td>Gerenciamento de arquivos</td>
<td>Pastas de trabalho</td>
<td>Sim</td>
<td>Não</td>
</tr>
<tr class="odd">
<td>NFS</td>
<td>Servidor NFS</td>
<td>Sim</td>
<td>Não</td>
</tr>
<tr class="even">
<td>Aplicativos</td>
<td>Hyper-V</td>
<td>Não recomendável</td>
<td>Sim</td>
</tr>
<tr class="odd">
<td>Aplicativos</td>
<td>Microsoft SQL Server</td>
<td>Não recomendável</td>
<td>Sim</td>
</tr>
</tbody>
</table>

\* Redirecionamento de pasta, arquivos Offline, perfis de usuário móvel ou diretórios base geram um grande número de gravações que deve ser gravado imediatamente no disco (sem buffer) ao usar compartilhamentos de arquivos continuamente disponíveis, reduzindo o desempenho em comparação com compartilhamentos de arquivos de finalidade geral. Compartilhamentos de arquivos disponíveis continuamente também são incompatíveis com o Gerenciador de Recursos de Servidor de Arquivos e PCs que executam o Windows XP. Além disso, arquivos offline podem não fazer a transição para o modo offline por 3 a 6 minutos depois que um usuário perde o acesso a um compartilhamento, o que pode frustrar os usuários que ainda não estão usando o modo Sempre offline de arquivos offline.

## <a name="practical-applications"></a>Aplicações práticas

Servidores de Arquivos de Escalabilidade Horizontal são ideais para armazenamento de aplicativos para servidores. Alguns exemplos de aplicativos de servidor que podem armazenar seus dados em um compartilhamento de arquivos de escalabilidade horizontal estão listados abaixo:

- O servidor Web IIS (Serviços de Informações da Internet) pode armazenar dados de configuração e de sites em um compartilhamento de arquivos de escalabilidade horizontal. Para obter mais informações, consulte [Configuração compartilhada](http://www.iis.net/learn/manage/managing-your-configuration-settings/shared-configuration_264).
- O Hyper-V pode armazenar configuração e discos virtuais dinâmicos em um compartilhamento de arquivos de escalabilidade horizontal. Para obter mais informações, consulte [Implantar Hyper-V no SMB](<https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2012-r2-and-2012/jj134187(v%3dws.11)>).
- O SQL Server pode armazenar arquivos de banco de dados dinâmicos em um compartilhamento de arquivo de escalabilidade horizontal. Para obter mais informações, consulte [Instalar SQL Server com compartilhamento de arquivo SMB como uma opção de armazenamento](https://docs.microsoft.com/sql/database-engine/install-windows/install-sql-server-with-smb-fileshare-as-a-storage-option).
- VMM (Virtual Machine Manager) pode armazenar um compartilhamento de biblioteca (que contém modelos de máquina virtual e arquivos relacionados) em um compartilhamento de arquivos de escalabilidade horizontal. No entanto, o servidor de biblioteca não pode ser um servidor de arquivos de escalabilidade horizontal, ele deve estar em um servidor autônomo ou um cluster de failover que não usa a função de cluster de servidor de arquivos de escalabilidade horizontal.

Se você usar um compartilhamento de arquivos de escalabilidade horizontal como um compartilhamento de biblioteca, só poderá usar tecnologias compatíveis com o servidor de arquivos de escalabilidade horizontal. Por exemplo, você não pode usar a Replicação do DFS para replicar um compartilhamento de biblioteca hospedado em um compartilhamento de arquivo de escalabilidade horizontal. Também é importante que o servidor de arquivos de escalabilidade horizontal tenha as atualizações de software mais recentes instaladas.

Para usar um compartilhamento de arquivos de escalabilidade horizontal como um compartilhamento de biblioteca, primeiro adicione um servidor de biblioteca (provavelmente uma máquina virtual) com um compartilhamento local ou nenhum compartilhamento. Ao adicionar um compartilhamento de biblioteca, escolha um compartilhamento de arquivos que esteja hospedado em um servidor de arquivos de escalabilidade horizontal. Esse compartilhamento deve ser gerenciado pelo VMM e criado exclusivamente para uso do servidor de biblioteca. Além disso, certifique-se de instalar as atualizações mais recentes no servidor de arquivos de escalabilidade horizontal. Para obter mais informações sobre como adicionar servidores de biblioteca do VMM e compartilhamentos de biblioteca, consulte [adicionar perfis à biblioteca do VMM](https://docs.microsoft.com/system-center/vmm/library-profiles?view=sc-vmm-1801). Para obter uma lista de hotfixes atualmente disponíveis para serviços de arquivo e armazenamento, consulte o [artigo da base de dados de conhecimento da Microsoft 2899011](https://support.microsoft.com/help/2899011/list-of-currently-available-hotfixes-for-the-file-services-technologie).

>[!NOTE]
>Alguns usuários, como profissionais da informação, têm cargas de trabalho com impacto maior no desempenho. Por exemplo, operações, como abrir e fechar arquivos, criar novos arquivos e renomear arquivos existentes, quando executada por vários usuários, têm um impacto no desempenho. Se um compartilhamento de arquivos estiver habilitado com disponibilidade contínua, ele fornecerá integridade de dados, mas também afetará o desempenho geral. Disponibilidade contínua exige que dados sejam gravados por meio de disco para garantir a integridade em caso de falha de um nó de cluster em um servidor de arquivos de escalabilidade horizontal. Portanto, um usuário que copia vários arquivos grandes em um servidor de arquivos pode esperar um desempenho significativamente mais lento no compartilhamento de arquivos continuamente disponíveis.

## <a name="features-included-in-this-scenario"></a>Recursos incluídos neste cenário

A tabela a seguir lista os recursos que fazem parte deste cenário e descreve como dar suporte a ele.

<table>
<thead>
<tr class="header">
<th>Recurso</th>
<th>Como este cenário tem suporte</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td><a href="failover-clustering.md">Clustering de failover</a></td>
<td>Clusters de failover adicionaram os seguintes recursos no Windows Server 2012 para dar suporte ao servidor de arquivos de escalabilidade horizontal: Nome de rede distribuída, o tipo de recurso do servidor de arquivos de escalabilidade horizontal, Cluster Shared Volumes (CSV) 2 e a função de expansão alta disponibilidade do servidor. Para obter mais informações sobre esses recursos, consulte <a href="https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2012-r2-and-2012/dn265972(v%3dws.11)">o que há de novo no Clustering de Failover do Windows Server 2012 [redirecionado]</a>.</td>
</tr>
<tr class="even">
<td><a href="https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2012-r2-and-2012/hh831795(v%3dws.11)">Protocolo SMB</a></td>
<td>SMB 3.0 adicionou os seguintes recursos no Windows Server 2012 para dar suporte ao servidor de arquivos de escalabilidade horizontal: SMB Transparent Failover, SMB Multichannel e SMB Direct.<br />
<br />
Para obter mais informações sobre funcionalidades novas e alteradas para SMB no Windows Server 2012 R2, consulte <a href="https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2012-r2-and-2012/hh831474(v%3dws.11)">novidades do SMB no Windows Server</a>.</td>
</tr>
</tbody>
</table>

## <a name="more-information"></a>Mais informações

- [Guia de considerações de Design de armazenamento definida por software](<https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2012-r2-and-2012/mt243829(v%3dws.11)>)
- [Aumentando a disponibilidade de rede, armazenamento e servidor](<https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2012-r2-and-2012/hh831437(v%3dws.11)>)
- [Implantar o Hyper-V no SMB](<https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2012-r2-and-2012/jj134187(v%3dws.11)>)
- [Implantando servidores de arquivos rápidos e eficientes para aplicativos de servidor](<https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2012-r2-and-2012/hh831723(v%3dws.11)>)
- [Escalar horizontalmente ou não, eis a questão](https://blogs.technet.com/b/filecab/archive/2013/12/05/to-scale-out-or-not-to-scale-out-that-is-the-question.aspx) (postagem de blog)
- [Redirecionamento de pasta, arquivos Offline e perfis de usuário móvel](<https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2012-r2-and-2012/hh848267(v%3dws.11)>)