---
title: Implantando o serviço guardião de host
ms.prod: windows-server
ms.topic: article
ms.assetid: 310b63d9-5ac7-4961-98ef-103af45d706a
manager: dongill
author: rpsqrd
ms.author: ryanpu
ms.technology: security-guarded-fabric
ms.date: 01/14/2020
ms.openlocfilehash: c1eea8c7f6da1140480d0a8deaafb2edb73528de
ms.sourcegitcommit: acfdb7b2ad283d74f526972b47c371de903d2a3d
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/05/2020
ms.locfileid: "87769144"
---
# <a name="deploying-the-host-guardian-service"></a>Implantando o serviço guardião de host

>Aplica-se a: Windows Server 2019, Windows Server (canal semestral), Windows Server 2016

Uma das metas mais importantes de fornecer um ambiente hospedado é garantir a segurança das máquinas virtuais em execução no ambiente. Como um provedor de serviços de nuvem ou um administrador de nuvem privada corporativa, você pode usar uma malha protegida para fornecer um ambiente mais seguro para as VMs. Uma malha protegida consiste em um HGS (Serviço de Guardião de Host), em geral, um cluster de três nós, além de um ou mais hosts protegidos e um conjunto de VMs (máquinas virtuais) protegidas.

## <a name="video-deploying-a-guarded-fabric"></a>Vídeo: Implantando uma malha protegida

> [!VIDEO https://www.microsoft.com/videoplayer/embed/dcd8e99f-36f1-4bc8-b3d2-9576da38d9f1?autoplay=false]

## <a name="deployment-tasks-for-guarded-fabrics-and-shielded-vms"></a>Tarefas de implantação para malhas protegidas e VMs blindadas

A tabela a seguir divide as tarefas para implantar uma malha protegida e criar VMs blindadas de acordo com diferentes funções de administrador. Observe que quando o administrador do HGS configura o HGS com hosts Hyper-V autorizados, um administrador de malha coletará e fornecerá informações de identificação sobre os hosts ao mesmo tempo.

| Etapa e link para o conteúdo | Imagem |
|--|--|--|
| 1- [verificar os pré-requisitos do HgS](guarded-fabric-prepare-for-hgs.md) | ![Etapa 1, verificar os pré-requisitos](../media/Guarded-Fabric-Shielded-VM/guarded-host-verify.png) |
| 2- [Configurar o primeiro nó HgS](guarded-fabric-choose-where-to-install-hgs.md) | ![Etapa 2, configurar o primeiro nó HGS](../media/Guarded-Fabric-Shielded-VM/guarded-host-configure-first-hgs-node.png) |
| 3- [Configurar nós HgS adicionais](guarded-fabric-configure-additional-hgs-nodes.md) | ![Etapa 3, configurar nós HGS adicionais](../media/Guarded-Fabric-Shielded-VM/guarded-host-configure-secondary-hgs-nodes.png) |
| 4- [Configurar o DNS de malha](guarded-fabric-configuring-fabric-dns.md) | ![Etapa 4, configurar o DNS de malha](../media/Guarded-Fabric-Shielded-VM/guarded-host-configure-fabric-dns.png) |
| 5- [verificar pré-requisitos de host (chave)](guarded-fabric-guarded-host-prerequisites.md#host-key-attestation) e [verificar pré-requisitos de host (TPM)](guarded-fabric-guarded-host-prerequisites.md#tpm-trusted-attestation) | ![Etapa 5, verificar a chave de pré-requisito do host e o TPM de pré-requisito do host](../media/Guarded-Fabric-Shielded-VM/guarded-host-verify.png) |
| 6- [criar chave de host (chave)](guarded-fabric-create-host-key.md) e[coletar informações do host (TPM)](guarded-fabric-tpm-trusted-attestation-capturing-hardware.md) | ![Etapa 6, criar chave de host e coletar informações do host](../media/Guarded-Fabric-Shielded-VM/guarded-host-collect-info-from-hosts.png) |
| 7- [Configurar o HgS com informações do host](guarded-fabric-add-host-information-to-hgs.md) | ![Etapa 7, adicionar informações do host ao HGS](../media/Guarded-Fabric-Shielded-VM/guarded-host-configure-hgs-with-host-info.png) |
| 8- [confirmar que os hosts podem atestar](guarded-fabric-confirm-hosts-can-attest-successfully.md) | ![Etapa 8, confirmar que o host pode atestar](../media/Guarded-Fabric-Shielded-VM/guarded-host-confirm-hosts-attest.png) |
| 9- [Configurar o VMM (opcional)](https://technet.microsoft.com/system-center-docs/vmm/scenario/guarded-overview) | ![Etapa 9, configurar o VMM (opcional)](../media/Guarded-Fabric-Shielded-VM/guarded-host-configure-vmm.png) |
| 10- [criar discos de modelo](guarded-fabric-create-a-shielded-vm-template.md) | ![Etapa 10, criar discos de modelo](../media/Guarded-Fabric-Shielded-VM/guarded-host-create-template-disk.png) |
| 11- [criar um disco auxiliar de blindagem de VM para o VMM (opcional)](guarded-fabric-vm-shielding-helper-vhd.md) | ![Etapa 11, criar um disco de ajuda de blindagem de VM para o VMM](../media/Guarded-Fabric-Shielded-VM/guarded-host-create-helper-disk.png) |
| 12- [configurar pacote do Microsoft Azure (opcional)](guarded-fabric-shielded-vm-windows-azure-pack.md) | ![Etapa 12, configurar Pacote do Microsoft Azure (opcional)](../media/Guarded-Fabric-Shielded-VM/guarded-host-windows-azure-pack.png) |
| 13- [criar arquivo de dados de blindagem](guarded-fabric-tenant-creates-shielding-data.md) | ![Etapa 13, criar um arquivo de dados de blindagem](../media/Guarded-Fabric-Shielded-VM/guarded-host-shielding-data-file.png) |
| 14- [criar VMs blindadas usando pacote do Microsoft Azure](guarded-fabric-shielded-vm-windows-azure-pack.md) | ![Etapa 14, criar VMs blindadas usando Pacote do Microsoft Azure](../media/Guarded-Fabric-Shielded-VM/guarded-host-shielded-vms.png) |
| 15- [criar VMs blindadas usando o VMM](https://technet.microsoft.com/system-center-docs/vmm/scenario/guarded-vms) | ![Etapa 15, criar VMs blindadas usando o VMM](../media/Guarded-Fabric-Shielded-VM/guarded-host-shielded-vms.png) |

## <a name="additional-references"></a>Referências adicionais

- [Malha protegida e VMs blindadas](guarded-fabric-and-shielded-vms-top-node.md)
