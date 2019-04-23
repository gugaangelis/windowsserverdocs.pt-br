---
ms.assetid: c9844427-27cf-4d76-b5bb-e06368b092f7
title: Clustering de failover
ms.prod: windows-server-threshold
layout: LandingPage
ms.topic: landing-page
ms.manager: dongill
author: JasonGerend
ms.author: jgerend
ms.technology: storage-failover-clustering
ms.date: 03/08/2019
ms.localizationpriority: high
ms.openlocfilehash: 445de065ff5b68b83481ee5bd83ebf18fdd180a7
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59848647"
---
# <a name="failover-clustering-in-windows-server"></a>Clustering de failover no Windows Server

> Aplica-se a: 2019, Windows Server 2016, Windows Server (canal semestral) do Windows Server

>[!TIP]
> Procurando informações sobre versões anteriores do Windows Server? Confira as outras [bibliotecas do Windows Server](/previous-versions/windows/) em docs.microsoft.com. Você também pode [pesquisar neste site](https://docs.microsoft.com/search/index?search=Windows+Server&dataSource=previousVersions) para obter informações específicas.

<hr />

Um cluster de failover é um conjunto de computadores independentes que trabalham em conjunto para aumentar a disponibilidade e escalabilidade de funções de cluster (antigamente chamadas de aplicações e serviços de cluster). Os servidores clusterizados (chamados de nós) são conectados por cabos físicos e por software. Se um ou mais dos nós do cluster falhar, o outro nó começará a fornecer o serviço (um processo conhecido como failover). Além disso, as funções de cluster são monitoradas de maneira proativa para verificar se estão funcionando adequadamente. Se o funcionamento não estiver correto, elas serão reiniciadas ou movidas para outro nó.

Os clusters de failover também fornecem a funcionalidade CSV (Volume Compartilhado Clusterizado) que, por sua vez, oferece um namespace consistente distribuído, o qual pode ser usado para acessar o armazenamento compartilhado em todos os nós. Com o recurso Clustering de Failover, os usuários passam pelo mínimo de interrupções no serviço.

O clustering de failover tem muitos aplicativos práticos, incluindo:
* Armazenamento de compartilhamento de arquivos altamente disponível ou continuamente disponível para aplicativos como máquinas virtuais de Microsoft SQL Server e Hyper-V
* Funções clusterizadas com alta disponibilidade que são executadas em servidores físicos ou em máquinas virtuais instaladas em servidores que executam o Hyper-V

<hr />

<ul class="cardsF panelContent">
<li>
                         <div class="cardSize">
                                <div class="cardPadding">
                                    <div class="card">
                                        <div class="cardImageOuter">
                                            <div class="cardImage">
                                                <img src="../media/i-whats-new.svg" alt="" />
                                            </div>
                                        </div>
                                        <div class="cardText">
                                        <h2><a href="whats-new-in-failover-clustering.md">Quais são as novidades no Clustering de Failover</a></h2>
                                        </div>
                                    </div>
                                </div>
                             </div>
                          </a>
                        </li>
                     </ul>
<HR />

<ul class="cardsF panelContent">

<li>
                         <div class="cardSize">
                                <div class="cardPadding">
                                    <div class="card">
                                        <div class="cardImageOuter">
                                            <div class="cardImage">
                                                <img src="../media/i-cluster.svg" alt="" />
                                            </div>
                                        </div>
                                        <div class="cardText">
                                        <h3>Entender</h3>
<HR />
                                        <p><a href="sofs-overview.md">Servidor de arquivos de escalabilidade horizontal para dados de aplicativo</a></p>
<HR />
                                        <p><a href="../storage/storage-spaces/understand-quorum.md">Quorum de cluster e pool</a></p>
<HR />
                                        <p><a href="fault-domains.md">Reconhecimento de domínio de falha</a></p>
<HR />
                                        <p><a href="smb-multichannel.md">Redes de cluster de multi-NIC e de SMB Multichannel simplificados</a></p>
<HR />
                                        <p><a href="vm-load-balancing-overview.md">Balanceamento de carga VM</a></p>
<HR />
                                        <p><a href="../storage/storage-spaces/cluster-sets.md">Conjuntos de cluster</a></p>
<HR />
                                        <p><a href="cluster-affinity.md">Afinidade de cluster</a></p>
                                        </div>
                                    </div>
                                </div>
                            </div>
                          </a>
                        </li>

<li>
                         <div class="cardSize">
                                <div class="cardPadding">
                                    <div class="card">
                                        <div class="cardImageOuter">
                                            <div class="cardImage">
                                                <img src="../media/i-cluster.svg" alt="" />
                                            </div>
                                        </div>
                                        <div class="cardText">
                                        <h3>Planejamento</h3>
<HR />
                                        <p><a href="clustering-requirements.md">Requisitos de Hardware de Clustering de failover e opções de armazenamento</a></p>
<HR />
                                        <p><a href="failover-cluster-csvs.md">Use Cluster Volumes Compartilhados (CSVs)</a></p>               
<HR />
                                        <p><a href="../storage/storage-spaces/storage-spaces-direct-in-vm.md">Usando clusters convidados da máquina virtual com espaços de armazenamento diretos</a></p>
                                        </div>
                                    </div>
                                </div>
                            </div>
                          </a>
                        </li>
<li>
                         <div class="cardSize">
                                <div class="cardPadding">
                                    <div class="card">
                                        <div class="cardImageOuter">
                                            <div class="cardImage">
                                                <img src="../media/i-cluster.svg" alt="" />
                                            </div>
                                        </div>
                                        <div class="cardText">
                                        <h3>Implantação</a></h3> 
<HR />
                                        <p><a href="prestage-cluster-adds.md">Pré-configurar os objetos de computador do Cluster nos serviços de domínio do Active Directory</a></p>
<HR />
                                        <p><a href="create-failover-cluster.md">Criando um Cluster de Failover</a></p> 
<HR />
                                        <p><a href="deploy-two-node-clustered-file-server.md">Implantar um servidor de arquivos com dois nós</a></p> 
<HR />
                                        <p><a href="manage-cluster-quorum.md">Gerenciar o quorum e testemunhas</a></p> 
<HR />
                                        <p><a href="deploy-cloud-witness.md">Implantar uma testemunha de nuvem</a></p>
<HR />
                                        <p><a href="file-share-witness.md">Implantar uma testemunha de compartilhamento de arquivo</a></p>
<HR />
                                        <p><a href="cluster-operating-system-rolling-upgrade.md">As atualizações sem interrupção do sistema operacional do cluster</a></p> 
<HR />
                                        <p><a href="upgrade-option-same-hardware.md">Atualizar um cluster de failover no mesmo hardware</a></p>
<HR />
                                        <p><a href="https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2012-R2-and-2012/dn265970\(v%3dws.11\)">Implantar um Cluster desanexado do Active Directory</a></p>
                                        </div>
                                    </div>
                                </div>
                            </div>
                          </a>
                        </li>
                     </ul>
<HR />
<ul class="cardsF panelContent">
<li>
                         <div class="cardSize">
                                <div class="cardPadding">
                                    <div class="card">
                                        <div class="cardImageOuter">
                                            <div class="cardImage">
                                                <img src="../media/i-cluster.svg" alt="" />
                                            </div>
                                        </div>
                                        <div class="cardText">
                                        <h3>Gerenciar</h3>
<HR />
                                        <p><a href="cluster-aware-updating.md">Cluster-Aware Updating</a></p> 
<HR />
                                        <p><a href="health-service-overview.md">Serviço de integridade</a></p>
<HR />
                                        <p><a href="cluster-domain-migration.md">Migração de domínio do cluster</a></p>
<HR />
                                        <p><a href="troubleshooting-using-wer-reports.md">Solução de problemas usando o relatório de erros do Windows</a></p> 
                                        </div>
                                    </div>
                                </div>
                            </div>
                          </a>
                        </li>
<li>
                         <div class="cardSize">
                                <div class="cardPadding">
                                    <div class="card">
                                        <div class="cardImageOuter">
                                            <div class="cardImage">
                                                <img src="../media/i-cluster.svg" alt="" />
                                            </div>
                                        </div>
                                        <div class="cardText">
                                        <h3>Ferramentas e configurações</a></h3>
<HR />
                                        <p><a href="https://docs.microsoft.com/powershell/module/failoverclusters/?view=win10-ps">Cmdlets do PowerShell de Clustering de failover</a></p> 
<HR />
                                        <p><a href="https://docs.microsoft.com/powershell/module/clusterawareupdating/?view=win10-ps">Com suporte a Cmdlets de PowerShell de atualização de cluster</a></p> 
                                        </div>
                                    </div>
                                </div>
                            </div>
                          </a>
                        </li>
<li>
                         <div class="cardSize">
                                <div class="cardPadding">
                                    <div class="card">
                                        <div class="cardImageOuter">
                                            <div class="cardImage">
                                                <img src="../media/i-cluster.svg" alt="" />
                                            </div>
                                        </div>
                                        <div class="cardText">
                                        <h3>Recursos da comunidade</a></h3>
<HR />
                                        <p><a href="https://go.microsoft.com/fwlink/p/?LinkId=230641">Fórum sobre alta disponibilidade (Clustering)</a></p> 
<HR />
                                        <p><a href="http://blogs.msdn.com/b/clustering/">Blog da equipe balanceamento de carga de rede e Clustering de failover</a></p> 
                                        </div>
                                    </div>
                                </div>
                            </div>
                          </a>
                        </li>
</ul>
