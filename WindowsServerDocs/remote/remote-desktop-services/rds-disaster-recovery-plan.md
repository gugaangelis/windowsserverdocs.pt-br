---
title: Criar seu plano de recuperação de desastres
description: Saiba como criar um plano de recuperação de desastre para sua implantação do RDS.
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: remote-desktop-services
ms.author: elizapo
ms.date: 05/05/2017
ms.tgt_pltfrm: na
ms.topic: article
author: lizap
manager: dongill
ms.openlocfilehash: 8ad759a73e4a0ce1dc28f2b8e8d80f4365895430
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59879497"
---
# <a name="create-your-disaster-recovery-plan-for-rds"></a>Criar seu plano de recuperação de desastre para RDS

>Aplica-se a: Windows Server (canal semestral), Windows Server 2016

Você pode criar um plano de recuperação de desastres no Azure Site Recovery para automatizar o processo de failover. Adicione todas as VMs do componente RDS para o plano de recuperação.

Use as etapas a seguir no Azure para criar seu plano de recuperação:

1. Abra o cofre do Azure Site Recovery no portal do Azure e, em seguida, clique em **planos de recuperação**.
2. Clique em **criar** e insira um nome para o plano.
3. Selecione suas **fonte** e **destino**. O destino é um secundário RDS site ou Azure.
4. Selecione as VMs que hospedam seus componentes RDS e, em seguida, clique em **Okey**.

As seções a seguir fornecem informações adicionais sobre a criação de planos de recuperação para os diferentes tipos de implantação do RDS.

## <a name="sessions-based-rds-deployment"></a>Implantação baseada em sessões RDS

Para uma implantação com base em sessões RDS, agrupe as VMs para que elas surgem em sequência:

1. Grupo de failover 1 - VM de Host de sessão
2. Grupo de failover 2 - VM do agente de Conexão
3. Grupo de failover 3: VM de acesso da Web

Seu plano será algo parecido com isso: 

![Um plano de recuperação de desastre para uma implantação de RDS com base em sessão](media/rds-asr-session-drplan.png)

## <a name="pooled-desktops-rds-deployment"></a>Implantação de RDS de áreas de trabalho

Para uma implantação do RDS com áreas de trabalho, agrupe as VMs para que elas surgem em sequência, adicionando scripts e etapas manuais.

1. Grupo de failover 1 - VM do agente de Conexão de RDS
2. Grupo 1 ação manual - atualização de DNS

   Execute o PowerShell no modo elevado na VM do agente de Conexão. Execute o seguinte comando e aguarde alguns minutos para garantir que o DNS é atualizado com o novo valor:

   ```
   ipconfig /registerdns
   ```
3. Grupo de 1 script - adicionar hosts de virtualização

   Modifique o script seja executada para cada host de virtualização na nuvem. Normalmente depois de adicionar um host de virtualização a um agente de Conexão, você precisará reiniciar o host. Certifique-se de que o host não tem uma reinicialização pendente antes de executar o script, caso contrário, ele falhará.

   ```
   Broker - broker.contoso.com
   Virtualization host - VH1.contoso.com

   ipmo RemoteDesktop; 
   add-rdserver –ConnectionBroker broker.contoso.com –Role RDS-VIRTUALIZATION –Server VH1.contoso.com 
   ```
4. Grupo de failover 2: modelo de VM
5. Script do grupo 2 1 - ativar fora do modelo de VM
   
   O modelo de VM quando recuperado para o site secundário será iniciado, mas é um VM com Sysprep e não é possível iniciar completamente. Também RDS requer que a VM seja desligada para criar uma configuração de VM em pool a partir dele. Portanto, é necessário desativá-lo. Se você tiver um único servidor VMM, o nome da VM de modelo é o mesmo no primário e secundário. Por isso, usamos a ID da VM conforme especificado pelo *contexto* variável no script a seguir. Se você tiver vários modelos, desativá-los todos.

   ```powershell
   ipmo virtualmachinemanager; 
   Foreach($vm in $VMsAsTemplate)
   {
      Get-SCVirtualMachine -ID $vm | Stop-SCVirtualMachine –Force
   } 
   ```
6. Grupo 2 script 2 - Remover VMs em pool existentes

   Você precisará remover as VMs em pool no site primário do agente de Conexão para que novas VMs podem ser criadas no site secundário. Nesse caso, você precisará especificar o host exato na qual criar a VM em pool. Observe que isso excluirá as VMs de apenas a coleção.

   ```powershell
   ipmo RemoteDesktop
   $desktops = Get-RDVirtualDesktop -CollectionName Win8Desktops; 
   Foreach($vm in $desktops){
      Remove-RDVirtualDesktopFromCollection -CollectionName Win8Desktops -VirtualDesktopName $vm.VirtualDesktopName –Force
   }
   ```
7. Ação manual do grupo 2 - atribuir novo modelo

   Você precisará atribuir o novo modelo para o agente de Conexão para a coleção para que você possa criar novas VMs em pool no site de recuperação. Vá para o agente de Conexão de RDS e identificar a coleção. Editar as propriedades e especifique uma nova imagem VM como seu modelo.
8. Grupo 2 script 3 - recriar todas as áreas de VMs

   Recrie as VMs em pool no site de recuperação por meio do agente de Conexão. Nesse caso, você precisará especificar o host exato na qual criar a VM em pool.

   O nome da VM em pool precisa ser exclusivo, usando o prefixo e sufixo. Se o nome da VM já existir, o script falhará. Além disso, se o lado primário as VMs são numeradas de 1 a 5, a numeração de site de recuperação continuará de 6.

   ```powershell
   ipmo RemoteDesktop; 
   Add-RDVirtualDesktopToCollection -CollectionName Win8Desktops -VirtualDesktopAllocation @{"RDVH1.contoso.com" = 1} 
   ```
9. Grupo de failover 3 – acesso via Web e a VM do servidor de Gateway

O plano de recuperação terá esta aparência:

![Um plano de recuperação de desastre para uma implantação do RDS com áreas de trabalho](media/rds-asr-pooled-drplan.png)

## <a name="personal-desktops-rds-deployment"></a>Implantação de RDS de áreas de trabalho pessoais

Para uma implantação do RDS com áreas de trabalho pessoais, agrupe as VMs para que elas surgem em sequência, adicionando scripts e etapas manuais.

1. Grupo de failover 1 - VM do agente de Conexão de RDS
2. Grupo 1 ação manual - atualização de DNS

   Execute o PowerShell no modo elevado na VM do agente de Conexão. Execute o seguinte comando e aguarde alguns minutos para garantir que o DNS é atualizado com o novo valor:

   ```
   ipconfig /registerdns
   ```
3. Grupo de 1 script - adicionar hosts de virtualização
      
   Modifique o script seja executada para cada host de virtualização na nuvem. Normalmente depois de adicionar um host de virtualização a um agente de Conexão, você precisará reiniciar o host. Certifique-se de que o host não tem uma reinicialização pendente antes de executar o script, caso contrário, ele falhará.

   ```powershell
   Broker - broker.contoso.com
   Virtualization host - VH1.contoso.com

   ipmo RemoteDesktop; 
   add-rdserver –ConnectionBroker broker.contoso.com –Role RDS-VIRTUALIZATION –Server VH1.contoso.com 
   ```
4. Grupo de failover 2: modelo de VM
5. Script do grupo 2 1 - ativar fora do modelo de VM
   
   O modelo de VM quando recuperado para o site secundário será iniciado, mas é um VM com Sysprep e não é possível iniciar completamente. Também RDS requer que a VM seja desligada para criar uma configuração de VM em pool a partir dele. Portanto, é necessário desativá-lo. Se você tiver um único servidor VMM, o nome da VM de modelo é o mesmo no primário e secundário. Por isso, usamos a ID da VM conforme especificado pelo *contexto* variável no script a seguir. Se você tiver vários modelos, desativá-los todos.

   ```powershell
   ipmo virtualmachinemanager; 
   Foreach($vm in $VMsAsTemplate)
   {
      Get-SCVirtualMachine -ID $vm | Stop-SCVirtualMachine –Force
   } 
   ```
6. Grupo de failover 3 - VMs pessoais
7. Grupo 3 script 1 – Remover VMs pessoais existentes e adicioná-los

   Remova as VMs pessoais no site primário do agente de Conexão para que novas VMs podem ser criadas no site secundário. Você precisa extrair as atribuições das máquinas virtuais e adicionar novamente as máquinas virtuais para o agente de Conexão com o hash de atribuições. Isso apenas removerá as VMs pessoais da coleção e adicioná-los novamente. A alocação da área de trabalho pessoal será exportada e importada novamente para a coleção.

   ```powershell
   ipmo RemoteDesktop
   $desktops = Get-RDVirtualDesktop -CollectionName CEODesktops; 
   Export-RDPersonalVirtualDesktopAssignment -CollectionName CEODesktops -Path ./Desktopallocations.txt -ConnectionBroker broker.contoso.com 

   Foreach($vm in $desktops){
     Remove-RDVirtualDesktopFromCollection -CollectionName CEODesktops -VirtualDesktopName $vm.VirtualDesktopName –Force
   }
   
   Import-RDPersonalVirtualDesktopAssignment -CollectionName CEODesktops -Path ./Desktopallocations.txt -ConnectionBroker broker.contoso.com 
   ```
8. Grupo de failover 3 – acesso via Web e a VM do servidor de Gateway

Seu plano será algo parecido com isso: 

![Um plano de recuperação de desastre para uma implantação de RDS de áreas de trabalho pessoais](media/rds-asr-personal-desktops-drplan.png)
