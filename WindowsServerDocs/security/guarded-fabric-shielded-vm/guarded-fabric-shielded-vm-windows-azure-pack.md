---
title: VMs blindadas para locatários-implantando uma VM blindada usando Pacote do Microsoft Azure
ms.custom: na
ms.prod: windows-server
ms.topic: article
ms.assetid: 095315e4-c4a7-4b80-91d8-528119b62c4c
manager: dongill
author: rpsqrd
ms.technology: security-guarded-fabric
ms.date: 08/29/2018
ms.openlocfilehash: ec9f12990e7e16aebb208edfe0d97d6671623da1
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71403538"
---
# <a name="shielded-vms--for-tenants---deploying-a-shielded-vm-by-using-windows-azure-pack"></a>VMs blindadas para locatários-implantando uma VM blindada usando Pacote do Microsoft Azure

>Aplica-se a: Windows Server (Canal Semestral), Windows Server 2019, Windows Server 2016

Se o seu provedor de serviços de hospedagem oferecer suporte a ele, você poderá usar Pacote do Microsoft Azure para implantar uma VM blindada.

Conclua as seguintes etapas:

1. Assine um ou mais planos oferecidos no Pacote do Microsoft Azure.

2. Crie uma VM blindada usando Pacote do Microsoft Azure.

    [Use máquinas virtuais blindadas](https://technet.microsoft.com/library/mt720674.aspx), que é descrita nos tópicos a seguir:

   - [Crie dados de blindagem](https://technet.microsoft.com/library/mt720672.aspx) (e carregue o arquivo de dados de blindagem, conforme descrito no segundo procedimento no tópico).
    
     > [!NOTE]
     > Como parte da criação de dados de blindagem, você baixará o arquivo de chave do guardião, que será um arquivo XML no formato UTF-8. Não altere o arquivo para UTF-16.
    
   - [Crie uma máquina virtual blindada](https://technet.microsoft.com/library/mt720673.aspx) -com **criação rápida**, por meio de um modelo blindado ou por meio de um modelo comum.
    
       > [!WARNING]
       > Se você [criar uma máquina virtual blindada usando um modelo regular](https://technet.microsoft.com/library/mt720673.aspx#Anchor_2), é importante observar que a VM é provisionada sem *blindagem*. Isso significa que o disco de modelo não é verificado em relação à lista de discos confiáveis no arquivo de dados de blindagem, nem aos segredos no arquivo de dados de blindagem usado para provisionar a VM. Se um modelo blindado estiver disponível, é preferível implantar uma VM blindada com um modelo blindado para fornecer proteção de ponta a ponta dos seus segredos.
    
   - [Converter uma máquina virtual de geração 2 em uma máquina virtual blindada](https://technet.microsoft.com/library/mt720670.aspx)
    
       > [!NOTE]
       > Se você converter uma máquina virtual em uma máquina virtual blindada, os pontos de verificação e os backups existentes não serão criptografados. Você deve excluir pontos de verificação antigos quando possível para impedir o acesso aos dados antigos descriptografados.

## <a name="see-also"></a>Consulte também

- [Etapas de configuração do provedor de serviços de hospedagem para hosts protegidos e VMs blindadas](guarded-fabric-configuration-scenarios-for-shielded-vms-overview.md)
- [Malha protegida e VMs blindadas](guarded-fabric-and-shielded-vms-top-node.md)
