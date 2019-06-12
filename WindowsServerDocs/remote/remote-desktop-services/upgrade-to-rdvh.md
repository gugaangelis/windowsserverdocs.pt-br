---
title: Atualizando seu Host de virtualização de área de trabalho remota para o Windows Server 2016
description: Este artigo descreve como atualizar as implantações existentes do serviços de área de trabalho remota para Windows Server 2016.
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: remote-desktop-services
ms.author: spatnaik
ms.date: 08/01/2016
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 5aed8ba7-f541-4416-b01c-4d3b1712e2b1
author: spatnaik
manager: scottman
ms.openlocfilehash: 9e624517e5e7910a32a68d1ebc38b3f8d5ab8459
ms.sourcegitcommit: d888e35f71801c1935620f38699dda11db7f7aad
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/07/2019
ms.locfileid: "66805214"
---
# <a name="upgrading-your-remote-desktop-virtualization-host-to-windows-server-2016"></a>Atualizando seu Host de virtualização de área de trabalho remota para o Windows Server 2016

>Aplica-se a: Windows Server (canal semestral), Windows Server 2016

## <a name="supported-os-upgrades-with-rds-role-installed"></a>Suporte para atualizações de sistema operacional com a função RDS instalada
Atualizações para o Windows Server 2016 têm suporte apenas no Windows Server 2012 R2 e Windows Server 2016 TP5.

## <a name="rd-virtualization-host-servers-in-the-deployment-where-vms-are-stored-locally"></a>Servidores de Host de virtualização de área de trabalho remota na implantação em que as VMs são armazenadas localmente
Esses servidores devem ser atualizados ao mesmo tempo. Siga as etapas a seguir para atualizar:

1. Faça logoff de todos os usuários.
1. Ativar ou desativar salvar todas as máquinas virtuais em cada host. 
1. Atualize os servidores para o Windows Server 2016. 
1. Todas as coleções devem estar disponível e funcional depois que as atualizações forem concluídas.      

## <a name="rd-virtualization-host-servers-in-the-deployment-where-vms-are-stored-in-cluster-shared-volumes-csv"></a>Servidores de Host de virtualização de área de trabalho remota na implantação em que as VMs são armazenadas no Cluster CSV (Volumes Compartilhados) 

1. Determine uma estratégia de atualização em que alguns dos servidores RDVH serão atualizados e alguns continuará para hospedar VMs no Windows Server 2012 R2.  
2. Isole um ou mais dos servidores RDVH, direcionados para a fase inicial de atualizando, migrando todas as máquinas virtuais para outros ' não para ser atualizado ainda' servidores RDVH que continuarão fazendo parte do cluster original do 2012 R2.
    1. Abra o Gerenciador de Cluster de Failover. 
    1. Clique em **Funções**. 
    1. Selecione uma ou mais VMs. Clique com botão direito para abrir o menu de contexto. 
    1. Clique em **mova** e escolha **Live** ou **migração rápida** para mover as VMs para um ou mais dos servidores de Host de virtualização de área de trabalho remota que não fazem parte da atualização inicial. Use **Live** ou **rápido** migração dependendo de fatores como ou requisitos on-line de compatibilidade de hardware. 
3. Remova os servidores RDVH, preparados para a atualização do cluster original. 
4. Atualize os servidores RDVH isolados. 
5. Depois que os servidores de destino RDVH foi atualizados com êxito, crie um novo cluster e CSV, que precisa estar em um volume SAN totalmente diferente.
6. Junte-se todos os servidores RDVH atualizados para o novo cluster. 
7. Crie uma estrutura de pastas no novo CSV que imita a estrutura de pasta existente no CSV existente. Isso incluirá as pastas da coleção e superior nível subpastas cada VM. 
8. De várias pastas de coleta da VM no CSV original, copie a pasta /IMGS e o conteúdo para as novas pastas da coleção nos mesmos locais no novo CSV. 
9. No computador RDVH de origem, use o Gerenciador de Cluster para remover a configuração da VM para alta disponibilidade:
    1. Inicie o Gerenciador de Cluster. 
    1. Clique em **Funções**. 
    1. Os objetos VM com o botão direito e, em seguida, clique em **remover**. 
10. Em um dos servidores não atualizados RDVH, use o Gerenciador do Hyper-V para mover todas as VMs para um dos servidores RDVH atualizados e CSV do novo Cluster:
    1. Abra o Gerenciador Hyper-V. 
    2. Selecione um dos servidores RDVH não atualizado. 
    3. Clique em um das VMs a ser movido e, em seguida, clique em **mover**. 
    4. Escolher **mover a máquina virtual**e, em seguida, clique em **próxima**. 
    5. Fornecer o destino atualizado RDVH nome do servidor sobre o **especificar computador de destino** página e, em seguida, clique em **próxima**. 
    6. Escolher **mover os dados da máquina virtual para um único local**e, em seguida, clique em **próxima**. 
    7. Navegue até o local de destino. 
       > [!IMPORTANT]
       > Verifique se que esse caminho está em uma pasta vazia para a VM específica. 

       > [!NOTE]
       > Conforme mencionado, você precisa já ter criado uma nova pasta de sub de destino antes dessa etapa. A caixa de diálogo Selecionar pasta não permitirá que você crie uma subpasta nesta etapa. 
    
       Clique em **Avançar**e em **Concluído**. 
11. Depois que as VMs são realocadas, adicioná-los como cluster **alta disponibilidade** objetos:
     1. Abra o Gerenciador de Cluster de Failover em um servidor de Host de virtualização de área de trabalho remota atualizado. 
     1. Clique com botão direito do **funções** nó e clique **configurar função**. Clique em **próxima** sobre o **iniciar** página do Assistente para alta disponibilidade. 
     1. Escolher **Máquina Virtual** na lista de funções disponíveis e clique **próxima**. Uma lista de VMs que não estão configurados será mostrada. 
     1. Selecione todas as VMs. Clique em **próxima** e, em seguida, clique em **próxima** novamente na página de confirmação para iniciar a tarefa de configuração.  
12. Depois de você ter realocado todas as máquinas virtuais, atualize os servidores RDVH restantes. Use as etapas acima para o balanceamento de locais da VM conforme apropriado.

> [!NOTE]  
> Não há suporte para servidores Hyper-V heterogêneos em um cluster. 
