---
title: Requisitos do sistema para o Hyper-V no Windows Server
description: Lista os requisitos de hardware e firmware para o Hyper-V no Windows Server
ms.topic: article
ms.assetid: bc4a4971-f727-40cd-91f5-2ee6d24b54cb
ms.author: benarm
author: BenjaminArmstrong
ms.date: 9/30/2016
ms.openlocfilehash: f56d6476bbe3db49b220f6e1652ea099b97e7108
ms.sourcegitcommit: dd1fbb5d7e71ba8cd1b5bfaf38e3123bca115572
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/17/2020
ms.locfileid: "90746711"
---
# <a name="system-requirements-for-hyper-v-on-windows-server"></a>Requisitos do sistema para o Hyper-V no Windows Server

>Aplica-se a: Windows Server 2016, Microsoft Hyper-V Server 2016, Windows Server 2019, Microsoft Hyper-V Server 2019

O Hyper-V tem requisitos de hardware específicos e alguns recursos do Hyper-V têm requisitos adicionais. Use os detalhes neste artigo para decidir quais requisitos seu sistema deve atender para que você possa usar o Hyper-V da maneira que planeja. Em seguida, examine o [catálogo do Windows Server](https://www.windowsservercatalog.com/). Tenha em mente que os requisitos do Hyper-V excedem os requisitos gerais mínimos para o Windows Server 2016 porque um ambiente de virtualização requer mais recursos de computação.

Se você já estiver usando o Hyper-V, é provável que você possa usar o hardware existente. Os requisitos gerais de hardware não mudaram significativamente do Windows Server 2012 R2.  Mas, você precisará de hardware mais recente para usar máquinas virtuais blindadas ou atribuição de dispositivo discreta. Esses recursos dependem de suporte a hardware específico, conforme descrito abaixo. Além disso, a principal diferença no hardware é que o SLAT (conversão de endereços de segundo nível) agora é necessário em vez de recomendado.

Para obter detalhes sobre as configurações máximas com suporte para o Hyper-V, como o número de máquinas virtuais em execução, consulte [planejar a escalabilidade do Hyper-v no Windows Server 2016](./plan/plan-hyper-v-scalability-in-windows-server.md). A lista de sistemas operacionais que você pode executar em suas máquinas virtuais é abordada em [sistemas operacionais convidados com suporte do Windows para o Hyper-V no Windows Server](Supported-Windows-guest-operating-systems-for-Hyper-V-on-Windows.md).

## <a name="general-requirements"></a>Requisitos gerais

Independentemente dos recursos do Hyper-V que você deseja usar, você precisará de:

- Um processador de 64 bits com SLAT (conversão de endereços de segundo nível). Para instalar os componentes de virtualização do Hyper-V, como o hipervisor do Windows, o processador deve ter o SLAT. No entanto, não é necessário instalar ferramentas de gerenciamento do Hyper-V como VMConnect (conexão de máquina virtual), Gerenciador do Hyper-V e os cmdlets do Hyper-V para o Windows PowerShell. Consulte "como verificar os requisitos do Hyper-V", abaixo, para descobrir se o processador tem SLAT.

- Extensões do modo monitor da VM

- Plano de memória suficiente para *pelo menos* 4 GB de RAM. Mais memória é melhor. Você precisará de memória suficiente para o host e todas as máquinas virtuais que deseja executar ao mesmo tempo.

- Suporte à virtualização ativado no BIOS ou UEFI:

  - Virtualização assistida por hardware. Isso está disponível em processadores que incluem uma opção de virtualização – especificamente processadores com tecnologia Intel virtualização (Intel® VT) ou AMD Virtualization (AMD-V).

  - A DEP (Prevenção de Execução de Dados) imposta por hardware deve estar disponível e habilitada. Para sistemas Intel, esse é o XD bit (Execute Disable bit). Para sistemas AMD, esse é o bit NX (nenhum bit de execução).

## <a name="how-to-check-for-hyper-v-requirements"></a>Como verificar os requisitos do Hyper-V

Abra o Windows PowerShell ou um prompt de comando e digite:

```cmd
Systeminfo.exe
```

Role até a seção requisitos do Hyper-V para examinar o relatório.

## <a name="requirements-for-specific-features"></a>Requisitos para recursos específicos

Aqui estão os requisitos para a atribuição de dispositivo discreto e máquinas virtuais blindadas. Para obter descrições desses recursos, consulte [o que há de novo no Hyper-V no Windows Server](What-s-new-in-Hyper-V-on-Windows.md).

### <a name="discrete-device-assignment"></a>Atribuição de dispositivo discreta

Os requisitos de **host** são semelhantes aos requisitos existentes para o recurso Sr-IOV no Hyper-V.

- O processador deve ter a tabela de página estendida da Intel (EPT) ou a tabela de página aninhada da AMD (NPT).

- O chipset deve ter:

  - Remapeamento de interrupção-VT da Intel com o recurso de remapeamento de interrupção (VT-D2) ou qualquer versão da unidade de gerenciamento de memória de e/s do AMD (e/s MMU).

  - Remapeamento de DMA-VT da Intel com invalidações em fila ou qualquer MMU de e/s de AMD.

  - ACS (serviços de controle de acesso) em portas raiz do PCI Express.

- As tabelas de firmware devem expor o MMU de e/s para o hipervisor do Windows. Observe que esse recurso pode estar desativado na UEFI ou no BIOS. Para obter instruções, consulte a documentação do hardware ou contate o fabricante do hardware.

Os **dispositivos** precisam de GPU ou de memória não volátil (NVMe). Para GPU, somente determinados dispositivos dão suporte à atribuição de dispositivo discreta. Para verificar, consulte a documentação do hardware ou contate o fabricante do hardware. Para obter detalhes sobre esse recurso, incluindo como usá-lo e considerações, consulte a postagem "[atribuição de dispositivo discreta--descrição e plano de fundo](https://blogs.technet.com/b/virtualization/archive/2015/11/19/discrete-device-assignment.aspx)" no blog de virtualização.

### <a name="shielded-virtual-machines"></a>Máquinas virtuais blindadas

Essas máquinas virtuais dependem da segurança baseada em virtualização e estão disponíveis a partir do Windows Server 2016.

Os requisitos do **host** são:

- UEFI 2.3.1 c – dá suporte à inicialização segura e medida

  Os dois seguintes são opcionais para segurança baseada em virtualização em geral, mas necessários para o host se você quiser a proteção que esses recursos fornecem:

- TPM v 2.0-protege os ativos de segurança da plataforma
- IOMMU (Intel VT-D) – portanto, o hipervisor pode fornecer proteção de acesso direto à memória (DMA)

Os requisitos da **máquina virtual** são:

- Geração 2
- Windows Server 2012 ou mais recente como o sistema operacional convidado