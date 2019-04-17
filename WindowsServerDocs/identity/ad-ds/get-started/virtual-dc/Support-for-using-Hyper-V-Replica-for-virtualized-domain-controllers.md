---
ms.assetid: 45a65504-70b5-46ea-b2e0-db45263fabaa
title: "Suporte para o uso da réplica do Hyper-V para controladores de domínio virtualizado"
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adds
ms.openlocfilehash: 0444198196ed08a22aba92a0f59cc6e7a2518a2e
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 12/12/2017
---
# <a name="support-for-using-hyper-v-replica-for-virtualized-domain-controllers"></a>Suporte para o uso da réplica do Hyper-V para controladores de domínio virtualizado

>Aplica-se a: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Este tópico explica o suporte do uso da réplica do Hyper-V para replicar uma máquina virtual (VM) que é executado como um controlador de domínio (DC). Réplica do Hyper-V é um novo recurso do Windows Server 2012 que fornece um mecanismo de replicação interno em um nível VM a partir do Hyper-V.  
  
Hyper-V Replica assincronamente replica VMs selecionadas de um host do Hyper-V principal para um host do Hyper-V réplica em LAN ou links WAN. Após a conclusão da replicação inicial, as mudanças subsequentes feitas são duplicadas em um intervalo definido pelo administrador.  
  
Failover pode ser planejado ou não. Um failover planejado será iniciado por um administrador na VM principal, e todas as alterações não replicadas são copiadas para a VM para evitar a perda de dados da réplica. Um failover não planejado é iniciado na réplica VM em resposta a uma falha inesperada da VM principal. Perda de dados é possível porque não há nenhuma oportunidade para transmitir as alterações na VM principal que pode não ter sido replicados ainda.  
  
Para saber mais sobre réplica do Hyper-V, consulte [visão geral da réplica do Hyper-V](https://technet.microsoft.com/library/jj134172.aspx) e [implantar Hyper-V Replica](https://technet.microsoft.com/library/jj134207.aspx).  
  
> [!NOTE]  
> Hyper-V Replica pode ser executado somente no Windows Server Hyper-V, não a versão do Hyper-V que é executado no Windows 8.  
  
## <a name="windows-server-2012-domain-controllers-required"></a>Controladores de domínio do Windows Server 2012 necessárias  
Windows Server 2012 Hyper-V também introduz VM-GenerationID (VMGenID). VMGenID fornece uma maneira para que o hipervisor para se comunicar com o sistema operacional convidado quando alterações significativas ocorreram. Por exemplo, o hipervisor pode se comunicar a um controlador de domínio virtualizado que uma restauração a partir instantâneo ocorreu (Hyper-V instantâneo restauração tecnologia, não backup e restauração). AD DS no Windows Server 2012 está ciente da tecnologia de VMGenID VM e usa-o para detectar quando o hipervisor operações serão executadas, como restaurar instantâneo, que permite que ele se proteger melhor.  
  
> [!NOTE]  
> Para reforçar a questão, somente o AD DS em controladores de domínio do Windows Server 2012 fornece essas medidas de segurança resultantes da VMGenID; Controladores de domínio que executam todas as versões anteriores do Windows Server estão sujeitas a problemas como a reversão do USN que podem ocorrer quando um controlador de domínio virtualizado é restaurado usando um mecanismo sem suporte, como restaurar instantâneo. Para saber mais sobre essas proteções e quando eles são acionados, consulte [arquitetura de controlador de domínio virtualizado](https://technet.microsoft.com/library/jj574118.aspx).  
  
Quando ocorre um failover de réplica do Hyper-V (planejado ou não), o Windows Server 2012 virtualizados DC detecta uma restauração VMGenID, disparando os recursos de segurança mencionados anteriormente. Operações do Active Directory, em seguida, continuará normalmente. A réplica VM é executado no lugar a VM principal.  
  
> [!NOTE]  
> Considerando que agora há duas instâncias do mesmo identificador de controlador de domínio, é possível para a instância principal e a instância replicada para ser executado. Embora réplica do Hyper-V tem mecanismos de controle em vigor para garantir a principal e réplica VMs não são executados simultaneamente, é possível que eles sejam executados ao mesmo tempo, no caso de falha no link entre eles após replicação da VM. No caso dessa ocorrência improvável, virtualizados controladores de domínio que executam o Windows Server 2012 têm proteções para ajudar a proteger o AD DS, enquanto virtualizados controladores de domínio que executam versões anteriores do Windows Server, não.  
  
Ao usar da réplica do Hyper-V, certifique-se de que você siga as práticas recomendadas para [com controladores de domínio virtual no Hyper-V](https://technet.microsoft.com/library/virtual_active_directory_domain_controller_virtualization_hyperv(v=WS.10).aspx). Este tópico aborda, por exemplo, recomendações para armazenar arquivos do Active Directory em discos SCSI virtuais, que fornece mais forte garantia de durabilidade de dados.  
  
## <a name="supported-and-unsupported-scenarios"></a>Cenários com suporte e sem suporte  
Somente VMs que executam o Windows Server 2012 têm suporte para failover não planejado e teste de failover. Até mesmo para failover planejado, Windows Server 2012 é recomendado para o controlador de domínio virtualizado para reduzir os riscos que um administrador inadvertidamente começa a VM principal e a VM replicada ao mesmo tempo.  
  
VMs que executam versões anteriores do Windows Server são suportadas para failover planejado, mas sem suporte para failover não planejado devido a possibilidade de reversão do USN. Para saber mais sobre a reversão do USN, consulte [USN e reversão do USN](https://technet.microsoft.com/library/d2cae85b-41ac-497f-8cd1-5fbaa6740ffe(v=ws.10)).  
  
> [!NOTE]  
> Não há nenhum requisitos de nível funcionais do domínio ou floresta. Há apenas os requisitos de sistema operacional para os controladores de domínio que são executados como VMs replicados usando réplica do Hyper-V. VMs podem ser implantadas em uma floresta que contém outros controladores físicos ou virtuais que executam as versões anteriores do Windows Server e podem ou não podem também ser replicados usando réplica do Hyper-V.  
  
Esta declaração de suporte baseia-se sobre os testes que foram executados em uma única domínio-floresta, mas também há suporte para configurações de floresta de vários domínios. Para esses testes, controladores de domínio virtualizado DC1 e DC2 são parceiros de replicação do Active Directory no mesmo site hospedados em um servidor que executa o Hyper-V no Windows Server 2012. O convidado da VM que executa DC2 tem réplica do Hyper-V habilitados. O servidor de réplica é hospedado em outro datacenter geograficamente distante. Para ajudar a explicar os processos de caso de teste descritos abaixo, a VM em execução no servidor de réplica é conhecida como DC2 Rec (embora na prática mantém o mesmo nome que a VM original).  
  
### <a name="windows-server-2012"></a>Windows Server 2012  
A tabela a seguir explica o suporte para controladores de domínio virtualizados que executam o Windows Server 2012 e casos de teste.  
  
|||  
|-|-|  
|Failover planejado|Failover não planejado|  
|Com suporte|Com suporte|  
|Caso de teste:<br /><br />-DC1 e DC2 estão executando o Windows Server 2012.<br /><br />-DC2 é desligado e um failover é realizado no DC2 Rec. O failover pode ser planejado ou não.<br /><br />-Depois DC2 Rec é iniciado, ele verifica se o valor de VMGenID que possui em seu banco de dados é igual ao valor do driver máquina virtual salvo pelo servidor da réplica do Hyper-V.<br /><br />-Como resultado, DC2 Rec dispara proteções de virtualização; em outras palavras, ela redefine sua InvocationID, descarta seu pool RID e define um requisito de sincronização inicial antes de ele presumirá uma função de mestre de operações. Para obter mais informações sobre o requisito de sincronização inicial, veja.<br /><br />-Rec DC2, em seguida, salva o novo valor de VMGenID no banco de dados e confirma as atualizações posteriores no contexto de InvocationID o novo.<br /><br />-Como um resultado de redefinição InvocationID, DC1 convergirão em todas as alterações de AD apresentadas, DC2 Rec mesmo se ele foi revertido no momento, ou seja, as atualizações de anúncio realizado em DC2 Rec após o failover convergirão com segurança|O caso de teste é igual a um failover planejado, com as seguintes exceções:<br /><br />-Qualquer anúncio atualiza DC2 recebido no, mas ainda não foram replicados pelo AD para um parceiro de replicação antes do evento de failover serão perdido.<br /><br />-Atualizações anúncios recebidas no DC2 depois que o tempo do ponto de recuperação que foram replicados pelo AD para DC1 será replicado de DC1 para DC2 Rec.|  
  
### <a name="windows-server-2008-r2-and-earlier-versions"></a>Windows Server 2008 R2 e versões anteriores  
A tabela a seguir explica o suporte para controladores de domínio virtualizados que executam o Windows Server 2008 R2 e versões anteriores.  
  
|||  
|-|-|  
|Failover planejado|Failover não planejado|  
|Suporte mas não recomendado porque os controladores de domínio que executam essas versões do Windows Server não dão suporte VMGenID ou use proteções de virtualização associado. Isso coloca em risco para a reversão do USN. Para obter mais informações, consulte [USN e reversão do USN](https://technet.microsoft.com/en-us/library/d2cae85b-41ac-497f-8cd1-5fbaa6740ffe(v=ws.10)).|Não tem suporte **Observação:** failover não planejado com suporte em que a reversão do USN não é um risco, como um único controlador de domínio na floresta (uma configuração que não é recomendada).|  
|Caso de teste:<br /><br />-DC1 e DC2 estão executando o Windows Server 2008 R2.<br /><br />-DC2 é desligado e um failover planejado é realizado no DC2 Rec. Todos os dados no DC2 é replicado para DC2 Rec antes do desligamento é concluído.<br /><br />-Depois DC2 Rec é iniciado, ele retoma replicação com DC1 usando a mesma invocationID como DC2.|N/D|  
  


