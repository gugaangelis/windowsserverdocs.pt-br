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
ms.openlocfilehash: 2c64914e840c4e8a84603144fd499e091f0cb46c
ms.sourcegitcommit: 568b924d32421256f64abfee171304f1daf320d2
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/18/2020
ms.locfileid: "85070537"
---
# <a name="overview-of-file-sharing-using-the-smb-3-protocol-in-windows-server"></a>Visão geral do compartilhamento de arquivos usando o protocolo SMB 3 no Windows Server

>Aplica-se a: Windows Server 2019, Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Este tópico descreve o recurso SMB 3 no Windows Server 2019, no Windows Server 2016, no Windows Server 2012 R2 e no Windows Server 2012 – os usos práticos do recurso, a funcionalidade nova ou atualizada mais significativa dessa versão em comparação com as versões anteriores e os requisitos de hardware. O SMB também é um protocolo de malha usado pelas soluções [SDDC (Data Center Definido por Software)](../../sddc.md), como Espaços de Armazenamento Diretos, Réplica de Armazenamento e outros. O SMB versão 3.0 foi introduzido com o Windows Server 2012 e aprimorado incrementalmente nas versões subsequentes.

## <a name="feature-description"></a>Descrição do recurso

Server Message Block (SMB) é um protocolo de compartilhamento de arquivos em rede que permite que os aplicativos de um computador leiam e gravem em arquivos e solicitem serviços dos programas do servidor em uma rede de computadores. O protocolo SMB pode ser usado sobre seu protocolo TCP/IP ou outros protocolos de rede. Utilizando o protocolo SMB, um aplicativo (ou o usuário de um aplicativo) pode acessar arquivos ou outros recursos em um servidor remoto. Isso permite que os aplicativos leiam, criem e atualizem arquivos no servidor remoto. O SMB também pode se comunicar com qualquer programa do servidor que esteja configurado para receber uma solicitação de um cliente SMB. O SMB é um protocolo de malha usado pelas tecnologias de computação SDDC (Data Center Definido por software), como Espaços de Armazenamento Diretos e Réplica de Armazenamento. Para obter mais informações, confira [Datacenter Definido por Software do Windows Server](../../sddc.md).

## <a name="practical-applications"></a>Aplicações práticas

Esta seção discute algumas novas maneiras práticas para usar o novo protocolo SMB 3.0.

* **Armazenamento de arquivos para virtualização (Hyper-V™ por SMB)** . O Hyper-V pode armazenar arquivos de máquina virtual, tais como arquivos de configuração, disco rígido Virtual (VHD) e instantâneos, em compartilhamentos de arquivos através do protocolo SMB 3.0. Isso pode ser usado tanto para servidores de arquivos autônomos quanto em servidores de arquivos em cluster que usam o Hyper-V em conjunto com armazenamento compartilhado de arquivos para o cluster.
* **Microsoft SQL Server por SMB**. O SQL Server pode armazenar arquivos de banco de dados de usuários em compartilhamentos de arquivos SMB. Atualmente, esse recurso tem suporte com o SQL Server 2008 R2 para servidores SQL autônomos. Futuras versões do SQL Server adicionarão suporte para servidores SQL em cluster e bancos de dados de sistema.
* **Armazenamento tradicional para dados de usuário final**. O protocolo SMB 3.0 proporciona melhorias às cargas de trabalho do Operador (ou cliente) de Informações. Essas melhorias incluem a redução das latências de aplicativos vivenciadas pelos usuários de escritórios filiais ao acessar dados por redes remotas e a proteção dos dados contra ataques de interceptação.

## <a name="new-and-changed-functionality"></a>Funcionalidade nova e alterada

As seções a seguir descrevem a funcionalidade que foi adicionada no SMB 3 e atualizações subsequentes.

## <a name="features-added-in-windows-server-2019-and-windows-10-version-1809"></a>Recursos adicionados no Windows Server 2019 e no Windows 10, versão 1809

| Recurso/funcionalidade  | Novo ou atualizado  | Resumo  |
| --------- | --------- | --------- |
| Capacidade de exigir write-through em disco em compartilhamentos de arquivos que não estão continuamente disponíveis | Novo | Para fornecer uma garantia adicional de que as gravações em um compartilhamento de arquivo percorram toda a pilha de software e hardware até o disco físico antes da operação de gravação retornar como concluída, habilite o write-through no compartilhamento de arquivo usando o comando `NET USE /WRITETHROUGH` ou o cmdlet `New-SMBMapping -UseWriteThrough` do PowerShell. Há algum impacto no desempenho em usar write-through. Confira a postagem no blog [Como controlar os comportamentos de write-through no SMB](https://techcommunity.microsoft.com/t5/storage-at-microsoft/controlling-write-through-behaviors-in-smb/bc-p/1083417#M677) para uma discussão adicional. |

## <a name="features-added-in-windows-server-version-1709-and-windows-10-version-1709"></a>Recursos adicionados no Windows Server, versão 1709 e Windows 10, versão 1709

| Recurso/funcionalidade  | Novo ou atualizado  | Resumo  |
| --------- | --------- | --------- |
| O acesso para convidado aos compartilhamentos de arquivos está desabilitado | Novo | O cliente SMB não permite mais as seguintes ações: Acesso de conta convidado a um servidor remoto; fallback para a conta convidado depois que credenciais inválidas forem fornecidas. Para obter detalhes, confira [Acesso para convidado no SMB2 desabilitado por padrão no Windows](https://support.microsoft.com/help/4046019/guest-access-in-smb2-disabled-by-default-in-windows-10-and-windows-ser). | 
| Mapeamento global de SMB | Novo | Mapeia um compartilhamento SMB remoto para uma letra da unidade acessível a todos os usuários no host local, incluindo contêineres. Isso é necessário para habilitar a E/S do contêiner no volume de dados para cruzar o ponto de montagem remoto. Saiba que, ao usar o mapeamento global SMB para contêineres, todos os usuários no host do contêiner poderão acessar o compartilhamento remoto. Qualquer aplicativo em execução no host do contêiner também tem acesso ao compartilhamento remoto mapeado. Para detalhes, confira [Suporte ao armazenamento de contêiner com CSV (Volumes Compartilhados do Cluster), Espaços de Armazenamento Diretos, Mapeamento Global de SMB](https://techcommunity.microsoft.com/t5/failover-clustering/container-storage-support-with-cluster-shared-volumes-csv/ba-p/372140). |
| Controle de dialeto SMB | Novo | Agora você pode definir valores do Registro para controlar a versão mínima do SMB (dialeto) e a versão máxima do SMB usada. Para obter detalhes, confira [Como controlar dialetos do SMB](https://techcommunity.microsoft.com/t5/storage-at-microsoft/controlling-smb-dialects/ba-p/860024). |

## <a name="features-added-in-smb-311-with-windows-server-2016-and-windows-10-version-1607"></a>Recursos adicionados no SMB 3.11 com o Windows Server 2016 e Windows 10, versão 1607

| Recurso/funcionalidade  | Novo ou atualizado  | Resumo  |
| --------- | --------- | --------- |
| Criptografia SMB     |   Atualizado      | A criptografia SMB 3.1.1 com AES-GCM (Advanced Encryption Standard-Galois/Counter Mode) é mais rápida que a assinatura SMB ou criptografia SMB anterior usando AES-CCM.   |
| Cache de diretório | Novo | O SMB 3.1.1 inclui aprimoramentos no cache de diretório. Os clientes do Windows agora podem armazenar em cache diretórios muito maiores, com aproximadamente 500 mil entradas. Os clientes Windows tentarão realizar consultas de diretório com buffers de 1 MB para reduzir viagens de ida e volta e aprimorar o desempenho. |
| Integridade da pré-autenticação | Novo |  No SMB 3.1.1, a integridade de pré-autenticação fornece proteção aprimorada contra adulteração feita por um invasor man-in-the-Middle com o estabelecimento de conexão do SMB e mensagens de autenticação. Para obter detalhes, confira [Integridade pré-autenticação do SMB 3.1.1 no Windows 10](https://docs.microsoft.com/archive/blogs/openspecification/smb-3-1-1-pre-authentication-integrity-in-windows-10). |
| Aprimoramentos de criptografia do SMB | Novo | O SMB 3.1.1 oferece um mecanismo para negociar o algoritmo de criptografia por conexão, com opções para AES-128-CCM e AES-128-GCM. O AES-128-GCM é o padrão para novas versões do Windows, enquanto as versões mais antigas continuam usando AES-128-CCM. |
| Suporte à atualização de cluster sem interrupção | Novo | Habilita [atualizações de cluster sem interrupção](../../failover-clustering/cluster-operating-system-rolling-upgrade.md) permitindo que o SMB pareça ser compatível com versões máximas do SMB para clusters no processo de atualização. Para obter mais detalhes sobre como permitir que o SMB comunique-se usando diferentes versões (dialetos) do protocolo, confira a postagem no blog [Como controlar dialetos do SMB](https://techcommunity.microsoft.com/t5/storage-at-microsoft/controlling-smb-dialects/ba-p/860024). |
| Suporte ao cliente SMB Direct no Windows 10 | Novo | O Windows 10 Enterprise, o Windows 10 Education e o Windows 10 Pro para Estações de Trabalho agora são compatíveis com o cliente SMB Direct. |
| Suporte nativo para chamadas à API FileNormalizedNameInformation | Novo | Adiciona suporte nativo para consultar o nome normalizado de um arquivo. Para obter detalhes, confira [FileNormalizedNameInformation](https://docs.microsoft.com/openspecs/windows_protocols/ms-fscc/20bcadba-808c-4880-b757-4af93e41edf6). |

Para obter detalhes adicionais, confira a postagem no blog [Novidades no SMB 3.1.1 do Windows Server 2016 Technical Preview 2](https://docs.microsoft.com/archive/blogs/josebda/whats-new-in-smb-3-1-1-in-the-windows-server-2016-technical-preview-2).

## <a name="features-added-in-smb-302-with-windows-server-2012-r2-and-windows-81"></a>Recursos adicionados no SMB 3.02 com o Windows Server 2012 R2 e o Windows 8.1

| Recurso/funcionalidade  | Novo ou atualizado  | Resumo  |
| --------- | --------- | --------- |
| Rebalanceamento automático de clientes do servidor de arquivos expandidos     |   Novo      | Aprimora a escalabilidade e a capacidade de gerenciamento de Servidores de Arquivos de Escalabilidade Horizontal. As conexões de clientes SMB são controladas por compartilhamento de arquivos (em vez de por servidor), e os clientes são então redirecionados para o nó de cluster com o melhor acesso ao volume usado pelo compartilhamento de arquivos. Isso melhora a eficiência, reduzindo o tráfego de redirecionamento entre nós de servidor de arquivos. Os clientes são redirecionados após uma conexão inicial e quando o armazenamento de cluster é reconfigurado.    |
| Desempenho pela WAN   | Atualizado  | O Windows 8.1 e o Windows 10 fornecem arquivos CopyFile SRV_COPYCHUNK aprimorados sobre o suporte a SMB quando você usa o Explorador de Arquivos para cópias remotas de uma localização em um computador remoto para outra cópia no mesmo servidor. Você copiará apenas uma pequena quantidade de metadados pela rede (1/2 KiB por 16 MiB de dados de arquivo é transmitido). Isso resulta em um aprimoramento significativo no desempenho. Essa é uma distinção no nível do sistema operacional e do Explorador de Arquivos para SMB. |
| SMB Direct     |   Atualizado      | Melhora o desempenho para pequenas cargas de trabalho de E/S, aumentando a eficiência durante a hospedagem de cargas de trabalho com E/S pequenas, como um banco de dados OLTP (transação online) em uma máquina virtual. Esses aprimoramentos são evidentes ao usar interfaces de rede de velocidade mais elevadas, como Ethernet de 40 Gbps e InfiniBand de 56 Gbps.  |
| Limites de largura de banda do SMB | Novo | Agora você pode usar [Set-SmbBandwidthLimit](https://docs.microsoft.com/powershell/module/smbshare/set-smbbandwidthlimit) para definir limites de largura de banda em três categorias: VirtualMachine (tráfego do Hyper-V sobre SMB), LiveMigration (tráfego de Migração ao Vivo do Hyper-V sobre SMB) ou padrão (todos os outros tipos de tráfego SMB).

Para obter mais informações sobre a funcionalidade SMB nova e alterada no Windows Server 2012 R2, confira [Novidades no SMB no Windows Server](<https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2012-r2-and-2012/hh831474(v%3dws.11)>).

## <a name="features-added-in-smb-30-with-windows-server-2012-and-windows-8"></a>Recursos adicionados no SMB 3.0 com o Windows Server 2012 e Windows 8

| Recurso/funcionalidade  | Novo ou atualizado  | Resumo  |
| --------- | --------- | --------- |
| Failover Transparente do SMB     |   Novo    | Permite aos administradores realizar a manutenção de hardware ou software dos nós em um servidor de arquivos em cluster, sem interromper os aplicativos do servidor que armazenam dados nesses compartilhamentos de arquivos. Além disso, se uma falha de hardware ou software ocorre em um nó do cluster, os clientes SMB se reconectam a outro nó do cluster de forma transparente, sem interromper os aplicativos do servidor que estão armazenando dados nesses compartilhamentos de arquivos.        |
| Expansão do SMB     |   Novo      | Suporte para várias instâncias SMB em um Servidor de Arquivos de Escalabilidade Horizontal. Utilizando Volumes Compartilhados de Cluster (CSV) versão 2, os administradores podem criar compartilhamentos de arquivos que fornecem acesso simultâneo a arquivos de dados, com E/S direta, através de todos os nós em um cluster de servidor de arquivos. Isso proporciona uma melhor usação da largura de banda da rede e balanceamento de carga dos clientes do servidor de arquivos, além de otimizar o desempenho para os aplicativos de servidor.  |
| SMB Multichannel     |   Novo      |  Permite agregação de largura de banda da rede e tolerância a falhas da rede se vários caminhos estão disponíveis entre o cliente e o servidor do SMB. Isso permite que aplicativos de servidor aproveitem ao máximo toda a largura de banda disponível na rede e sejam resilientes a uma falha da rede.<br><br>O SMB Multichannel no SMB 3 contribui para um aumento significativo no desempenho em comparação às versões anteriores do SMB. |
| SMB Direct     |   Novo      | Dá suporte ao uso de adaptadores de rede com capacidade RDMA e pode funcionar a toda a velocidade com latência muito baixa, usando muito pouco da CPU. Para cargas de trabalho como o Hyper-V ou o Microsoft SQL Server, isso permite que um servidor de arquivos remoto se pareça com um armazenamento local.<br><br>O SMB Direct no SMB 3 contribui para um aumento significativo no desempenho em comparação às versões anteriores do SMB.  |
| Contadores de desempenho para aplicativos de servidor     |   Novo      |  Os novos contadores de desempenho do SMB fornecem informações detalhadas por compartilhamento sobre taxa de transferência, latência e E/S por segundo (ESPS), permitindo aos administradores analisar o desempenho dos compartilhamentos de arquivos do SMB em que seus dados estão armazenados. Esses contadores foram concebidos especificamente para aplicativos de servidor, tais como o Hyper-V e o SQL Server, que armazenam arquivos em compartilhamentos remotos de arquivos.      |
| Otimizações de desempenho    |  Atualizado   | Tanto o cliente quanto o servidor do SMB foram otimizados para pequenas E/S de leitura/gravação aleatórias, o que é comum em aplicativos de servidor como o SQL Server OLTP. Além disso, a Unidade Máxima de Transmissão (MTU) grande está ativada por padrão, o que melhora significativamente o desempenho em grandes transferências sequenciais, tais como data warehouse do SQL Server, backup ou restauração de banco de dados, implantação ou cópia de discos rígidos virtuais. |
| Cmdlets do Windows PowerShell específicos do SMB     |   Novo      |  Com os cmdlets do Windows PowerShell para SMB, um administrador pode gerenciar compartilhamentos de arquivos no servidor de arquivos, de ponta a ponta, a partir da linha de comando.   |
| Criptografia SMB     |   Novo      | Proporciona criptografia de ponta a ponta dos dados do SMB e protege os dados contra ocorrências de interceptação em redes não confiáveis. Não exige novos custos de implantação e não precisa de protocolo IPsec, hardware especializado ou aceleradores de WAN. Pode ser configurado a cada compartilhamento ou para todo o servidor de arquivos e pode ser habilitado para uma variedade de cenários onde os dados percorrem redes não confiáveis. |
| Concessão de Diretório do SMB     |  Novo | Melhora o tempo de resposta de aplicativos em escritórios de filial. Com o uso de concessões de diretório, as consultas de via dupla do cliente ao servidor são reduzidas, uma vez que os metadados são recuperados de um cache de diretório mais duradouro. A coerência do cache é mantida porque os clientes são notificados quando há alterações nas informações do diretório no servidor. Concessões de diretório funcionam com cenários para HomeFolder (leitura/gravação sem compartilhamento) e Publicação (somente leitura com compartilhamento).    |
| Desempenho pela WAN   | Novo   | Os oplocks (bloqueios oportunistas de diretório) e as concessões de oplock foram introduzidos no SMB 3.0. Para cargas de trabalho típicas de escritório/cliente, as oplocks/concessões são mostradas para reduzir as viagens de ida e volta da rede em aproximadamente 15%.<br><br>No SMB 3, a implementação do SMB do Windows foi refinada para aprimorar o comportamento de cache no cliente, bem como a capacidade de enviar por push taxas de transferência maiores.<br><br>O SMB 3 oferece aprimoramentos à API CopyFile(), bem como a ferramentas associadas, como o Robocopy, para enviar por push significativamente mais dados pela rede. |
| Negociação de dialeto seguro | Novo | Ajuda na proteção contra uma tentativa de man-in-the-middle para fazer downgrade da negociação de dialeto. A ideia é impedir que um invasor faça o downgrade do dialeto negociado inicialmente e das funcionalidades entre o cliente e o servidor. Para obter detalhes, confira [Negociação de Dialeto Seguro do SMB3](https://docs.microsoft.com/archive/blogs/openspecification/smb3-secure-dialect-negotiation). Observe que isso foi substituído pelo recurso de [integridade de pré-autenticação do SMB 3.1.1 no Windows 10](https://docs.microsoft.com/archive/blogs/openspecification/smb-3-1-1-pre-authentication-integrity-in-windows-10) no SMB 3.1.1. |


## <a name="hardware-requirements"></a>Requisitos de hardware

O Failover Transparente do SMB tem os seguintes requisitos:

* Um cluster de failover executando o Windows Server 2012 ou o Windows Server 2016 com pelo menos dois nós configurados. O cluster precisa passar pelos testes de validação de cluster incluídos no assistente de validação.
* Os compartilhamentos de arquivos devem ser criados com a propriedade de Disponibilidade Contínua (CA), que é o padrão.
* Os compartilhamentos de arquivos devem ser criados em caminhos de volume CSV para obtenção da Expansão do SMB.
* Os computadores cliente devem executar o Windows® 8 ou o Windows Server 2012, ambos incluindo o cliente SMB atualizado compatível com disponibilidade contínua.

> [!NOTE]
> Os clientes de nível inferior podem se conectar aos compartilhamentos de arquivos que têm a propriedade CA, mas o failover transparente não será compatível com esses clientes.

O SMB Multichannel tem os seguintes requisitos:

* Pelo menos dois computadores que executam o Windows Server 2012 são necessários. Nenhum recurso extra precisa ser instalado; a tecnologia está ativada por padrão.
* Para obter mais informações sobre configurações de rede recomendadas, veja a seção Consulte também no final deste tópico de visão geral.

O SMB Direct tem os seguintes requisitos:

* Pelo menos dois computadores que executam o Windows Server 2012 são necessários. Nenhum recurso extra precisa ser instalado; a tecnologia está ativada por padrão.
* São necessários adaptadores de rede com capacidade RDMA. Atualmente, estes adaptadores estão disponíveis em três diferentes tipos: iWARP, Infiniband ou RoCE (RDMA over Converged Ethernet).

## <a name="more-information"></a>Mais informações

A lista a seguir fornece recursos adicionais na Web sobre SMB e tecnologias relacionadas no Windows Server 2012 R2, no Windows Server 2012 e no Windows Server 2016.

* [Armazenamento no Windows Server](../storage.yml)
* [Servidor de Arquivos de Escalabilidade Horizontal para Dados de Aplicativos](../../failover-clustering/sofs-overview.md)
* [Aprimorar o desempenho de um servidor de arquivos com o SMB Direct](smb-direct.md)
* [Implantar o Hyper-V no SMB](<https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2012-r2-and-2012/jj134187(v%3dws.11)>)
* [Implantar o SMB Multichannel](<https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2012-r2-and-2012/dn610980(v%3dws.11)>)
* [Como implantar servidores de arquivos rápidos e eficientes em aplicativos para servidores](<https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2012-r2-and-2012/hh831723(v%3dws.11)>)
* [SMB: Guia de solução de problemas](<https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2012-r2-and-2012/dn659439(v%3dws.11)>)
