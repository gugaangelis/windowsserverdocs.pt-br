---
title: Implantar VMs blindadas
ms.custom: na
ms.prod: windows-server-threshold
ms.topic: article
ms.assetid: 5d1a06c9-24e1-4e14-9c9a-efb2adbfeddd
manager: dongill
author: rpsqrd
ms.technology: security-guarded-fabric
ms.date: 08/29/2018
ms.openlocfilehash: fa6da3f4a98686f83fff3937c2dc44fd4d623fe3
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59871007"
---
# <a name="deploy-shielded-vms"></a>Implantar VMs blindadas


>Aplica-se a: Windows Server 2019, Windows Server (canal semestral), Windows Server 2016

Os tópicos a seguir descrevem como um locatário pode trabalhar com VMs blindadas.

1. (Opcional) [Criar um disco de modelo do Windows](guarded-fabric-create-a-shielded-vm-template.md) ou [criar um disco de modelo Linux](guarded-fabric-create-a-linux-shielded-vm-template.md). O disco de modelo pode ser criado por locatário ou o provedor de serviços de hospedagem. 

2. (Opcional) [Converter uma VM existente do Windows em uma VM blindada](guarded-fabric-vm-shielding-helper-vhd.md). 

3. [Criar dados de blindagem para definir uma VM blindada](guarded-fabric-tenant-creates-shielding-data.md).

    Para obter uma descrição e diagrama de um arquivo de dados de blindagem, consulte [o que é de dados de blindagem e por que é necessário?](guarded-fabric-and-shielded-vms.md#what-is-shielding-data-and-why-is-it-necessary)
    
    Para obter informações sobre como criar um arquivo de resposta para incluir em um arquivo de dados blindados, consulte [VMs Blindadas - gerar um arquivo de resposta usando a função New-ShieldingDataAnswerFile](guarded-fabric-sample-unattend-xml-file.md).

4. Crie uma VM blindada:
 
    - Usando o **Windows Azure Pack**: [Implantar uma VM blindada usando o Windows Azure Pack](guarded-fabric-shielded-vm-windows-azure-pack.md)

    - Usando o **Virtual Machine Manager**: [Implantar uma VM blindada usando o Virtual Machine Manager](guarded-fabric-tenant-deploys-shielded-vm-using-vmm.md)

## <a name="next-step"></a>Próximas etapas

>[!div class="nextstepaction"]
[Criar um modelo VM blindado](guarded-fabric-create-a-shielded-vm-template.md)

## <a name="see-also"></a>Consulte também

- [Malha protegida e VMs blindadas](guarded-fabric-and-shielded-vms-top-node.md)
