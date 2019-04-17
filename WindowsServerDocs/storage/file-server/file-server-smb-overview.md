---
title: Visão geral do compartilhamento de arquivos usando o protocolo SMB 3 no Windows Server
description: Visão geral do uso do protocolo SMB 3 para compartilhamentos de arquivos e operando de arquivo com o Windows Server.
ms.prod: windows-server-threshold
ms.topic: article
author: JasonGerend
ms.author: jgerend
ms.technology: storage
ms.date: 07/09/2018
ms.localizationpriority: medium
ms.openlocfilehash: fc4c8b341ee78db80f862ee412400f0a930fe810
ms.sourcegitcommit: 375e94dc70c76e7aa5679c32f0f4dbc26cf7dd83
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/10/2018
ms.locfileid: "2233482"
---
# <a name="overview-of-file-sharing-using-the-smb-3-protocol-in-windows-server"></a>Visão geral do compartilhamento de arquivos usando o protocolo SMB 3 no Windows Server

>Aplica-se a: Windows Server 2012 R2, o Windows Server 2012, o Windows Server 2016

Este tópico descreve o recurso de SMB 3.0 no Windows Server® 2012, Windows Server 2012 R2 e Windows Server 2016 — práticos usa para o recurso, o mais significativo novos ou atualizados funcionalidade nesta versão comparada a versões anteriores e o hardware requisitos.

## <a name="feature-description"></a>Descrição do recurso

Server Message Block (SMB) é um protocolo de compartilhamento de arquivos em rede que permite que os aplicativos de um computador leiam e gravem em arquivos e solicitem serviços dos programas do servidor em uma rede de computadores. O protocolo SMB pode ser usado sobre seu protocolo TCP/IP ou outros protocolos de rede. Utilizando o protocolo SMB, um aplicativo (ou o usuário de um aplicativo) pode acessar arquivos ou outros recursos em um servidor remoto. Isso permite que os aplicativos leiam, criem e atualizem arquivos no servidor remoto. Ele também pode se comunicar com qualquer programa do servidor que esteja configurado para receber uma solicitação de um cliente SMB. Windows Server 2012 introduz a nova versão 3.0 do protocolo SMB.

## <a name="practical-applications"></a>Aplicações práticas

Esta seção discute algumas novas maneiras de práticas para usar o novo protocolo SMB 3.0.

* **Armazenamento de arquivo para virtualização (Hyper-V™ em SMB)**. Hyper-V pode armazenar arquivos de máquina virtual, configuração, Virtual (VHD) no disco rígido arquivos e instantâneos, em compartilhamentos de arquivos através do protocolo SMB 3.0. Isso pode ser usado para servidores de arquivo autônomo e servidores de arquivos em cluster que usam o Hyper-V em conjunto com o armazenamento de arquivos compartilhados para o cluster.
* **Microsoft SQL Server em SMB**. SQL Server pode armazenar arquivos de banco de dados do usuário em compartilhamentos de arquivos SMB. Atualmente, isso é suportado com o SQL Server 2008 R2 para servidores autônomos do SQL. Versões futuras do SQL Server adicionará suporte para servidores em cluster do SQL e bancos de dados do sistema.
* **Armazenamento tradicional para dados de usuário final**. O protocolo SMB 3.0 fornece aprimoramentos para o profissional de informação (ou cliente) cargas de trabalho. Essas melhorias incluem reduzindo as latências de aplicativo experimentadas por usuários da filial quando acessando dados em redes de longa distância (WAN) e proteção de dados contra a interceptação attacks.

## <a name="new-and-changed-functionality"></a>Funcionalidade nova e alterada

Para obter informações sobre a funcionalidade de nova e alterada no Windows Server 2012 R2, consulte [What's New in SMB no Windows Server](<https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2012-r2-and-2012/hh831474(v%3dws.11)>).

SMB no Windows Server 2012 e Windows Server 2016 inclui o novo protocolo SMB 3.0 e novas melhorias que estão descritas na tabela a seguir.

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
<td><p>Failover transparente de SMB</p></td>
<td><p>Novo</p></td>
<td><p>Permite que os administradores executar a manutenção do hardware ou software de nós em um servidor de arquivos em cluster sem interromper os aplicativos de servidor armazenando dados nesses compartilhamentos de arquivo. Além disso, se ocorrer uma falha de hardware ou software em um nó de cluster, clientes SMB transparente se reconectar ao outro nó de cluster sem interromper aplicativos para servidores que estão armazenando dados nesses compartilhamentos de arquivo.</p></td>
</tr>
<tr class="even">
<td><p>Dimensionamento SMB</p></td>
<td><p>Novo</p></td>
<td><p>Usando Cluster compartilhados Volumes (CSV) versão 2, os administradores podem criar compartilhamentos de arquivos que fornecem acesso simultâneo aos arquivos de dados, com direto e/s, por meio de todos os nós em um cluster de servidor de arquivo. Isso oferece a melhor utilização de largura de banda de rede e balanceamento de carga dos clientes do servidor de arquivo e otimiza o desempenho para aplicativos de servidor.</p></td>
</tr>
<tr class="odd">
<td><p>SMB multicanais</p></td>
<td><p>Novo</p></td>
<td><p>Habilita a agregação de largura de banda de rede e de tolerância a falhas de rede se houver vários caminhos disponíveis entre o cliente SMB 3.0 e o servidor de SMB 3.0. Isso permite que aplicativos de servidor para aproveitar totalmente toda largura de banda de rede disponível e ser resiliente a uma falha de rede.</p></td>
</tr>
<tr class="even">
<td><p>SMB direto</p></td>
<td><p>Novo</p></td>
<td><p>Suporta o uso de adaptadores de rede que têm a capacidade RDMA e pode funcionar em velocidade máxima com muito baixa latência, durante a utilização de CPU muito pouco. Para cargas de trabalho como Hyper-V ou Microsoft SQL Server, isso permite que um servidor de arquivo remoto para se parecer com o armazenamento local.</p></td>
</tr>
<tr class="odd">
<td><p>Contadores de desempenho para aplicativos de servidor</p></td>
<td><p>Novo</p></td>
<td><p>O desempenho de SMB novos contadores fornecem detalhadas, por-compartilhamento de informações sobre a taxa de transferência, latência e e/s por segundo (IOPS), permitindo que os administradores a analisar o desempenho de compartilhamentos de arquivos SMB 3.0 onde seus dados estão armazenados. Esses contadores especificamente projetados para aplicativos de servidor, Hyper-V e o SQL Server, qual armazenar arquivos em compartilhamentos de arquivos remotos.</p></td>
</tr>
<tr class="even">
<td><p>Otimizações de desempenho</p></td>
<td><p>Atualizado</p></td>
<td><p>O cliente SMB 3.0 e o servidor SMB 3.0 foram otimizadas para e/s de leitura/gravação aleatória pequeno, que é comum em aplicativos de servidor como OLTP do SQL Server. Além disso, grande Maximum Transmission Unit (MTU) estão ativados por padrão, o que aumenta significativamente o desempenho em sequenciais transferências grandes, como armazém de dados do SQL Server, o backup do banco de dados ou a restauração, implantação ou copiando discos rígidos virtuais.</p></td>
</tr>
<tr class="odd">
<td><p>Cmdlets do Windows PowerShell SMB específicas</p></td>
<td><p>Novo</p></td>
<td><p>Com os cmdlets do Windows PowerShell para SMB, um administrador pode gerenciar compartilhamentos de arquivos no servidor de arquivos, ponta a ponta da linha de comando.</p></td>
</tr>
<tr class="even">
<td><p>Criptografia SMB</p></td>
<td><p>Novo</p></td>
<td><p>Fornece a criptografia de ponta a ponta de dados SMB e protege os dados contra interceptação ocorrências em redes não confiáveis. Exige nenhum novos custos de implantação e sem a necessidade de Internet Protocol security (IPsec), hardware especializada ou aceleradores de WAN. Ele pode ser configurado em uma base por compartilhar ou para o servidor de arquivo inteiro e pode ser habilitado para uma variedade de cenários onde dados percorrem redes não confiáveis.</p></td>
</tr>
<tr class="odd">
<td><p>Leasing de diretório SMB</p></td>
<td><p>Novo</p></td>
<td><p>Melhora os tempos de resposta do aplicativo nas filiais. Com o uso de leasing de diretório, percursos circulares de cliente para servidor são reduzidos desde que metadados é recuperado de um estilo de vida mais cache de diretório. Coerência de cache é mantida porque os clientes são notificados quando as informações de diretório no servidor é alterada. Funciona com cenários para <em>HomeFolder</em> (leitura/gravação sem compartilhamento) e a <em>publicação</em> (somente leitura no compartilhamento).</p></td>
</tr>
</tbody>
</table>

## <a name="hardware-requirements"></a>Requisitos de hardware

Failover transparente de SMB tem os seguintes requisitos:

* Um cluster de failover executando o Windows Server 2012 ou Windows Server 2016 com pelo menos dois nós configurados. O cluster deve passar os testes de validação de cluster incluídos no Assistente de validação.
* Compartilhamentos de arquivos devem ser criados com a propriedade disponibilidade contínua (CA), que é o padrão.
* Compartilhamentos de arquivos devem ser criados em caminhos de volume CSV atinja o dimensionamento de SMB.
* Computadores cliente devem estar executando Windows® 8 ou Windows Server 2012, dois dos quais incluem o cliente SMB atualizado que oferece suporte à disponibilidade contínua.

>[!NOTE]
>Clientes de nível inferior podem se conectar a compartilhamentos de arquivos que têm a propriedade da autoridade de certificação, mas não será suportado failover transparente para estes clientes.

SMB multicanais tem os seguintes requisitos:

* Pelo menos dois computadores que executam o Windows Server 2012 são necessário. Sem recursos extras precisam ser instalados — a tecnologia está habilitado por padrão.
* Para obter informações sobre as configurações de rede recomendados, consulte a seção Consulte também no final deste tópico de visão geral.

SMB direto tem os seguintes requisitos:

* Pelo menos dois computadores que executam o Windows Server 2012 são necessário. Sem recursos extras precisam ser instalados — a tecnologia está habilitado por padrão.
* Adaptadores de rede com a capacidade RDMA são necessários. Atualmente, esses adaptadores estão disponíveis em três tipos diferentes: iWARP, Infiniband ou RoCE (RDMA over Ethernet convergiu).

## <a name="more-information"></a>Mais informações

A lista a seguir fornece recursos adicionais na web sobre SMB e as tecnologias relacionadas no Windows Server 2012 R2, Windows Server 2012 e Windows Server 2016.

* [Armazenamento no Windows Server](../storage.md)
* [Servidor de arquivos de dimensionamento para dados de aplicativo](../../failover-clustering/sofs-overview.md)
* [Melhorar o desempenho de um servidor de arquivos com SMB direto](smb-direct.md)
* [Implantar o Hyper-V no SMB](<https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2012-r2-and-2012/jj134187(v%3dws.11)>)
* [Implantar SMB multicanais](<https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2012-r2-and-2012/dn610980(v%3dws.11)>)
* [Implantando servidores de arquivo rápida e eficiente para aplicativos de servidor](<https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2012-r2-and-2012/hh831723(v%3dws.11)>)
* [SMB: Guia de resolução de problemas](<https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2012-r2-and-2012/dn659439(v%3dws.11)>)