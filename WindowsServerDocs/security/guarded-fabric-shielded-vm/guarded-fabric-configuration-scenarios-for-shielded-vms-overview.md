---
title: Implantar VMs blindadas
ms.topic: article
ms.assetid: 5d1a06c9-24e1-4e14-9c9a-efb2adbfeddd
manager: dongill
author: rpsqrd
ms.author: ryanpu
ms.date: 08/29/2018
ms.openlocfilehash: 0eed5d3166410cd006469eaac97ff152dfc8304e
ms.sourcegitcommit: dfa48f77b751dbc34409aced628eb2f17c912f08
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/07/2020
ms.locfileid: "87971343"
---
# <a name="deploy-shielded-vms"></a>Implantar VMs blindadas


>Aplica-se a: Windows Server 2019, Windows Server (canal semestral), Windows Server 2016

Os tópicos a seguir descrevem como um locatário pode trabalhar com VMs blindadas.

1. Adicional [Crie um disco de modelo do Windows](guarded-fabric-create-a-shielded-vm-template.md) ou [crie um disco de modelo do Linux](guarded-fabric-create-a-linux-shielded-vm-template.md). O disco de modelo pode ser criado pelo locatário ou pelo provedor de serviços de hospedagem.

2. Adicional [Converta uma VM do Windows existente em uma VM blindada](guarded-fabric-vm-shielding-helper-vhd.md).

3. [Crie dados de blindagem para definir uma VM blindada](guarded-fabric-tenant-creates-shielding-data.md).

    Para obter uma descrição e um diagrama de um arquivo de dados de blindagem, consulte [o que são os dados de blindagem e por que ele é necessário?](guarded-fabric-and-shielded-vms.md#what-is-shielding-data-and-why-is-it-necessary)

    Para obter informações sobre como criar um arquivo de resposta para incluir em um arquivo de dados blindado, consulte [VMs blindadas – gerar um arquivo de resposta usando a função New-ShieldingDataAnswerFile](guarded-fabric-sample-unattend-xml-file.md).

4. Criar uma VM blindada:

    - Usando **pacote do Microsoft Azure**: [implantar uma VM blindada usando pacote do Microsoft Azure](guarded-fabric-shielded-vm-windows-azure-pack.md)

    - Usando **Virtual Machine Manager**: [implantar uma VM blindada usando Virtual Machine Manager](guarded-fabric-tenant-deploys-shielded-vm-using-vmm.md)

## <a name="next-step"></a>Próxima etapa

> [!div class="nextstepaction"]
> [Criar um modelo de VM blindado](guarded-fabric-create-a-shielded-vm-template.md)

## <a name="additional-references"></a>Referências adicionais

- [Malha protegida e VMs blindadas](guarded-fabric-and-shielded-vms-top-node.md)
