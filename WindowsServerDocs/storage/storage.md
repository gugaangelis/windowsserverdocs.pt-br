---
title: Armazenamento
description: ''
author: JasonGerend
manager: elizapo
layout: LandingPage
ms.prod: windows-server-threshold
ms.technology: storage
ms.assetid: 6b74bc7c-a58d-4915-af8e-2cc27f2c4726
ms.topic: landing-page
ms.author: jgerend
ms.localizationpriority: medium
ms.date: 03/08/2019
ms.openlocfilehash: d83d51ebf56d38f93c176d403ea5c6c14f625ee2
ms.sourcegitcommit: 419bf9d6d18e7a32cba2cbcd98587b81a79adc51
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/11/2019
ms.locfileid: "9152173"
---
# Armazenamento

>Aplica-se a: Windows Server 2019, Windows Server 2016, Windows Server (canal semestral)

>[!TIP]
> Procurando informações sobre versões anteriores do Windows Server? Confira nossas outras [bibliotecas do Windows Server](/previous-versions/windows/) em docs.microsoft.com. Você também pode [pesquisar este site](https://docs.microsoft.com/search/index?search=Windows+Server&dataSource=previousVersions) para obter informações específicas.

<hr />
O armazenamento no Windows Server oferece recursos novos e aprimorados para clientes SDDC (datacenter definido pelo software) que privilegiam cargas de trabalho virtualizadas. O Windows Server também fornece suporte extensivo para clientes corporativos usando servidores de arquivos com cargas de trabalho existentes.

<hr />
<ul class="cardsF panelContent">
<li>
 <a href="whats-new-in-storage.md">
                            <div class="cardSize">
                                <div class="cardPadding">
                                    <div class="card">
                                        <div class="cardImageOuter">
                                            <div class="cardImage">
                                                <img src="../media/i-whats-new.svg" alt="" />
                                            </div>
                                        </div>
                                        <div class="cardText">
                                            <h2>Quais são as novidades?</h2>
                                            <p>Descubra o que há de novo no armazenamento do Windows Server</p>
                                        </div>
                                    </div>
                                </div>
                            </div>
                          </a>
                        </li>
</ul>
<hr />
<ul class="cardsF panelContent">
<li>
        <div class="cardSize">
            <div class="cardPadding">
                <div class="card">
                    <div class="cardImageOuter">
                        <div class="cardImage">
                            <img src="../media/i-store.svg" alt="" />
                        </div>
                    </div>
                    <div class="cardText">
                        <h3>Armazenamento definido pelo software para cargas de trabalho virtualizadas</h3>
<HR />
                        <p><h3><a href="storage-spaces/storage-spaces-direct-overview.md">Espaços de Armazenamento Diretos</a></h3> Armazenamento local, incluindo dispositivos SATA e NVME, para otimizar o uso do disco após adicionar novos discos físicos e para o disco virtual mais rápido do tempo de reparo conectado diretamente. Consulte também <a href="storage-spaces/overview.md">Espaços de armazenamento</a> para obter informações sobre SAS compartilhados e espaços de armazenamento autônomo.</p>
<HR />
                        <p><h3><a href="storage-replica/storage-replica-overview.md">Réplica de armazenamento</a></h3> Independente de armazenamento, em nível de bloco a replicação síncrona entre clusters ou servidores para prontidão contra desastre e recuperação, bem como a expansão de um cluster de failover entre sites para alta disponibilidade. A replicação síncrona habilita o espelhamento de dados em locais físicos com volumes consistentes com falha, fazendo com que não haja perda de dados no nível do sistema de arquivos.</p>
<HR />
                        <p><h3><a href="storage-qos/storage-qos-overview.md">QoS (Qualidade de armazenamento do serviço)</a></h3> Monitorar e gerenciar centralmente o desempenho de armazenamento para máquinas virtuais usando Hyper-V e as funções de servidor de arquivos de escalabilidade horizontal automaticamente improveing igualdade de recursos de armazenamento entre várias máquinas virtuais usando o mesmo cluster de servidor de arquivos.</p>
<HR />
                        <p><h3><a href="data-deduplication/overview.md">Eliminação de Duplicação de Dados</a></h3> Otimiza o espaço livre em um volume, examinando os dados no volume para eliminação de duplicação. Uma vez identificadas, as duplicatas do conjunto de dados do volume são armazenadas uma vez e (opcionalmente) são compactadas para economizar ainda mais espaço. A Eliminação de Duplicação de Dados otimiza redundâncias sem comprometer a fidelidade ou a integridade dos dados.</p>
                    </div>
                </div>
            </div>
        </div>
    </li>
<li>
        <div class="cardSize">
            <div class="cardPadding">
                <div class="card">
                    <div class="cardImageOuter">
                        <div class="cardImage">
                            <img src="../media/i-store.svg" alt="" />
                        </div>
                    </div>
                    <div class="cardText">
                        <h3>Servidores de arquivos de finalidade geral</h3>
<HR />
                        <p><h3><a href="storage-migration-service/overview.md">Serviço de migração do armazenamento</a></h3>Migrar servidores para uma versão mais recente do Windows Server usando uma ferramenta gráfica que faz o inventário de dados em servidores, transfere os dados e a configuração para servidores mais recentes e, opcionalmente, move as identidades dos servidores antigos para os novos servidores assim que usuários e aplicativos não precisa alterar nada.</p>
<HR />
                        <p><h3><a href="work-folders/work-folders-overview.md">Pastas de Trabalho</a></h3> Acesso e armazenamento de arquivos de trabalho em PCs e dispositivos, também conhecidos como traga seu próprio dispositivo (BYOD), além de computadores corporativos. Os usuários obtêm um local conveniente para armazenar arquivos de trabalho e acessá-los em qualquer lugar. As organizações mantêm controle sobre dados corporativos armazenando os arquivos em servidores de arquivos gerenciados centralmente e, opcionalmente, especificando políticas de dispositivo de usuário como criptografia e senhas de bloqueio de tela.</p>
<HR />
                        <p><h3><a href="folder-redirection/folder-redirection-rup-overview.md">Arquivos offline e redirecionamento de pasta</a></h3> Redirecione o caminho de pastas locais (como a pasta de documentos) para um local de rede, enquanto armazena em cache o conteúdo localmente para aumentar a velocidade e a disponibilidade.</p>
<HR />
                        <p><h3><a href="folder-redirection/deploy-roaming-user-profiles.md">Perfis de usuário</a></h3> Redirecione um perfil de usuário para um local de rede.</p>
<HR />
                        <p><h3><a href="dfs-namespaces/dfs-overview.md">Namespaces DFS</a></h3> Agrupe pastas compartilhadas localizadas em diferentes servidores em um ou mais namespaces estruturados logicamente. Cada namespace aparece aos usuários como uma única pasta compartilhada com uma série de subpastas. No entanto, a estrutura de base do namespace pode ser composta por vários compartilhamentos de arquivos localizados em diferentes servidores e em vários locais.</p>
<HR />
                        <p><h3><a href="dfs-replication/dfsr-overview.md">Replicação do DFS</a></h3> Replica pastas (incluindo aquelas referenciadas por um caminho de namespace do DFS) em vários servidores e sites. A Replicação DFS usa um algoritmo de compactação conhecido como RDC (compactação diferencial remota). A RDC detecta alterações nos dados de um arquivo e permite que a Replicação DFS replique somente os blocos de arquivo alterados, em vez do arquivo inteiro.</p>
<HR />
                        <p><h3><a href="fsrm/fsrm-overview.md">Gerenciador de Recursos de Servidor de Arquivos</a></h3> Gerencie e classifique dados armazenados em servidores de arquivos.<p>
<HR />
                        <p><h3><a href="iscsi/iscsi-target-server.md">iSCSI Target Server</a></h3> Oferece armazenamento em bloco para outros servidores e aplicativos na rede, usando o padrão iSCSI (Internet SCSI).</p>
<HR />
                       <p><h3><a href="iscsi/iscsi-boot-overview.md">iSCSI Target Server</a></h3> Inicializar centenas de computadores de uma imagem de sistema operacional único que é armazenado em um local centralizado. Isso aumenta a eficiência, a capacidade de gerenciamento, a disponibilidade e a segurança.</p>
                    </div>
                </div>
            </div>
        </div>
    </li>
<li>
        <div class="cardSize">
            <div class="cardPadding">
                <div class="card">
                    <div class="cardImageOuter">
                        <div class="cardImage">
                            <img src="../media/i-store.svg" alt="" />
                        </div>
                    </div>
                    <div class="cardText">
                        <h3>Sistemas de arquivos, protocolos, etc.</h3>
<HR />
                        <p><h3><a href="refs/refs-overview.md">ReFS</a></h3> Um sistema de arquivos resiliente que maximiza a disponibilidade de dados, dimensiona de forma eficiente conjuntos de dados muito grandes em diversas cargas de trabalho e oferece integridade de dados por meio de resiliência à corrupção (independentemente de falhas de software ou hardware).<p>
<HR />
                        <p><h3><a href="file-server/file-server-smb-overview.md">Protocolo do Server Message Block (SMB)</a></h3> Um protocolo que permite que os aplicativos em um computador leiam e gravem em arquivos e solicitem serviços dos programas do servidor em uma rede de computador de compartilhamento de arquivos de rede. O protocolo SMB pode ser usado sobre seu protocolo TCP/IP ou outros protocolos de rede. Utilizando o protocolo SMB, um aplicativo (ou o usuário de um aplicativo) pode acessar arquivos ou outros recursos em um servidor remoto. Isso permite que os aplicativos leiam, criem e atualizem arquivos no servidor remoto. Ele também pode se comunicar com qualquer programa do servidor que esteja configurado para receber uma solicitação de um cliente SMB.<p>
<HR />
                        <p><h3><a href="storage-spaces/Storage-class-memory-health.md">Memória de classe de armazenamento</a></h3> Oferece desempenho semelhante à memória do computador (realmente rápida), mas com a persistência de dados de unidades de armazenamento normal. O Windows trata a memória de classe de armazenamento da mesma forma que unidades normais (só que mais rapidamente), mas há algumas diferenças na maneira como a integridade do dispositivo é gerenciada.<p>
<HR />
                        <p><h3><a href="https://technet.microsoft.com/library/cc766295(v=ws.10).aspx">Criptografia de Unidade de Disco BitLocker</a></h3> Armazena dados em volumes em um formato criptografado mesmo se o computador for violado ou quando o sistema operacional não está em execução. Ele ajuda a proteger contra ataques offline, ataques feitos para desabilitar ou descartar o sistema operacional instalado ou feitos removendo fisicamente a unidade de disco rígido para atacar os dados separadamente.<p>
<HR />
                        <p><h3><a href="https://technet.microsoft.com/library/dn466522(v=ws.11).aspx">NTFS</a></h3> O sistema de arquivos primário para versões recentes do Windows e Windows Server — fornece um conjunto completo de recursos, incluindo descritores de segurança, criptografia, cotas de disco e metadados sofisticados e pode ser usado com Volumes de compartilhados de Cluster (CSV) para fornecer continuamente volumes disponíveis que podem ser acessados simultaneamente em vários nós de um Cluster de Failover.<p>
<HR />
                        <p><h3><a href="https://technet.microsoft.com/library/jj592688(v=ws.11).aspx">Sistema de arquivos de rede (NFS)</a></h3> Oferece uma solução para empresas com ambientes heterogêneos do Windows e não Windows computadores de compartilhamento de arquivos.<p>
                    </div>
                </div>
            </div>
        </div>
    </li>
</ul>

---


## No Azure

* [Armazenamento do Azure](https://azure.microsoft.com/documentation/services/storage/)
* [Azure StorSimple](https://www.microsoft.com/en-us/cloud-platform/azure-storsimple)
