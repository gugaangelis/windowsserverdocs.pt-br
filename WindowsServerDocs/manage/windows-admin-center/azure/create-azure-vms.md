---
title: Implantar máquinas virtuais do Azure usando o centro de administração do Windows
description: Implantando máquinas virtuais do Azure com o centro de administração do Windows. Configurando máquinas virtuais do Azure como parte do centro de administração do Windows – cenários gerenciados.
ms.technology: manage
ms.topic: article
author: nedpyle
ms.author: nedpyle
manager: jgerend
ms.date: 01/28/2020
ms.localizationpriority: medium
ms.prod: windows-server
ms.openlocfilehash: 2249a69f60fe87758c74a58aa13b47124da41361
ms.sourcegitcommit: da7b9bce1eba369bcd156639276f6899714e279f
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/26/2020
ms.locfileid: "80319377"
---
# <a name="deploy-azure-virtual-machines-from-within-windows-admin-center"></a>Implantar máquinas virtuais do Azure de dentro do centro de administração do Windows

>Aplica-se a: centro de administração do Windows, versão prévia do centro de administração do Windows

O centro de administração do Windows versão 1910 permite implantar máquinas virtuais do Azure. Isso integra a implantação de VM no centro de administração do Windows-cargas de trabalho gerenciadas, como [serviço de migração de armazenamento](../../../storage/storage-migration-service/overview.md) e réplica de [armazenamento](../../../storage/storage-replica/storage-replica-overview.md). Em vez de criar novos servidores e VMs no portal do Azure manualmente antes de implantar sua carga de trabalho, e possivelmente faltam as etapas necessárias e a configuração-o centro de administração do Windows pode implantar a VM do Azure, configurar seu armazenamento, associá-la ao seu domínio, instalar funções e em seguida, configure seu sistema distribuído. Você também pode implantar novas VMs do Azure sem uma carga de trabalho na página conexões do centro de administração do Windows.

O centro de administração do Windows também gerencia uma variedade de serviços do Azure. [Saiba mais sobre as opções de integração do Azure disponíveis com o centro de administração do Windows](../plan/azure-integration-options.md).

## <a name="scenarios"></a>Cenários

O centro de administração do Windows versão 1910 implantação de VM do Azure dá suporte aos seguintes cenários:

- [Serviço de migração de armazenamento](../../../storage/storage-migration-service/overview.md)
- [Réplica de armazenamento](../../../storage/storage-replica/storage-replica-overview.md)
- [Novo servidor autônomo (sem funções)](index.md#extend-on-premises-capacity-with-azure)

## <a name="requirements"></a>{1&gt;{2&gt;Requisitos&lt;2}&lt;1}

A criação de uma nova VM do Azure de dentro do centro de administração do Windows exige que você tenha:

- Uma [assinatura do Azure](https://azure.microsoft.com).
- Um [gateway do centro de administração do Windows registrado com o Azure](azure-integration.md)
- Um [grupo de recursos do Azure](https://docs.microsoft.com/azure/azure-resource-manager/management/overview) existente no qual você tem permissões de criação.
- Uma [rede virtual do Azure](https://docs.microsoft.com/azure/virtual-network/virtual-networks-overview) e uma sub-rede existentes.
- Uma solução de [rota expressa](https://azure.microsoft.com/services/expressroute/) do Azure ou de [VPN do Azure](https://azure.microsoft.com/services/vpn-gateway/) vinculada à rede virtual e à sub-rede que permite a conectividade de VMs do Azure para seus clientes locais, controladores de domínio, o computador do centro de administração do Windows e quaisquer servidores que exijam comunicação com essa VM como parte de uma implantação de carga de trabalho. Por exemplo, para usar o serviço de migração de armazenamento para migrar o armazenamento para uma VM do Azure, o computador Orchestrator e o computador de origem devem ser capazes de contatar a VM do Azure de destino para a qual você está migrando.

## <a name="usage"></a>Uso

As etapas e os assistentes de implantação de VM do Azure variam de acordo com o cenário. Examine a documentação da carga de trabalho para obter informações detalhadas sobre o cenário geral.

### <a name="deploying-azure-vms-as-part-of-storage-migration-service"></a>Implantando VMs do Azure como parte do serviço de migração de armazenamento

1. Da ferramenta de *serviço de migração de armazenamento* no centro de administração do Windows, execute um inventário de um ou mais servidores de origem.
2. Quando estiver na fase *transferir dados* , selecione **criar uma nova VM do Azure** na página *especificar um destino* e clique em **criar VM**.<br><br>
Isso inicia uma ferramenta de criação passo a passo que seleciona um Windows Server 2012 R2, Windows Server 2016 ou Windows Server 2019 VM do Azure como um destino para a migração. O serviço de migração de armazenamento fornece tamanhos de VM recomendados para corresponder à sua fonte, mas você pode substituí-los clicando em **Ver todos os tamanhos**.
<br><br>Os dados do servidor de origem também são usados para configurar automaticamente seus discos gerenciados e seus sistemas de arquivos, bem como unir sua nova VM do Azure ao seu domínio Active Directory. Se a VM for o Windows Server 2019 (que recomendamos), o centro de administração do Windows instalará o recurso de proxy de serviço de migração de armazenamento. Depois de criar a VM do Azure, o centro de administração do Windows retorna ao fluxo de trabalho de transferência do serviço de migração de armazenamento normal.  

Aqui está um vídeo mostrando como usar o serviço de migração de armazenamento para migrar para VMs do Azure.

> [!VIDEO https://www.youtube-nocookie.com/embed/k8Z9LuVL0xQ] 

### <a name="deploying-azure-vms-as-part-of-storage-replica"></a>Implantando VMs do Azure como parte da réplica de armazenamento

1. Na ferramenta *de réplica de armazenamento* no centro de administração do Windows, na guia *parceiros* , selecione **novo** e, em seguida, em *replicar com outro servidor* , selecione **usar uma nova VM do Azure** e selecione **Avançar**.
2. Especifique as informações do servidor de origem e o nome do grupo de replicação e, em seguida, selecione **Avançar**.<br><br>
Isso inicia um processo que seleciona automaticamente uma VM do Azure do Windows Server 2016 ou Windows Server 2019 como um destino para a origem de migração. O serviço de migração de armazenamento recomenda tamanhos de VM para corresponder à sua fonte, mas você pode substituir isso selecionando **Ver todos os tamanhos**. Os dados de inventário são usados para configurar automaticamente seus discos gerenciados e seus sistemas de arquivos, bem como ingressar sua nova VM do Azure em seu domínio de Active Directory. 
3. Depois que o centro de administração do Windows criar a VM do Azure, forneça um nome de grupo de replicação e, em seguida, selecione **criar**. O centro de administração do Windows começa o processo de sincronização inicial da réplica de armazenamento normal para começar a proteger seus dados.

Aqui está um vídeo mostrando como usar a réplica de armazenamento para replicar para VMs do Azure.

> [!VIDEO https://www.youtube-nocookie.com/embed/_VqD7HjTewQ] 

### <a name="deploying-a-new-standalone-azure-vm"></a>Implantando uma nova VM autônoma do Azure

1. Na página *todas as conexões* no centro de administração do Windows, selecione **Adicionar**.
2. Na seção *VM do Azure* , selecione **criar novo**.<br><br> Isso inicia uma ferramenta de criação passo a passo que permitirá que você selecione uma VM do Windows Server 2012 R2, Windows Server 2016 ou Windows Server 2019, escolha um tamanho, adicione discos gerenciados e, opcionalmente, ingresse em seu domínio de Active Directory.

Aqui está um vídeo mostrando como usar o centro de administração do Windows para criar VMs do Azure.

> [!VIDEO https://www.youtube-nocookie.com/embed/__A8J9aC_Jk] 
