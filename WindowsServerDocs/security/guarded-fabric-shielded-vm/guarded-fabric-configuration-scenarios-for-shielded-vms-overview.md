---
title: Implantar VMs blindadas
ms.custom: na
ms.prod: windows-server
ms.topic: article
ms.assetid: 5d1a06c9-24e1-4e14-9c9a-efb2adbfeddd
manager: dongill
author: rpsqrd
ms.technology: security-guarded-fabric
ms.date: 08/29/2018
ms.openlocfilehash: f7892dabb028b99cb4cb1c9045764a8e36aba7dc
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71386771"
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
 
    - Usando **pacote do Microsoft Azure**: [Implantar uma VM blindada usando Pacote do Microsoft Azure](guarded-fabric-shielded-vm-windows-azure-pack.md)

    - Usando **Virtual Machine Manager**: [Implantar uma VM blindada usando Virtual Machine Manager](guarded-fabric-tenant-deploys-shielded-vm-using-vmm.md)

## <a name="next-step"></a>Próximas etapas

> [!div class="nextstepaction"]
> [Criar um modelo de VM blindada](guarded-fabric-create-a-shielded-vm-template.md)

## <a name="see-also"></a>Consulte também

- [Malha protegida e VMs blindadas](guarded-fabric-and-shielded-vms-top-node.md)
