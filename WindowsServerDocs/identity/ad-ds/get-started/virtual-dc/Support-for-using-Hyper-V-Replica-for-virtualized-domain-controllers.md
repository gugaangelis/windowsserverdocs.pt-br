---
ms.assetid: 45a65504-70b5-46ea-b2e0-db45263fabaa
title: Suporte para usar a réplica do Hyper-V para controladores de domínio virtualizados
description: ''
author: MicrosoftGuyJFlo
ms.author: joflore
manager: mtillman
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adds
ms.openlocfilehash: 0203c6de55a4e691d7c484351a3280c49891f317
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59866277"
---
# <a name="support-for-using-hyper-v-replica-for-virtualized-domain-controllers"></a>Suporte para usar a réplica do Hyper-V para controladores de domínio virtualizados

>Aplica-se a: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Este tópico explica a capacidade de suporte do uso da Réplica do Hyper-V para replicar uma VM (máquina virtual) que é executada como um DC (controlador de domínio). A Réplica do Hyper-V é uma nova funcionalidade do Hyper-V oferecida a partir do Windows Server 2012, que oferece um mecanismo interno de replicação no nível de uma VM.  
  
A Réplica do Hyper-V replica de modo assíncrono as VMs escolhidas em um host Hyper-V primário para um host Hyper-V de réplica entre links de LAN (rede local) ou WAN (rede de longa distância). Depois que a replicação inicial é concluída, as alterações seguintes são replicadas a um intervalo definido pelo administrador.  
  
O failover pode ser planejado ou não planejado. Um failover planejado é iniciado por um administrador na VM primária e as alterações não replicadas são copiadas para a VM para evitar qualquer perda de dados de réplica. Um failover não planejado é iniciado na VM de réplica em resposta a uma falha inesperada da VM primária. A perda de dados é possível porque não há oportunidade de transmitir as alterações da VM primária, que podem ainda não ter sido replicadas.  
  
Para obter mais informações sobre a réplica do Hyper-V, consulte [visão geral da réplica do Hyper-V](https://technet.microsoft.com/library/jj134172.aspx) e [implantar a réplica do Hyper-V](https://technet.microsoft.com/library/jj134207.aspx).  
  
> [!NOTE]  
> A Réplica do Hyper-V só pode ser executada no Hyper-V do Windows Server, e não na versão do Hyper-V executada no Windows 8.  
  
## <a name="windows-server-2012-or-newer-domain-controllers-required"></a>Windows Server 2012 ou mais recentes controladores de domínio são necessários

Windows Server 2012 Hyper-V introduziu VM-GenerationID (VMGenID). O VMGenID fornece um caminho para que o hipervisor se comunique com o SO convidado quando ocorrem alterações significativas. Por exemplo, o hipervisor pode comunicar a um DC virtualizado que houve restauração de conteúdo de um instantâneo (tecnologia de restauração de instantâneos do Hyper-V, não de restauração de backup). AD DS no Windows Server 2012 e mais recente está ciente da tecnologia de VM VMGenID e usa-o para detectar quando operações de hipervisor são executadas, como restauração de instantâneo, o que permite que ele se proteger melhor.  
  
> [!NOTE]
> Apenas o AD DS nos controladores de domínio do Windows Server 2012 ou mais recente fornecer essas medidas de segurança resultantes do VMGenID; Controladores de domínio que executam todas as versões anteriores do Windows Server estão sujeitos a problemas como a reversão de USN que podem ocorrer quando um DC virtualizado for restaurado usando um mecanismo sem suporte, como a restauração de instantâneo. Para obter mais informações sobre essas garantias e quando elas são acionadas, consulte [arquitetura do controlador de domínio virtualizado](https://technet.microsoft.com/library/jj574118.aspx).  
  
Quando ocorre um failover de réplica de Hyper-V (planejado ou não planejado), o controlador de domínio virtualizado detecta uma reinicialização de vmgenid, acionando os recursos de segurança mencionados acima. As operações do Active Directory continuarão normalmente. A VM de réplica será executada no lugar da VM primária.  
  
> [!NOTE]  
> Considerando que agora há duas instâncias da mesma identidade de DC, há um potencial para a instância primária e replicada para ser executado. Embora a Réplica do Hyper-V tenha mecanismos de controle estabelecidos para garantir que as VMs primária e de réplica não sejam executadas simultaneamente, isso será possível caso o link entre elas falhe após a replicação da VM. No caso dessa ocorrência improvável, os DCs virtualizados executando o Windows Server 2012 têm garantias para ajudar a proteger o AD DS, enquanto os DCs virtualizados executando versões anteriores do Windows Server não têm.  
  
Ao usar réplica do Hyper-V, certifique-se de que você siga as práticas recomendadas para [executando controladores de domínio virtual no Hyper-V](https://technet.microsoft.com/library/virtual_active_directory_domain_controller_virtualization_hyperv(v=WS.10).aspx). São discutidas, por exemplo, as recomendações de armazenamento de arquivos do Active Directory em discos SCSI virtuais, o que oferece garantias mais sólidas de durabilidade dos dados.  
  
## <a name="supported-and-unsupported-scenarios"></a>Cenários com e sem suporte

Somente as VMs que são mais recente ou execução do Windows Server 2012 e com suporte para failover não planejado para failover de teste. Até mesmo para o failover planejado, Windows Server 2012 ou mais recente é recomendado para o controlador de domínio virtualizado para atenuar os riscos que um administrador inicie inadvertidamente a VM primária e a VM replicada ao mesmo tempo.  
  
As VMs executando versões anteriores do Windows Server têm suporte para o failover planejado, mas não para o failover não planejado em virtude da possibilidade de reversão de USN. Para obter mais informações sobre a reversão do USN, consulte [USN e reversão de USN](https://technet.microsoft.com/library/d2cae85b-41ac-497f-8cd1-5fbaa6740ffe(v=ws.10)).  
  
> [!NOTE]  
> Não existem requisitos de nível funcional para o domínio ou a floresta. Existem apenas requisitos de sistema operacional para os DCs executados como VMs replicadas usando a Réplica do Hyper-V. As VMs podem ser implantadas em uma floresta que contenha outros DCs físicos ou virtuais executando versões anteriores do Windows Server, podendo ou não ser replicadas com a Réplica do Hyper-V.  
  
Essa instrução de suporte é baseada em testes realizados em um domínio/floresta individual, embora configurações de floresta com vários domínios também tenham suporte. Para esses testes, os controladores de domínio virtualizados DC1 e DC2 são parceiros de replicação do Active Directory em um mesmo site, hospedados em um servidor executando o Hyper-V no Windows Server 2012. A Réplica do Hyper-V fica habilitada no convidado de VM que executa o DC2. O servidor da réplica é hospedado em outro datacenter distante geograficamente. Para ajudar a explicar os processos de caso de teste destacados abaixo, a VM em execução no servidor de réplica é chamada de DC2-Rec (apesar de, na prática, ela ter o mesmo nome da VM original).  
  
### <a name="windows-server-2012"></a>Windows Server 2012

A tabela a seguir explica o suporte a DCs virtualizados que executam o Windows Server 2012 e os casos de teste.  
  
|||  
|-|-|  
|Failover planejado|Failover não planejado|  
|Com suporte|Com suporte|  
|Caso de teste:<br /><br />-DC1 e DC2 executam o Windows Server 2012.<br /><br />-DC2 é desligado e um failover é realizado no DC2-Rec. O failover pode ser planejado ou não planejado.<br /><br />-Depois DC2-Rec for iniciado, ele verifica se o valor de VMGenID em seu banco de dados é igual ao valor do driver de máquina virtual salvo pelo servidor de réplica do Hyper-V.<br /><br />-Como resultado, o DC2-Rec aciona garantias de virtualização; em outras palavras, ele redefinirá seu InvocationID, descartará o pool RID e define um requisito de sincronização inicial antes de assumir uma função de mestre de operações. Para saber mais sobre o requisito de sincronização inicial, confira.<br /><br />-Em seguida, DC2-Rec salva o novo valor de VMGenID em seu banco de dados e confirma todas as atualizações no contexto do novo InvocationID.<br /><br />-Como resultado da redefinição do InvocationID, DC1 converge todas as alterações do AD introduzidas pelo DC2-Rec, mesmo se ele foi revertido no tempo, que significa que as atualizações AD realizado no DC2-Rec após o failover serão convergidas com segurança|O caso de teste é o mesmo do failover planejado, mas com estas exceções:<br /><br />-Qualquer AD atualiza recebidas no DC2, mas ainda não foram replicadas pelo AD a um parceiro de replicação antes do evento de failover serão perdido.<br /><br />-Atualizações de AD recebidas no DC2 após a hora do ponto de recuperação que tiverem sido replicadas pelo AD no DC1 será replicada do DC1 para DC2-Rec.|  
  
### <a name="windows-server-2008-r2-and-earlier-versions"></a>Windows Server 2008 R2 e versões anteriores

A tabela a seguir explica o suporte a DCs virtualizados executando o Windows Server 2008 R2 e versões anteriores.  
  
|||  
|-|-|  
|Failover planejado|Failover não planejado|  
|Com suporte, mas não recomendável, porque os DCs que executam essas versões do Windows Server não dão suporte a VMGenID nem usam garantias de virtualização associadas. Isso os coloca em risco para a reversão de USN. Para obter mais informações, consulte [USN e reversão de USN](https://technet.microsoft.com/library/d2cae85b-41ac-497f-8cd1-5fbaa6740ffe(v=ws.10)).|Não tem suporte **Observação:** O failover não planejado teria suporte nos casos em que a reversão de USN não representasse um risco, como um DC individual na floresta (uma configuração que não é recomendada).|  
|Caso de teste:<br /><br />-DC1 e DC2 executam o Windows Server 2008 R2.<br /><br />-DC2 é desligado e um failover planejado é realizado no DC2-Rec. Todos os dados no DC2 são replicados para o DC2-Rec antes que o desligamento seja concluído.<br /><br />-Depois DC2-Rec for iniciado, ele retoma a replicação com DC1 usando o mesmo invocationID que DC2.|N/D|  
