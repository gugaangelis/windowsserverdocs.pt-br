---
title: Atualização das suas implantações de Host de Área de Trabalho Remota para o Windows Server 2016
description: Este artigo descreve como atualizar as implantações existentes de Serviços de Área de Trabalho Remota para o Windows Server 2016.
ms.author: spatnaik
ms.date: 08/01/2016
ms.topic: article
ms.assetid: 5aed8ba7-f541-4416-b01c-4d3b1712e2b1
author: spatnaik
manager: scottman
ms.openlocfilehash: 4260748ada0371e637edef23a579e7253721f4f4
ms.sourcegitcommit: dfa48f77b751dbc34409aced628eb2f17c912f08
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/07/2020
ms.locfileid: "87948819"
---
# <a name="upgrading-your-remote-desktop-virtualization-host-to-windows-server-2016"></a>Atualização das suas implantações de Host de Área de Trabalho Remota para o Windows Server 2016

>Aplica-se a: Windows Server (Canal Semestral), Windows Server 2016

## <a name="supported-os-upgrades-with-rds-role-installed"></a>Atualizações de sistema operacional compatíveis com a função RDS instalada
Os upgrades para o Windows Server 2016 são compatíveis somente a partir do Windows Server 2012 R2 e Windows Server 2016 TP5.

## <a name="rd-virtualization-host-servers-in-the-deployment-where-vms-are-stored-locally"></a>Servidores de Host de Virtualização de Área de Trabalho Remota na implantação em que as VMs são armazenadas localmente
Esses servidores devem ser atualizados ao mesmo tempo. Siga as etapas a seguir para atualizar:

1. Faça logoff de todos os usuários.
1. Desative ou salve todas as máquinas virtuais em cada host.
1. Atualize os servidores para o Windows Server 2016.
1. Todas as coleções devem estar disponíveis e funcionando depois que as atualizações forem concluídas.

## <a name="rd-virtualization-host-servers-in-the-deployment-where-vms-are-stored-in-cluster-shared-volumes-csv"></a>Servidores de Host de Virtualização de Área de Trabalho Remota na implantação em que as VMs são armazenadas nos Volumes Compartilhados de Cluster (CSV)

1. Determine uma estratégia de atualização em que alguns dos servidores RDVH serão atualizados e alguns continuarão a hospedar VMs no Windows Server 2012 R2.
2. Isole um ou mais servidores RDVH direcionados para a fase inicial de atualização migrando todas as VMs para outros servidores RDVH "que ainda não serão atualizados" que continuarão fazendo parte do cluster original do 2012 R2.
    1. Abra o Gerenciador de Cluster de Failover.
    1. Clique em **Funções**.
    1. Selecione uma ou mais VMs. Clique com botão direito do mouse para abrir o menu de contexto.
    1. Clique em **Mover** e escolha **Ao Vivo** ou **Migração Rápida** para mover as VMs para um ou mais dos Servidores de Host de Virtualização de Área de Trabalho Remota que não fazem parte da atualização inicial. Use **Live** ou **Migração** Rápida dependendo de fatores como compatibilidade de hardware ou requisitos online.
3. Remova os servidores RDVH preparados para a atualização do cluster original.
4. Atualize os servidores RDVH isolados.
5. Depois que os servidores de destino RDVH forem atualizados com êxito, crie um novo cluster e CSV que precisa estar em um volume SAN totalmente diferente.
6. Junte-se a todos os servidores RDVH atualizados para o novo cluster.
7. Crie uma estrutura de pastas no novo CSV que imita a estrutura de pasta existente no CSV atual. Isso incluirá as pastas de coleta e as subpastas de nível superior de cada VM.
8. Nas várias pastas de Coleta da VM no CSV original, copie a pasta /IMGS e o conteúdo dela para as novas pastas da coleção nos mesmos locais no novo CSV.
9. No computador RDVH de origem, use o Gerenciador de Cluster para remover a configuração da VM para alta disponibilidade:
    1. Iniciar o Gerenciador de Cluster.
    1. Clique em **Funções**.
    1. Clique com o botão direito do mouse nos objetos de VM e em **Remover**.
10. Em um dos servidores RDVH não atualizados, use o Gerenciador do Hyper-V para mover todas as VMs para um dos servidores RDVH atualizados e o novo Cluster CSV:
    1. Abra o Gerenciador do Hyper-V.
    2. Selecione um dos servidores RDVH não atualizados.
    3. Clique com o botão direito do mouse em uma das VMs a ser movida e clique em **Mover**.
    4. Escolha **Mover a máquina virtual** e clique em **Avançar**.
    5. Forneça o nome do servidor RDVH atualizado direcionado na página **Especificar Computador de Destino** e clique em **Avançar**.
    6. Escolha **Mover os dados da máquina virtual para um único local** e clique em **Avançar**.
    7. Navegue até o local de destino.
       > [!IMPORTANT]
       > Verifique se esse caminho é para uma pasta vazia da VM específica.

       > [!NOTE]
       > Conforme mencionado, você precisa já ter criado uma nova subpasta de destino antes dessa etapa. A caixa de diálogo Selecionar Pasta não permitirá que você crie uma subpasta nesta etapa.

       Clique em **Avançar**e em **Concluído**.
11. Depois que as VMs forem realocadas, adicione-as como objetos de cluster de **Alta Disponibilidade**:
     1. Abra o Gerenciador de Cluster de Failover em um servidor de Host de Virtualização de Área de Trabalho Remota atualizado.
     1. Clique com o botão direito do mouse no nó **Funções** e clique em **Função Configurar**. Clique em **Avançar** na página **Inicial** do Assistente de Alta Disponibilidade.
     1. Escolha **Máquina Virtual** na lista de funções disponíveis e clique em **Próxima**. Uma lista de VMs que não estão configurados será mostrada.
     1. Selecione todas as VMs. Clique em **Próxima** e clique em **Próxima** novamente na página de confirmação para iniciar a tarefa de configuração.
12. Depois de realocar todas as máquinas virtuais, atualize os servidores RDVH restantes. Use as etapas acima para balancear os locais da VM conforme apropriado.

> [!NOTE]
> Não há suporte para servidores Hyper-V heterogêneos em um cluster.
