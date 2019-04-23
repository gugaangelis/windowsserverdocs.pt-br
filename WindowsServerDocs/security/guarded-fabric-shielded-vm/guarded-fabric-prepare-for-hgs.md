---
title: Examine os pré-requisitos HGS
ms.custom: na
ms.prod: windows-server-threshold
ms.topic: article
ms.assetid: f4b4d1a8-bf6d-4881-9150-ddeca8b48038
manager: dongill
author: rpsqrd
ms.technology: security-guarded-fabric
ms.date: 08/29/2018
ms.openlocfilehash: dddf694aaceab93bd102456dbe86df17a001cb01
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59879877"
---
# <a name="review-prerequisites-for-the-host-guardian-service"></a>Examinar os pré-requisitos para o serviço guardião de Host

>Aplica-se a: Windows Server 2019, Windows Server (canal semestral), Windows Server 2016


Este tópico aborda os pré-requisitos HGS e as etapas iniciais para se preparar para a implantação de HGS.

## <a name="prerequisites"></a>Pré-requisitos 

-   **Hardware**: HGS pode ser executado em máquinas físicas ou virtuais, mas as máquinas físicas são recomendadas.

    Se você quiser executar o HGS como um cluster de três nós físico (para disponibilidade), você deve ter três servidores físicos. (Como uma prática recomendada para clustering, os três servidores devem ter hardware muito semelhante).
  
-   **Sistema operacional**: Atestado de chaves do host requer 2019 do Windows Server Standard ou Datacenter edition operando com [Atestado v2](guarded-fabric-tpm-trusted-attestation-capturing-hardware.md#versioned-attestation-policies). Para atestado de TPM, HGS pode executar 2019 do Windows Server ou Windows Server 2016, Standard ou Datacenter edition.

-   **Funções de servidor**: Serviço guardião de host e dar suporte a funções de servidor.

-   **Permissões de configuração/privilégios para o domínio de malha (host)**: Você precisará configurar o encaminhamento de DNS entre o domínio de malha (host) e o domínio do HGS. 
    
## <a name="upgrading-hgs"></a>Atualizando o HGS

Se você já implantou o HGS e quiser atualizar seu sistema operacional, execute as [as diretrizes de atualização](guarded-fabric-upgrade-to-2019.md) para atualizar seus servidores HGS e Hyper-V para o sistema operacional mais recente.

## <a name="next-step"></a>Próximas etapas

>[!div class="nextstepaction"]
[Obter certificados para HGS](guarded-fabric-obtain-certs.md)