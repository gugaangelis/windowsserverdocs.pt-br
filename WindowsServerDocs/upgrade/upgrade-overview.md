---
title: Visão geral sobre as atualizações do Windows Server | Microsoft Docs
description: Conheça algumas informações gerais de atualização do Windows Server, juntamente com o que pensar antes de fazer a atualização real.
ms.prod: windows server
ms.technology: server-general
ms.topic: upgrade
author: RobHindman
ms.author: robhind
ms.date: 09/10/2019
ms.openlocfilehash: 6f57e52657ca3c80c92d56c54ea87e43aabd1e99
ms.sourcegitcommit: 27f0caf74e88781054250455c3c1adf06deb6234
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/19/2019
ms.locfileid: "71124786"
---
# <a name="overview-about-windows-server-upgrades"></a>Visão geral sobre as atualizações do Windows Server

O processo de atualização para uma versão mais recente do Windows Server pode variar muito, dependendo de qual sistema operacional você está iniciando e do caminho que você tomar. Usamos os seguintes termos para distinguir entre ações diferentes, que podem estar envolvidas em uma nova implantação do Windows Server.

- **Melhora.** Também conhecido como "atualização in-loco". Você passa de uma versão mais antiga do sistema operacional para uma versão mais recente, enquanto permanece no mesmo hardware físico. **Esse é o método que abordaremos nesta seção.**

    >[!Important]
    >As atualizações in-loco também podem ser suportadas por empresas públicas ou privadas da nuvem; no entanto, você deve verificar com seu provedor de nuvem os detalhes. Além disso, você não poderá executar uma atualização in-loco em qualquer servidor Windows configurado para **inicializar do VHD**.

- **Instalação.** Também conhecido como "instalação limpa". Você passa de uma versão mais antiga do sistema operacional para uma versão mais recente, excluindo o sistema operacional mais antigo.

- **As.** Você passa de uma versão mais antiga do sistema operacional para uma versão mais recente do sistema operacional transferindo para um conjunto diferente de hardware ou máquina virtual.

- **Atualização sem interrupção do sistema operacional do cluster.** Você atualiza o sistema operacional de seus nós de cluster sem interromper o Hyper-V ou as cargas de trabalho de Servidor de Arquivos de Escalabilidade Horizontal. Esse recurso permite que você evite o tempo de inatividade que poderia afetar os Contratos de nível de serviço. Para obter mais informações, consulte [atualização sem interrupção do sistema operacional do cluster](../failover-clustering/cluster-operating-system-rolling-upgrade.md)

- **Conversão de licenças.** Converta uma edição específica da versão em outra edição da mesma versão em uma única etapa com um comando simples e a chave de licença apropriada. Chamamos essa "conversão de licença". Por exemplo, se seu servidor estiver executando o Windows Server 2016 Standard, será possível convertê-lo para o Windows Server 2016 Datacenter.

## <a name="which-version-of-windows-server-should-i-upgrade-to"></a>Qual versão do Windows Server devo atualizar?

É recomendável atualizar para a versão mais recente do Windows Server: Windows Server 2019. A execução da versão mais recente do Windows Server permite que você use os recursos mais recentes, incluindo os recursos de segurança mais recentes, e oferece o melhor desempenho.

No entanto, percebemos que nem sempre é possível. Você pode usar o diagrama a seguir para descobrir a versão do Windows Server para a qual você pode atualizar, com base na versão em que você está atualmente:

![Caminhos de atualização in-loco disponíveis](media/upgrade-paths.png)

O Windows Server pode ser normalmente atualizado por meio de pelo menos uma e, às vezes, de duas versões. Por exemplo, o Windows Server 2012 R2 e o Windows Server 2016 podem ser atualizados in-loco para o Windows Server 2019.

Você também pode atualizar de uma versão de avaliação do sistema operacional para uma versão comercial, de uma versão de varejo mais antiga para uma versão mais recente ou, em alguns casos, de uma edição licenciada por volume do sistema operacional para uma edição de varejo comum. Para obter mais informações sobre as opções de atualização diferentes da atualização in-loco, consulte [Opções de atualização e conversão para o Windows Server](../get-started/supported-upgrade-paths.md).
