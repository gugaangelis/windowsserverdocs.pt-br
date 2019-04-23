---
title: Visão geral da tecnologia Hyper-V
description: Descreve o que é Hyper-V, como obter, os principais recursos e usos comuns.
ms.prod: windows-server-threshold
ms.service: na
manager: dongill
ms.technology: compute-hyper-v
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: ac069fed-7bf5-4cc3-aff5-25a2766040b8
author: KBDAzure
ms.author: kathydav
ms.date: 11/29/2016
ms.openlocfilehash: 9ae3c9dce36ad7d67a19ce167c9cb875b3c91810
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59864307"
---
# <a name="hyper-v-technology-overview"></a>Visão geral da tecnologia Hyper-V

>Aplica-se a: Windows Server 2016, Microsoft Hyper-V Server 2016, Windows Server 2019, Microsoft Hyper-V Server 2019

Hyper-V é um produto de virtualização de hardware da Microsoft. Permite que você crie e execute uma versão de software de um computador, chamado de um *máquina virtual*. Cada máquina virtual funciona como um computador completo, executando um sistema operacional e programas. Quando você precisar de recursos de computação, máquinas virtuais oferecem mais flexibilidade, ajudar a economizar tempo e dinheiro e são uma maneira mais eficiente de usar o hardware do que simplesmente executar um sistema operacional no hardware físico.

Hyper-V é executado a cada máquina virtual em seu próprio espaço isolado, o que significa que você pode executar mais de uma máquina virtual no mesmo hardware ao mesmo tempo. Você talvez queira fazer isso para evitar problemas, como uma falha que afeta outras cargas de trabalho, ou para dar acesso de pessoas, grupos ou serviços diferentes para diferentes sistemas.

## <a name="some-ways-hyper-v-can-help-you"></a>Algumas maneiras de Hyper-V pode ajudá-lo

Hyper-V pode ajudá-lo:

- **Estabelecer ou expandir um ambiente de nuvem privada.** Fornecer serviços de TI mais flexíveis e sob demanda, movendo para ou expandir seu uso dos recursos compartilhados e ajustar a utilização conforme a demanda.

- **Use o hardware com mais eficiência.** Consolide servidores e cargas de trabalho em computadores físicos, menos e mais eficaz usar menos energia e espaço físico.

- **Melhore a continuidade dos negócios.** Minimize o impacto do tempo de inatividade programado e das cargas de trabalho.

- **Estabelecer ou expandir uma virtual desktop infrastructure (VDI).** Use uma estratégia de área de trabalho centralizada com VDI pode ajudar a aumentar a agilidade dos negócios e a segurança de dados, bem como simplificar a conformidade normativa e gerenciar aplicativos e sistemas operacionais de desktop. Implante o Hyper-V e o Host de virtualização de área de trabalho remota (Host de virtualização de área de trabalho remota) no mesmo servidor para tornar áreas de trabalho virtuais pessoais ou pools de área de trabalho virtual disponível para seus usuários.

- **Tornar o desenvolvimento e teste mais eficiente.** Reproduza diferentes ambientes de computação sem ter que comprar ou manter todo o hardware que você precisaria se você usou apenas sistemas físicos.

## <a name="hyper-v-and-other-virtualization-products"></a>Hyper-V e outros produtos de virtualização

Hyper-V no Windows e Windows Server substitui os produtos de virtualização de hardware mais antigos, como o Microsoft Virtual PC, Microsoft Virtual Server e Windows Virtual PC. Hyper-V oferece recursos de rede, desempenho, armazenamento e segurança não está disponíveis nesses produtos mais antigos.

Hyper-V e a maioria dos aplicativos de virtualização de terceiros que exigem os mesmos recursos de processador não são compatíveis. Isso ocorre porque os recursos do processador, conhecidos como extensões de virtualização de hardware, são projetados para não ser compartilhado. Para obter detalhes, consulte [aplicativos de virtualização não funcionam em conjunto com o Hyper-V, o Device Guard e Credential Guard](https://support.microsoft.com/kb/3204980).

## <a name="what-features-does-hyper-v-have"></a>Quais recursos o Hyper-V tem?

Hyper-V oferece muitos recursos. Isso é uma visão geral, agrupada por que fornecem os recursos ou ajudam você a fazer.

**Ambiente de computação** -máquina virtual A Hyper-V inclui as mesmas partes básicas, como um computador físico, como processador, memória, armazenamento e rede. Todas essas partes têm recursos e as opções que você pode configurar diferentes maneiras de atender a necessidades diferentes. Armazenamento e rede podem cada ser consideradas categorias de seus próprios, por causa de diversas formas, você pode configurá-los.

**Backup e recuperação de desastres** -recuperação de desastres, a réplica do Hyper-V cria cópias de máquinas virtuais, se destina a ser armazenado em outro local físico, para que você possa restaurar a máquina virtual da cópia. Para backup, o Hyper-V oferece dois tipos. Um usa estados salvos e o outro usa o Volume Shadow Copy Service (VSS) para que você possa fazer backups consistentes com aplicativos para programas que dão suporte ao VSS.

**Otimização** -cada sistema operacional convidado com suporte tem um conjunto personalizado de serviços e drivers, chamados *do integration services*, que torna mais fácil de usar o sistema operacional em uma máquina virtual de Hyper-V.

**Portabilidade** - recursos, como migração ao vivo, migração de armazenamento, e a importação/exportação tornam mais fácil de mover ou distribuir uma máquina virtual.

**A conectividade remota** -Hyper-V inclui a Conexão de máquina Virtual, uma ferramenta de conexão remota para uso com o Windows e Linux. Ao contrário da área de trabalho remota, essa ferramenta permite que você console acesso, você pode ver o que está acontecendo na convidada, mesmo quando o sistema operacional não é inicializado ainda.

**Segurança** -a inicialização segura e máquinas virtuais blindadas ajudam a proteger contra malware e outro acesso não autorizado a uma máquina virtual e seus dados.

Para obter um resumo dos recursos introduzidos nesta versão, consulte [o que há de novo no Hyper-V no Windows Server](What-s-new-in-Hyper-V-on-Windows.md). Alguns recursos ou partes têm um limite a quantos podem ser configurados. Para obter detalhes, consulte [planejar a escalabilidade do Hyper-V no Windows Server 2016](plan/Plan-for-Hyper-V-scalability-in-Windows-Server-2016.md).

## <a name="how-to-get-hyper-v"></a>Como obter o Hyper-V

Hyper-V está disponível no Windows Server e Windows, como uma função de servidor disponível para x64 versões do Windows Server. Para obter instruções de servidor, consulte [instalar a função Hyper-V no Windows Server](get-started/Install-the-Hyper-V-role-on-Windows-Server.md). No Windows, está disponível como [recurso](https://docs.microsoft.com/virtualization/hyper-v-on-windows/index) em algumas versões de 64 bits do Windows. Ele também está disponível como um produto de servidor autônomo pode ser baixado, [Microsoft Hyper-V Server](https://www.microsoft.com/evalcenter/evaluate-hyper-v-server-2019).

## <a name="supported-operating-systems"></a>Sistemas operacionais compatíveis

Muitos sistemas operacionais serão executados em máquinas virtuais. Em geral, um sistema operacional que usa uma arquitetura será executado em uma máquina virtual de Hyper-V de x86. Nem todos os sistemas operacionais que podem ser executados são testados e suporte da Microsoft, no entanto. Para listas de suporte, consulte:

- [Linux e FreeBSD máquinas virtuais com suporte para Hyper-V no Windows](Supported-Linux-and-FreeBSD-virtual-machines-for-Hyper-V-on-Windows.md)

- [Sistemas de operacionais de convidados Windows com suporte para Hyper-V no Windows Server](Supported-Windows-guest-operating-systems-for-Hyper-V-on-Windows.md)

## <a name="how-hyper-v-works"></a>Como funciona o Hyper-V

Hyper-V é uma tecnologia de virtualização baseada em hipervisor. Hyper-V usa o hipervisor do Windows, que requer um processador físico com recursos específicos. Para obter detalhes de hardware, consulte [requisitos de sistema do Hyper-V no Windows Server](System-requirements-for-Hyper-V-on-Windows.md).

Na maioria dos casos, o hipervisor gerencia as interações entre o hardware e as máquinas virtuais. Esse acesso controlado de hipervisor ao hardware fornece o ambiente isolado em que são executadas de máquinas virtuais. Em algumas configurações, uma máquina virtual ou o sistema operacional executado na máquina virtual tem acesso direto ao hardware de gráficos, rede ou armazenamento.

## <a name="what-does-hyper-v-consist-of"></a>O que podem consistir de Hyper-V?

Exigida partes que trabalham juntos para que você pode criar e executar máquinas virtuais do Hyper-V. Juntas, essas partes são chamadas de plataforma de virtualização. Eles são instalados como um conjunto quando você instala a função Hyper-V. As partes necessárias incluem hipervisor do Windows, serviço de gerenciamento de máquina Virtual do Hyper-V, o provedor WMI de virtualização, a máquina virtual barramento VMbus, provedor de serviços de virtualização (VSP) e infraestrutura virtual VID (unidade).

Hyper-V também possui ferramentas para gerenciamento e conectividade. Você pode instalá-los no mesmo computador em que a função Hyper-V é instalada em e em computadores sem a função Hyper-V instalada. Essas ferramentas são:

- Gerenciador Hyper-V
- [Módulo Hyper-V para o Windows PowerShell](https://docs.microsoft.com/powershell/module/hyper-v/index)
- [Conexão de máquina virtual](https://docs.microsoft.com/windows-server/virtualization/hyper-v/learn-more/hyper-v-virtual-machine-connect) \(às vezes chamado de VMConnect\)
- [Windows PowerShell Direct](manage/Manage-Windows-virtual-machines-with-PowerShell-Direct.md)

## <a name="related-technologies"></a>Tecnologias relacionadas

Essas são algumas tecnologias da Microsoft que geralmente são usadas com o Hyper-v:

- [Clustering de failover](../../failover-clustering/whats-new-in-failover-clustering.md)
- [Serviços de área de trabalho remota](../../remote/remote-desktop-services/Host-desktops-and-apps-in-Remote-Desktop-Services.md)
- [System Center Virtual Machine Manager](https://docs.microsoft.com/system-center/vmm/overview)

Várias tecnologias de armazenamento: SMB 3.0, os volumes compartilhados do cluster de espaços de armazenamento diretos

Contêineres do Windows oferecem outra abordagem de virtualização. Consulte a [contêineres do Windows](https://docs.microsoft.com/virtualization/windowscontainers/index) biblioteca no MSDN.
