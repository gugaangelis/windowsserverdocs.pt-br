---
title: Implantando o serviço guardião de Host
ms.custom: na
ms.prod: windows-server-threshold
ms.topic: article
ms.assetid: 310b63d9-5ac7-4961-98ef-103af45d706a
manager: dongill
author: rpsqrd
ms.technology: security-guarded-fabric
ms.date: 08/29/2018
ms.openlocfilehash: c6b891cee905f917fab16869cd6deccc66d91a2e
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59857417"
---
# <a name="deploying-the-host-guardian-service"></a>Implantando o serviço guardião de Host 

>Aplica-se a: Windows Server 2019, Windows Server (canal semestral), Windows Server 2016

Um dos objetivos mais importantes de fornecer um ambiente hospedado é garantir a segurança das máquinas virtuais em execução no ambiente. Como um provedor de serviços em nuvem ou um administrador de nuvem privada corporativa, você pode usar uma malha protegida para fornecer um ambiente mais seguro para as VMs. Uma malha protegida consiste em um Serviço de Guardião de Host (HGS), em geral, um cluster de três nós, além de um ou mais hosts protegidos e um conjunto de máquinas virtuais protegidas (VMs).

## <a name="video-deploying-a-guarded-fabric"></a>Vídeo: Implantar uma malha protegida 

> [!VIDEO https://www.microsoft.com/en-us/videoplayer/embed/dcd8e99f-36f1-4bc8-b3d2-9576da38d9f1?autoplay=false]

## <a name="deployment-tasks-for-guarded-fabrics-and-shielded-vms"></a>Tarefas de implantação para malhas de protegidos e VMs blindadas

A tabela a seguir divide as tarefas para implantar uma malha protegida e criar VMs blindadas acordo com diferentes funções de administrador. Observe que quando o administrador do HGS configura HGS com autorizados hosts do Hyper-V, um administrador de malha coletará e forneça informações de identificação sobre os hosts ao mesmo tempo.    

|<img src="../media/Guarded-Fabric-Shielded-VM/guarded-host-hgs-administrator-tasks.png" alt="Host Guardian Service administrator tasks" width="238" height="62" align="left" /> | <img src="../media/Guarded-Fabric-Shielded-VM/guarded-host-fabric-administrator-tasks.png" alt="Fabric administrator tasks" width="300" height="62" align="left" /> | <img src="../media/Guarded-Fabric-Shielded-VM/guarded-host-tenant-administrator-tasks.png" alt="Tenant administrator tasks" width="184" height="66" align="left" /> |
|-------------------------------------|--------------------------------|-----------------------------------------|
|<img src="../media/Guarded-Fabric-Shielded-VM/1111.png" alt="Step 1" hspace="8" align="left" /> [Verificar os pré-requisitos HGS](guarded-fabric-prepare-for-hgs.md) <img src="../media/Guarded-Fabric-Shielded-VM/guarded-host-verify.png" alt="Step 1" hspace="8" align="right" />| | |
|<img src="../media/Guarded-Fabric-Shielded-VM/2222.png" alt="Step 2" hspace="8" align="left" /> [Configurar o HGS primeiro&nbsp;nó](guarded-fabric-choose-where-to-install-hgs.md)&nbsp;<img src="../media/Guarded-Fabric-Shielded-VM/guarded-host-configure-first-hgs-node.png" alt="Step 2" hspace="8" align="right" />| | |
|<img src="../media/Guarded-Fabric-Shielded-VM/3333.png" alt="Step 3" hspace="8" align="left" /> [Configurar o HGS adicionais&nbsp;nós](guarded-fabric-configure-additional-hgs-nodes.md) <img src="../media/Guarded-Fabric-Shielded-VM/guarded-host-configure-secondary-hgs-nodes.png" alt="Step 3" hspace="8" align="right" />| | |
|<img src="../media/Guarded-Fabric-Shielded-VM/4444.png" alt="Step 4" hspace="8" align="left" /> [Verificar a configuração de HGS](guarded-fabric-verify-hgs-configuration.md) <img src="../media/Guarded-Fabric-Shielded-VM/guarded-host-verify-hgs-configuration.png" alt="Step 4" hspace="8" align="right" />| | |
| &nbsp; |<img src="../media/Guarded-Fabric-Shielded-VM/5555.png" alt="Step 5" hspace="8" align="left" /> [Configurar DNS de malha](guarded-fabric-configuring-fabric-dns.md) <img src="../media/Guarded-Fabric-Shielded-VM/guarded-host-configure-fabric-dns.png" alt="Step 5" hspace="8" align="right" />| |
| &nbsp; |<img src="../media/Guarded-Fabric-Shielded-VM/6666.png" alt="Step 6" hspace="8" align="left" /> [Verifique se o host&nbsp;pré-requisitos (chave)](guarded-fabric-guarded-host-prerequisites.md#host-key-attestation)<br>[Verifique se o host&nbsp;pré-requisitos (TPM)](guarded-fabric-guarded-host-prerequisites.md#tpm-trusted-attestation)<img src="../media/Guarded-Fabric-Shielded-VM/guarded-host-verify.png" alt="Step 6" hspace="8" align="right" />| |
|<img src="../media/Guarded-Fabric-Shielded-VM/8888.png" alt="Step 8" hspace="8" align="left" /> [Configurar o HGS com informações de host](guarded-fabric-add-host-information-to-hgs.md) <img src="../media/Guarded-Fabric-Shielded-VM/guarded-host-configure-hgs-with-host-info.png" alt="Step 8" hspace="8" align="right" />|<img src="../media/Guarded-Fabric-Shielded-VM/7777.png" alt="Step 7" hspace="8" align="left" /> [Criar chave de host (chave)](guarded-fabric-create-host-key.md)<br>[Coletar informações de host (TPM)](guarded-fabric-tpm-trusted-attestation-capturing-hardware.md) <img src="../media/Guarded-Fabric-Shielded-VM/guarded-host-collect-info-from-hosts.png" alt="Step 7" hspace="8" align="right" />| |
| &nbsp; |<img src="../media/Guarded-Fabric-Shielded-VM/9999.png" alt="Step 9" hspace="8" align="left" /> [Confirmar hosts podem atestar](guarded-fabric-confirm-hosts-can-attest-successfully.md) <img src="../media/Guarded-Fabric-Shielded-VM/guarded-host-confirm-hosts-attest.png" alt="Step 9" hspace="8" align="right" />| |
| &nbsp; |<img src="../media/Guarded-Fabric-Shielded-VM/101010.png" alt="Step 10" hspace="8" align="left" /> [Configurar o VMM (opcional)](https://technet.microsoft.com/system-center-docs/vmm/scenario/guarded-overview) <img src="../media/Guarded-Fabric-Shielded-VM/guarded-host-configure-vmm.png" alt="Step 10" hspace="8" align="right" />| |
| &nbsp; |<img src="../media/Guarded-Fabric-Shielded-VM/11eleven.png" alt="Step 11" hspace="8" align="left" /> [Criar discos de modelo](guarded-fabric-create-a-shielded-vm-template.md) <img src="../media/Guarded-Fabric-Shielded-VM/guarded-host-create-template-disk.png" alt="Step 11" hspace="8" align="right" />| |
| &nbsp; |<img src="../media/Guarded-Fabric-Shielded-VM/121212.png" alt="Step 12" hspace="8" align="left" /> [Criar um disco de auxiliar de blindagem de VM do VMM (opcional)](guarded-fabric-vm-shielding-helper-vhd.md) <img src="../media/Guarded-Fabric-Shielded-VM/guarded-host-create-helper-disk.png" alt="Step 12" hspace="8" align="right" />| |
| &nbsp; |<img src="../media/Guarded-Fabric-Shielded-VM/131313.png" alt="Step 13" hspace="8" align="left" /> [Configurar o Windows Azure Pack (opcional)](guarded-fabric-shielded-vm-windows-azure-pack.md) <img src="../media/Guarded-Fabric-Shielded-VM/guarded-host-windows-azure-pack.png" alt="Step 13" hspace="8" align="right" />| |
| &nbsp; | &nbsp; |<img src="../media/Guarded-Fabric-Shielded-VM/141414.png" alt="Step 14" hspace="8" align="left" /> [Criar arquivo de dados de blindagem](guarded-fabric-tenant-creates-shielding-data.md) <img src="../media/Guarded-Fabric-Shielded-VM/guarded-host-shielding-data-file.png" alt="Step 14" hspace="8" align="right" />|
| &nbsp; | &nbsp; |<img src="../media/Guarded-Fabric-Shielded-VM/151515.png" alt="Step 15" hspace="8" align="left" /> [Criar VMs blindadas, usando o Windows Azure Pack](guarded-fabric-shielded-vm-windows-azure-pack.md) <img src="../media/Guarded-Fabric-Shielded-VM/guarded-host-shielded-vms.png" alt="Step 15" hspace="8" align="right" /><br>[Criar VMs blindadas, usando o VMM](https://technet.microsoft.com/system-center-docs/vmm/scenario/guarded-vms) <img src="../media/Guarded-Fabric-Shielded-VM/guarded-host-shielded-vms.png" alt="Step 15" hspace="8" align="right" />|


## <a name="see-also"></a>Consulte também

- [Malha protegida e VMs blindadas](guarded-fabric-and-shielded-vms-top-node.md)
