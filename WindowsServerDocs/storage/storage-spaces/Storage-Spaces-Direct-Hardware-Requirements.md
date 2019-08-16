---
title: Requisitos de hardware dos Espaços de Armazenamento Diretos
ms.prod: windows-server-threshold
description: Requisitos mínimos de hardware para testes de Espaços de Armazenamento Diretos.
ms.author: eldenc
ms.manager: eldenc
ms.technology: storage-spaces
ms.topic: article
author: eldenchristensen
ms.date: 08/05/2019
ms.localizationpriority: medium
ms.openlocfilehash: 59c04a858ceae44ee51c1de10fc40b27dc22ef90
ms.sourcegitcommit: e04565e4a1fb7aaed04addd2bc87cc6ec4c82e81
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/16/2019
ms.locfileid: "69529887"
---
# <a name="storage-spaces-direct-hardware-requirements"></a>Requisitos de hardware dos Espaços de Armazenamento Diretos

> Aplica-se a: Windows Server 2019, Windows Server 2016

Este tópico descreve os requisitos mínimos de hardware para Espaços de Armazenamento Diretos.

Para produção, a Microsoft recomenda a compra de uma solução de hardware/software validada de nossos parceiros, que incluem ferramentas e procedimentos de implantação. Essas soluções são projetadas, montadas e validadas em relação à nossa arquitetura de referência para garantir a compatibilidade e a confiabilidade, para que você comece a trabalhar rapidamente. Para soluções do Windows Server 2019, visite o [site de soluções Azure Stack HCI](https://azure.microsoft.com/overview/azure-stack/hci). Para soluções do Windows Server 2016, saiba mais em [definição de software do Windows Server](https://microsoft.com/wssd).

   > [!TIP]
   > Quer avaliar Espaços de Armazenamento Diretos mas não tem hardware? Use as máquinas virtuais Hyper-V ou Azure, conforme descrito em [usando espaços de armazenamento diretos em clusters de máquinas virtuais](storage-spaces-direct-in-vm.md)convidadas.

## <a name="base-requirements"></a>Requisitos básicos

Os sistemas, componentes, dispositivos e drivers devem ser **certificados pelo Windows server 2016 pelo** [catálogo do Windows Server](https://www.windowsservercatalog.com). Além disso, recomendamos que servidores, unidades, adaptadores de barramento de host e adaptadores de rede tenham as qualificações adicionais do **SDDC (Data Center)** e/ou do **Data Center** (AQS) Premium definidas pelo software, como pictureed acima. Há mais de 1.000 componentes com o SDDC AQs.

![captura de tela do catálogo do Windows Server mostrando o AQs do SDDC](media/hardware-requirements/sddc-aqs.png)

O cluster totalmente configurado (servidores, rede e armazenamento) deve passar todos os [testes de validação de cluster](https://technet.microsoft.com/library/cc732035(v=ws.10).aspx) por assistente no Gerenciador de cluster de failover ou com `Test-Cluster` o [cmdlet](https://docs.microsoft.com/powershell/module/failoverclusters/test-cluster?view=win10-ps) no PowerShell.

Além disso, os seguintes requisitos se aplicam:

## <a name="servers"></a>Servidores

- Mínimo de 2 servidores, máximo de 16 servidores
- Recomendado que todos os servidores tenham o mesmo fabricante e modelo

## <a name="cpu"></a>CPU

- Processador Intel Nehalem ou superior compatível; or
- Processador compatível com AMD EPYC ou posterior

## <a name="memory"></a>Memória

- Memória para Windows Server, VMs e outros aplicativos ou cargas de trabalho; acrescido
- 4 GB de RAM por terabyte (TB) de capacidade de unidade de cache em cada servidor, para metadados de Espaços de Armazenamento Diretos

## <a name="boot"></a>Inicialização

- Qualquer dispositivo de inicialização com suporte do Windows Server, que [agora inclui SATADOM](https://cloudblogs.microsoft.com/windowsserver/2017/08/30/announcing-support-for-satadom-boot-drives-in-windows-server-2016/)
- O espelho RAID 1 **não** é necessário, mas tem suporte para inicialização
- Recomendado: tamanho mínimo de 200 GB

## <a name="networking"></a>Rede

Espaços de Armazenamento Diretos requer uma alta largura de banda altamente confiável, conexão de rede de baixa latência entre cada nó.  

Interconexão mínima para o nó 2-3 de pequena escala
- NIC (placa de interface de rede) de 10 Gbps ou mais rápido
- Duas ou mais conexões de rede de cada nó recomendado para redundância e desempenho

Interconexão recomendada para alto desempenho, em escala ou implantações de 4 + 
- NICs que são compatíveis com RDMA (acesso remoto direto à memória), iWARP (recomendado) ou RoCE
- Duas ou mais conexões de rede de cada nó recomendado para redundância e desempenho
- NIC de 25 Gbps ou mais rápido

Interconexões de nó alternadas ou alternadas
- Alternado Os comutadores de rede devem ser configurados corretamente para manipular a largura de banda e o tipo de rede.  Se estiver usando RDMA que implementa o protocolo RoCE, a configuração do dispositivo de rede e do comutador será ainda mais importante. 
- SWITCHLESS: Os nós podem ser interconectados usando conexões diretas, evitando o uso de um comutador.  É necessário que cada nó tenha uma conexão direta com todos os outros nós do cluster.


## <a name="drives"></a>Unidades

Espaços de Armazenamento Diretos funciona com unidades SATA, SAS ou NVMe com conexão direta que estão fisicamente anexadas a apenas um servidor. Para obter mais ajuda na escolha de unidades, consulte o tópico [Escolhendo unidades](choosing-drives.md).

- Unidades SATA, SAS e NVMe (M. 2, U. 2 e add-in-Card) são todas suportadas
- todas as unidades nativas 512n, 512e e 4K têm suporte
- Unidades de estado sólido devem fornecer [proteção contra perda de energia](https://blogs.technet.microsoft.com/filecab/2016/11/18/dont-do-it-consumer-ssd/)
- Mesmo número e tipos de unidades em cada servidor – consulte [considerações de simetria da unidade](drive-symmetry-considerations.md)
- Os dispositivos de cache devem ter 32 GB ou mais
- Ao usar dispositivos de memória persistentes como dispositivos de cache, você deve usar dispositivos de capacidade NVMe ou SSD (não é possível usar HDDs)
- O driver NVMe é o driver NVMe in box ou atualizado da Microsoft.
- Recomendado: O número de unidades de capacidade é um múltiplo inteiro do número de unidades de cache
- Recomendado: As unidades de cache devem ter alta Endurance de gravação: pelo menos 3 unidades-gravações por dia (DWPD) ou pelo menos 4 terabytes gravados (TBW) por dia – consulte [noções básicas sobre gravações de unidade por dia (DWPD), terabytes gravados (TBW) e o mínimo recomendado para espaços de armazenamento diretos ](https://blogs.technet.microsoft.com/filecab/2017/08/11/understanding-dwpd-tbw/)

Veja como as unidades podem ser conectadas para Espaços de Armazenamento Diretos:

- Unidades SATA com conexão direta
- Unidades NVMe com conexão direta
- HBA (adaptador de barramento de host) SAS com unidades SAS
- HBA (adaptador de barramento de host) SAS com unidades SATA
- **SEM SUPORTE:** Placas de controlador RAID ou armazenamento SAN (Fibre Channel, iSCSI, FCoE). Os cartões HBA (adaptador de barramento de host) devem implementar o modo de passagem simples.

![diagrama de interconexões de unidade com suporte](media/hardware-requirements/drive-interconnect-support-1.png)

As unidades podem ser internas ao servidor ou em um compartimento externo que esteja conectado a apenas um servidor. Os serviços de compartimento SCSI (SES) são necessários para o mapeamento e a identificação do slot. Cada compartimento externo deve apresentar um identificador exclusivo (ID exclusiva).

- Unidades internas para o servidor
- Unidades em um compartimento externo ("JBOD") conectado a um servidor
- **SEM SUPORTE:** Compartimentos SAS compartilhados conectados a vários servidores ou a qualquer forma de MPIO (Multipath IO), em que as unidades podem ser acessadas por vários caminhos.

![diagrama de interconexões de unidade com suporte](media/hardware-requirements/drive-interconnect-support-2.png)

### <a name="minimum-number-of-drives-excludes-boot-drive"></a>Número mínimo de unidades (exclui a unidade de inicialização)

- Se houver unidades usadas como cache, deverá haver pelo menos 2 por servidor
- Deve haver pelo menos 4 unidades de capacidade (sem cache) por servidor.

| Tipos de unidade presentes   | Número mínimo necessário |
|-----------------------|-------------------------|
| Toda a memória persistente (mesmo modelo) | 4 memória persistente |
| Tudo NVMe (mesmo modelo) | 4 NVMes                  |
| Tudo SSD (mesmo modelo)  | 4 SSDs                   |
| Memória persistente + NVMe ou SSD | 2 memória persistente + 4 NVMe ou SSD |
| NVMe + SSD            | 2 NVMes + 4 SSDs          |
| NVMe + HDD            | 2 NVMes + 4 HDDs          |
| SSD + HDD             | 2 SSDs + 4 HDDs           |
| NVMe + SSD + HDD      | 2 NVMes + 4 outras       |

   >[!NOTE]
   > Esta tabela fornece o mínimo para implantações de hardware. Se você estiver implantando com máquinas virtuais e armazenamento virtualizado, como no Microsoft Azure, consulte [usando espaços de armazenamento diretos em clusters de máquinas virtuais](storage-spaces-direct-in-vm.md)convidadas.

### <a name="maximum-capacity"></a>Capacidade máxima

| Máximos                | Windows Server 2019  | Windows Server 2016  |
| ---                     | ---------            | ---------            |
| Capacidade bruta por servidor | 400 TB               | 100 TB               |
| Capacidade do pool           | 4 PB (4.000 TB)      | 1 PB                 |
