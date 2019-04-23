---
title: RDS - qual tecnologia de virtualização de gráficos é ideal para você?
description: Informações de planejamento para ajudá-lo a escolher o gráfico ideal a opção de virtualização para sua implantação do RDS.
ms.custom: na
ms.prod: windows-server-threshold
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
ms.openlocfilehash: 7cf7fdf3510fcaaa955bd0031fb3564fe4372472
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59875797"
---
# <a name="which-graphics-virtualization-technology-is-right-for-you"></a>Qual tecnologia de virtualização de gráficos é ideal para você?

Você tem uma variedade de opções quando se trata de habilitar a renderização de gráficos nos serviços de área de trabalho remota. Ao planejar seu ambiente de virtualização, as seguintes considerações de unidade que você escolher a tecnologia de processamento de gráficos:

![Considerações de renderização de gráficos - compara escala do usuário e requisitos de GPU para determinar a melhor tecnologia GPU para o seu ambiente](media/rds-gpu.png)

No Windows Server 2016, você tem duas tecnologias de virtualização de gráficos disponíveis com o Hyper-V que permitem que você aproveite o hardware GPU:

- [Atribuição de dispositivo discretos (DDA)](#discrete-device-assignment) - o para o mais alto desempenho usando um ou mais GPUs dedicado a uma VM que fornece suporte nativo de driver GPU dentro da VM. A densidade é baixa porque ela é limitada pelo número de GPUs físicos disponíveis no servidor. 
- [VGPU do Remote FX](#remotefx-vgpu) - para o trabalhador de conhecimento e cenários GPU de intermitência alta, em que várias VMs aproveitam um ou mais GPUs por meio de paravirtualização. Essa solução fornece uma densidade de usuário por servidor.

A ilustração a seguir mostra os elementos gráficos as opções de virtualização no Windows Server 2016.

![As opções de virtualização de gráficos no Windows Server 2016 com o RDS - mostra as três tecnologias disponíveis e como eles diferem em escala e desempenho](media/rds-graphics-virtualization.png)

## <a name="discrete-device-assignment"></a>Atribuição de dispositivo discreto
Atribuição de dispositivo discretos (DDA) é uma solução de passagem de hardware que oferece o melhor desempenho, considerando que a VM tem acesso completo à GPU usando o driver nativo. O usuário da VM pode acessar os recursos completos do seu dispositivo como bem driver nativo do dispositivo. Isso significa que os recursos e capacidades de executar o dispositivo em um espelho VM em execução no mesmo dispositivo em bare-metal.

Para obter mais informações sobre DDA, fazer check-out [planejar a implantação de atribuição de dispositivo discretos](../../virtualization/hyper-v/plan/plan-for-deploying-devices-using-discrete-device-assignment.md).

## <a name="remotefx-vgpu"></a>VGPU RemoteFX 
VGPU do RemoteFX é uma tecnologia de virtualização de elementos gráficos que permite que o poder de processamento de uma GPU ser divididos em vários sistemas operacionais de convidados para habilitar cenários de trabalhador de Conhecimento (consulte o primeiro gráfico acima). Os avanços no Windows Server 2016 permitem que os outros aprimoramentos para cenários de intermitência GPU, por exemplo, para visualização de aplicativos e dados de designer. Outros aprimoramentos incluem:

-   Suporte para VMs convidadas de geração 2, Windows Server 2016 convidado VMs e hosts do Hyper-V do cliente Windows.
   >[!NOTE] 
   > Não há suporte para o Host de sessão de área de trabalho remota em um convidado do Windows Server 2016 VM; sessão apenas 1 pode ser hospedado por VM de convidado do Windows Server 2016.

-   Compatibilidade de aplicativo aprimorada e estabilidade.
-   VM Connect modo de sessão avançado, permitindo que o redirecionamento de USB e área de transferência por meio de VM se conectar a uma VM que está habilitada para vGPU do RemoteFX.

Para obter mais informações, confira [definir e configurar vGPU do RemoteFX para serviços de área de trabalho remota](rds-remotefx-vgpu.md).

## <a name="which-should-you-use"></a>Qual devo usar?

Considerações importantes em que a virtualização tecnologia pode depender de especificações de hardware ou requisitos de aplicativo em seu ambiente. Aqui está uma tabela resumida sobre os recursos de vGPU DDA e do RemoteFX:

| Recurso               | VGPU RemoteFX                                                                       | Atribuição de dispositivo discreto                                             |
|-----------------------|-------------------------------------------------------------------------------------|------------------------------------------------------------------------|
| Atribuição de dispositivo de GPU | Paravirtualizados (várias VMs para um ou mais GPUs)                                     | GPU de 1 ou mais para 1 VM                                                  |
| Escala                 | Melhor forma de escalonar / 1 GPU para várias VMs                                                      | Dimensionamento de baixa / 1 ou mais GPUs a 1 VM                                     |
| Compatibilidade de aplicativos     | DX 11.1, OpenGL 4.4, OpenCL 1.1                                                     | Todos os recursos GPU fornecidos pelo fornecedor (DX 12, OpenGL, CUDA)          |
| AVC444                | Habilitado por padrão (Windows 10 e Windows Server 2016)                             | Disponível por meio da diretiva de grupo (Windows 10 e Windows Server 2016)    |
| VRAM DA GPU              | Até 1 GB dedicado VRAM                                                           | Até VRAM suportado pela GPU                                        |
| Taxa de quadros            | Até 30fps                                                                         | Até 60fps                                                            |
| Driver da GPU no convidado   | Driver de vídeo RemoteFX 3D adaptador (Microsoft)                                      | Driver do fornecedor GPU (Nvidia, AMD, Intel)                                 |
| Suporte de sistema operacional convidado      |  Windows Server 2012 R2  Windows Server 2016  Windows 7 SP1  Windows 8.1 Windows 10 |  Windows Server 2012 R2  Windows Server 2016  Windows 10 Linux         |
| Hipervisor            | Microsoft Hyper-V                                                                   | Microsoft Hyper-V                                                      |
| Disponibilidade do sistema operacional do host  |  Windows Server 2012 R2  Windows Server 2016 Windows 10                             | Windows Server 2016                                                    |
| Hardware de GPU          | Enterprise GPUs (por exemplo, Nvidia Quadro/GRID ou AMD FirePro)                         | Enterprise GPUs (por exemplo, Nvidia Quadro/GRID ou AMD FirePro)            |
| Hardware de servidor       | Não há requisitos especiais                                                             | Servidor moderno, expõe o IOMMU ao sistema operacional (geralmente hardware compatível com SR-IOV) |

Um regra geral é usar DDA a melhor compatibilidade de aplicativos, pois a máquina virtual terá acesso direto à GPU. Se seus aplicativos ou cargas de trabalho não têm como rígidos requisitos quanto a GPU e você desejar para o servidor de uma base mais ampla de usuários, a vGPU do RemoteFX pode funcionar melhor para você.