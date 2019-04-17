---
title: Proteger suas máquinas de virtuais do Hyper-V com o Azure Site Recovery e o Windows Admin Center
description: Use o Windows Admin Center (Project Honolulu) para proteger as máquinas virtuais do Hyper-V com o Azure Site Recovery.
ms.technology: manage
ms.topic: article
author: haley-rowland
ms.author: harowl
ms.date: 07/17/2018
ms.localizationpriority: low
ms.prod: windows-server-threshold
ms.openlocfilehash: 8363343d3378f382a3b7f467e5df2a1948c07381
ms.sourcegitcommit: f1edfc6525e09dd116b106293f9260123a94de0c
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/12/2019
ms.locfileid: "9296854"
---
# Proteger suas máquinas de virtuais do Hyper-V com o Azure Site Recovery e o Windows Admin Center

>Aplicável a: Windows Admin Center Preview, o Windows Admin Center

[Saiba mais sobre a integração do Azure com o Windows Admin Center.](../plan/azure-integration-options.md)

O Windows Admin Center simplifica o processo de replicação de suas máquinas virtuais nos servidores do Hyper-V ou clusters, tornando mais fácil aproveitar os recursos do Windows Azure do seu próprio datacenter. Para automatizar a instalação, você pode conectar o gateway do Windows Admin Center no Azure.

Use as informações a seguir para definir configurações de replicação e criar um plano de recuperação de dentro do portal do Azure, permitindo que o Windows Admin Center inicie a replicação da VM e proteja-as.

## O que é o Azure Site Recovery e como ele funciona com o Windows Admin Center? 

O **Azure Site Recovery** é um serviço Azure que replica as cargas de trabalho em execução nas VMs para proteger a infraestrutura essencial dos negócios em caso de desastre.  [Saiba mais sobre o Azure Site Recovery](https://docs.microsoft.com/azure/site-recovery/site-recovery-overview).

O Azure Site Recovery consiste em dois componentes: **replicação** e **failover**. A parte de replicação protege suas VMs em caso de desastre, replicando o VHD da VM de destino para uma conta de armazenamento do Azure. Em seguida, você pode fazer o failover das VMs e executá-las no Windows Azure em caso de desastre. Você também pode realizar um failover de teste sem afetar suas VMs principais para testar o processo de recuperação no Windows Azure.

A conclusão da instalação para o componente de replicação é suficiente para proteger sua VM em caso de desastre. No entanto, você não conseguirá iniciar a VM no Azure até configurar a parte de failover. Você pode configurar a parte de failover, quando você deseja executar failover em uma VM do Azure - isso não é necessário como parte da instalação inicial. Se o servidor host cair e ainda não tenha configurado o componente de failover, você pode configurá-lo nesse momento e acessar as cargas de trabalho da VM protegido. No entanto, é uma boa prática para configurar o failover relacionada configurações antes de um desastre.
 

## Pré-requisitos e planejamento

- Os servidores de destino que hospedam as VMs em que você queira proteger devem ter acesso à Internet para replicar para o Azure.
- [Conectar seu gateway do Windows Admin Center para Azure](azure-integration.md).
- [Revisar a ferramenta de planejamento de capacidade para avaliar os requisitos de replicação com êxito e failover](https://docs.microsoft.com/azure/site-recovery/hyper-v-site-walkthrough-capacity).

## Etapa 1: configurar a proteção de máquina virtual no seu host de destino

> [!NOTE] 
> Você precisará executar esta etapa uma vez por servidor host ou cluster contendo VMs direcionado para proteção.

1. Navegue até o servidor ou cluster hospedar VMs que você queira proteger (tanto com o Gerenciador do servidor ou o Gerenciador de Cluster de Hyper-Converged).
2. Vá para **Máquinas virtuais** > **Inventário**.
3. Selecione qualquer VM (isso não precisa ser a VM que você queira proteger).
4. Selecione **mais** > **Configurar a proteção da VM**.
5. Logon na conta do Azure.
6. Insira as informações necessárias:

 - **Assinatura:** a assinatura do Azure que você deseja usar para a replicação de VMs nesse host.
 - **Local:** a região do Azure onde os recursos de ASR devem ser criados.
 - **Conta de armazenamento:** a conta de armazenamento onde as cargas de trabalho replicadas de VM nesse host serão salvas.
 - **Cofre:** escolha um nome para o cofre do Azure Site Recovery para VMs protegidas nesse host.

7.  Selecione a **ASR de instalação**.
8.  Aguarde até que você veja a notificação: **Configuração de recuperação de site concluída**.
 
Isso pode levar até 10 minutos. Você pode observar o andamento em **Notificações** (o ícone de sino no canto superior direito).

>[!NOTE]
> Esta etapa automaticamente instala o agente de ASR no servidor de destino ou nós (se a configuração em um cluster), cria um **Grupo de recursos** com a **Conta de armazenamento** e o **Cofre** especificado, no **Local** especificado. Isso também registrar o host de destino com o serviço de ASR e configurar uma política de replicação padrão.

## Etapa 2: Selecionar máquinas virtuais para proteger

1. Navegue de volta para o servidor ou cluster que você configurou na etapa 2 acima e vá para **Máquinas virtuais > Inventário**.
2. Selecione a máquina virtual que você queira proteger.
3. Selecione **mais** > **proteger VM**.
4. Examine os [requisitos de capacidade para proteger a VM](https://docs.microsoft.com/azure/site-recovery/site-recovery-capacity-planner).

    Se você quiser usar uma conta de armazenamento premium, [Crie um no portal do Azure](https://docs.microsoft.com/azure/storage/common/storage-premium-storage). A opção **Criar novo** fornecida no painel do Windows Admin Center cria uma conta de armazenamento padrão.

5. Insira o nome da **Conta de armazenamento** a ser usado para replicação dessa VM e selecione **Proteger VM**. Esta etapa permite que a replicação para a máquina virtual selecionada. 

6. ASR iniciará a replicação. A replicação é concluída e a VM está protegida quando o valor na coluna **Protegida** da grade de **Inventário de máquina virtual** é alterado para **Sim**. Isso pode demorar vários minutos.  

## Etapa 3: Configurar e executar um teste de failover no portal do Azure

 Embora não seja necessário concluir esta etapa ao iniciar a replicação de VM (a VM já estará protegida com apenas a replicação), é recomendável que você defina configurações de failover ao configurar o Azure Site Recovery. Caso deseje se preparar para o failover de uma VM do Azure, conclua as seguintes etapas:

1. [Configurar uma rede Azure](https://docs.microsoft.com/azure/site-recovery/hyper-v-site-walkthrough-prepare-azure) a VM de failover será anexada a este VNET. Observe que as outras etapas listadas na página vinculada são preenchidas automaticamente pelo Windows Admin Center; você deve apenas configurar a rede do Azure.

2. [Executar um teste de failover](https://docs.microsoft.com/azure/site-recovery/hyper-v-site-walkthrough-test-failover).

## Etapa 4: criar planos de recuperação

**Plano de recuperação** é um recurso do Azure Site Recovery que permite executar o failover e recuperar um aplicativo inteiro que consiste em uma coleção de máquinas virtuais. Embora seja possível recuperar VMs protegidas individualmente adicionando VMs que compõem um aplicativo a um plano de recuperação, você poderá executar o failover do aplicativo por meio do plano de recuperação. Você também pode usar o recurso de failover do Plano de recuperação para testar a recuperação do aplicativo. Plano de recuperação permite agrupar VMs, a ordem na qual devem ser colocadas na sequência de backup durante um failover e automatizar as etapas adicionais que devem ser executadas como parte do processo de recuperação. Depois de proteger suas VMs, você pode ir para o cofre do Azure Site Recovery no portal do Windows Azure e criar planos de recuperação para essas VMs. [Saiba mais sobre os planos de recuperação](https://docs.microsoft.com/azure/site-recovery/site-recovery-create-recovery-plans).

## Monitoramento de VMs replicadas no Azure ##

Para verificar se não há falhas no registro do servidor, acesse o **portal do Azure** > **Todos os recursos** > **Cofre de serviços de recuperação** (aquele que foi especificado na etapa 2) > **Trabalhos** > **Trabalhos de recuperação de sites**.

Você pode monitorar a replicação da VM indo para o **Cofre de serviços de recuperação** > **Itens replicados**.

Para ver todos os servidores que estão registrados no cofre, vá para **Cofre dos serviços de recuperação** > **Infraestrutura de recuperação de site** > **hosts Hyper-V** (sob a seção de sites do Hyper-V).

## Problema conhecido ##

Ao registrar ASR com um cluster, se um nó não conseguir instalar ASR ou registre-se ao serviço ASR, suas VMs podem não estar protegidas. Verificar se todos os nós do cluster são registrados no portal do Azure indo para **cofre de Serviços de recuperação** > **Trabalhos** > **Trabalhos de recuperação de sites**.