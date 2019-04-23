---
title: Requisitos de sistema do Hyper-V no Windows Server
description: Lista os requisitos de hardware e firmware para o Hyper-V no Windows Server
ms.prod: windows-server-threshold
ms.service: na
manager: dongill
ms.technology: compute-hyper-v
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: bc4a4971-f727-40cd-91f5-2ee6d24b54cb
author: KBDAzure
ms.author: kathydav
ms.date: 9/30/2016
ms.openlocfilehash: 55114821b5ac2f1cc028c662217f4bee6980c923
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59845197"
---
# <a name="system-requirements-for-hyper-v-on-windows-server"></a>Requisitos de sistema do Hyper-V no Windows Server

>Aplica-se a: Windows Server 2016, Microsoft Hyper-V Server 2016, Windows Server 2019, Microsoft Hyper-V Server 2019

Hyper-V tem requisitos de hardware específicos, e alguns recursos do Hyper-V têm requisitos adicionais. Use os detalhes neste artigo para decidir quais requisitos de seu sistema deve atender para que possa usar o maneira como você planeja Hyper-V. Em seguida, examine os [catálogo do Windows Server](https://www.windowsservercatalog.com/). Tenha em mente que os requisitos do Hyper-V excederem gerais requisitos mínimos do Windows Server 2016 como um ambiente de virtualização requer mais recursos de computação.

Se você já estiver usando o Hyper-V, é provável que você pode usar o hardware existente. Os requisitos de hardware geral ainda não tiverem alterado significativamente no Windows Server 2012 R2.  Mas, você precisará de hardware mais recente para atribuição de dispositivo discretos ou use blindado as máquinas virtuais. Esses recursos contam com suporte de hardware específico, conforme descrito abaixo. Além disso, a principal diferença em hardware é esse endereço de segundo nível slat (conversão) agora é necessário em vez de recomendado.

Para obter detalhes sobre configurações com suporte máximo para o Hyper-V, como o número de máquinas virtuais em execução, consulte [planejar a escalabilidade do Hyper-V no Windows Server 2016](plan/Plan-for-Hyper-V-scalability-in-Windows-Server-2016.md). A lista de sistemas operacionais que você pode executar em suas máquinas virtuais é abordada [sistemas operacionais convidados do Windows com suporte para Hyper-V no Windows Server](Supported-Windows-guest-operating-systems-for-Hyper-V-on-Windows.md).

## <a name="general-requirements"></a>Requisitos gerais

Independentemente dos recursos do Hyper-V que você deseja usar, você precisará de:

- Um processador de 64 bits com a conversão de endereços de segundo nível (SLAT). Para instalar os componentes de virtualização do Hyper-V, como o hipervisor do Windows, o processador deve ter para SLAT. No entanto, não é necessário para instalar as ferramentas de gerenciamento do Hyper-V como Conexão de máquina Virtual (VMConnect), o Gerenciador do Hyper-V e os cmdlets do Hyper-V para o Windows PowerShell. Consulte "Como verificar se há requisitos do Hyper-V," abaixo descobrir se seu processador tem SLAT.

- Extensões do modo de Monitor de VM

- Memória insuficiente - plano para *pelo menos* 4 GB de RAM. Mais memória é melhor. Você precisará memória suficiente para o host e todas as máquinas virtuais que você deseja executar ao mesmo tempo.

- Suporte à virtualização ativado no BIOS ou UEFI:

  - Virtualização assistida por hardware. Isso está disponível em processadores que incluem uma opção de virtualização - especificamente aqueles com Intel Virtualization Technology (Intel VT) ou AMD Virtualization (AMD-V).

  - A DEP (Prevenção de Execução de Dados) imposta por hardware deve estar disponível e habilitada. Para sistemas Intel, essa é a parte de XD (bit execute disable). Para sistemas AMD, esse é o bit NX (nenhum bit no execute).

## <a name="bkmk_CheckReq"></a>Como verificar se há requisitos do Hyper-V

Abra o Windows PowerShell ou um prompt de comando e digite:

```cmd
Systeminfo.exe
```

Role até a seção de requisitos do Hyper-V para revisar o relatório.

## <a name="requirements-for-specific-features"></a>Requisitos para recursos específicos

Aqui estão os requisitos para a atribuição de dispositivo discretos e máquinas virtuais blindadas. Para obter descrições desses recursos, consulte [o que há de novo no Hyper-V no Windows Server](What-s-new-in-Hyper-V-on-Windows.md).

### <a name="discrete-device-assignment"></a>Atribuição de dispositivo discretos

**Host** requisitos são semelhantes aos requisitos existentes para o recurso de SR-IOV no Hyper-V.

- O processador deve ter qualquer um da Intel Extended página tabela EPT () ou aninhados página tabela NPT da AMD ().

- O chipset deve ter:

  - Interromper o remapeamento - VT-d da Intel, com a capacidade de interromper o remapeamento (VT-d2) ou qualquer versão do AMD e/s memória gerenciamento MMU (unidade de e/s).

  - O remapeamento DMA - VT-d da Intel com invalidações enfileiradas ou qualquer MMU de e/s do AMD.

  - Serviços de controle de acesso (ACS) nas portas de raiz de PCI Express.

- As tabelas de firmware devem expor a MMU de e/s para o hipervisor do Windows. Observe que esse recurso pode ter sido desligado no BIOS ou UEFI. Para obter instruções, consulte a documentação do hardware ou entre em contato com seu fabricante de hardware.

**Dispositivos** precisa de memória não volátil ou GPU express (NVMe). Para GPU, somente determinados dispositivos dão suporte a atribuição de dispositivo discretos. Para verificar, consulte a documentação do hardware ou entre em contato com seu fabricante de hardware. Para obter detalhes sobre esse recurso, incluindo como usá-lo e considerações, consulte a postagem "[atribuição de dispositivo discretos – descrição e o plano de fundo](https://blogs.technet.com/b/virtualization/archive/2015/11/19/discrete-device-assignment.aspx)" no blog de virtualização.

### <a name="shielded-virtual-machines"></a>Máquinas virtuais blindadas

Essas máquinas virtuais confiar na segurança baseada em virtualização e estão disponíveis começando com o Windows Server 2016.

**Host** requisitos são:

- UEFI 2.3.1c - dá suporte à inicialização segura, medida

  Os seguintes dois são opcionais para segurança baseada em virtualização em geral, mas necessário para o host, se desejar que a proteção que esses recursos fornecem:

- TPM v2.0 - protege os ativos de segurança de plataforma
- IOMMU (Intel VT-D) - para que o hipervisor possa fornecer proteção de acesso (DMA) de memória direta

**Máquina virtual** requisitos são:

- 2ª geração
- Windows Server 2012 ou mais recente, como o sistema operacional convidado

