---
title: RDS – qual tecnologia de virtualização de gráficos é a certa para você?
description: Informações de planejamento para ajudá-lo a escolher a opção de virtualização de o gráfico certa para sua implantação do RDS.
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: remote-desktop-services
ms.author: elizapo
ms.date: 03/16/2017
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: d6ff5b22-7695-4fee-b1bd-6c9dce5bd0e8
author: lizap
manager: scottman
ms.openlocfilehash: d5fe737fcdc664a757518f495e3815968d17c331
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71387497"
---
# <a name="which-graphics-virtualization-technology-is-right-for-you"></a>Qual tecnologia de virtualização de gráficos é a certa para você?

Você tem uma variedade de opções quando se trata de habilitar a renderização de gráficos nos Serviços de Área de Trabalho Remota. Ao planejar seu ambiente de virtualização, as considerações a seguir determinam qual tecnologia de processamento de gráficos você escolhe:

![Considerações sobre renderização de gráficos – compara escala do usuário e requisitos de GPU para determinar a melhor tecnologia de GPU para o seu ambiente](media/rds-gpu.png)

No Windows Server 2016, você tem duas tecnologias de virtualização de gráficos disponíveis com o Hyper-V que permitem que você aproveite o hardware de GPU:

- [DDA (Atribuição de dispositivo discreto)](#discrete-device-assignment) – para o mais alto desempenho usando uma ou mais GPUs dedicadas a uma VM que fornece suporte nativo de driver de GPU dentro da VM. A densidade é baixa porque ela é limitada pelo número de GPUs físicas disponíveis no servidor. 
- [vGPU do Remote FX](#remotefx-vgpu) – para o trabalhador do conhecimento e cenários com GPU de intermitência alta, em que várias VMs aproveitam uma ou mais GPUs por meio de paravirtualização. Essa solução fornece uma maior densidade de usuários por servidor.

A ilustração a seguir mostra as opções de virtualização de gráficos no Windows Server 2016.

![Opções de virtualização de gráficos no Windows Server 2016 com o RDS – mostra as três tecnologias disponíveis e como elas diferem em escala e desempenho](media/rds-graphics-virtualization.png)

## <a name="discrete-device-assignment"></a>Atribuição de dispositivo discreto
A DDA (atribuição de dispositivo discreto) é uma solução de passagem de hardware que oferece o melhor desempenho, considerando que a VM tem acesso completo à GPU usando o driver nativo. O usuário da VM pode acessar as funcionalidades completas do seu dispositivo, bem como o driver nativo desse dispositivo. Isso significa que os recursos e capacidades de executar o dispositivo em uma VM são os mesmos de executar o mesmo dispositivo em um computador físico.

Para obter mais informações sobre DDA, confira [Planejar a implantação de Atribuição de Dispositivo Discreto](../../virtualization/hyper-v/plan/plan-for-deploying-devices-using-discrete-device-assignment.md).

## <a name="remotefx-vgpu"></a>vGPU do RemoteFX 
A vGPU do RemoteFX é uma tecnologia de virtualização de gráficos que permite que o poder de processamento de uma GPU seja dividido entre vários sistemas operacionais convidados para habilitar cenários de trabalhador do conhecimento (veja o primeiro gráfico acima). Os avanços no Windows Server 2016 permitem outros aprimoramentos para cenários de GPU com intermitência, por exemplo, para visualização de dados e aplicativos de designer. Outros aprimoramentos de recursos incluem:

- Suporte para VMs convidadas de geração 2, VMs convidadas do Windows Server 2016 e host Hyper-V do cliente Windows.
  >[!NOTE] 
  > O Host da Sessão da Área de Trabalho Remota não é compatível com uma VM convidada do Windows Server 2016; apenas uma sessão pode ser hospedada por VM convidada do Windows Server 2016.

- Compatibilidade e estabilidade do aplicativo aprimoradas.
- Modo de sessão aprimorada do VM Connect, permitindo redirecionamento via USB e área de transferência por meio do VM Connect para uma VM habilitada para vGPU do RemoteFX.

Para obter mais informações, confira [Instalar e configurar o vGPU do RemoteFX para os Serviços de Área de Trabalho Remota](rds-remotefx-vgpu.md).

## <a name="which-should-you-use"></a>Qual você deve usar?

Considerações importantes sobre qual tecnologia de virtualização usar podem depender de especificações de hardware ou de requisitos do aplicativo em seu ambiente. Aqui está uma tabela resumida sobre as funcionalidades de vGPU do RemoteFX e de DDA:

| Recurso               | vGPU do RemoteFX                                                                       | Atribuição de dispositivo discreto                                             |
|-----------------------|-------------------------------------------------------------------------------------|------------------------------------------------------------------------|
| Atribuição de GPU do dispositivo | Paravirtualizadas (várias VMs para uma ou mais GPUs)                                     | 1 ou mais GPU para 1 VM                                                  |
| Escala                 | Melhor escala/1 GPU para várias VMs                                                      | Baixa escala/1 ou mais GPUs para 1 VM                                     |
| Compatibilidade do aplicativo     | DX 11.1, OpenGL 4.4, OpenCL 1.1                                                     | Todas as funcionalidades de GPU oferecidas pelo fornecedor (DX 12, OpenGL, CUDA)          |
| AVC444                | Habilitado por padrão (Windows 10 e Windows Server 2016)                             | Disponível por meio da Política de Grupo (Windows 10 e Windows Server 2016)    |
| VRAM da GPU              | Até 1 GB de VRAM dedicada                                                           | Até o limite de VRAM compatível com a GPU                                        |
| Taxa de quadros            | Até 30 fps                                                                         | Até 60 fps                                                            |
| Driver da GPU no convidado   | Driver de vídeo do adaptador 3D RemoteFX (Microsoft)                                      | Driver do fornecedor da GPU (Nvidia, AMD, Intel)                                 |
| Suporte a SO convidado      |  Windows Server 2012 R2 Windows Server 2016 Windows 7 SP1 Windows 8.1 Windows 10 |  Windows Server 2012 R2 Windows Server 2016 Windows 10 Linux         |
| Hipervisor            | Microsoft Hyper-V                                                                   | Microsoft Hyper-V                                                      |
| Disponibilidade do sistema operacional do host  |  Windows Server 2012 R2 Windows Server 2016 Windows 10                             | Windows Server 2016                                                    |
| Hardware de GPU          | GPUs empresariais (por exemplo, Nvidia Quadro/GRID ou AMD FirePro)                         | GPUs empresariais (por exemplo, Nvidia Quadro/GRID ou AMD FirePro)            |
| Hardware de servidor       | Sem requisitos especiais                                                             | Servidor moderno, expõe o IOMMU ao sistema operacional (geralmente hardware em conformidade com SR-IOV) |

Uma regra geral é usar DDA para obter a melhor compatibilidade do aplicativo, pois a máquina virtual terá acesso direto à GPU. Se os aplicativos ou cargas de trabalho não têm requisitos de GPU tão estritos e você deseja atender a uma base mais ampla de usuários, a vGPU RemoteFX pode funcionar melhor para você.