---
title: Visão geral do compartilhamento de arquivos usando o protocolo SMB 3 no Windows Server
description: Uma visão geral do uso do protocolo SMB 3 para compartilhamentos de arquivos e serviços de arquivos com o Windows Server.
ms.prod: windows-server
ms.topic: article
author: JasonGerend
ms.author: jgerend
ms.technology: storage
ms.date: 07/09/2018
ms.localizationpriority: medium
ms.openlocfilehash: b40c179d242a0c48c6eb176db1225979f9e6a123
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71402090"
---
# <a name="overview-of-file-sharing-using-the-smb-3-protocol-in-windows-server"></a>Visão geral do compartilhamento de arquivos usando o protocolo SMB 3 no Windows Server

>Aplica-se a: Windows Server 2012 R2, Windows Server 2012, Windows Server 2016

Este tópico descreve o recurso SMB 3,0 no Windows Server® 2012, Windows Server 2012 R2 e Windows Server 2016 – usos práticos para o recurso, a funcionalidade nova ou atualizada mais significativa nesta versão em comparação com as versões anteriores e o hardware requirement.

## <a name="feature-description"></a>Descrição do recurso

Server Message Block (SMB) é um protocolo de compartilhamento de arquivos em rede que permite que os aplicativos de um computador leiam e gravem em arquivos e solicitem serviços dos programas do servidor em uma rede de computadores. O protocolo SMB pode ser usado sobre seu protocolo TCP/IP ou outros protocolos de rede. Utilizando o protocolo SMB, um aplicativo (ou o usuário de um aplicativo) pode acessar arquivos ou outros recursos em um servidor remoto. Isso permite que os aplicativos leiam, criem e atualizem arquivos no servidor remoto. Ele também pode se comunicar com qualquer programa do servidor que esteja configurado para receber uma solicitação de um cliente SMB. O Windows Server 2012 apresenta a nova versão 3,0 do protocolo SMB.

## <a name="practical-applications"></a>Aplicações práticas

Esta seção discute algumas novas maneiras práticas para usar o novo protocolo SMB 3.0.

* **Armazenamento de arquivos para virtualização (Hyper-V™ por SMB)** . O Hyper-V pode armazenar arquivos de máquina virtual, tais como arquivos de configuração, disco rígido Virtual (VHD) e instantâneos, em compartilhamentos de arquivos através do protocolo SMB 3.0. Isso pode ser usado tanto para servidores de arquivos autônomos quanto em servidores de arquivos em cluster que usam o Hyper-V em conjunto com armazenamento compartilhado de arquivos para o cluster.
* **Microsoft SQL Server por SMB**. O SQL Server pode armazenar arquivos de banco de dados de usuários em compartilhamentos de arquivos SMB. Atualmente, esse recurso tem suporte com o SQL Server 2008 R2 para servidores SQL autônomos. Futuras versões do SQL Server adicionarão suporte para servidores SQL em cluster e bancos de dados de sistema.
* **Armazenamento tradicional para dados de usuário final**. O protocolo SMB 3.0 proporciona melhorias às cargas de trabalho do Operador (ou cliente) de Informações. Essas melhorias incluem a redução das latências de aplicativos vivenciadas pelos usuários de escritórios filiais ao acessar dados por redes remotas e a proteção dos dados contra ataques de interceptação.

## <a name="new-and-changed-functionality"></a>Funcionalidade nova e alterada

Para obter informações sobre a funcionalidade nova e alterada no Windows Server 2012 R2, consulte [novidades do SMB no Windows Server](<https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2012-r2-and-2012/hh831474(v%3dws.11)>).

O SMB no Windows Server 2012 e no Windows Server 2016 inclui o novo protocolo SMB 3,0 e muitas novas melhorias que são descritas na tabela a seguir.

<table>
<colgroup>
<col style="width: 33%" />
<col style="width: 33%" />
<col style="width: 33%" />
</colgroup>
<thead>
<tr class="header">
<th><p>Recurso/funcionalidade</p></th>
<th><p>Novo ou atualizado</p></th>
<th><p>Resumo</p></th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td><p>Failover Transparente do SMB</p></td>
<td><p>Novo</p></td>
<td><p>Permite aos administradores realizar a manutenção de hardware ou software dos nós em um servidor de arquivos em cluster, sem interromper os aplicativos do servidor que armazenam dados nesses compartilhamentos de arquivos. Além disso, se uma falha de hardware ou software ocorre em um nó do cluster, os clientes SMB se reconectam a outro nó do cluster de forma transparente, sem interromper os aplicativos do servidor que estão armazenando dados nesses compartilhamentos de arquivos.</p></td>
</tr>
<tr class="even">
<td><p>Expansão do SMB</p></td>
<td><p>Novo</p></td>
<td><p>Utilizando Volumes Compartilhados de Cluster (CSV) versão 2, os administradores podem criar compartilhamentos de arquivos que fornecem acesso simultâneo a arquivos de dados, com E/S direta, através de todos os nós em um cluster de servidor de arquivos. Isso proporciona uma melhor usação da largura de banda da rede e balanceamento de carga dos clientes do servidor de arquivos, além de otimizar o desempenho para os aplicativos de servidor.</p></td>
</tr>
<tr class="odd">
<td><p>SMB Multichannel</p></td>
<td><p>Novo</p></td>
<td><p>Permite agregação de largura de banda da rede e tolerância a falhas da rede se vários caminhos estão disponíveis entre o cliente SMB 3.0 e o servidor SMB 3.0. Isso permite que aplicativos de servidor aproveitem ao máximo toda a largura de banda disponível na rede e sejam resilientes a uma falha da rede.</p></td>
</tr>
<tr class="even">
<td><p>SMB Direct</p></td>
<td><p>Novo</p></td>
<td><p>Dá suporte ao uso de adaptadores de rede com capacidade RDMA e pode funcionar a toda a velocidade com latência muito baixa, usando muito pouco da CPU. Para cargas de trabalho como o Hyper-V ou o Microsoft SQL Server, isso permite que um servidor de arquivos remoto se pareça com um armazenamento local.</p></td>
</tr>
<tr class="odd">
<td><p>Contadores de desempenho para aplicativos de servidor</p></td>
<td><p>Novo</p></td>
<td><p>Os novos contadores de desempenho do SMB fornecem informações detalhadas por compartilhamento sobre taxa de transferência, latência e E/S por segundo (ESPS), permitindo aos administradores analisar o desempenho dos compartilhamentos de arquivos de SMB 3.0 onde seus dados estão armazenados. Esses contadores foram concebidos especificamente para aplicativos de servidor, tais como o Hyper-V e o SQL Server, que armazenam arquivos em compartilhamentos remotos de arquivos.</p></td>
</tr>
<tr class="even">
<td><p>Otimizações de desempenho</p></td>
<td><p>Atualizado</p></td>
<td><p>Tanto o cliente SMB 3.0 quanto o servidor SMB 3.0 foram otimizados para pequenas E/S de leitura/gravação aleatórias, o que é comum em aplicativos de servidor como o SQL Server OLTP. Além disso, a Unidade Máxima de Transmissão (MTU) grande está ativada por padrão, o que melhora significativamente o desempenho em grandes transferências sequenciais, tais como data warehouse do SQL Server, backup ou restauração de banco de dados, implantação ou cópia de discos rígidos virtuais.</p></td>
</tr>
<tr class="odd">
<td><p>Cmdlets do Windows PowerShell específicos do SMB</p></td>
<td><p>Novo</p></td>
<td><p>Com os cmdlets do Windows PowerShell para SMB, um administrador pode gerenciar compartilhamentos de arquivos no servidor de arquivos, de ponta a ponta, a partir da linha de comando.</p></td>
</tr>
<tr class="even">
<td><p>Criptografia SMB</p></td>
<td><p>Novo</p></td>
<td><p>Proporciona criptografia de ponta a ponta dos dados do SMB e protege os dados contra ocorrências de interceptação em redes não confiáveis. Não exige novos custos de implantação e não precisa de protocolo IPsec, hardware especializado ou aceleradores de WAN. Pode ser configurado a cada compartilhamento ou para todo o servidor de arquivos e pode ser habilitado para uma variedade de cenários onde os dados percorrem redes não confiáveis.</p></td>
</tr>
<tr class="odd">
<td><p>Concessão de Diretório do SMB</p></td>
<td><p>Novo</p></td>
<td><p>Melhora o tempo de resposta de aplicativos em escritórios de filial. Com o uso de concessões de diretório, as consultas de via dupla do cliente ao servidor são reduzidas, uma vez que os metadados são recuperados de um cache de diretório mais duradouro. A coerência do cache é mantida porque os clientes são notificados quando há alterações nas informações do diretório no servidor. Funciona em cenários de <em>HomeFolder</em> (leitura/gravação sem compartilhamento) e <em>Publicação</em> (somente leitura com compartilhamento).</p></td>
</tr>
</tbody>
</table>

## <a name="hardware-requirements"></a>Requisitos de hardware

O Failover Transparente do SMB tem os seguintes requisitos:

* Um cluster de failover executando o Windows Server 2012 ou o Windows Server 2016 com pelo menos dois nós configurados. O cluster precisa passar pelos testes de validação de cluster incluídos no assistente de validação.
* Os compartilhamentos de arquivos devem ser criados com a propriedade de Disponibilidade Contínua (CA), que é o padrão.
* Os compartilhamentos de arquivos devem ser criados em caminhos de volume CSV para obtenção da Expansão do SMB.
* Os computadores cliente devem estar executando o Windows® 8 ou o Windows Server 2012, ambos incluindo o cliente SMB atualizado que dá suporte à disponibilidade contínua.

>[!NOTE]
>Os clientes de nível inferior podem se conectar a compartilhamentos de arquivos que têm a propriedade de autoridade de certificação, mas o failover transparente não terá suporte para esses clientes.

O SMB Multichannel tem os seguintes requisitos:

* Pelo menos dois computadores que executam o Windows Server 2012 são necessários. Nenhum recurso extra precisa ser instalado; a tecnologia está ativada por padrão.
* Para obter mais informações sobre configurações de rede recomendadas, veja a seção Consulte também no final deste tópico de visão geral.

O SMB Direct tem os seguintes requisitos:

* Pelo menos dois computadores que executam o Windows Server 2012 são necessários. Nenhum recurso extra precisa ser instalado; a tecnologia está ativada por padrão.
* São necessários adaptadores de rede com capacidade RDMA. Atualmente, estes adaptadores estão disponíveis em três diferentes tipos: iWARP, Infiniband ou RoCE (RDMA over Converged Ethernet).

## <a name="more-information"></a>Mais informações

A lista a seguir fornece recursos adicionais na Web sobre SMB e tecnologias relacionadas no Windows Server 2012 R2, no Windows Server 2012 e no Windows Server 2016.

* [Armazenamento no Windows Server](../storage.md)
* [Servidor de Arquivos de Escalabilidade Horizontal para dados de aplicativo](../../failover-clustering/sofs-overview.md)
* [Melhorar o desempenho de um servidor de arquivos com SMB Direct](smb-direct.md)
* [Implantar o Hyper-V em SMB](<https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2012-r2-and-2012/jj134187(v%3dws.11)>)
* [Implantar o SMB multicanal](<https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2012-r2-and-2012/dn610980(v%3dws.11)>)
* [Implantando servidores de arquivos rápidos e eficientes para aplicativos de servidor](<https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2012-r2-and-2012/hh831723(v%3dws.11)>)
* [SMB: Guia de solução de problemas @ no__t-0