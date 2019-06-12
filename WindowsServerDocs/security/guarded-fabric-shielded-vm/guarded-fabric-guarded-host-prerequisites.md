---
title: Pré-requisitos de host protegido
ms.custom: na
ms.prod: windows-server-threshold
ms.topic: article
manager: dongill
author: rpsqrd
ms.technology: security-guarded-fabric
ms.date: 08/29/2018
ms.openlocfilehash: 40c0f6df31061268b1e1ef8c15b0a02b0f50b0de
ms.sourcegitcommit: eaf071249b6eb6b1a758b38579a2d87710abfb54
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 05/31/2019
ms.locfileid: "66447469"
---
# <a name="prerequisites-for-guarded-hosts"></a>Pré-requisitos para hosts protegidos

>Aplica-se a: Windows Server 2019, Windows Server (canal semestral), Windows Server 2016

Examine os pré-requisitos de host para o modo de Atestado escolhido, clique na próxima etapa para adicionar hosts protegidos.

## <a name="tpm-trusted-attestation"></a>Atestado de TPM confiável

Hosts protegidos usando o modo TPM devem cumprir os seguintes pré-requisitos:

-   **Hardware**: Um host é necessário para a implantação inicial. Para testar a migração ao vivo do Hyper-V para as VMs blindadas, você deve ter pelo menos dois hosts.

    Hosts devem ter:
    
    - O IOMMU e o segundo endereço de nível Slat (conversão)
    - TPM 2.0
    - A UEFI 2.3.1 ou posterior
    - Configurado para inicialização usando UEFI (não o BIOS ou o modo "herdado")
    - Inicialização segura habilitada
        
-   **Sistema operacional**: Windows Server 2016 Datacenter edition ou posterior

    > [!IMPORTANT]
    > Verifique se você instalou o [atualização cumulativa mais recente](https://support.microsoft.com/help/4000825/windows-10-and-windows-server-2016-update-history).  

-   **Função e recursos**: Função Hyper-V e o recurso de suporte de Hyper-V de guardião de Host. O recurso de suporte de Hyper-V de guardião de Host só está disponível nas edições do Datacenter do Windows Server. 

> [!WARNING]
> O recurso de suporte de Hyper-V de guardião de Host permite que a proteção baseada em virtualização de integridade de código que pode ser incompatível com alguns dispositivos. É altamente recomendável testar essa configuração em seu laboratório antes de habilitar esse recurso. Deixar de fazer isso pode resultar em falhas inesperadas, inclusive perda de dados ou um erro de tela azul (também chamado de erro de parada). Para obter mais informações, consulte [hardware compatível com a proteção baseada em virtualização do Windows Server de integridade de código](guarded-fabric-compatible-hardware-with-virtualization-based-protection-of-code-integrity.md).

**Próxima etapa:** 
> [!div class="nextstepaction"]
> [Captura de informações do TPM](guarded-fabric-tpm-trusted-attestation-capturing-hardware.md)

## <a name="host-key-attestation"></a>Atestado de chaves do host

Hosts protegidos usando o atestado de chaves do host devem atender aos seguintes pré-requisitos:

- **Hardware**: Qualquer servidor capaz de executar o início do Hyper-V com o Windows Server 2019
- **Sistema operacional**: Windows Server 2019 Datacenter Edition
- **Função e recursos**: Função do Hyper-V e o recurso de suporte de Hyper-V de guardião de Host 

O host pode ser unido a um domínio ou um grupo de trabalho. 

Para atestado de chave de host, HGS deve estar executando o Windows Server 2019 e operar com o atestado de v2. Para obter mais informações, consulte [pré-requisitos HGS](guarded-fabric-prepare-for-hgs.md#prerequisites). 

**Próxima etapa:** 
> [!div class="nextstepaction"]
> [Criar um par de chaves](guarded-fabric-create-host-key.md)

## <a name="admin-trusted-attestation"></a>Atestado de Admin confiável

>[!IMPORTANT]
>Atestado de Admin confiável (modo AD) é substituído, começando com o Windows Server 2019. Para ambientes em que o atestado de TPM não for possível, configure [atestado de chaves de host](#host-key-attestation). Atestado de chaves do host fornece garantia semelhante para o modo do AD e é mais simples de configurar. 

Hosts Hyper-V devem cumprir os seguintes pré-requisitos para o modo do AD:

-   **Hardware**: Qualquer servidor capaz de executar o início do Hyper-V com o Windows Server 2016. Um host é necessário para a implantação inicial. Para testar a migração ao vivo do Hyper-V para as VMs blindadas, você precisa de pelo menos dois hosts.

-   **Sistema operacional**: Windows Server 2016 Datacenter edition

    > [!IMPORTANT]
    > Instalar o [atualização cumulativa mais recente](https://support.microsoft.com/help/4000825/windows-10-and-windows-server-2016-update-history).

-   **Função e recursos**: Função Hyper-V e o recurso de suporte de Hyper-V de guardião de Host, que só está disponível no Windows Server 2016 Datacenter edition. 

> [!WARNING]
> O recurso de suporte de Hyper-V de guardião de Host permite que a proteção baseada em virtualização de integridade de código que pode ser incompatível com alguns dispositivos. É altamente recomendável testar essa configuração em seu laboratório antes de habilitar esse recurso. Deixar de fazer isso pode resultar em falhas inesperadas, inclusive perda de dados ou um erro de tela azul (também chamado de erro de parada). Para obter mais informações, consulte [hardware compatível com a proteção baseada em virtualização do Windows Server 2016 de integridade de código](guarded-fabric-compatible-hardware-with-virtualization-based-protection-of-code-integrity.md).

**Próxima etapa:** 
> [!div class="nextstepaction"]
> [Coloque os hosts protegidos em um grupo de segurança](guarded-fabric-admin-trusted-attestation-creating-a-security-group.md)