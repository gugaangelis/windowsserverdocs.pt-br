---
title: Criar o plano de recuperação de desastre
description: Saiba como criar um plano de recuperação de desastre para a implantação do RDS.
ms.author: elizapo
ms.date: 05/05/2017
ms.topic: article
author: lizap
manager: dongill
ms.openlocfilehash: e7bf323d1a0506e9f9718d2afb8da392f0118929
ms.sourcegitcommit: dfa48f77b751dbc34409aced628eb2f17c912f08
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/07/2020
ms.locfileid: "87961730"
---
# <a name="create-your-disaster-recovery-plan-for-rds"></a>Criar o plano de recuperação de desastre para RDS

>Aplica-se a: Windows Server (Canal Semestral), Windows Server 2019, Windows Server 2016

É possível criar um plano de recuperação de desastre no Azure Site Recovery para automatizar o processo de failover. Adicione todas as VMs do componente RDS ao plano de recuperação.

Use as etapas a seguir no Azure para criar o plano de recuperação:

1. Abra o cofre de recuperação do Azure Site Recovery no portal do Azure e, em seguida, clique em **Planos de recuperação**.
2. Clique em **Criar** e insira um nome para o plano.
3. Selecione a **Origem** e o **Destino**. O destino é um site de RDS secundário ou o Azure.
4. Selecione as VMs que hospedam seus componentes RDS e, em seguida, clique em **Ok**.

As seções a seguir fornecem informações adicionais sobre a criação de planos de recuperação para os diferentes tipos de implantação de RDS.

## <a name="sessions-based-rds-deployment"></a>Implantação de RDS baseada em sessões

Para uma implantação de RDS com base em sessões, agrupe as VMs para que elas surjam em sequência:

1. Grupo de failover 1 - VM do Host da Sessão
2. Grupo de failover 2 - VM do Agente de Conexão
3. Grupo de failover 3 - VM de Acesso da Web

O plano terá a aparência a seguir:

![Um plano de recuperação de desastre para uma implantação de RDS com base em sessão](media/rds-asr-session-drplan.png)

## <a name="pooled-desktops-rds-deployment"></a>Implantação de RDS em áreas de trabalho em pool

Para uma implantação de RDS com áreas de trabalho em pool, agrupe as VMs para que elas surjam em sequência, adicionando scripts e etapas manuais.

1. Grupo de failover 1 - VM do Agente de Conexão de RDS
2. Ação manual do grupo 1 - Atualização do DNS

   Execute o PowerShell em um modo elevado na VM do Agente de Conexão. Execute o comando a seguir e aguarde alguns minutos para garantir que o DNS seja atualizado com o novo valor:

   ```
   ipconfig /registerdns
   ```
3. Script do grupo 1 - Adicionar hosts de virtualização

   Modifique o script abaixo para executar cada host de virtualização na nuvem. Normalmente, após adicionar um host de virtualização a um Agente de Conexão, será necessário reiniciar o host. Certifique-se de que o host não tem uma reinicialização pendente antes de executar o script ou, caso contrário, ele falhará.

   ```
   Broker - broker.contoso.com
   Virtualization host - VH1.contoso.com

   ipmo RemoteDesktop;
   add-rdserver –ConnectionBroker broker.contoso.com –Role RDS-VIRTUALIZATION –Server VH1.contoso.com
   ```
4. Grupo de failover 2 - VM de Modelo
5. Script 1 do grupo 2 - Desligar a VM de Modelo

   A VM de modelo será iniciada quando for recuperada para o site secundário, mas como é uma VM com sysprep, não pode iniciar completamente. Além disso, o RDS requer que a VM seja desligada para criar uma configuração de VM em pool a partir dele. Portanto, é necessário desativá-la. Se você tiver um único servidor VMM, o nome da VM de modelo será o mesmo no primário e secundário. Por isso, usamos a ID da VM conforme especificado pelo *Contexto* variável no script a seguir. Se você tiver vários modelos, desative todos.

   ```powershell
   ipmo virtualmachinemanager;
   Foreach($vm in $VMsAsTemplate)
   {
      Get-SCVirtualMachine -ID $vm | Stop-SCVirtualMachine –Force
   }
   ```
6. Script 2 do grupo 2 - Remover VMs em pool existentes

   Você precisará remover as VMs em pool no site primário do Agente de Conexão para que novas VMs possam ser criadas no site secundário. Nesse caso, será necessário especificar o host exato na qual criar a VM em pool. Observe que isso excluirá as VMs de apenas a coleção.

   ```powershell
   ipmo RemoteDesktop
   $desktops = Get-RDVirtualDesktop -CollectionName Win8Desktops;
   Foreach($vm in $desktops){
      Remove-RDVirtualDesktopFromCollection -CollectionName Win8Desktops -VirtualDesktopName $vm.VirtualDesktopName –Force
   }
   ```
7. Ação manual do grupo 2 - Atribuir novo modelo

   Será necessário atribuir o novo modelo ao Agente de Conexão para a coleção de modo a possibilitar a criação de VMs em pool no site de recuperação. Vá para o agente de Conexão de RDS e identifique a coleção. Edite as propriedades e especifique uma nova imagem de VM como o modelo dela.
8. Script 3 do grupo 2 - Recriar todas as VMs em pool

   Recrie as VMs em pool no site de recuperação por meio do Agente de Conexão. Nesse caso, será necessário especificar o host exato na qual criar a VM em pool.

   O nome da VM em pool deve ser exclusivo, usando o prefixo e o sufixo. Se o nome da VM já existir, o script falhará. Além disso, se no lado primário as VMs são numeradas de 1 a 5, a numeração do site de recuperação continuará de 6.

   ```powershell
   ipmo RemoteDesktop;
   Add-RDVirtualDesktopToCollection -CollectionName Win8Desktops -VirtualDesktopAllocation @{"RDVH1.contoso.com" = 1}
   ```
9. Grupo de failover 3 - Acesso via Web e VM do servidor de gateway

O plano de recuperação terá esta aparência:

![Um plano de recuperação de desastre para uma implantação de RDS com áreas de trabalho em pool](media/rds-asr-pooled-drplan.png)

## <a name="personal-desktops-rds-deployment"></a>Implantação de RDS em áreas de trabalho pessoais

Para uma implantação de RDS com áreas de trabalho pessoais, agrupe as VMs para que elas surjam em sequência, adicionando scripts e etapas manuais.

1. Grupo de failover 1 - VM do Agente de Conexão de RDS
2. Ação manual do grupo 1 - Atualização do DNS

   Execute o PowerShell em um modo elevado na VM do Agente de Conexão. Execute o comando a seguir e aguarde alguns minutos para garantir que o DNS seja atualizado com o novo valor:

   ```
   ipconfig /registerdns
   ```
3. Script do grupo 1 - Adicionar hosts de virtualização

   Modifique o script abaixo para executar cada host de virtualização na nuvem. Normalmente, após adicionar um host de virtualização a um Agente de Conexão, será necessário reiniciar o host. Certifique-se de que o host não tem uma reinicialização pendente antes de executar o script ou, caso contrário, ele falhará.

   ```powershell
   Broker - broker.contoso.com
   Virtualization host - VH1.contoso.com

   ipmo RemoteDesktop;
   add-rdserver –ConnectionBroker broker.contoso.com –Role RDS-VIRTUALIZATION –Server VH1.contoso.com
   ```
4. Grupo de failover 2 - VM de Modelo
5. Script 1 do grupo 2 - Desligar a VM de Modelo

   A VM de modelo será iniciada quando for recuperada para o site secundário, mas como é uma VM com sysprep, não pode iniciar completamente. Além disso, o RDS requer que a VM seja desligada para criar uma configuração de VM em pool a partir dele. Portanto, é necessário desativá-la. Se você tiver um único servidor VMM, o nome da VM de modelo será o mesmo no primário e secundário. Por isso, usamos a ID da VM conforme especificado pelo *Contexto* variável no script a seguir. Se você tiver vários modelos, desative todos.

   ```powershell
   ipmo virtualmachinemanager;
   Foreach($vm in $VMsAsTemplate)
   {
      Get-SCVirtualMachine -ID $vm | Stop-SCVirtualMachine –Force
   }
   ```
6. Grupo de failover 3 - VMs pessoais
7. Script 1 do grupo 3 - Remover VMs pessoais existentes e adicioná-las

   Remova as VMs pessoais no site primário do Agente de Conexão para que novas VMs possam ser criadas no site secundário. É necessário extrair as atribuições das VMs e adicionar novamente as máquinas virtuais ao Agente de Conexão com o hash de atribuições. Isso apenas removerá as VMs pessoais da coleção e as adicionará novamente. A alocação da área de trabalho pessoal será exportada e importada novamente para a coleção.

   ```powershell
   ipmo RemoteDesktop
   $desktops = Get-RDVirtualDesktop -CollectionName CEODesktops;
   Export-RDPersonalVirtualDesktopAssignment -CollectionName CEODesktops -Path ./Desktopallocations.txt -ConnectionBroker broker.contoso.com

   Foreach($vm in $desktops){
     Remove-RDVirtualDesktopFromCollection -CollectionName CEODesktops -VirtualDesktopName $vm.VirtualDesktopName –Force
   }

   Import-RDPersonalVirtualDesktopAssignment -CollectionName CEODesktops -Path ./Desktopallocations.txt -ConnectionBroker broker.contoso.com
   ```
8. Grupo de failover 3 - Acesso via Web e VM do servidor de gateway

O plano terá a aparência a seguir:

![Um plano de recuperação de desastre para uma implantação de RDS em áreas de trabalho pessoais](media/rds-asr-personal-desktops-drplan.png)
