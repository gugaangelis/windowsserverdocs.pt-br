---
title: Examinar os pré-requisitos do HGS
ms.topic: article
ms.assetid: f4b4d1a8-bf6d-4881-9150-ddeca8b48038
manager: dongill
author: rpsqrd
ms.author: ryanpu
ms.date: 08/29/2018
ms.openlocfilehash: a8a0f469e2fe0052a3894a2f8b59a4a3ab233a9e
ms.sourcegitcommit: dfa48f77b751dbc34409aced628eb2f17c912f08
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/07/2020
ms.locfileid: "87971293"
---
# <a name="review-prerequisites-for-the-host-guardian-service"></a>Examinar os pré-requisitos para o serviço guardião de host

>Aplica-se a: Windows Server 2019, Windows Server (canal semestral), Windows Server 2016


Este tópico aborda os pré-requisitos do HGS e as etapas iniciais para se preparar para a implantação do HGS.

## <a name="prerequisites"></a>Pré-requisitos

-   **Hardware**: o HgS pode ser executado em máquinas físicas ou virtuais, mas as máquinas físicas são recomendadas.

    Se você quiser executar o HGS como um cluster físico de três nós (para disponibilidade), deverá ter três servidores físicos. (Como prática recomendada para clustering, os três servidores devem ter hardware muito semelhante.)

-   **Sistema operacional**: o atestado de chave de host requer o Windows Server 2019 Standard ou Datacenter Edition operando com o [atestado v2](guarded-fabric-tpm-trusted-attestation-capturing-hardware.md#versioned-attestation-policies). Para atestado baseado em TPM, o HGS pode executar o Windows Server 2019 ou o Windows Server 2016, Standard ou Datacenter Edition.

-   **Funções de servidor**: serviço guardião de host e funções de servidor de suporte.

-   **Permissões/privilégios de configuração para o domínio de malha (host)**: você precisará configurar o encaminhamento de DNS entre o domínio de malha (host) e o domínio HgS.

## <a name="upgrading-hgs"></a>Atualizando HGS

Se você já tiver implantado o HGS e quiser atualizar seu sistema operacional, siga as [diretrizes de atualização](guarded-fabric-upgrade-to-2019.md) para atualizar seus servidores HgS e Hyper-V para o sistema operacional mais recente.

## <a name="next-step"></a>Próxima etapa

> [!div class="nextstepaction"]
> [Obter certificados para HGS](guarded-fabric-obtain-certs.md)