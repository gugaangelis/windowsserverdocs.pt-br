---
title: Requisitos de hardware dos Espaços de Armazenamento Diretos
ms.prod: windows-server-threshold
description: Requisitos mínimos de hardware para testes de Espaços de Armazenamento Diretos.
ms.author: eldenc
ms.manager: eldenc
ms.technology: storage-spaces
ms.topic: article
author: eldenchristensen
ms.date: 04/12/2018
ms.localizationpriority: medium
ms.openlocfilehash: 84d10ab3e25500720dd13e2ba057dc3c5bf05a6f
ms.sourcegitcommit: 515b4fd5c40fcbae0e12a2c30090384533972353
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 11/29/2018
ms.locfileid: "8232525"
---
# Requisitos de hardware dos Espaços de Armazenamento Diretos

> Aplica-se a: Windows Server 2016, Windows Server Insider Preview

Este tópico descreve os requisitos mínimos de hardware para espaços de armazenamento diretos.

Para produção, a Microsoft recomenda essas ofertas de hardware e software [Definido pelo Software do Windows Server](https://microsoft.com/wssd) de nossos parceiros, que inclui procedimentos e ferramentas de implantação. Eles são projetados, montados e validados nossa arquitetura de referência para garantir a compatibilidade e confiabilidade, assim, você obtém e comecem a trabalhar rapidamente. Saiba mais em [https://microsoft.com/wssd](https://microsoft.com/wssd).

![logotipos de nossos parceiros definido pelo Software do Windows Server](media/hardware-requirements/wssd-partners.png)

   > [!TIP]
   > Deseja avaliar espaços de armazenamento diretos, mas não têm o hardware? Use o Hyper-V ou máquinas virtuais do Azure como descrito [Usando espaços de armazenamento diretos em clusters de máquina virtual convidada](storage-spaces-direct-in-vm.md).

## Requisitos de base

Sistemas, componentes, dispositivos e drivers devem ser **Certificados do Windows Server 2016** pelo [Catálogo do Windows Server](https://www.windowsservercatalog.com). Além disso, recomendamos que adaptadores de rede, unidades, adaptadores de barramento do host e servidores tenham os **padrão de Software-Defined Data Center (SDDC)** e/ou **Software-Defined Data Center (SDDC) Premium** qualificações adicionais (AQs), conforme a Figura abaixo. Há mais de 1.000 componentes com o AQs SDDC.

![captura de tela do catálogo do Windows Server mostrando o AQs SDDC](media/hardware-requirements/sddc-aqs.png)

O cluster totalmente configurado (servidores, rede e armazenamento) deve passar em todos os [testes de validação de cluster](https://technet.microsoft.com/library/cc732035(v=ws.10).aspx) do assistente no Gerenciador de Cluster de Failover ou com o `Test-Cluster` [cmdlet](https://docs.microsoft.com/powershell/module/failoverclusters/test-cluster?view=win10-ps) do PowerShell.

Além disso, os requisitos a seguir se aplicam:

## Servidores

- Mínimo de 2 servidores, máximo de 16 servidores
- Recomendado que todos os servidores seja o mesmo fabricante e modelo

## CPU

- Intel Nehalem ou processador posterior compatível; ou
- AMD EPYC ou processador posterior compatível

## Memória

- Memória para Windows Server, VMs e outros aplicativos ou cargas de trabalho; Plus
- 4 GB de RAM por terabyte (TB) da capacidade da unidade de cache em cada servidor para metadados espaços de armazenamento diretos

## Inicialização

- Qualquer dispositivo de inicialização é compatível com o Windows Server, que [agora inclui SATADOM](https://cloudblogs.microsoft.com/windowsserver/2017/08/30/announcing-support-for-satadom-boot-drives-in-windows-server-2016/)
- RAID 1 espelho é **não** obrigatório, mas há suporte para inicialização
- Recomendado: tamanho mínimo de 200 GB

## Rede

Mínimo (por nó pequena escala 2-3)
- Interface de rede de 10 Gbps
- Conexão direta (cluster sem switch) é compatível conosco de 2

Recomendado (para alto desempenho, a escala ou implantações de 4 + nós)
- NICs compatíveis acesso remoto direto memória (RDMA) capaz, iWARP (recomendado) ou RoCE
- Dois ou mais NICs para redundância e desempenho
- 25 Gbps de interface de rede ou superior

## Unidades

Espaços de armazenamento diretos funciona com conexão direta NVMe, SAS ou SATA unidades que estão fisicamente conectadas a apenas um servidor. Para obter mais ajuda na escolha de unidades, consulte o tópico [Escolhendo unidades](choosing-drives.md).

- NVMe, SAS e SATA unidades (M.2, U.2 e adicionar no cartão) são compatíveis
- 512n, 512e e 4K nativas unidades são todos compatíveis
- Unidades de estado sólido devem fornecer [proteção contra perda de energia](https://blogs.technet.microsoft.com/filecab/2016/11/18/dont-do-it-consumer-ssd/)
- Mesmo número e dos tipos de unidades em cada servidor – consulte [Considerações simetria de unidade de disco](drive-symmetry-considerations.md)
- NVMe driver é nativo da Microsoft ou driver NVMe atualizado.
- Recomendado: O número de unidades de capacidade é um inteiro múltiplo do número de unidades de cache
- Recomendado: Unidades de Cache devem ter resistência elevada à gravação: pelo menos 3 unidade-gravações por dia (DWPD) ou pelo menos 4 terabytes (TBW) escrito por dia – consulte [Entendendo gravações de unidade por dia (DWPD), terabytes gravados (TBW) e o mínimo recomendado para armazenamento Espaços diretos](https://blogs.technet.microsoft.com/filecab/2017/08/11/understanding-dwpd-tbw/)

Aqui está como as unidades podem ser conectadas para espaços de armazenamento diretos:

1. Conexão direta unidades SATA
2. Unidades de NVMe direta
3. Adaptador de barramento do host (HBA) do SAS com unidades SAS
4. Adaptador de barramento do host (HBA) do SAS com unidades SATA
5. **Não tem suporte:** RAID cartões de controlador ou SAN (Fibre Channel, iSCSI, FCoE) armazenamento. Placas de barramento do host (HBA) devem implementar o modo de passagem simple.

![diagrama da unidade com suporte interconexões](media/hardware-requirements/drive-interconnect-support-1.png)

As unidades podem ser internas para o servidor ou em um compartimento externo que é conectado a apenas um servidor. SCSI Enclosure Services (SES) é necessário para identificação e mapeamento de slot. Cada compartimento externo deve apresentar um identificador exclusivo (ID exclusiva).

1. Unidades internas para o servidor
2. Unidades em um compartimento externo conectado a um servidor ("JBOD")
3. **Não tem suporte:** Compartimentos SAS compartilhados conectados a vários servidores ou qualquer forma de vários caminhos e/s (MPIO) onde as unidades são acessíveis por vários caminhos.

![diagrama da unidade com suporte interconexões](media/hardware-requirements/drive-interconnect-support-2.png)

### Número mínimo de unidades (exclui a unidade de inicialização)

- Se houver unidades usadas como cache, deverá haver pelo menos 2 por servidor
- Deve haver pelo menos 4 unidades de capacidade (sem cache) por servidor.

| Tipos de unidade presentes   | Número mínimo necessário |
|-----------------------|-------------------------|
| Tudo NVMe (mesmo modelo) | 4 NVMes                  |
| Tudo SSD (mesmo modelo)  | 4 SSDs                   |
| NVMe + SSD            | 2 NVMes + 4 SSDs          |
| NVMe + HDD            | 2 NVMes + 4 HDDs          |
| SSD + HDD             | 2 SSDs + 4 HDDs           |
| NVMe + SSD + HDD      | 2 NVMes + 4 outras       |

   >[!NOTE]
   > Esta tabela fornece o mínimo para implantações de hardware. Se você estiver implantando com máquinas virtuais e virtualizado armazenamento, como no Microsoft Azure, consulte [Usando espaços de armazenamento diretos em clusters de máquina virtual convidada](storage-spaces-direct-in-vm.md).

### Capacidade máxima

- Recomendado: Capacidade de armazenamento bruta do máximo de 100 terabytes (TB) por servidor
- 1 petabyte (1.000 TB) capacidade bruta máxima no pool de armazenamento
