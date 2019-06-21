---
title: Requisitos de hardware dos Espaços de Armazenamento Diretos
ms.prod: windows-server-threshold
description: Requisitos mínimos de hardware para testes de Espaços de Armazenamento Diretos.
ms.author: eldenc
ms.manager: eldenc
ms.technology: storage-spaces
ms.topic: article
author: eldenchristensen
ms.date: 06/13/2019
ms.localizationpriority: medium
ms.openlocfilehash: 7fa4560e0c050c8decbcb4e9456a884976e447e2
ms.sourcegitcommit: afb0602767de64a76aaf9ce6a60d2f0e78efb78b
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/20/2019
ms.locfileid: "67284414"
---
# <a name="storage-spaces-direct-hardware-requirements"></a>Requisitos de hardware dos Espaços de Armazenamento Diretos

> Aplica-se a: Windows Server 2019, Windows Server 2016

Este tópico descreve os requisitos mínimos de hardware para espaços de armazenamento diretos.

Para a produção, a Microsoft recomenda a compra de uma solução de hardware/software validado de nossos parceiros, que incluem procedimentos e ferramentas de implantação. Essas soluções são projetadas, reunimos e validadas em relação a nossa arquitetura de referência para garantir a compatibilidade e confiabilidade, para que você começar a trabalhar em funcionamento rapidamente. Para soluções do Windows Server 2019, visite o [site de soluções do Azure Stack HCI](https://azure.microsoft.com/overview/azure-stack/hci). Para soluções do Windows Server 2016, saiba mais em [definida pelo Software do Windows Server](https://microsoft.com/wssd).

   > [!TIP]
   > Para avaliar a espaços de armazenamento diretos, mas não tiver o hardware? Usar máquinas virtuais do Hyper-V ou Azure conforme descrito em [usando espaços de armazenamento diretos em clusters de máquinas virtuais de convidados](storage-spaces-direct-in-vm.md).

## <a name="base-requirements"></a>Requisitos base

Sistemas, componentes, dispositivos e drivers devem ser **certificados do Windows Server 2016** pela [catálogo do Windows Server](https://www.windowsservercatalog.com). Além disso, é recomendável tem adaptadores de rede, unidades, adaptadores de barramento do host e servidores de **Standard da Central de dados definido por software (SDDC)** e/ou **Centro de dados definido por software (SDDC) Premium** qualificações adicionais (AQs), conforme ilustrado abaixo. Há mais de 1.000 componentes com a AQs SDDC.

![captura de tela do catálogo do Windows Server, mostrando a AQs SDDC](media/hardware-requirements/sddc-aqs.png)

O cluster totalmente configurado (servidores, rede e armazenamento) deve passar por todos [testes de validação de cluster](https://technet.microsoft.com/library/cc732035(v=ws.10).aspx) pelo assistente no Gerenciador de Cluster de Failover ou com o `Test-Cluster` [cmdlet](https://docs.microsoft.com/powershell/module/failoverclusters/test-cluster?view=win10-ps) no PowerShell.

Além disso, os seguintes requisitos se aplicam:

## <a name="servers"></a>Servidores

- Mínimo de 2 servidores, máximo de 16 servidores
- Recomenda que todos os servidores seja o mesmo fabricante e modelo

## <a name="cpu"></a>CPU

- Intel Nehalem ou posterior compatível processador; ou
- EPYC AMD ou posterior processador compatível

## <a name="memory"></a>Memória

- Memória para o Windows Server, VMs e outros aplicativos ou cargas de trabalho; sinal de adição
- 4 GB de RAM por terabyte (TB) de capacidade de disco de cache em cada servidor, para os metadados de espaços de armazenamento diretos

## <a name="boot"></a>Inicialização

- Qualquer dispositivo de inicialização com suporte pelo Windows Server, que [agora inclui SATADOM](https://cloudblogs.microsoft.com/windowsserver/2017/08/30/announcing-support-for-satadom-boot-drives-in-windows-server-2016/)
- RAID 1 espelho estiver **não** necessário, mas há suporte para inicialização
- Recomendado: Tamanho mínimo de 200 GB

## <a name="networking"></a>Rede

Mínimo (para o nó de pequena escala 2 e 3)
- Interface de rede de 10 Gbps
- Conexão direta (cluster sem switch) é compatível com 2 nós

Recomendado (para alto desempenho, a escala ou implantações de nós 4 +)
- NICs que são o acesso remoto direto de memória (RDMA) capaz, iWARP (recomendado) ou RoCE
- Dois ou mais NICs para redundância e desempenho
- Interface de rede de 25 Gbps ou superior

## <a name="drives"></a>Unidades

Espaços de armazenamento diretos funciona com conexão direta NVMe, SAS ou SATA unidades que estão conectadas fisicamente apenas um servidor. Para obter mais ajuda na escolha de unidades, consulte o tópico [Escolhendo unidades](choosing-drives.md).

- Unidades de NVMe, SAS e SATA de (M.2, U.2 e adicionar no cartão) têm suporte
- 512n, 512e e unidades nativas de 4K têm suporte
- Unidades de estado sólido devem fornecer [proteção contra perda de energia](https://blogs.technet.microsoft.com/filecab/2016/11/18/dont-do-it-consumer-ssd/)
- Mesmo número e tipos de unidades em cada servidor – consulte [considerações de simetria de unidade](drive-symmetry-considerations.md)
- Dispositivos de cache devem ser de 32 GB ou maior
- Ao usar dispositivos de memória persistentes como dispositivos de cache, você deve usar dispositivos NVMe ou SSD de capacidade (você não pode usar HDDs)
- Driver de NVMe é na caixa da Microsoft ou NVMe driver atualizado.
- Recomendado: Número de unidades de capacidade é um múltiplo inteiro do número de unidades de cache
- Recomendado: Unidades de cache devem ter a gravação alta durabilidade: pelo menos 3 unidade gravações-por dia (DWPD) ou pelo menos 4 terabytes escritos (TBW) por dia – consulte [unidade Noções básicas sobre gravações por dia (DWPD), terabytes escrito (TBW) e o mínimo recomendado para armazenamento Espaços diretos](https://blogs.technet.microsoft.com/filecab/2017/08/11/understanding-dwpd-tbw/)

Aqui está como unidades podem ser conectadas para espaços de armazenamento diretos:

- Discos SATA de conexão direta
- Unidades de NVMe direta
- SAS controladora (HBA) com unidades SAS
- SAS controladora (HBA) com unidades SATA
- **NÃO TEM SUPORTE:** RAID SAN (Fibre Channel, iSCSI, FCoE) ou placas do controlador de armazenamento. Placas de barramento de host (HBA) devem implementar o modo de passagem simples.

![interconexões do diagrama da unidade com suporte](media/hardware-requirements/drive-interconnect-support-1.png)

Unidades podem ser internas ao servidor ou em um aparelho externo que é conectado a apenas um servidor. Serviços de compartimento SCSI (SES) é necessária para identificação e mapeamento do slot. Cada compartimento externo deve apresentar um identificador exclusivo (ID exclusiva).

- Unidades internas ao servidor
- Unidades em um aparelho externo conectado a um servidor ("JBOD")
- **NÃO TEM SUPORTE:** Compartimentos SAS compartilhados conectado a vários servidores ou de qualquer forma de múltiplos caminhos IO (MPIO) em que as unidades são acessíveis por vários caminhos.

![interconexões do diagrama da unidade com suporte](media/hardware-requirements/drive-interconnect-support-2.png)

### <a name="minimum-number-of-drives-excludes-boot-drive"></a>Número mínimo de unidades (exclui a unidade de inicialização)

- Se houver unidades usadas como cache, deverá haver pelo menos 2 por servidor
- Deve haver pelo menos 4 unidades de capacidade (sem cache) por servidor.

| Tipos de unidade presentes   | Número mínimo necessário |
|-----------------------|-------------------------|
| Toda a memória persistente (mesmo modelo) | 4 de memória persistente |
| Tudo NVMe (mesmo modelo) | 4 NVMes                  |
| Tudo SSD (mesmo modelo)  | 4 SSDs                   |
| Memória persistente + NVMe ou SSD | Memória persistente 2 + 4 NVMe ou SSD |
| NVMe + SSD            | 2 NVMes + 4 SSDs          |
| NVMe + HDD            | 2 NVMes + 4 HDDs          |
| SSD + HDD             | 2 SSDs + 4 HDDs           |
| NVMe + SSD + HDD      | 2 NVMes + 4 outras       |

   >[!NOTE]
   > Esta tabela fornece o requisito mínimo para implantações de hardware. Se você estiver implantando com máquinas virtuais e virtualizados armazenamento, como no Microsoft Azure, veja [usando espaços de armazenamento diretos em clusters de máquinas virtuais de convidados](storage-spaces-direct-in-vm.md).

### <a name="maximum-capacity"></a>Capacidade máxima

| Limites máximos                | Windows Server 2019  | Windows Server 2016  |
| ---                     | ---------            | ---------            |
| Capacidade bruta por servidor | 100 TB               | 100 TB               |
| Capacidade do pool           | 4 PB (4.000 TB)      | 1 PB                 |
