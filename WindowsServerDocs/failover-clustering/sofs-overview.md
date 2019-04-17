---
title: Servidor de arquivos de dimensionamento para visão geral de dados de aplicativo
description: Visão geral do recurso de servidor de arquivos de dimensionamento para Windows Server 201 R2, Windows Server 2012 e Windows Server 2016.
ms.prod: windows-server-threshold
ms.topic: article
author: JasonGerend
ms.author: jgerend
ms.technology: storage-failover-clustering
ms.date: 04/26/2018
ms.localizationpriority: medium
ms.openlocfilehash: 04e25e9c69062611d9d14c220614f148ac5de770
ms.sourcegitcommit: e0479b0114eac7f232e8b1e45eeede96ccd72b26
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/22/2018
ms.locfileid: "2081806"
---
# <a name="scale-out-file-server-for-application-data-overview"></a>Servidor de arquivos de dimensionamento para visão geral de dados de aplicativo

>Aplica-se a: Windows Server 2012 R2, o Windows Server 2012, o Windows Server 2016

Servidor de arquivos de dimensionamento é um recurso que foi projetado para fornecer os compartilhamentos de arquivos de dimensionamento continuamente disponíveis para o armazenamento de aplicativos de servidor baseado em arquivo. Compartilhamentos de arquivos de dimensionamento fornece a capacidade de compartilhar a mesma pasta de vários nós do mesmo cluster. Esse cenário aborda como planejar e implantar o servidor de arquivos de dimensionamento.

Você pode implantar e configurar um servidor de arquivos em cluster usando um dos seguintes métodos:

- **Servidor de arquivos de dimensionamento para dados de aplicativo** Esse recurso de servidor de arquivos em cluster foi introduzido no Windows Server 2012 e permite que você armazenar dados de aplicativo de servidor, como arquivos de máquina virtual do Hyper-V, em compartilhamentos de arquivos e obter um nível semelhante de confiabilidade, disponibilidade, capacidade de gerenciamento e alta desempenho que você esperaria de uma rede de área de armazenamento. Todos os compartilhamentos de arquivo são online simultaneamente em todos os nós. Compartilhamentos de arquivos associados a esse tipo de servidor de arquivos em cluster são chamados de compartilhamentos de arquivos de dimensionamento. Às vezes, isso é conhecido como ativa. Este é o tipo de servidor de arquivos recomendado ao implantar o Hyper-V sobre bloco de mensagens do servidor (SMB) ou Microsoft SQL Server em SMB.
- **Servidor de arquivos para uso geral** Esta é a continuação do servidor de arquivos em cluster com suporte no Windows Server desde o lançamento do cluster de Failover. Esse tipo de servidor de arquivos em cluster e, portanto, todos os compartilhamentos associados ao servidor de arquivos em cluster, está online em um nó de cada vez. Às vezes, isso é conhecido como ativa-passiva ou duplos ativos. Compartilhamentos de arquivos associados a esse tipo de servidor de arquivos em cluster são chamados de compartilhamentos de arquivos em cluster. Este é o tipo de servidor de arquivos recomendado ao implantar cenários de trabalhador de informações.

## <a name="scenario-description"></a>Descrição do cenário

Com compartilhamentos de arquivos de dimensionamento, você pode compartilhar a mesma pasta de vários nós de um cluster. Por exemplo, se você tiver um cluster de servidor de arquivo de quatro nós que está usando o bloco de mensagens do servidor (SMB) dimensionamento, um computador que executa o Windows Server 2012 R2 ou Windows Server 2012 pode acessar os compartilhamentos de arquivos de qualquer um dos quatro nós. Isso é obtido aproveitando os novos recursos do serviço de cluster de Failover do Windows Server e os recursos do protocolo de servidor de arquivo do Windows, SMB 3.0. Administradores do servidor de arquivos podem fornecer serviços de arquivos disponíveis continuamente a aplicativos de servidor e compartilhamentos de arquivos de dimensionamento e responder rapidamente às demandas de aumento, trazendo simplesmente mais servidores online. Tudo isso pode ser feito em um ambiente de produção e é completamente transparente para o aplicativo de servidor.

Principais benefícios fornecidos pelo servidor de arquivos de dimensionamento no incluem:

- **Compartilhamentos de arquivos ativa-ativa**. Todos os nós de cluster podem aceitar e atender a solicitações de clientes SMB. Tornando o arquivo para compartilhar o conteúdo acessível através de todos os nós de cluster simultaneamente, clientes e os clusters de SMB 3.0 cooperam para fornecer failover transparente para nós de cluster alternativo durante manutenção planejada e não planejadas falhas de serviço interrupção.
- **Maior largura de banda**. A largura de banda máxima de compartilhamento é a largura de banda total de todos os nós de cluster de servidores de arquivo. Diferentemente das versões anteriores do Windows Server, a largura de banda total não mais é restrita a largura de banda de um único nó de cluster; mas, em vez disso, a capacidade do sistema de armazenamento de apoio define as restrições. Você pode aumentar a largura de banda total, adicionando nós.
- **CHKDSK com tempo de inatividade zero**. CHKDSK no Windows Server 2012 significativamente é aprimorado para reduzir drasticamente o tempo de que um sistema de arquivos está offline para reparar. Volume compartilhado de cluster (CSVs) levar este uma etapa posterior, eliminando a fase offline. Um sistema de arquivo CSV (CSVFS) pode usar CHKDSK sem afetar os aplicativos com identificadores abertos no sistema de arquivos.
- **Cache clusterizado Volume compartilhado**. CSVs no Windows Server 2012 introduz o suporte para um cache de leitura, que pode melhorar significativamente o desempenho em determinados cenários, como no Virtual Desktop Infrastructure (VDI).
- **Gerenciamento mais simples**. Com o servidor de arquivos de dimensionamento, você cria os servidores de arquivo de dimensionamento e adicione os CSVs necessárias e compartilhamentos de arquivos. Não é necessário criar vários servidores de arquivos em cluster, cada um com discos de cluster separado, e, então, desenvolver políticas de posicionamento para garantir a atividade em cada nó do cluster.
- **Automática reequilíbrio dos clientes do servidor de arquivos de dimensionamento**. No Windows Server 2012 R2, reequilíbrio automática melhora a escalabilidade e a capacidade de gerenciamento para servidores de arquivos de dimensionamento. Conexões de cliente SMB são rastreadas por SMS (em vez de por servidor), e os clientes serão redirecionados para o nó de cluster com o melhor acesso ao volume usado pelo compartilhamento de arquivo. Isso aumenta a eficiência, reduzindo o tráfego de redirecionamento entre nós de servidores de arquivo. Os clientes serão redirecionados após uma conexão inicial e quando o armazenamento em cluster é reconfigurado.

## <a name="in-this-scenario"></a>Neste cenário

Os tópicos a seguir estão disponíveis para ajudá-lo a implantar um servidor de arquivos de dimensionamento:

- [Plano para o servidor de arquivos de dimensionamento](<https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2012-r2-and-2012/jj134258(v%3dws.11)>)

  - [Etapa 1: Planejar o armazenamento no servidor de arquivos de dimensionamento](<https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2012-r2-and-2012/jj134181%28v%3dws.11%29>)
  - [Etapa 2: Planeje de rede no servidor de arquivos de dimensionamento](<https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2012-r2-and-2012/jj134253%28v%3dws.11%29>)

- [Implantar um servidor de arquivos de dimensionamento](<https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2012-r2-and-2012/hh831359%28v%3dws.11%29>)

  - [Etapa 1: Instalar os pré-requisitos para o servidor de arquivos de dimensionamento](<https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2012-r2-and-2012/hh831478%28v%3dws.11%29>)
  - [Etapa 2: Configurar o servidor de arquivos de dimensionamento](<https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2012-r2-and-2012/hh831718%28v%3dws.11%29>)
  - [Etapa 3: Configurar o Hyper-V para usar o servidor de arquivos de dimensionamento](<https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2012-r2-and-2012/hh831463%28v%3dws.11%29>)
  - [Etapa 4: Configurar o Microsoft SQL Server para usar o servidor de arquivos de dimensionamento](<https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2012-r2-and-2012/hh831815%28v%3dws.11%29>)

## <a name="when-to-use-scale-out-file-server"></a>Quando usar o servidor de arquivos de dimensionamento

Você não deve usar o servidor de arquivos de dimensionamento se sua carga de trabalho gera um alto número de operações de metadados, como a abertura de arquivos, fechando arquivos, criação de novos arquivos ou renomeando arquivos existentes. Um operador de informações típica poderia gerar muitas operações de metadados. Se você está interessado a escalabilidade e a simplicidade que ele oferece e se você precisar apenas de tecnologias que são compatíveis com o servidor de arquivos de dimensionamento, você deve usar um servidor de arquivos de dimensionamento.

A tabela a seguir lista os recursos no SMB 3.0, os sistemas de arquivos comuns do Windows, tecnologias de gerenciamento de dados de servidor de arquivo e cargas de trabalho comuns. Você pode ver se a tecnologia é compatível com o servidor de arquivos de dimensionamento, ou se ele requer um servidor de arquivos em cluster tradicional (também conhecido como um servidor de arquivos para uso geral).

<table>
<thead>
<tr class="header">
<th>Área de tecnologia</th>
<th>Recurso</th>
<th>Cluster de servidor de arquivo de uso geral</th>
<th>Servidor de arquivos de dimensionamento</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td>SMB</td>
<td>Disponibilidade contínua de SMB</td>
<td>Sim</td>
<td>Sim</td>
</tr>
<tr class="even">
<td>SMB</td>
<td>SMB multicanais</td>
<td>Sim</td>
<td>Sim</td>
</tr>
<tr class="odd">
<td>SMB</td>
<td>SMB direto</td>
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
<td>Failover transparente para SMB</td>
<td>Sim (se disponibilidade contínua estiver habilitada)</td>
<td>Sim</td>
</tr>
<tr class="even">
<td>Sistema de arquivos</td>
<td>NTFS</td>
<td>Sim</td>
<td>NA</td>
</tr>
<tr class="odd">
<td>Sistema de arquivos</td>
<td>Sistema de arquivos resiliente (<a href="https://docs.microsoft.com/windows-server/storage/refs/refs-overview">ReFS</a>)</td>
<td>Recomendado com armazenamento espaça direto</td>
<td>Recomendado com armazenamento espaça direto</td>
</tr>
<tr class="even">
<td>Sistema de arquivos</td>
<td>Sistema de arquivos de Volume (CSV) compartilhados do cluster</td>
<td>NA</td>
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
<td>Eliminação da duplicação de dados (Windows Server 2012)</td>
<td>Sim</td>
<td>Não</td>
</tr>
<tr class="odd">
<td>Gerenciamento de arquivos</td>
<td>Eliminação da duplicação de dados (Windows Server 2012 R2)</td>
<td>Sim</td>
<td>Sim (VDI)</td>
</tr>
<tr class="even">
<td>Gerenciamento de arquivos</td>
<td>Raiz do servidor de raiz de Namespace DFS (DFSN)</td>
<td>Sim</td>
<td>Não</td>
</tr>
<tr class="odd">
<td>Gerenciamento de arquivos</td>
<td>Servidor de destino de pasta de Namespace DFS (DFSN)</td>
<td>Sim</td>
<td>Sim</td>
</tr>
<tr class="even">
<td>Gerenciamento de arquivos</td>
<td>Replicação do DFS (DFSR)</td>
<td>Sim</td>
<td>Não</td>
</tr>
<tr class="odd">
<td>Gerenciamento de arquivos</td>
<td>File Server Resource Manager (telas e cotas)</td>
<td>Sim</td>
<td>Não</td>
</tr>
<tr class="even">
<td>Gerenciamento de arquivos</td>
<td>Infraestrutura de classificação de arquivo</td>
<td>Sim</td>
<td>Não</td>
</tr>
<tr class="odd">
<td>Gerenciamento de arquivos</td>
<td>Controle de acesso dinâmico (acesso baseado em declarações, COBRIR)</td>
<td>Sim</td>
<td>Não</td>
</tr>
<tr class="even">
<td>Gerenciamento de arquivos</td>
<td>Redirecionamento de pasta</td>
<td>Sim</td>
<td>Não é recomendável *</td>
</tr>
<tr class="odd">
<td>Gerenciamento de arquivos</td>
<td>Arquivos offline (o cache do cliente)</td>
<td>Sim</td>
<td>Não é recomendável *</td>
</tr>
<tr class="even">
<td>Gerenciamento de arquivos</td>
<td>Perfis de usuário móvel</td>
<td>Sim</td>
<td>Não é recomendável *</td>
</tr>
<tr class="odd">
<td>Gerenciamento de arquivos</td>
<td>Diretórios locais</td>
<td>Sim</td>
<td>Não é recomendável *</td>
</tr>
<tr class="even">
<td>Gerenciamento de arquivos</td>
<td>Pastas de trabalho</td>
<td>Sim</td>
<td>Não</td>
</tr>
<tr class="odd">
<td>NFS</td>
<td>Servidor</td>
<td>Sim</td>
<td>Não</td>
</tr>
<tr class="even">
<td>Aplicativos</td>
<td>Hyper-V</td>
<td>Não recomendado</td>
<td>Sim</td>
</tr>
<tr class="odd">
<td>Aplicativos</td>
<td>Microsoft SQL Server</td>
<td>Não recomendado</td>
<td>Sim</td>
</tr>
</tbody>
</table>

\ * Redirecionamento de pasta, arquivos Offline, perfis de usuários móveis ou diretórios inicial geram um grande número de gravações que devem ser gravados imediatamente em disco (sem buffer) ao usar os compartilhamentos de arquivos disponíveis continuamente, reduzindo o desempenho em comparação com compartilhamentos de arquivos de finalidade geral. Continuamente compartilhamentos de arquivos disponíveis também são incompatíveis com o File Server Resource Manager e PCs que executam o Windows XP. Além disso, arquivos Offline talvez não fazer a transição para o modo offline por 3 a 6 minutos depois que um usuário perde o acesso a um compartilhamento, o qual poderia frustram os usuários que ainda não estiverem usando o modo Offline sempre dos arquivos Offline.

## <a name="practical-applications"></a>Aplicações práticas

Servidores de arquivo de dimensionamento são ideais para armazenamento de aplicativos de servidor. Alguns exemplos de aplicativos para servidores que podem armazenar seus dados em um compartilhamento de arquivo de dimensionamento estão listados abaixo:

- O servidor Web do Internet Information Services (IIS) pode armazenar dados de configuração e para sites da Web em um compartilhamento de arquivos de dimensionamento. Para obter mais informações, consulte [Configuração compartilhada](http://www.iis.net/learn/manage/managing-your-configuration-settings/shared-configuration_264).
- Hyper-V pode armazenar configuração e discos virtuais ao vivo em um compartilhamento de arquivos de dimensionamento. Para obter mais informações, consulte [Implantar o Hyper-V em SMB](<https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2012-r2-and-2012/jj134187(v%3dws.11)>).
- SQL Server pode armazenar arquivos de banco de dados ao vivo em um compartilhamento de arquivo de dimensionamento. Para obter mais informações, consulte [instalar o SQL Server com arquivos SMB compartilhar como uma opção de armazenamento](https://docs.microsoft.com/sql/database-engine/install-windows/install-sql-server-with-smb-fileshare-as-a-storage-option).
- Virtual Machine Manager (VMM) pode armazenar um compartilhamento de biblioteca (que contém os modelos de máquina virtual e arquivos relacionados) em um compartilhamento de arquivos de dimensionamento. No entanto, o servidor da biblioteca não pode ser um servidor de arquivos de dimensionamento — ele deve estar em um servidor autônomo ou em um cluster de failover que não usa a função de cluster do servidor de arquivos de dimensionamento.

Se você usar um compartilhamento de arquivos de dimensionamento como um compartilhamento de biblioteca, você pode usar somente as tecnologias que são compatíveis com o servidor de arquivos de dimensionamento. Por exemplo, você não pode usar replicação DFS para replicar um compartilhamento de biblioteca hospedado em um compartilhamento de arquivos de dimensionamento. Também é importante que o servidor de arquivos de dimensionamento tenha as atualizações de software mais recentes instaladas.

Para usar um compartilhamento de arquivos de dimensionamento como um compartilhamento de biblioteca, primeiro adicione um biblioteca servidor (provavelmente uma máquina virtual) com um local de compartilhamento ou nenhuma compartilhamentos nisso. Quando você adiciona um compartilhamento de biblioteca, escolha um compartilhamento de arquivo que está hospedado em um servidor de arquivos de dimensionamento. Esse compartilhamento deve ser criada exclusivamente para uso pelo servidor de biblioteca e gerenciados VMM. Também Certifique-se de instalar as atualizações mais recentes no servidor de arquivos de dimensionamento. Para obter mais informações sobre como adicionar servidores de biblioteca do VMM e compartilhamentos de biblioteca, consulte [Adicionar perfis à biblioteca do VMM](https://docs.microsoft.com/system-center/vmm/library-profiles?view=sc-vmm-1801). Para obter uma lista dos hotfixes atualmente disponíveis para os serviços de armazenamento de arquivos e, consulte o [artigo da Base de Conhecimento Microsoft 2899011](https://support.microsoft.com/help/2899011/list-of-currently-available-hotfixes-for-the-file-services-technologie).

>[!NOTE]
>Alguns usuários, como os operadores de informações têm cargas de trabalho que têm um maior impacto no desempenho. Por exemplo, operações, como abrir e fechar arquivos, criação de novos arquivos e renomeando arquivos existentes, quando executada por vários usuários, têm um impacto no desempenho. Se um compartilhamento de arquivos estiver habilitado com disponibilidade contínua, ele fornece a integridade dos dados, mas também afeta o desempenho geral. Disponibilidade contínua exige que dados grava por meio do disco para garantir a integridade no caso de falha de um nó de cluster em um servidor de arquivos de dimensionamento. Portanto, um usuário que copia vários arquivos grandes para um servidor de arquivos pode esperar o desempenho significativamente mais lento no compartilhamento de arquivos disponíveis continuamente.

## <a name="features-included-in-this-scenario"></a>Recursos incluídos neste cenário

A tabela a seguir lista os recursos que fazem parte deste cenário e descreve como suporte.

<table>
<thead>
<tr class="header">
<th>Recurso</th>
<th>Como ele oferece suporte a esse cenário</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td><a href="failover-clustering.md">Clustering de failover</a></td>
<td>Clusters de failover adicionados os seguintes recursos no Windows Server 2012 para dar suporte ao servidor de arquivos de dimensionamento: distribuído nome de rede, o tipo de recurso de servidor de arquivos de dimensionamento, Cluster compartilhados Volumes (CSV) 2 e a função de dimensionamento alta disponibilidade do servidor. Para obter mais informações sobre esses recursos, consulte <a href="https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2012-r2-and-2012/dn265972(v%3dws.11)">What's New no cluster de Failover no Windows Server 2012 [redirecionados]</a>.</td>
</tr>
<tr class="even">
<td><a href="https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2012-r2-and-2012/hh831795(v%3dws.11)">Protocolo SMB</a></td>
<td>SMB 3.0 adicionados os seguintes recursos no Windows Server 2012 para dar suporte ao dimensionamento servidor de arquivos: SMB de Failover transparente para SMB multicanais e SMB direto.<br />
<br />
Para obter mais informações sobre a funcionalidade de nova e alterada para SMB no Windows Server 2012 R2, consulte <a href="https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2012-r2-and-2012/hh831474(v%3dws.11)">What's New in SMB no Windows Server</a>.</td>
</tr>
</tbody>
</table>

## <a name="more-information"></a>Mais informações

- [Guia de considerações de Design de armazenamento definido pelo software](<https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2012-r2-and-2012/mt243829(v%3dws.11)>)
- [Aumentar a disponibilidade da rede, armazenamento e servidor](<https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2012-r2-and-2012/hh831437(v%3dws.11)>)
- [Implantar o Hyper-V no SMB](<https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2012-r2-and-2012/jj134187(v%3dws.11)>)
- [Implantando servidores de arquivo rápida e eficiente para aplicativos de servidor](<https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2012-r2-and-2012/hh831723(v%3dws.11)>)
- [Para dimensionar check-out ou não para dimensionamento, que é a pergunta](https://blogs.technet.com/b/filecab/archive/2013/12/05/to-scale-out-or-not-to-scale-out-that-is-the-question.aspx) (postagem em blog)
- [Visão geral de Redirecionamento de Pasta, Arquivos Offline e perfis de usuários móveis](<https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2012-r2-and-2012/hh848267(v%3dws.11)>)