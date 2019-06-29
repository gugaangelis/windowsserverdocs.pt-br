---
title: VMs blindadas para locatários – Implantando uma VM blindada usando o Windows Azure Pack
ms.custom: na
ms.prod: windows-server-threshold
ms.topic: article
ms.assetid: 095315e4-c4a7-4b80-91d8-528119b62c4c
manager: dongill
author: rpsqrd
ms.technology: security-guarded-fabric
ms.date: 08/29/2018
ms.openlocfilehash: 4cb5ed3433e04b7a5fe7004e517eb4cc38c7eb53
ms.sourcegitcommit: 63926404009f9e1330a4a0aa8cb9821a2dd7187e
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/29/2019
ms.locfileid: "67469657"
---
# <a name="shielded-vms--for-tenants---deploying-a-shielded-vm-by-using-windows-azure-pack"></a>VMs blindadas para locatários – Implantando uma VM blindada usando o Windows Azure Pack

>Aplica-se a: Windows Server (canal semestral), Windows Server 2019, Windows Server 2016

Se seu provedor de serviços de hospedagem oferece suporte a ele, você pode usar o Windows Azure Pack para implantar uma VM blindada.

Conclua as seguintes etapas:

1. Inscrever-se um ou mais planos oferecidos no Windows Azure Pack.

2. Crie uma VM blindada usando o Windows Azure Pack.

    [Máquinas virtuais blindadas de uso](https://technet.microsoft.com/library/mt720674.aspx), que é descrito nos tópicos a seguir:

   - [Criar dados de blindagem](https://technet.microsoft.com/library/mt720672.aspx) (e carregue o arquivo de dados de blindagem, conforme descrito no segundo procedimento no tópico).
    
     > [!NOTE]
     > Como parte da criação de dados de blindagem, você baixará o arquivo de chave de guardião, que será um arquivo XML em formato UTF-8. Não altere o arquivo como UTF-16.
    
   - [Criar uma máquina virtual blindada](https://technet.microsoft.com/library/mt720673.aspx) - com **criação rápida**, por meio de um modelo blindado ou por meio de um modelo regular.
    
       > [!WARNING]
       > Se você [criar uma máquina virtual blindada usando um modelo regular](https://technet.microsoft.com/library/mt720673.aspx#Anchor_2), é importante observar que a VM é provisionada *não blindado*. Isso significa que o disco de modelo não é verificado em relação à lista de discos confiáveis em seu arquivo de dados de blindagem, nem os segredos em seu arquivo de dados de blindagem que são usados para provisionar a VM. Se um modelo blindado estiver disponível, é preferível implantar uma VM blindada com um modelo blindado para fornecer proteção de ponta a ponta de seus segredos.
    
   - [Converter uma máquina virtual de geração 2 em uma máquina virtual blindada](https://technet.microsoft.com/library/mt720670.aspx)
    
       > [!NOTE]
       > Se você converter uma máquina virtual em uma máquina virtual blindada, backups e pontos de verificação existentes não são criptografados. Você deve excluir o antigos pontos de verificação sempre que possível para impedir o acesso aos seus dados antigos, descriptografados.

## <a name="see-also"></a>Consulte também

- [Etapas de configuração de provedor de serviço de hospedagem de hosts protegidos e VMs blindadas](guarded-fabric-configuration-scenarios-for-shielded-vms-overview.md)
- [Malha protegida e VMs blindadas](guarded-fabric-and-shielded-vms-top-node.md)
