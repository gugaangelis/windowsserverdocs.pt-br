---
title: Malha protegida e VMs blindadas
ms.custom: na
ms.prod: windows-server-threshold
ms.topic: article
ms.assetid: 5c7ada81-2d97-41d4-87cf-1a7ccf06cd20
manager: dongill
author: rpsqrd
ms.author: justinha
ms.technology: security-guarded-fabric
ms.date: 08/29/2018
ms.openlocfilehash: c2c16574439569eeb1181977f23bae238f43865c
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59857427"
---
# <a name="guarded-fabric-and-shielded-vms"></a>Malha protegida e VMs blindadas

>Aplica-se a: Windows Server 2019, Windows Server (canal semestral), Windows Server 2016

Um dos objetivos mais importantes de fornecer um ambiente hospedado é garantir a segurança das máquinas virtuais em execução no ambiente. Como um provedor de serviços em nuvem ou um administrador de nuvem privada corporativa, você pode usar uma malha protegida para fornecer um ambiente mais seguro para as VMs. Uma malha protegida consiste em um Serviço de Guardião de Host (HGS), em geral, um cluster de três nós, além de um ou mais hosts protegidos e um conjunto de máquinas virtuais protegidas (VMs).

> [!IMPORTANT]
> Certifique-se de que você tenha instalado a atualização cumulativa mais recente antes de implantar máquinas virtuais blindadas na produção.

## <a name="videos-blog-and-overview-topic-about-guarded-fabrics-and-shielded-vms"></a>Tópico de visão geral, blog e vídeos sobre protegida malhas e VMs blindadas

- Vídeo: [Como proteger sua malha de virtualização de ameaças internas com o Windows Server 2019](https://myignite.techcommunity.microsoft.com/sessions/64690)
- Vídeo: [Introdução às máquinas virtuais Blindadas no Windows Server 2016](https://channel9.msdn.com/Shows/Mechanics/Introduction-to-Shielded-Virtual-Machines-in-Windows-Server-2016)
- Vídeo: [Informações detalhadas sobre as VMs Blindadas com o Windows Server 2016 Hyper-V](https://channel9.msdn.com/events/Ignite/2016/BRK3124)
- Vídeo: [Implantar VMs Blindadas e uma malha protegida com o Windows Server 2016](https://mva.microsoft.com/en-US/training-courses/deploying-shielded-vms-and-a-guarded-fabric-with-windows-server-2016-17131?l=WFLef7vUD_4604300474)
- Blog: [Blog de segurança de nuvem privada e Datacenter](https://blogs.technet.microsoft.com/datacentersecurity/)
- Visão geral: [Visão de geral de VMs blindada e malha protegida](Guarded-Fabric-and-Shielded-VMs.md)

## <a name="planning-topics"></a>Tópicos de planejamento

- [Guia de planejamento para hosters](guarded-fabric-planning-for-hosters.md)
- [Guia de planejamento para locatários](guarded-fabric-shielded-vm-planning-for-tenants.md)

## <a name="deployment-topics"></a>Tópicos de implantação

- [Guia de implantação](guarded-fabric-deploying-hgs-overview.md)
    - [Guia de início rápido](guarded-fabric-deployment-overview.md)
    - [Implantar o HGS](guarded-fabric-setting-up-the-host-guardian-service-hgs.md)
    - [Implantar hosts protegidos](guarded-fabric-configure-hgs-with-authorized-hyper-v-hosts.md)
        - [Configurando a malha DNS para hosts que se tornarão hosts protegidos](guarded-fabric-configuring-fabric-dns.md)
        - [Implantar um host protegido usando o modo de AD](guarded-fabric-admin-trusted-attestation-creating-a-security-group.md)
        - [Implantar um host protegido usando o modo TPM](guarded-fabric-tpm-trusted-attestation-capturing-hardware.md)
        - [Confirme se os hosts protegidos podem atestar](guarded-fabric-confirm-hosts-can-attest-successfully.md)
        - [VMs blindadas - provedor de hospedagem de serviço implanta hosts protegidos no VMM](https://technet.microsoft.com/system-center-docs/vmm/scenario/guarded-hosts)
    - [Implantar VMs blindadas](guarded-fabric-configuration-scenarios-for-shielded-vms-overview.md)
        - [Criar um modelo VM blindado](guarded-fabric-create-a-shielded-vm-template.md)
        - [Preparar um VHD do auxiliar de blindagem VM](guarded-fabric-vm-shielding-helper-vhd.md)
        - [Configurar o Windows Azure Pack](guarded-fabric-hoster-sets-up-windows-azure-pack.md)
        - [Criar um arquivo de dados de blindagem](guarded-fabric-tenant-creates-shielding-data.md)
        - [Implantar uma VM blindada usando o Windows Azure Pack](guarded-fabric-shielded-vm-windows-azure-pack.md)
        - [Implantar uma VM blindada usando o Virtual Machine Manager](guarded-fabric-tenant-deploys-shielded-vm-using-vmm.md)

## <a name="operations-and-management-topic"></a>Tópico de gerenciamento e operações

- [Gerenciando o serviço guardião de Host](guarded-fabric-manage-hgs.md)
