---
title: Visão geral sobre atualizações do Windows Server | Microsoft Docs
description: Conheça algumas informações gerais sobre a atualização do Windows Server, juntamente com pontos a considerar antes de fazer a atualização de fato.
ms.prod: windows server
ms.technology: server-general
ms.topic: upgrade
author: RobHindman
ms.author: robhind
ms.date: 09/10/2019
ms.openlocfilehash: 6f57e52657ca3c80c92d56c54ea87e43aabd1e99
ms.sourcegitcommit: 27f0caf74e88781054250455c3c1adf06deb6234
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/19/2019
ms.locfileid: "71124786"
---
# <a name="overview-about-windows-server-upgrades"></a>Visão geral sobre as atualizações do Windows Server

O processo de atualizar para uma versão mais nova do Windows Server pode variar muito, dependendo do sistema operacional com o qual você está começando e do caminho que você segue. Usamos os termos a seguir para distinguir diferentes ações, qualquer uma delas pode estar envolvida em uma nova implantação do Windows Server.

- **Atualização.** Também conhecida como "atualização in-loco". Você passa de uma versão mais antiga do sistema operacional para uma versão mais recente permanecendo no mesmo hardware físico. **Esse é o método que abordaremos nesta seção.**

    >[!Important]
    >As atualizações in-loco também podem ser compatíveis com empresas de nuvem pública ou privada, porém, você deve consultar os detalhes com seu provedor de nuvem. Além disso, você não poderá executar uma atualização in-loco em nenhum Windows Server configurado para **Inicialização do VHD**.

- **Instalação.** Também conhecida como "instalação limpa". Você passa de uma versão mais antiga do sistema operacional para uma versão mais recente excluindo o sistema operacional mais antigo.

- **Migração.** Você passa de uma versão mais antiga do sistema operacional para uma versão mais recente do sistema operacional transferindo para um conjunto diferente de hardware ou máquina virtual.

- **Atualização sem interrupção do SO do cluster.** Você atualiza o sistema operacional de seus nós de cluster sem interromper o Hyper-V nem as cargas de trabalho do Servidor de Arquivos de Escalabilidade Horizontal. Esse recurso permite que você evite o tempo de inatividade que poderia afetar os Contratos de nível de serviço. Para saber mais, confira [Atualização sem interrupção do sistema operacional do cluster](../failover-clustering/cluster-operating-system-rolling-upgrade.md)

- **Conversão de licença.** Converta uma edição específica da versão para outra edição da mesma versão em uma só etapa, usando um simples comando e com a chave de licença adequada. Chamamos isso de "conversão da licença". Por exemplo, se o servidor estiver executando o Windows Server 2016 Standard, é possível convertê-lo para o Windows Server 2016 Datacenter.

## <a name="which-version-of-windows-server-should-i-upgrade-to"></a>Para qual versão do Windows Server devo atualizar?

É recomendável atualizar para a versão mais recente do Windows Server: Windows Server 2019. Executar a versão mais nova do Windows Server permite que você use os recursos mais recentes, incluindo os de segurança, além de oferecer o melhor desempenho.

No entanto, sabemos que nem sempre isso é possível. Você pode usar o seguinte diagrama para descobrir a versão do Windows Server para a qual você pode atualizar conforme a versão em que você está atualmente:

![Caminhos de atualização in-loco disponíveis](media/upgrade-paths.png)

O Windows Server normalmente pode ser atualizado por pelo menos uma e, às vezes, duas versões. Por exemplo, o Windows Server 2012 R2 e o Windows Server 2016 podem ser atualizados in-loco para o Windows Server 2019.

Também é possível atualizar de uma versão de avaliação do sistema operacional para uma versão comercial, de uma versão comercial mais antiga para uma versão mais nova ou, em alguns casos, de uma edição com licença de volume do sistema operacional para uma edição comercial comum. Para obter mais informações sobre outras opções de atualização além da atualização in-loco, confira [Opções de atualização e conversão para o Windows Server](../get-started/supported-upgrade-paths.md).
