---
title: Visão geral da tecnologia Hyper-V
description: Descreve o que é o Hyper-V, como obtê-lo, recursos principais e usos comuns.
ms.prod: windows-server
ms.service: na
manager: dongill
ms.technology: compute-hyper-v
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: ac069fed-7bf5-4cc3-aff5-25a2766040b8
author: KBDAzure
ms.author: kathydav
ms.date: 11/29/2016
ms.openlocfilehash: 053f92f1ef07a2e574c93412626ee792d4d982e3
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71366789"
---
# <a name="hyper-v-technology-overview"></a>Visão geral da tecnologia Hyper-V

>Aplica-se a: Windows Server 2016, Microsoft Hyper-V Server 2016, Windows Server 2019, Microsoft Hyper-V Server 2019

O Hyper-V é um produto de virtualização de hardware da Microsoft. Ele permite que você crie e execute uma versão de software de um computador, chamada de *máquina virtual*. Cada máquina virtual age como um computador completo, executando um sistema operacional e programas. Quando você precisa de recursos de computação, as máquinas virtuais oferecem mais flexibilidade, ajudam a economizar tempo e dinheiro e são uma maneira mais eficiente de usar o hardware do que simplesmente executar um sistema operacional em hardware físico.

O Hyper-V executa cada máquina virtual em seu próprio espaço isolado, o que significa que você pode executar mais de uma máquina virtual no mesmo hardware ao mesmo tempo. Talvez você queira fazer isso para evitar problemas, como uma falha que afete as outras cargas de trabalho, ou conceder a diferentes pessoas, grupos ou serviços acesso a diferentes sistemas.

## <a name="some-ways-hyper-v-can-help-you"></a>Algumas maneiras pelas quais o Hyper-V pode ajudá-lo

O Hyper-V pode ajudá-lo a:

- **Estabeleça ou expanda um ambiente de nuvem privada.** Forneça serviços de ti mais flexíveis e sob demanda ao migrar ou expandir o uso de recursos compartilhados e ajustar a utilização conforme as alterações de demanda.

- **Use o hardware com mais eficiência.** Consolide servidores e cargas de trabalho em menos computadores físicos mais potentes para usar menos energia e espaço físico.

- **Melhore a continuidade dos negócios.** Minimize o impacto do tempo de inatividade agendado e não agendado de suas cargas de trabalho.

- **Estabeleça ou expanda uma VDI (Virtual Desktop Infrastructure).** Usar uma estratégia de área de trabalho centralizada com o VDI pode ajudá-lo a aumentar a agilidade e a segurança dos dados, bem como simplificar a conformidade regulatória e gerenciar sistemas operacionais e aplicativos de desktop. Implante o Hyper-V e o Host de Virtualização de Área de Trabalho Remota (host de Virtualização RD) no mesmo servidor para disponibilizar áreas de trabalho virtuais pessoais ou pools de áreas de trabalho virtuais para seus usuários.

- **Torne o desenvolvimento e o teste mais eficientes.** Reproduza diferentes ambientes de computação sem precisar comprar ou manter todo o hardware necessário se você usava apenas sistemas físicos.

## <a name="hyper-v-and-other-virtualization-products"></a>Hyper-V e outros produtos de virtualização

O Hyper-V no Windows e no Windows Server substitui os produtos de virtualização de hardware mais antigos, como o Microsoft Virtual PC, o Microsoft Virtual Server e o Windows Virtual PC. O Hyper-V oferece recursos de rede, desempenho, armazenamento e segurança não disponíveis nesses produtos mais antigos.

O Hyper-V e a maioria dos aplicativos de virtualização de terceiros que exigem os mesmos recursos do processador não são compatíveis. Isso ocorre porque os recursos do processador, conhecidos como extensões de virtualização de hardware, foram projetados para não serem compartilhados. Para obter detalhes, consulte [aplicativos de virtualização não funcionam junto com o Hyper-V, o Device Guard e o Credential Guard](https://support.microsoft.com/kb/3204980).

## <a name="what-features-does-hyper-v-have"></a>Quais recursos o Hyper-V tem?

O Hyper-V oferece muitos recursos. Essa é uma visão geral, agrupada pelo que os recursos fornecem ou ajudam você a fazer.

**Ambiente computacional** – uma máquina virtual Hyper-V inclui as mesmas partes básicas que um computador físico, como memória, processador, armazenamento e rede. Todas essas partes têm recursos e opções que você pode configurar diferentes maneiras de atender a diferentes necessidades. O armazenamento e a rede podem ser considerados categorias próprias, por conta das várias maneiras que você pode configurá-las.

**Recuperação de desastres e backup** para recuperação de desastres, a réplica do Hyper-V cria cópias de máquinas virtuais, destinadas a serem armazenadas em outro local físico, para que você possa restaurar a máquina virtual da cópia. Para backup, o Hyper-V oferece dois tipos. Uma usa Estados salvos e a outra usa Serviço de Cópias de Sombra de Volume (VSS) para que você possa fazer backups consistentes com o aplicativo para programas que dão suporte ao VSS.

**Otimização** : cada sistema operacional convidado com suporte tem um conjunto personalizado de serviços e drivers, chamados *Integration Services*, que facilitam o uso do sistema operacional em uma máquina virtual do Hyper-V.

**Portabilidade** -recursos como migração ao vivo, migração de armazenamento e importação/exportação facilitam a movimentação ou a distribuição de uma máquina virtual.

**Conectividade remota** – o Hyper-V inclui conexão de máquina virtual, uma ferramenta de conexão remota para uso com Windows e Linux. Ao contrário do Área de Trabalho Remota, essa ferramenta oferece acesso ao console, para que você possa ver o que está acontecendo no convidado mesmo quando o sistema operacional ainda não foi inicializado.

**Segurança** -as máquinas virtuais blindadas e de inicialização segura ajudam a proteger contra malware e outros acessos não autorizados a uma máquina virtual e a seus dados.

Para obter um resumo dos recursos introduzidos nesta versão, consulte [What ' s New in Hyper-V in Windows Server](What-s-new-in-Hyper-V-on-Windows.md). Alguns recursos ou partes têm um limite de quantos podem ser configurados. Para obter detalhes, consulte [planejar a escalabilidade do Hyper-V no Windows Server 2016](plan/Plan-for-Hyper-V-scalability-in-Windows-Server-2016.md).

## <a name="how-to-get-hyper-v"></a>Como obter o Hyper-V

O Hyper-V está disponível no Windows Server e no Windows, como uma função de servidor disponível para versões x64 do Windows Server. Para obter instruções sobre o servidor, consulte [instalar a função Hyper-V no Windows Server](get-started/Install-the-Hyper-V-role-on-Windows-Server.md). No Windows, ele está disponível como [recurso](https://docs.microsoft.com/virtualization/hyper-v-on-windows/index) em algumas versões de 64 bits do Windows. Ele também está disponível como um produto de servidor autônomo e baixável, [Microsoft Hyper-V Server](https://www.microsoft.com/evalcenter/evaluate-hyper-v-server-2019).

## <a name="supported-operating-systems"></a>Sistemas operacionais com suporte

Muitos sistemas operacionais serão executados em máquinas virtuais. Em geral, um sistema operacional que usa uma arquitetura x86 será executado em uma máquina virtual do Hyper-V. No entanto, nem todos os sistemas operacionais que podem ser executados são testados e têm suporte da Microsoft. Para obter listas de o que tem suporte, consulte:

- [Máquinas virtuais Linux e FreeBSD com suporte para Hyper-V no Windows](Supported-Linux-and-FreeBSD-virtual-machines-for-Hyper-V-on-Windows.md)

- [Sistemas operacionais convidados do Windows com suporte para o Hyper-V no Windows Server](Supported-Windows-guest-operating-systems-for-Hyper-V-on-Windows.md)

## <a name="how-hyper-v-works"></a>Como funciona o Hyper-V

O Hyper-V é uma tecnologia de virtualização baseada em hipervisor. O Hyper-V usa o hipervisor do Windows, que exige um processador físico com recursos específicos. Para obter detalhes sobre o hardware, consulte [requisitos do sistema para o Hyper-V no Windows Server](System-requirements-for-Hyper-V-on-Windows.md).

Na maioria dos casos, o hipervisor gerencia as interações entre o hardware e as máquinas virtuais. Esse acesso controlado por hipervisor ao hardware fornece às máquinas virtuais o ambiente isolado no qual elas são executadas. Em algumas configurações, uma máquina virtual ou o sistema operacional em execução na máquina virtual tem acesso direto a gráficos, redes ou hardware de armazenamento.

## <a name="what-does-hyper-v-consist-of"></a>O que consiste no Hyper-V?

O Hyper-V exigiu partes que funcionam juntas para que você possa criar e executar máquinas virtuais. Juntas, essas partes são chamadas de plataforma de virtualização. Eles são instalados como um conjunto quando você instala a função Hyper-V. As partes necessárias incluem o hipervisor do Windows, o serviço gerenciamento de máquinas virtuais do Hyper-V, o provedor WMI de virtualização, o VMbus (barramento de máquina virtual), o VSP (provedor de serviços de virtualização) e o VID (driver de infraestrutura virtual).

O Hyper-V também tem ferramentas para gerenciamento e conectividade. Você pode instalá-los no mesmo computador em que a função Hyper-V está instalada e em computadores sem a função Hyper-V instalada. Essas ferramentas são:

- Gerenciador Hyper-V
- [Módulo do Hyper-V para Windows PowerShell](https://docs.microsoft.com/powershell/module/hyper-v/index)
- [Conexão de máquina Virtual](https://docs.microsoft.com/windows-server/virtualization/hyper-v/learn-more/hyper-v-virtual-machine-connect) \(Sometimes chamada VMConnect @ no__t-2
- [Windows PowerShell Direct](manage/Manage-Windows-virtual-machines-with-PowerShell-Direct.md)

## <a name="related-technologies"></a>Tecnologias relacionadas

Essas são algumas tecnologias da Microsoft que geralmente são usadas com o Hyper-V:

- [Clustering de failover](../../failover-clustering/whats-new-in-failover-clustering.md)
- [Serviços de área de trabalho remota](../../remote/remote-desktop-services/Host-desktops-and-apps-in-Remote-Desktop-Services.md)
- [System Center Virtual Machine Manager](https://docs.microsoft.com/system-center/vmm/overview)

Várias tecnologias de armazenamento: volumes compartilhados do cluster, SMB 3,0, espaços de armazenamento diretos

Os contêineres do Windows oferecem outra abordagem à virtualização. Consulte a biblioteca de [contêineres do Windows](https://docs.microsoft.com/virtualization/windowscontainers/index) no msdn.
