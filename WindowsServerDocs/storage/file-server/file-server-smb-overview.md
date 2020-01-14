---
title: Visão geral do compartilhamento de arquivos usando o protocolo SMB 3 no Windows Server
description: Uma visão geral do uso do protocolo SMB 3 para compartilhamentos de arquivos e serviços de arquivos com o Windows Server.
ms.prod: windows-server
ms.topic: article
author: JasonGerend
ms.author: jgerend
ms.technology: storage
ms.date: 01/10/2020
ms.localizationpriority: medium
ms.openlocfilehash: 416145a8c4ec20eaf46cf4b5ac88a0cdf38bdf33
ms.sourcegitcommit: 76469d1b7465800315eaca3e0c7f0438fc3939ed
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 01/13/2020
ms.locfileid: "75919881"
---
# <a name="overview-of-file-sharing-using-the-smb-3-protocol-in-windows-server"></a>Visão geral do compartilhamento de arquivos usando o protocolo SMB 3 no Windows Server

>Aplica-se a: Windows Server 2019, Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Este tópico descreve o recurso SMB 3 no Windows Server 2019, Windows Server 2016, Windows Server 2012 R2 e Windows Server 2012 – usos práticos para o recurso, a funcionalidade nova ou atualizada mais significativa nesta versão em comparação com as versões anteriores e os requisitos de hardware. O SMB também é um protocolo de malha usado pelas soluções [SDDC (Data Center definidas pelo software)](../../sddc.md) , como espaços de armazenamento diretos, réplica de armazenamento e outros. A versão 3,0 do SMB foi introduzida com o Windows Server 2012 e foi aprimorada incrementalmente nas versões subsequentes.

## <a name="feature-description"></a>Descrição do recurso

Server Message Block (SMB) é um protocolo de compartilhamento de arquivos em rede que permite que os aplicativos de um computador leiam e gravem em arquivos e solicitem serviços dos programas do servidor em uma rede de computadores. O protocolo SMB pode ser usado sobre seu protocolo TCP/IP ou outros protocolos de rede. Utilizando o protocolo SMB, um aplicativo (ou o usuário de um aplicativo) pode acessar arquivos ou outros recursos em um servidor remoto. Isso permite que os aplicativos leiam, criem e atualizem arquivos no servidor remoto. O SMB também pode se comunicar com qualquer programa de servidor configurado para receber uma solicitação de cliente SMB. O SMB é um protocolo de malha usado pelas tecnologias de computação SDDC (Data Center) definidas pelo software, como Espaços de Armazenamento Diretos, a réplica de armazenamento. Para obter mais informações, consulte [datacenter definido pelo software do Windows Server](../../sddc.md).

## <a name="practical-applications"></a>Aplicações práticas

Esta seção discute algumas novas maneiras práticas para usar o novo protocolo SMB 3.0.

* **Armazenamento de arquivos para virtualização (Hyper-V™ por SMB)** . O Hyper-V pode armazenar arquivos de máquina virtual, tais como arquivos de configuração, disco rígido Virtual (VHD) e instantâneos, em compartilhamentos de arquivos através do protocolo SMB 3.0. Isso pode ser usado tanto para servidores de arquivos autônomos quanto em servidores de arquivos em cluster que usam o Hyper-V em conjunto com armazenamento compartilhado de arquivos para o cluster.
* **Microsoft SQL Server por SMB**. O SQL Server pode armazenar arquivos de banco de dados de usuários em compartilhamentos de arquivos SMB. Atualmente, esse recurso tem suporte com o SQL Server 2008 R2 para servidores SQL autônomos. Futuras versões do SQL Server adicionarão suporte para servidores SQL em cluster e bancos de dados de sistema.
* **Armazenamento tradicional para dados de usuário final**. O protocolo SMB 3.0 proporciona melhorias às cargas de trabalho do Operador (ou cliente) de Informações. Essas melhorias incluem a redução das latências de aplicativos vivenciadas pelos usuários de escritórios filiais ao acessar dados por redes remotas e a proteção dos dados contra ataques de interceptação.

## <a name="new-and-changed-functionality"></a>Funcionalidade nova e alterada

As seções a seguir descrevem a funcionalidade que foi adicionada no SMB 3 e atualizações subsequentes.

## <a name="features-added-in-windows-server-2019-and-windows-10-version-1809"></a>Recursos adicionados no Windows Server 2019 e Windows 10, versão 1809

| Recurso/funcionalidade  | Novo ou atualizado  | Resumo  |
| --------- | --------- | --------- |
| Capacidade de exigir Write-Through em disco em compartilhamentos de arquivos que não estão continuamente disponíveis | Novo | Para fornecer uma garantia adicional que grava em um compartilhamento de arquivos, faça todo o caminho por meio da pilha de software e hardware para o disco físico antes da operação de gravação retornar como concluída, você pode habilitar o write-through no compartilhamento de arquivos usando o comando `NET USE /WRITETHROUGH`, ou seja, o cmdlet `New-SMBMapping -UseWriteThrough` PowerShell. Há alguma quantidade de impacto de desempenho no uso de write-through; consulte a postagem [no blog controlando os comportamentos de write-through no SMB](https://techcommunity.microsoft.com/t5/storage-at-microsoft/controlling-write-through-behaviors-in-smb/bc-p/1083417#M677) para uma discussão adicional. |

## <a name="features-added-in-windows-server-version-1709-and-windows-10-version-1709"></a>Recursos adicionados no Windows Server, versão 1709 e Windows 10, versão 1709

| Recurso/funcionalidade  | Novo ou atualizado  | Resumo  |
| --------- | --------- | --------- |
| O acesso de convidado aos compartilhamentos de arquivos está desabilitado | Novo | O cliente SMB não permite mais as seguintes ações: acesso à conta de convidado a um servidor remoto; Fallback para a conta de convidado depois que credenciais inválidas forem fornecidas. Para obter detalhes, consulte [acesso de convidado em SMB2 desabilitado por padrão no Windows](https://support.microsoft.com/help/4046019/guest-access-in-smb2-disabled-by-default-in-windows-10-and-windows-ser). | 
| Mapeamento global SMB | Novo | Mapeia um compartilhamento SMB remoto para uma letra de unidade que é acessível a todos os usuários no host local, incluindo contêineres. Isso é necessário para habilitar a e/s do contêiner no volume de dados para atravessar o ponto de montagem remoto. Lembre-se de que, ao usar o mapeamento global SMB para contêineres, todos os usuários no host do contêiner poderão acessar o compartilhamento remoto. Qualquer aplicativo em execução no host do contêiner também tem acesso ao compartilhamento remoto mapeado. Para obter detalhes, consulte [suporte ao armazenamento de contêineres com CSV (volumes compartilhados do cluster), espaços de armazenamento diretos, mapeamento global SMB](https://techcommunity.microsoft.com/t5/failover-clustering/container-storage-support-with-cluster-shared-volumes-csv/ba-p/372140). |
| Controle de dialeto SMB | Novo | Agora você pode definir valores de registro para controlar a versão mínima do SMB (dialeto) e a versão SMB máxima usada. Para obter detalhes, consulte [controlando dialetos SMB](https://techcommunity.microsoft.com/t5/storage-at-microsoft/controlling-smb-dialects/ba-p/860024). |

## <a name="features-added-in-smb-311-with-windows-server-2016-and-windows-10-version-1607"></a>Recursos adicionados no SMB 3,11 com o Windows Server 2016 e Windows 10, versão 1607

| Recurso/funcionalidade  | Novo ou atualizado  | Resumo  |
| --------- | --------- | --------- |
| Criptografia do SMB     |   Atualizado      | A criptografia SMB 3.1.1 com criptografia AES-Galois/modo de contador (AES-GCM) é mais rápida que a assinatura SMB ou criptografia SMB anterior usando AES-CCM.   |
| Cache de diretório | Novo | O SMB 3.1.1 inclui aprimoramentos no cache de diretório. Os clientes do Windows agora podem armazenar diretórios muito maiores, aproximadamente 500 mil entradas. Os clientes Windows tentarão realizar consultas de diretório com buffers de 1 MB para reduzir viagens de ida e volta e melhorar o desempenho. |
| Integridade da pré-autenticação | Novo |  No SMB 3.1.1, a integridade de pré-autenticação fornece proteção aprimorada de um invasor Man-in-the-Middle com o estabelecimento de conexão do SMB e as mensagens de autenticação. Para obter detalhes, consulte [integridade de pré-autenticação do SMB 3.1.1 no Windows 10](https://docs.microsoft.com/archive/blogs/openspecification/smb-3-1-1-pre-authentication-integrity-in-windows-10). |
| Aprimoramentos de criptografia SMB | Novo | O SMB 3.1.1 oferece um mecanismo para negociar o algoritmo de criptografia por conexão, com opções para AES-128-CCM e AES-128-GCM. O AES-128-GCM é o padrão para novas versões do Windows, enquanto as versões mais antigas continuarão a usar AES-128-CCM. |
| Suporte à atualização de cluster sem interrupção | Novo | Permite [atualizações de cluster sem interrupção](../../failover-clustering/cluster-operating-system-rolling-upgrade.md) , permitindo que o SMB pareça oferecer suporte a versões máximas do SMB para clusters no processo de atualização. Para obter mais detalhes sobre como permitir que o SMB se comunique usando diferentes versões (dialetos) do protocolo, consulte a postagem no blog [controlando dialetos SMB](https://techcommunity.microsoft.com/t5/storage-at-microsoft/controlling-smb-dialects/ba-p/860024). |
| Suporte ao cliente SMB Direct no Windows 10 | Novo | O Windows 10 Enterprise, o Windows 10 Education e o Windows 10 pro para estações de trabalho agora incluem suporte ao cliente SMB Direct. |
| Suporte nativo para chamadas à API FileNormalizedNameInformation | Novo | Adiciona suporte nativo para consultar o nome normalizado de um arquivo. Para obter detalhes, consulte [FileNormalizedNameInformation](https://docs.microsoft.com/openspecs/windows_protocols/ms-fscc/20bcadba-808c-4880-b757-4af93e41edf6). |

Para obter detalhes adicionais, consulte a postagem de blog [novidades em SMB 3.1.1 no Windows Server 2016 Technical Preview 2](https://docs.microsoft.com/archive/blogs/josebda/whats-new-in-smb-3-1-1-in-the-windows-server-2016-technical-preview-2).

## <a name="features-added-in-smb-302-with-windows-server-2012-r2-and-windows-81"></a>Recursos adicionados no SMB 3, 2 com o Windows Server 2012 R2 e Windows 8.1

| Recurso/funcionalidade  | Novo ou atualizado  | Resumo  |
| --------- | --------- | --------- |
| Rebalanceamento automático de clientes do servidor de arquivos expandidos     |   Novo      | Melhora a escalabilidade e a capacidade de gerenciamento para servidores de arquivos de escalabilidade horizontal. As conexões de clientes SMB são controladas por compartilhamento de arquivos (em vez de por servidor), e os clientes são então redirecionados para o nó de cluster com o melhor acesso ao volume usado pelo compartilhamento de arquivos. Isso melhora a eficiência, reduzindo o tráfego de redirecionamento entre nós de servidor de arquivos. Os clientes são redirecionados após uma conexão inicial e quando o armazenamento de cluster é reconfigurado.    |
| Desempenho pela WAN   | Atualizado  | O Windows 8.1 e o Windows 10 fornecem arquivos CopyFile aprimorados SRV_COPYCHUNK sobre o suporte a SMB quando você usa o explorador de arquivo para cópias remotas de um local em um computador remoto para outra cópia no mesmo servidor. Você copiará apenas uma pequena quantidade de metadados pela rede (1/2KiB por 16MiB de dados de arquivo é transmitido). Isso resulta em uma melhoria significativa no desempenho. Essa é uma distinção no nível do sistema operacional e do explorador de arquivos para SMB. |
| SMB Direct     |   Atualizado      | Melhora o desempenho para pequenas cargas de trabalho de E/S, aumentando a eficiência durante a hospedagem de cargas de trabalho com E/S pequenas, como um banco de dados OLTP (transação online) em uma máquina virtual. Esses aprimoramentos são evidentes ao usar interfaces de rede de velocidade mais alta, como Ethernet de 40 Gbps e 56 Gbps InfiniBand.  |
| Limites de largura de banda SMB | Novo | Agora você pode usar [set-SmbBandwidthLimit](https://docs.microsoft.com/powershell/module/smbshare/set-smbbandwidthlimit) para definir limites de largura de banda em três categorias: VirtualMachine (Hyper-v sobre tráfego SMB), LiveMigration (Hyper-v migração ao vivo tráfego sobre SMB) ou padrão (todos os outros tipos de tráfego SMB).

Para obter mais informações sobre a funcionalidade SMB nova e alterada no Windows Server 2012 R2, consulte [novidades do SMB no Windows Server](<https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2012-r2-and-2012/hh831474(v%3dws.11)>).

## <a name="features-added-in-smb-30-with-windows-server-2012-and-windows-8"></a>Recursos adicionados no SMB 3,0 com o Windows Server 2012 e o Windows 8

| Recurso/funcionalidade  | Novo ou atualizado  | Resumo  |
| --------- | --------- | --------- |
| Failover Transparente do SMB     |   Novo    | Permite aos administradores realizar a manutenção de hardware ou software dos nós em um servidor de arquivos em cluster, sem interromper os aplicativos do servidor que armazenam dados nesses compartilhamentos de arquivos. Além disso, se uma falha de hardware ou software ocorre em um nó do cluster, os clientes SMB se reconectam a outro nó do cluster de forma transparente, sem interromper os aplicativos do servidor que estão armazenando dados nesses compartilhamentos de arquivos.        |
| Expansão do SMB     |   Novo      | Suporte para várias instâncias SMB em um Servidor de Arquivos de Escalabilidade Horizontal. Utilizando Volumes Compartilhados de Cluster (CSV) versão 2, os administradores podem criar compartilhamentos de arquivos que fornecem acesso simultâneo a arquivos de dados, com E/S direta, através de todos os nós em um cluster de servidor de arquivos. Isso proporciona uma melhor usação da largura de banda da rede e balanceamento de carga dos clientes do servidor de arquivos, além de otimizar o desempenho para os aplicativos de servidor.  |
| SMB Multichannel     |   Novo      |  Habilita a agregação de largura de banda de rede e tolerância a falhas de rede se vários caminhos estiverem disponíveis entre o cliente e o servidor SMB. Isso permite que aplicativos de servidor aproveitem ao máximo toda a largura de banda disponível na rede e sejam resilientes a uma falha da rede.<br><br>O SMB multicanal no SMB 3 contribui para um aumento significativo no desempenho em comparação com as versões anteriores do SMB. |
| SMB Direct     |   Novo      | Dá suporte ao uso de adaptadores de rede com capacidade RDMA e pode funcionar a toda a velocidade com latência muito baixa, usando muito pouco da CPU. Para cargas de trabalho como o Hyper-V ou o Microsoft SQL Server, isso permite que um servidor de arquivos remoto se pareça com um armazenamento local.<br><br>O SMB Direct no SMB 3 contribui para um aumento significativo no desempenho em comparação com as versões anteriores do SMB.  |
| Contadores de desempenho para aplicativos de servidor     |   Novo      |  Os novos contadores de desempenho SMB fornecem informações detalhadas e por compartilhamento sobre taxa de transferência, latência e e/s por segundo (IOPS), permitindo que os administradores analisem o desempenho de compartilhamentos de arquivos SMB onde seus dados são armazenados. Esses contadores foram concebidos especificamente para aplicativos de servidor, tais como o Hyper-V e o SQL Server, que armazenam arquivos em compartilhamentos remotos de arquivos.      |
| Otimizações de desempenho    |  Atualizado   | O cliente e o servidor SMB foram otimizados para e/s de leitura/gravação aleatória pequena, o que é comum em aplicativos de servidor, como SQL Server OLTP. Além disso, a Unidade Máxima de Transmissão (MTU) grande está ativada por padrão, o que melhora significativamente o desempenho em grandes transferências sequenciais, tais como data warehouse do SQL Server, backup ou restauração de banco de dados, implantação ou cópia de discos rígidos virtuais. |
| Cmdlets do Windows PowerShell específicos do SMB     |   Novo      |  Com os cmdlets do Windows PowerShell para SMB, um administrador pode gerenciar compartilhamentos de arquivos no servidor de arquivos, de ponta a ponta, a partir da linha de comando.   |
| Criptografia do SMB     |   Novo      | Proporciona criptografia de ponta a ponta dos dados do SMB e protege os dados contra ocorrências de interceptação em redes não confiáveis. Não exige novos custos de implantação e não precisa de protocolo IPsec, hardware especializado ou aceleradores de WAN. Pode ser configurado a cada compartilhamento ou para todo o servidor de arquivos e pode ser habilitado para uma variedade de cenários onde os dados percorrem redes não confiáveis. |
| Concessão de Diretório do SMB     |  Novo | Melhora o tempo de resposta de aplicativos em escritórios de filial. Com o uso de concessões de diretório, as consultas de via dupla do cliente ao servidor são reduzidas, uma vez que os metadados são recuperados de um cache de diretório mais duradouro. A coerência do cache é mantida porque os clientes são notificados quando há alterações nas informações do diretório no servidor. As concessões de diretório funcionam com cenários para HomeFolder (leitura/gravação sem compartilhamento) e publicação (somente leitura com compartilhamento).    |
| Desempenho pela WAN   | Novo   | Os bloqueios oportunistas de diretório (oplocks) e as concessões de oplock foram introduzidos no SMB 3,0. Para cargas de trabalho típicas do Office/cliente, as oplocks/concessões são mostradas para reduzir as viagens de ida e volta da rede em aproximadamente 15%.<br><br>No SMB 3, a implementação do SMB do Windows foi refinada para melhorar o comportamento de cache no cliente, bem como a capacidade de enviar mais taxas de transferência.<br><br>O SMB 3 oferece melhorias à API CopyFile (), bem como a ferramentas associadas, como o Robocopy, para enviar significativamente mais dados pela rede. |
| Negociação de dialeto seguro | Novo | Ajuda a proteger contra uma tentativa no meio do Man-in-the-Middle para fazer downgrade da negociação de dialeto. A ideia é impedir que um bisbilhoteiro rebaixar o dialeto negociado inicialmente e os recursos entre o cliente e o servidor. Para obter detalhes, consulte [SMB3 Secure dialeto Negotiation](https://docs.microsoft.com/archive/blogs/openspecification/smb3-secure-dialect-negotiation). Observe que isso foi substituído pela integridade de pré-autenticação do [SMB 3.1.1 no recurso do Windows 10](https://docs.microsoft.com/archive/blogs/openspecification/smb-3-1-1-pre-authentication-integrity-in-windows-10) no SMB 3.1.1. |


## <a name="hardware-requirements"></a>Requisitos de hardware

O Failover Transparente do SMB tem os seguintes requisitos:

* Um cluster de failover executando o Windows Server 2012 ou o Windows Server 2016 com pelo menos dois nós configurados. O cluster precisa passar pelos testes de validação de cluster incluídos no assistente de validação.
* Os compartilhamentos de arquivos devem ser criados com a propriedade de Disponibilidade Contínua (CA), que é o padrão.
* Os compartilhamentos de arquivos devem ser criados em caminhos de volume CSV para obtenção da Expansão do SMB.
* Os computadores cliente devem estar executando o Windows® 8 ou o Windows Server 2012, ambos incluindo o cliente SMB atualizado que dá suporte à disponibilidade contínua.

> [!NOTE]
> Os clientes de nível inferior podem se conectar a compartilhamentos de arquivos que têm a propriedade de autoridade de certificação, mas o failover transparente não terá suporte para esses clientes.

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
* [SMB: guia de solução de problemas](<https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2012-r2-and-2012/dn659439(v%3dws.11)>)
