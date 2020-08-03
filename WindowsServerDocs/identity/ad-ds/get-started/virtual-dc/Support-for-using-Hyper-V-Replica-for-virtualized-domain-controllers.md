---
ms.assetid: 45a65504-70b5-46ea-b2e0-db45263fabaa
title: Suporte para usar a réplica do Hyper-V para controladores de domínio virtualizados
author: MicrosoftGuyJFlo
ms.author: joflore
manager: mtillman
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adds
ms.openlocfilehash: 10252626e3732197e681e12851d5ef66bad1cc80
ms.sourcegitcommit: 3632b72f63fe4e70eea6c2e97f17d54cb49566fd
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/03/2020
ms.locfileid: "87519073"
---
# <a name="support-for-using-hyper-v-replica-for-virtualized-domain-controllers"></a>Suporte para usar a réplica do Hyper-V para controladores de domínio virtualizados

> Aplica-se a: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Este tópico explica a capacidade de suporte do uso da Réplica do Hyper-V para replicar uma VM (máquina virtual) que é executada como um DC (controlador de domínio). A Réplica do Hyper-V é uma nova funcionalidade do Hyper-V oferecida a partir do Windows Server 2012, que oferece um mecanismo interno de replicação no nível de uma VM.

A Réplica do Hyper-V replica de modo assíncrono as VMs escolhidas em um host Hyper-V primário para um host Hyper-V de réplica entre links de LAN (rede local) ou WAN (rede de longa distância). Depois que a replicação inicial é concluída, as alterações seguintes são replicadas a um intervalo definido pelo administrador.

O failover pode ser planejado ou não planejado. Um failover planejado é iniciado por um administrador na VM primária, e todas as alterações não replicadas são copiadas na VM de réplica para evitar qualquer perda de dados. Um failover não planejado é iniciado na VM de réplica em resposta a uma falha inesperada da VM primária. A perda de dados é possível porque não há oportunidade de transmitir as alterações da VM primária, que podem ainda não ter sido replicadas.

Para obter mais informações sobre a réplica do Hyper-V, consulte [visão geral da réplica do Hyper-v](/previous-versions/windows/it-pro/windows-server-2012-R2-and-2012/jj134172(v=ws.11)) e [implantar réplica do Hyper-v](/previous-versions/windows/it-pro/windows-server-2012-R2-and-2012/jj134207(v=ws.11)).

> [!NOTE]
> A Réplica do Hyper-V só pode ser executada no Hyper-V do Windows Server, e não na versão do Hyper-V executada no Windows 8.

## <a name="windows-server-2012-or-newer-domain-controllers-required"></a>Windows Server 2012 ou controladores de domínio mais recentes necessários

O Hyper-V do Windows Server 2012 introduziu VM-Generationid (VMGenID). O VMGenID fornece um caminho para que o hipervisor se comunique com o SO convidado quando ocorrem alterações significativas. Por exemplo, o hipervisor pode comunicar a um DC virtualizado que houve restauração de conteúdo de um instantâneo (tecnologia de restauração de instantâneos do Hyper-V, não de restauração de backup). AD DS no Windows Server 2012 e mais recente estão cientes da tecnologia de VM VMGenID e a usa para detectar quando as operações do hipervisor são executadas, como a restauração de instantâneo, o que permite que ela se proteja melhor.

> [!NOTE]
> Somente AD DS nos DCs do Windows Server 2012 ou mais recente fornecem essas medidas de segurança resultantes de VMGenID; Os DCs que executam todas as versões anteriores do Windows Server estão sujeitos a problemas como reversão de USN que podem ocorrer quando um controlador de domínio virtualizado é restaurado usando um mecanismo sem suporte, como a restauração de instantâneo. Para obter mais informações sobre essas proteções e quando elas são disparadas, consulte [arquitetura do controlador de domínio virtualizado](./virtualized-domain-controller-architecture.md).

Quando ocorre um failover de réplica do Hyper-V (planejado ou não planejado), o DC virtualizado detecta uma redefinição de VMGenID, disparando os recursos de segurança mencionados anteriormente. As operações do Active Directory continuarão normalmente. A VM de réplica será executada no lugar da VM primária.

> [!NOTE]
> Como não existirão duas instâncias da mesma identidade de DC, é possível que as instâncias primária e replicada sejam executadas. Embora a Réplica do Hyper-V tenha mecanismos de controle estabelecidos para garantir que as VMs primária e de réplica não sejam executadas simultaneamente, isso será possível caso o link entre elas falhe após a replicação da VM. No caso dessa ocorrência improvável, os DCs virtualizados executando o Windows Server 2012 têm garantias para ajudar a proteger o AD DS, enquanto os DCs virtualizados executando versões anteriores do Windows Server não têm.

Ao usar a réplica do Hyper-V, certifique-se de seguir as práticas recomendadas para [executar controladores de domínio virtual no Hyper-v](/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/dd363553(v=ws.10)). São discutidas, por exemplo, as recomendações de armazenamento de arquivos do Active Directory em discos SCSI virtuais, o que oferece garantias mais sólidas de durabilidade dos dados.

## <a name="supported-and-unsupported-scenarios"></a>Cenários com e sem suporte

Somente as VMs que executam o Windows Server 2012 ou mais recente têm suporte para failover não planejado e para teste de failover. Mesmo para o failover planejado, o Windows Server 2012 ou mais recente é recomendado para o DC virtualizado a fim de reduzir os riscos caso um administrador inicie inadvertidamente a VM primária e a VM replicada ao mesmo tempo.

As VMs executando versões anteriores do Windows Server têm suporte para o failover planejado, mas não para o failover não planejado em virtude da possibilidade de reversão de USN. Para obter mais informações sobre a reversão de USN, consulte [USN e reversão de USN](/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/dd363553(v=ws.10)).

> [!NOTE]
> Não existem requisitos de nível funcional para o domínio ou a floresta. Existem apenas requisitos de sistema operacional para os DCs executados como VMs replicadas usando a Réplica do Hyper-V. As VMs podem ser implantadas em uma floresta que contenha outros DCs físicos ou virtuais executando versões anteriores do Windows Server, podendo ou não ser replicadas com a Réplica do Hyper-V.

Essa instrução de suporte é baseada em testes realizados em um domínio/floresta individual, embora configurações de floresta com vários domínios também tenham suporte. Para esses testes, os controladores de domínio virtualizados DC1 e DC2 são parceiros de replicação do Active Directory em um mesmo site, hospedados em um servidor executando o Hyper-V no Windows Server 2012. A Réplica do Hyper-V fica habilitada no convidado de VM que executa o DC2. O servidor da réplica é hospedado em outro datacenter distante geograficamente. Para ajudar a explicar os processos de caso de teste destacados abaixo, a VM em execução no servidor de réplica é chamada de DC2-Rec (apesar de, na prática, ela ter o mesmo nome da VM original).

### <a name="windows-server-2012"></a>Windows Server 2012

A tabela a seguir explica o suporte a DCs virtualizados que executam o Windows Server 2012 e os casos de teste.

| Failover planejado | Failover não planejado |
|--|--|
| Com suporte | Com suporte |
| Caso de teste:<p>-DC1 e DC2 estão executando o Windows Server 2012.<p>-DC2 é desligado e um failover é executado em DC2-rec. O failover pode ser planejado ou não planejado.<p>-Depois que DC2-REC é iniciado, ele verifica se o valor de VMGenID que ele tem em seu banco de dados é o mesmo que o valor do driver de máquina virtual salvo pelo servidor de réplica do Hyper-V.<p>-Como resultado, DC2-REC dispara proteções de virtualização; em outras palavras, ele redefine sua invocação, descarta seu pool RID e define um requisito de sincronização inicial antes de assumir uma função de mestre de operações. Para saber mais sobre o requisito de sincronização inicial, confira.<p>-DC2-REC, em seguida, salva o novo valor de VMGenID em seu banco de dados e confirma todas as atualizações subsequentes no contexto da nova invocação.<p>-Como resultado da redefinição de ininvocaid, DC1 convergirá em todas as alterações do AD introduzidas por DC2-REC, mesmo que tenha sido revertida no tempo, o que significa que todas as atualizações do AD executadas no DC2-REC após o failover irão convergir com segurança | O caso de teste é o mesmo do failover planejado, mas com estas exceções:<p>-Quaisquer atualizações do AD recebidas no DC2, mas ainda não foram replicadas pelo AD para um parceiro de replicação antes que o evento de failover seja perdido.<p>-As atualizações do AD recebidas no DC2 após a hora do ponto de recuperação que foram replicadas pelo AD para DC1 serão replicadas de DC1 de volta para DC2-rec. |

### <a name="windows-server-2008-r2-and-earlier-versions"></a>Windows Server 2008 R2 e versões anteriores

A tabela a seguir explica o suporte a DCs virtualizados executando o Windows Server 2008 R2 e versões anteriores.

| Failover planejado | Failover não planejado |
|--|--|
| Com suporte, mas não recomendável, porque os DCs que executam essas versões do Windows Server não dão suporte a VMGenID nem usam garantias de virtualização associadas. Isso os coloca em risco para a reversão de USN. Para obter mais informações, consulte [USN e reversão de USN](/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/dd363553(v=ws.10)). | Sem suporte<p>**Observação:** O failover não planejado teria suporte quando a reversão do USN não for um risco, como um único DC na floresta (uma configuração que não é recomendada). |
| Caso de teste:<p>-DC1 e DC2 estão executando o Windows Server 2008 R2.<p>-DC2 é desligado e um failover planejado é executado em DC2-rec. Todos os dados no DC2 são replicados para DC2-REC antes que o desligamento seja concluído.<p>-Depois de DC2-REC ser iniciado, ele retoma a replicação com DC1 usando a mesma invocaid do DC2. | N/D |
