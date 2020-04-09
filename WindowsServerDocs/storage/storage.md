---
title: Armazenamento
author: JasonGerend
manager: elizapo
layout: LandingPage
ms.prod: windows-server
ms.technology: storage
ms.assetid: 6b74bc7c-a58d-4915-af8e-2cc27f2c4726
ms.topic: landing-page
ms.author: jgerend
ms.localizationpriority: medium
ms.date: 03/08/2019
ms.openlocfilehash: 5c8e6831e7e424896722c65d2ca6f34b3cc15e8d
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80820910"
---
# <a name="storage"></a>Armazenamento

>Aplica-se a: Windows Server 2019, Windows Server 2016, Windows Server (canal semestral)

>[!TIP]
> Procurando informações sobre versões anteriores do Windows Server? Confira as outras [bibliotecas do Windows Server](/previous-versions/windows/) em docs.microsoft.com. Você também pode [pesquisar neste site](https://docs.microsoft.com/search/index?search=Windows+Server&dataSource=previousVersions) para obter informações específicas.

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
                                            <p>Descubra as novidades no armazenamento do Windows Server</p>
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
                        <p><h3><a href="storage-spaces/storage-spaces-direct-overview.md">Espaços de Armazenamento Diretos</a></h3> Armazenamento local anexado diretamente, incluindo dispositivos SATA e NVME, para otimizar o uso do disco depois de adicionar novos discos físicos e para tempos de reparo de disco virtual mais rápidos. Consulte também <a href="storage-spaces/overview.md">Espaços de armazenamento</a> para obter informações sobre SAS compartilhados e espaços de armazenamento autônomo.</p>
<HR />
                        <p><h3><a href="storage-replica/storage-replica-overview.md">Réplica de armazenamento</a></h3> Replicação síncrona independente de armazenamento, em nível de bloco entre clusters ou servidores para preparação e recuperação de desastre, bem como o alargamento de um cluster de failover entre sites para alta disponibilidade. A replicação síncrona habilita o espelhamento de dados em locais físicos com volumes consistentes com falha, fazendo com que não haja perda de dados no nível do sistema de arquivos.</p>
<HR />
                        <p><h3><a href="storage-qos/storage-qos-overview.md">QoS (qualidade de serviço) de armazenamento</a></h3> Monitore e gerencie centralmente o desempenho de armazenamento para máquinas virtuais usando o Hyper-V e as funções de Servidor de Arquivos de Escalabilidade Horizontal aprimorar automaticamente a integridade de recursos de armazenamento entre várias máquinas virtuais usando o mesmo cluster de servidor de arquivos.</p>
<HR />
                        <p><h3><a href="data-deduplication/overview.md">Eliminação de Duplicação de Dados</a></h3> Otimiza o espaço livre em um volume examinando os dados no volume para duplicação. Uma vez identificadas, as duplicatas do conjunto de dados do volume são armazenadas uma vez e (opcionalmente) são compactadas para economizar ainda mais espaço. A Eliminação de Duplicação de Dados otimiza redundâncias sem comprometer a fidelidade ou a integridade dos dados.</p>
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
                        <p><h3><a href="storage-migration-service/overview.md">Serviço de migração de armazenamento</a></h3>Migre servidores para uma versão mais recente do Windows Server usando uma ferramenta gráfica que inventaria dados em servidores, transfere os dados e a configuração para servidores mais recentes e, opcionalmente, move as identidades dos servidores antigos para os novos servidores para que os aplicativos e usuários não precisem alterar nada.</p>
<HR />
                        <p><h3><a href="work-folders/work-folders-overview.md">Pastas de trabalho</a></h3> Armazene e acesse arquivos de trabalho em computadores e dispositivos pessoais, geralmente conhecidos como BYOD (Traga seu próprio dispositivo), além dos PCs corporativos. Os usuários obtêm um local conveniente para armazenar arquivos de trabalho e acessá-los em qualquer lugar. As organizações mantêm controle sobre dados corporativos armazenando os arquivos em servidores de arquivos gerenciados centralmente e, opcionalmente, especificando políticas de dispositivo de usuário como criptografia e senhas de bloqueio de tela.</p>
<HR />
                        <p><h3><a href="folder-redirection/folder-redirection-rup-overview.md">Arquivos Offline e redirecionamento de pasta</a></h3> Redirecione o caminho das pastas locais (como a pasta documentos) para um local de rede, enquanto armazena o conteúdo em cache localmente para aumentar a velocidade e a disponibilidade.</p>
<HR />
                        <p><h3><a href="folder-redirection/deploy-roaming-user-profiles.md">Perfis de usuário de roaming</a></h3> Redirecionar um perfil de usuário para um local de rede.</p>
<HR />
                        <p><h3><a href="dfs-namespaces/dfs-overview.md">Namespaces do DFS</a></h3> Agrupe pastas compartilhadas que estão localizadas em servidores diferentes em um ou mais namespaces logicamente estruturados. Cada namespace aparece aos usuários como uma única pasta compartilhada com uma série de subpastas. No entanto, a estrutura de base do namespace pode ser composta por vários compartilhamentos de arquivos localizados em diferentes servidores e em vários locais.</p>
<HR />
                        <p><h3><a href="dfs-replication/dfsr-overview.md">Replicação do DFS</a></h3> Replique pastas (incluindo aquelas referenciadas por um caminho de namespace do DFS) em vários servidores e sites. A Replicação DFS usa um algoritmo de compactação conhecido como RDC (compactação diferencial remota). A RDC detecta alterações nos dados de um arquivo e permite que a Replicação DFS replique somente os blocos de arquivo alterados, em vez do arquivo inteiro.</p>
<HR />
                        <p><h3><a href="fsrm/fsrm-overview.md">Gerenciador de Recursos de Servidor de Arquivos</a></h3> Gerencie e classifique os dados armazenados em servidores de arquivos.<p>
<HR />
                        <p><h3><a href="iscsi/iscsi-target-server.md">Servidor de destino iSCSI</a></h3> Fornece armazenamento em bloco para outros servidores e aplicativos na rede, usando Internet SCSI (iSCSI) padrão.</p>
<HR />
                       <p><h3><a href="iscsi/iscsi-boot-overview.md">Servidor de destino iSCSI</a></h3> O pode inicializar centenas de computadores de uma única imagem do sistema operacional armazenada em um local centralizado. Isso aumenta a eficiência, a capacidade de gerenciamento, a disponibilidade e a segurança.</p>
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
                        <p><h3><a href="refs/refs-overview.md">ReFS</a></h3> Um sistema de arquivos resiliente que maximiza a disponibilidade de dados, escala com eficiência para conjuntos de dados muito grandes entre diversas cargas de trabalho e fornece integridade de dados por meio de resiliência para corrupção (independentemente de falhas de software ou hardware).<p>
<HR />
                        <p><h3><a href="file-server/file-server-smb-overview.md">Protocolo SMB (Server Message Block)</a></h3> Um protocolo de compartilhamento de arquivos de rede que permite que aplicativos em um computador leiam e gravem em arquivos e solicitem serviços de programas de servidor em uma rede de computador. O protocolo SMB pode ser usado sobre seu protocolo TCP/IP ou outros protocolos de rede. Utilizando o protocolo SMB, um aplicativo (ou o usuário de um aplicativo) pode acessar arquivos ou outros recursos em um servidor remoto. Isso permite que os aplicativos leiam, criem e atualizem arquivos no servidor remoto. Ele também pode se comunicar com qualquer programa do servidor que esteja configurado para receber uma solicitação de um cliente SMB.<p>
<HR />
                        <p><h3><a href="storage-spaces/Storage-class-memory-health.md">Memória de classe de armazenamento</a></h3> Fornece desempenho semelhante à memória do computador (muito rápido), mas com a persistência de dados de unidades de armazenamento normal. O Windows trata a memória de classe de armazenamento da mesma forma que unidades normais (só que mais rapidamente), mas há algumas diferenças na maneira como a integridade do dispositivo é gerenciada.<p>
<HR />
                        <p><h3><a href="https://technet.microsoft.com/library/cc766295(v=ws.10).aspx">Criptografia de Unidade de Disco BitLocker</a></h3> Armazena dados em volumes em um formato criptografado, mesmo que o computador seja adulterado ou quando o sistema operacional não estiver em execução. Ele ajuda a proteger contra ataques offline, ataques feitos para desabilitar ou descartar o sistema operacional instalado ou feitos removendo fisicamente a unidade de disco rígido para atacar os dados separadamente.<p>
<HR />
                        <p><h3><a href="https://technet.microsoft.com/library/dn466522(v=ws.11).aspx">NT</a></h3> O sistema de arquivos primário para versões recentes do Windows e do Windows Server — fornece um conjunto completo de recursos, incluindo descritores de segurança, criptografia, cotas de disco e metadados avançados, e pode ser usado com CSV (volumes compartilhados de cluster) para fornecer volumes continuamente disponíveis que podem ser acessados simultaneamente de vários nós de um cluster de failover.<p>
<HR />
                        <p><h3><a href="https://technet.microsoft.com/library/jj592688(v=ws.11).aspx">NFS (sistema de arquivos de rede)</a></h3> O fornece uma solução de compartilhamento de arquivos para empresas que têm ambientes heterogêneos que consistem em computadores Windows e não Windows.<p>
                    </div>
                </div>
            </div>
        </div>
    </li>
</ul>

---


## <a name="in-azure"></a>No Azure

* [Armazenamento do Azure](https://azure.microsoft.com/documentation/services/storage/)
* [Azure StorSimple](https://www.microsoft.com/cloud-platform/azure-storsimple)
