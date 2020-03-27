---
title: Planejar a aceleração de GPU no Windows Server
description: Saiba mais sobre as diferentes tecnologias Hyper-V para aceleração de GPU, incluindo DDA e vGPU RemoteFX
ms.prod: windows-server
ms.reviewer: rickman
author: rick-man
ms.author: rickman
manager: stevelee
ms.topic: article
ms.date: 08/21/2019
ms.openlocfilehash: 7ca8d29b58dc8682575d9cb8b0f26aa49b257335
ms.sourcegitcommit: da7b9bce1eba369bcd156639276f6899714e279f
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/26/2020
ms.locfileid: "80307852"
---
# <a name="plan-for-gpu-acceleration-in-windows-server"></a>Planejar a aceleração de GPU no Windows Server

> Aplica-se a: Windows Server 2016, Microsoft Hyper-V Server 2016, Windows Server 2019, Microsoft Hyper-V Server 2019

Este artigo apresenta os recursos de virtualização de gráficos disponíveis no Windows Server.

## <a name="when-to-use-gpu-acceleration"></a>Quando usar a aceleração de GPU

Dependendo de sua carga de trabalho, talvez você queira considerar a aceleração da GPU. Veja o que você deve considerar antes de escolher a aceleração de GPU:

- **Cargas de trabalho de aplicativos e de comunicação remota (VDI/DaaS)** : se você estiver criando um aplicativo ou um serviço de comunicação remota de área de trabalho com o Windows Server, considere o catálogo de aplicativos que você espera que os usuários executem. Alguns tipos de aplicativos, como aplicativos CAD/CAM, aplicativos de simulação, jogos e aplicativos de renderização/visualização, dependem muito da renderização 3D para fornecer interatividade contínua e responsiva. A maioria dos clientes considera as GPUs uma necessidade por uma experiência de usuário razoável com esses tipos de aplicativos.
- **Cargas de trabalho de renderização, codificação e visualização remotas**: essas cargas de trabalho orientadas por gráficos tendem a depender muito dos recursos especializados de uma GPU, como a renderização 3D eficiente e a codificação/decodificação de quadros, a fim de obter metas de produtividade e economia. Para esse tipo de carga de trabalho, uma única VM habilitada para GPU pode ser capaz de corresponder à taxa de transferência de várias VMs somente de CPU.
- **Cargas de trabalho do HPC e do ml**: para cargas de trabalho computacionais de alto desempenho, como treinamento ou inferência de modelo de computação e aprendizado de máquina, as GPUs podem reduzir drasticamente o tempo para resultar, o tempo de inferência e o tempo de treinamento. Como alternativa, eles podem oferecer melhor relação custo-benefício do que uma arquitetura somente de CPU em um nível de desempenho comparável. Muitas estruturas do HPC e do Machine Learning têm uma opção para habilitar a aceleração de GPU; Considere se isso pode beneficiar sua carga de trabalho específica.

## <a name="gpu-virtualization-in-windows-server"></a>Virtualização de GPU no Windows Server

As tecnologias de virtualização de GPU habilitam a aceleração de GPU em um ambiente virtualizado, normalmente em máquinas virtuais. Se sua carga de trabalho for virtualizada com o Hyper-V, você precisará empregar a virtualização de gráficos para fornecer a aceleração de GPU da GPU física para seus aplicativos ou serviços virtualizados. No entanto, se sua carga de trabalho for executada diretamente em hosts físicos do Windows Server, você não precisará de virtualização de gráficos; seus aplicativos e serviços já têm acesso a recursos de GPU e APIs com suporte nativo no Windows Server.

As seguintes tecnologias de virtualização de gráficos estão disponíveis para VMs do Hyper-V no Windows Server:

- [Atribuição de dispositivo discreta (DDA)](#discrete-device-assignment-dda)
- [RemoteFX vGPU](#remotefx-vgpu)

Além das cargas de trabalho de VM, o Windows Server também dá suporte à aceleração de GPU de cargas de trabalho em contêineres em contêineres do Windows. Para obter mais informações, consulte [aceleração de GPU em contêineres do Windows](https://docs.microsoft.com/virtualization/windowscontainers/deploy-containers/gpu-acceleration).

## <a name="discrete-device-assignment-dda"></a>Atribuição de dispositivo discreta (DDA)

A atribuição de dispositivo discreta (DDA), também conhecida como passagem de GPU, permite dedicar uma ou mais GPUs físicas a uma máquina virtual. Em uma implantação DDA, as cargas de trabalho virtualizadas são executadas no driver nativo e normalmente têm acesso completo à funcionalidade da GPU. DDA oferece o nível mais alto de compatibilidade de aplicativos e de desempenho potencial. O DDA também pode fornecer aceleração de GPU para VMs do Linux, sujeito a suporte.

Uma implantação DDA pode acelerar apenas um número limitado de máquinas virtuais, já que cada GPU física pode fornecer aceleração para, no máximo, uma VM. Se você estiver desenvolvendo um serviço cuja arquitetura dá suporte a máquinas virtuais compartilhadas, considere hospedar várias cargas de trabalho aceleradas por VM. Por exemplo, se você estiver criando um serviço de comunicação remota de área de trabalho com RDS, poderá melhorar a escala do usuário aproveitando os recursos de várias sessões do Windows Server para hospedar vários desktops de usuário em cada VM. Esses usuários irão compartilhar os benefícios da aceleração de GPU.

Para saber mais, consulte estes tópicos:

- [Planejar a implantação de uma atribuição de dispositivo discreta](plan-for-deploying-devices-using-discrete-device-assignment.md)
- [Implantar dispositivos gráficos usando a atribuição de dispositivo discreta](../deploy/Deploying-graphics-devices-using-dda.md)

## <a name="remotefx-vgpu"></a>vGPU do RemoteFX

> [!NOTE]
> O vGPU RemoteFX tem suporte total no Windows Server 2016, mas não tem suporte no Windows Server 2019.

O RemoteFX vGPU é uma tecnologia de virtualização de gráficos que permite que uma única GPU física seja compartilhada entre várias máquinas virtuais. Em uma implantação de vGPU do RemoteFX, as cargas de trabalho virtualizadas são executadas no adaptador 3D RemoteFX da Microsoft, que coordena as solicitações de processamento de GPU entre o host e os convidados. O vGPU RemoteFX é mais adequado para o Worker de conhecimento e cargas de trabalho de alta intermitência em que os recursos de GPU dedicados não são necessários. VGPU RemoteFX só pode fornecer aceleração de GPU para VMs do Windows.

Para saber mais, consulte estes tópicos:

- [Implantar dispositivos gráficos usando um vGPU do RemoteFX](../deploy/deploy-graphics-devices-using-remotefx-vgpu.md)
- [Suporte a vGPU (adaptador de vídeo 3D) do RemoteFX](../../../remote/remote-desktop-services/rds-supported-config.md#remotefx-3d-video-adapter-vgpu-support)

## <a name="comparing-dda-and-remotefx-vgpu"></a>Comparando DDA e vGPU RemoteFX

Considere as seguintes funcionalidades e diferenças de suporte entre as tecnologias de virtualização de gráficos ao planejar sua implantação:

|                       | vGPU do RemoteFX                                                                       | Atribuição de dispositivo discreto                                                          |
|-----------------------|-------------------------------------------------------------------------------------|-------------------------------------------------------------------------------------|
| Modelo de recurso de GPU    | Dedicado ou compartilhado                                                                 | Somente dedicado                                                                      |
| Densidade de VM            | Alta (uma ou mais GPUs para várias VMs)                                                 | Baixo (uma ou mais GPUs para uma VM)                                                    |
| Compatibilidade do aplicativo     | DX 11.1, OpenGL 4.4, OpenCL 1.1                                                     | Todas as funcionalidades de GPU oferecidas pelo fornecedor (DX 12, OpenGL, CUDA)                       |
| AVC444                | Habilitado por padrão                                                                  | Disponível por meio de Política de Grupo                                                      |
| VRAM da GPU              | Até 1 GB de VRAM dedicada                                                           | Até o limite de VRAM compatível com a GPU                                                     |
| Taxa de quadros            | Até 30 fps                                                                         | Até 60 fps                                                                         |
| Driver da GPU no convidado   | Driver de vídeo do adaptador 3D RemoteFX (Microsoft)                                      | Driver de fornecedor de GPU (NVIDIA, AMD, Intel)                                              |
| Suporte do SO host       | Windows Server 2016                                                                 | Windows Server 2016; Windows Server 2019                                            |
| Suporte a SO convidado      | Windows Server 2012 R2; Windows Server 2016; Windows 7 SP1; Windows 8.1; Windows 10 | Windows Server 2012 R2; Windows Server 2016; Windows Server 2019; Windows 10; Linux |
| Hipervisor            | Microsoft Hyper-V                                                                   | Microsoft Hyper-V                                                                   |
| Hardware de GPU          | GPUs empresariais (por exemplo, Nvidia Quadro/GRID ou AMD FirePro)                         | GPUs empresariais (por exemplo, Nvidia Quadro/GRID ou AMD FirePro)                         |
| Hardware de servidor       | Sem requisitos especiais                                                             | Servidor moderno, expõe o IOMMU ao sistema operacional (geralmente hardware em conformidade com SR-IOV)              |
