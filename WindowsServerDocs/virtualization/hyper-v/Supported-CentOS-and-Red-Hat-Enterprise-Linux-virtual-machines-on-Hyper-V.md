---
title: Suporte para CentOS e Red Hat Enterprise Linux máquinas virtuais do Hyper-V
description: Lista as versões do Linux integration services para compatíveis do CentOS e Red Hat Enterprise distribuições
ms.prod: windows-server-threshold
ms.service: na
ms.technology: compute-hyper-v
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 4bf8783d-dee5-4b3e-8cce-2b11b117c189
author: danihalfin
ms.author: daniha
ms.date: 12/20/2017
ms.openlocfilehash: 6c9ed85c2249a4671e52eb7d512298a75f53b309
ms.sourcegitcommit: 8ba2c4de3bafa487a46c13c40e4a488bf95b6c33
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 05/25/2019
ms.locfileid: "66222668"
---
# <a name="supported-centos-and-red-hat-enterprise-linux-virtual-machines-on-hyper-v"></a>Suporte para CentOS e Red Hat Enterprise Linux máquinas virtuais do Hyper-V

>Aplica-se a: Windows Server 2016, Hyper-V Server 2016, Windows Server 2012 R2, Hyper-V Server 2012 R2, Windows Server 2012, do Hyper-V Server 2012, Windows Server 2008 R2, Windows 10, Windows 8.1, Windows 8, Windows 7.1, Windows 7

Os seguintes mapas de distribuição de recurso indicam os recursos que estão presentes nas versões internas e que pode ser baixadas do Integration Services do Linux. Os problemas conhecidos e soluções alternativas para cada distribuição são listadas após as tabelas.

Os drivers do Red Hat Enterprise Linux Integration Services internos para o Hyper-V (disponível desde o Red Hat Enterprise Linux 6.4) são suficientes para convidados do Red Hat Enterprise Linux ser executado usando os dispositivos sintéticos de alto desempenho em hosts Hyper-V. Esses drivers internos são certificados pela Red Hat para esse uso. Configurações de certificados podem ser exibidas nesta página da web Red Hat: [Catálogo de certificação Red Hat](https://access.redhat.com/ecosystem/search/#/ecosystem/Red%20Hat%20Enterprise%20Linux?sort=sortTitle%20asc&vendors=Microsoft&category=Server). Não é necessário baixar e instalar pacotes do Integration Services do Linux do Microsoft Download Center e fazer assim pode limitar o suporte do Red Hat conforme descrito no artigo da Base de conhecimento do Red Hat 1067: [Base de conhecimento do Red Hat 1067](https://access.redhat.com/articles/1067).

Por causa de conflitos potenciais entre o suporte interno a LIS e o suporte para download do LIS quando você atualiza o kernel, desabilitar as atualizações automáticas, desinstale os pacotes para download do LIS, atualize o kernel reinicializar e então instalar o LIS mais recente de versão, e a reinicialização novamente.

>[!NOTE]
>Informações de certificação oficiais do Red Hat Enterprise Linux estão disponíveis por meio de [Portal do cliente do Red Hat](https://access.redhat.com/ecosystem/search/#/category/Server?sort=sortTitle%20asc&query=windows%20server&ecosystem=Red%20Hat%20Enterprise%20Linux).

Nesta seção:

* [RHEL/CentOS 7.x Series](Supported-CentOS-and-Red-Hat-Enterprise-Linux-virtual-machines-on-Hyper-V.md#BKMK_7x)

* [RHEL/CentOS 6.x Series](Supported-CentOS-and-Red-Hat-Enterprise-Linux-virtual-machines-on-Hyper-V.md#BKMK_6x)

* [RHEL/CentOS 5.x Series](Supported-CentOS-and-Red-Hat-Enterprise-Linux-virtual-machines-on-Hyper-V.md#BKMK_5x)

* [Notas](Supported-CentOS-and-Red-Hat-Enterprise-Linux-virtual-machines-on-Hyper-V.md#BKMK_notes)

## <a name="table-legend"></a>Legenda da tabela

* **Criado** -LIS são incluídos como parte de sua distribuição Linux. Os números de versão do módulo de kernel para a LIS interna (conforme mostrado pelas **lsmod**, por exemplo) são diferentes do número da versão do pacote de download do LIS fornecidas pela Microsoft. Uma incompatibilidade não indica a LIS interna está desatualizada.

* &#10004;-Recurso disponível

* (*em branco*)-recurso não disponível

## <a name="BKMK_7x"></a>RHEL/CentOS 7.x Series

Esta série tem apenas os kernels de 64 bits.

|**Recurso**|**Versão do Windows Server**|**7.5-7.6**|**7.3-7.4**|**7.0-7.2**|**7.5-7.6**|**7.4**|**7.3**|**7.2**|**7.1**|**7.0**|
|-|-|-|-|-|-|-|-|-|-|-|
|**Disponibilidade**||[LIS 4.2](https://www.microsoft.com/download/details.aspx?id=55106)|[LIS 4.2](https://www.microsoft.com/download/details.aspx?id=55106)|[LIS 4.2](https://www.microsoft.com/download/details.aspx?id=55106)|Criado|Criado|Criado|Criado|Criado|Criado||
|**[Core](Feature-Descriptions-for-Linux-and-FreeBSD-virtual-machines-on-Hyper-V.md#core)**|2019, 2016, 2012 R2, 2012, 2008 R2|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;|
|Hora precisa do Windows Server 2016|2019, 2016|&#10004;|&#10004;|||||||
|**[Sistema de rede](Feature-Descriptions-for-Linux-and-FreeBSD-virtual-machines-on-Hyper-V.md#networking)**|||||||
|Quadros jumbo|2019, 2016, 2012 R2, 2012, 2008 R2|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;|
|Marcação de VLAN e entroncamento|2019, 2016, 2012 R2, 2012, 2008 R2|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;|
|Migração ao vivo|2019, 2016, 2012 R2, 2012, 2008 R2|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;|
|Injeção de endereço IP estático|2019, 2016, 2012 R2, 2012|&#10004;Observação 2|&#10004;Observação 2|&#10004;Observação 2|&#10004;Observação 2|&#10004;Observação 2|&#10004;Observação 2|&#10004;Observação 2|&#10004;Observação 2|&#10004;Observação 2|
|vRSS|2019, 2016, 2012 R2|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;||
|Descarregamento de soma de verificação e de segmentação TCP|2019, 2016, 2012 R2, 2012, 2008 R2|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;||
|SR-IOV|2019, 2016|&#10004;|&#10004;||&#10004;|&#10004;|&#10004;|||
|**[Armazenamento](Feature-Descriptions-for-Linux-and-FreeBSD-virtual-machines-on-Hyper-V.md#storage)**||||
|Redimensionamento VHDX|2019, 2016, 2012 R2|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;|
|Fibre Channel Virtual|2019, 2016, 2012 R2|&#10004;Observação 3|&#10004;Observação 3|&#10004;Observação 3|&#10004;Observação 3|&#10004;Observação 3|&#10004;Observação 3|&#10004;Observação 3|&#10004;Observação 3|&#10004;Observação 3|
|Backup de máquina virtual ao vivo|2019, 2016, 2012 R2|&#10004;Observação 5|&#10004;Observação 5|&#10004;Observação 5|&#10004;Observação 4,5|&#10004;Observação 4, 5|&#10004;Observação 4, 5|&#10004;Observação 4, 5|&#10004;Observação 4, 5|&#10004;Observação 4, 5|
|Suporte de CORTE|2019, 2016, 2012 R2|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;|||
|SCSI WWN|2019, 2016, 2012 R2|&#10004;|&#10004;|&#10004;|&#10004;|||||
|**[Memória](Feature-Descriptions-for-Linux-and-FreeBSD-virtual-machines-on-Hyper-V.md#memory)**||||
|Suporte do Kernel PAE|2019, 2016, 2012 R2, 2012, 2008 R2|N/D|N/D|N/D|N/D|N/D|N/D|N/D|N/D|N/D|
|Configuração de lacuna MMIO|2019, 2016, 2012 R2|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;|
|Memória dinâmica - quente|2019, 2016, 2012 R2, 2012|&#10004;Observe a 8, 9, 10|&#10004;Observe a 8, 9, 10|&#10004;Observe a 8, 9, 10|&#10004;Observação 9, 10|&#10004;Observação 9, 10|&#10004;Observação 9, 10|&#10004;Observação 9, 10|&#10004;Observação 9, 10|&#10004;Observe a 8, 9, 10|
|Memória dinâmica - inflação|2019, 2016, 2012 R2, 2012|&#10004;Observe a 8, 9, 10|&#10004;Observe a 8, 9, 10|&#10004;Observe a 8, 9, 10|&#10004;Observação 9, 10|&#10004;Observação 9, 10|&#10004;Observação 9, 10|&#10004;Observação 9, 10|&#10004;Observação 9, 10|&#10004;Observe a 8, 9, 10|
|Redimensionamento de memória de tempo de execução|2019, 2016|&#10004;|&#10004;|&#10004;|||||||
|**[Vídeo](Feature-Descriptions-for-Linux-and-FreeBSD-virtual-machines-on-Hyper-V.md#video)**||||
|Dispositivo de vídeo específico do Hyper-V|2019, 2016, 2012 R2, 2012, 2008 R2|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;|
|**[Diversos](Feature-Descriptions-for-Linux-and-FreeBSD-virtual-machines-on-Hyper-V.md#miscellaneous)**||||
|Par chave-valor|2019, 2016, 2012 R2, 2012, 2008 R2|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;|
|Interrupção não mascarável|2019, 2016, 2012 R2|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;|
|Cópia do arquivo do host para a convidada|2019, 2016, 2012 R2|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;||
|comando lsvmbus|2019, 2016, 2012 R2, 2012, 2008 R2|&#10004;|&#10004;|&#10004;|||||||
|Soquetes do Hyper-V|2019, 2016|&#10004;|&#10004;|&#10004;|||||||
|Passagem/DDA de PCI|2019, 2016|&#10004;|&#10004;||&#10004;|&#10004;|&#10004;||||
|**[Máquinas virtuais de geração 2](Feature-Descriptions-for-Linux-and-FreeBSD-virtual-machines-on-Hyper-V.md#generation-2-virtual-machines)**|||||
|Inicialização usando UEFI|2019, 2016, 2012 R2|&#10004;Observação 14|&#10004;Observação 14|&#10004;Observação 14|&#10004;Observação 14|&#10004;Observação 14|&#10004;Observação 14|&#10004;Observação 14|&#10004;Observação 14|&#10004;Observação 14|
|Inicialização segura|2019, 2016|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;|

## <a name="BKMK_6x"></a>RHEL/CentOS 6.x Series

O kernel de 32 bits para esta série é PAE habilitada. Não há nenhum suporte interno do LIS para o RHEL/CentOS 6.0-6.3.

|**Recurso**|**Versão do Windows Server**|**6.4-6.10**|**6.0-6.3**|**6.10, 6.9, 6.8**|**6.6, 6.7**|**6.5**|**6.4**|
|-|-|-|-|-|-|-|-|
|**Disponibilidade**||[LIS 4.2](https://www.microsoft.com/download/details.aspx?id=55106)|[LIS 4.2](https://www.microsoft.com/download/details.aspx?id=55106)|Criado|Criado|Criado|Criado|
|**[Core](Feature-Descriptions-for-Linux-and-FreeBSD-virtual-machines-on-Hyper-V.md#core)**|2019, 2016, 2012 R2, 2012, 2008 R2|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;|
|Hora precisa do Windows Server 2016|2019, 2016||||||
|**[Sistema de rede](Feature-Descriptions-for-Linux-and-FreeBSD-virtual-machines-on-Hyper-V.md#networking)**|
|Quadros jumbo|2019, 2016, 2012 R2, 2012, 2008 R2|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;|
|Marcação de VLAN e entroncamento|2019, 2016, 2012 R2, 2012, 2008 R2|&#10004;Observação 1|&#10004;Observação 1|&#10004;Observação 1|&#10004;Observação 1|&#10004;Observação 1|&#10004;Observação 1|
|Migração ao vivo|2019, 2016, 2012 R2, 2012, 2008 R2|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;|
|Injeção de endereço IP estático|2019, 2016, 2012 R2, 2012|&#10004;Observação 2|&#10004;Observação 2|&#10004;Observação 2|&#10004;Observação 2|&#10004;Observação 2|&#10004;Observação 2|
|vRSS|2019, 2016, 2012 R2|&#10004;|&#10004;|&#10004;|&#10004;|||
|Descarregamento de soma de verificação e de segmentação TCP|2019, 2016, 2012 R2, 2012, 2008 R2|&#10004;|&#10004;|&#10004;|&#10004;|||
|SR-IOV|2019, 2016|||||||
|**[Armazenamento](Feature-Descriptions-for-Linux-and-FreeBSD-virtual-machines-on-Hyper-V.md#storage)**|
|Redimensionamento VHDX|2019, 2016, 2012 R2|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;||
|Fibre Channel Virtual|2019, 2016, 2012 R2|&#10004;Observação 3|&#10004;Observação 3|&#10004;Observação 3|&#10004;Observação 3|&#10004;Observação 3||
|Backup de máquina virtual ao vivo|2019, 2016, 2012 R2|&#10004;Observação 5|&#10004;Observação 5|&#10004;Observação 4, 5|&#10004;Observação 4, 5|&#10004;Observe a 4, 5, 6|&#10004;Observe a 4, 5, 6|
|Suporte de CORTE|2019, 2016, 2012 R2|&#10004;|&#10004;|&#10004;||||
|SCSI WWN|2019, 2016, 2012 R2|&#10004;|&#10004;|||||
|**[Memória](Feature-Descriptions-for-Linux-and-FreeBSD-virtual-machines-on-Hyper-V.md#memory)**|
|Suporte do Kernel PAE|2019, 2016, 2012 R2, 2012, 2008 R2|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;|
|Configuração de lacuna MMIO|2019, 2016, 2012 R2|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;|
|Memória dinâmica - quente|2019, 2016, 2012 R2, 2012|&#10004;Observe a 7, 9, 10|&#10004;Observe a 7, 9, 10|&#10004;Observe a 7, 9, 10|&#10004;Observe a 7, 8, 9, 10|&#10004;Observe a 7, 8, 9, 10||
|Memória dinâmica - inflação|2019, 2016, 2012 R2, 2012|&#10004;Observe a 7, 9, 10|&#10004;Observe a 7, 9, 10|&#10004;Observe a 7, 9, 10|&#10004;Observe a 7, 9, 10|&#10004;Observe a 7, 9, 10|&#10004;Observe a 7, 9, 10, 11|
|Redimensionamento de memória de tempo de execução|2019, 2016|||||||
|**[Vídeo](Feature-Descriptions-for-Linux-and-FreeBSD-virtual-machines-on-Hyper-V.md#video)**|
|Dispositivo de vídeo específico do Hyper-V|2019, 2016, 2012 R2, 2012, 2008 R2|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;||
|**[Diversos](Feature-Descriptions-for-Linux-and-FreeBSD-virtual-machines-on-Hyper-V.md#miscellaneous)**|
|Par chave-valor|2019, 2016, 2012 R2, 2012, 2008 R2|&#10004;|&#10004;|&#10004;Observação 12|&#10004;Observação 12|&#10004;Observação 12, 13|&#10004;Observação 12, 13|
|Interrupção não mascarável|2019, 2016, 2012 R2|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;|
|Cópia do arquivo do host para a convidada|2019, 2016, 2012 R2|&#10004;|&#10004;|&#10004;|&#10004;|||
|comando lsvmbus|2019, 2016, 2012 R2, 2012, 2008 R2|&#10004;|&#10004;||||||
|Soquetes do Hyper-V|2019, 2016|&#10004;|&#10004;|||||
|Passagem/DDA de PCI|2019, 2016|||||||
|**[Máquinas virtuais de geração 2](Feature-Descriptions-for-Linux-and-FreeBSD-virtual-machines-on-Hyper-V.md#generation-2-virtual-machines)**|
|Inicialização usando UEFI|2012 R2|||||||
||2019, 2016|&#10004;Observação 14|&#10004;Observação 14|&#10004;Observação 14||||
|Inicialização segura|2019, 2016||||||

## <a name="BKMK_5x"></a>RHEL/CentOS 5.x Series

Esta série tem um kernel de PAE de 32 bits com suporte disponível. Não há nenhum suporte interno do LIS para o RHEL/CentOS antes 5.9.

|**Recurso**|**Versão do Windows Server**|5.2 -5.11|**5.2-5.11**|**5.9 - 5.11**|
|-|-|-|-|-|
|**Disponibilidade**||[LIS 4.2](https://www.microsoft.com/download/details.aspx?id=55106)|[LIS 4.1](https://www.microsoft.com/download/details.aspx?id=51612)|Criado|
|**[Core](Feature-Descriptions-for-Linux-and-FreeBSD-virtual-machines-on-Hyper-V.md#core)**|2019, 2016, 2012 R2, 2012, 2008 R2|&#10004;|&#10004;|&#10004;|
|Hora precisa do Windows Server 2016|2019, 2016||||
|**[Sistema de rede](Feature-Descriptions-for-Linux-and-FreeBSD-virtual-machines-on-Hyper-V.md#networking)**|
|Quadros jumbo|2019, 2016, 2012 R2, 2012, 2008 R2|&#10004;|&#10004;|&#10004;|
|Marcação de VLAN e entroncamento|2019, 2016, 2012 R2, 2012, 2008 R2|&#10004;Observação 1|&#10004;Observação 1|&#10004;Observação 1|
|Migração ao vivo|2019, 2016, 2012 R2, 2012, 2008 R2|&#10004;|&#10004;|&#10004;|
|Injeção de endereço IP estático|2019, 2016, 2012 R2, 2012|&#10004;Observação 2|&#10004;Observação 2|&#10004;Observação 2|
|vRSS|2019, 2016, 2012 R2||||
|Descarregamento de soma de verificação e de segmentação TCP|2019, 2016, 2012 R2, 2012, 2008 R2|&#10004;|&#10004;||
|SR-IOV|2019, 2016||||||
|**[Armazenamento](Feature-Descriptions-for-Linux-and-FreeBSD-virtual-machines-on-Hyper-V.md#storage)**|
|Redimensionamento VHDX|2019, 2016, 2012 R2|&#10004;|&#10004;||
|Fibre Channel Virtual|2019, 2016, 2012 R2|&#10004;Observação 3|&#10004;Observação 3||
|Backup de máquina virtual ao vivo|2019, 2016, 2012 R2|&#10004;Observação 5, 15|&#10004;Observação 5|&#10004;Observe a 4, 5, 6|
|Suporte de CORTE|2019, 2016, 2012 R2||||
|SCSI WWN|2019, 2016, 2012 R2||||
|**[Memória](Feature-Descriptions-for-Linux-and-FreeBSD-virtual-machines-on-Hyper-V.md#memory)**|
|Suporte do Kernel PAE|2019, 2016, 2012 R2, 2012, 2008 R2|&#10004;|&#10004;|&#10004;|
|Configuração de lacuna MMIO|2019, 2016, 2012 R2|&#10004;|&#10004;|&#10004;|
|Memória dinâmica - quente|2019, 2016, 2012 R2, 2012||||
|Memória dinâmica - inflação|2019, 2016, 2012 R2, 2012|&#10004;Observe a 7, 9, 10, 11|&#10004;Observe a 7, 9, 10, 11||
|Redimensionamento de memória de tempo de execução|2019, 2016||||
|**[Vídeo](Feature-Descriptions-for-Linux-and-FreeBSD-virtual-machines-on-Hyper-V.md#video)**|
|Dispositivo de vídeo específico do Hyper-V|2019, 2016, 2012 R2, 2012, 2008 R2|&#10004;|&#10004;||
|**[Diversos](Feature-Descriptions-for-Linux-and-FreeBSD-virtual-machines-on-Hyper-V.md#miscellaneous)**|
|Par chave-valor|2019, 2016, 2012 R2, 2012, 2008 R2|&#10004;|&#10004;||
|Interrupção não mascarável|2019, 2016, 2012 R2|&#10004;|&#10004;|&#10004;|
|Cópia do arquivo do host para a convidada|2019, 2016, 2012 R2|&#10004;|&#10004;||
|comando lsvmbus|2019, 2016, 2012 R2, 2012, 2008 R2||||
|Soquetes do Hyper-V|2019, 2016||||
|Passagem/DDA de PCI|2019, 2016||||||
|**[Máquinas virtuais de geração 2](Feature-Descriptions-for-Linux-and-FreeBSD-virtual-machines-on-Hyper-V.md#generation-2-virtual-machines)**|
|Inicialização usando UEFI|2019, 2016, 2012 R2||||
|Inicialização segura|2019, 2016||||

## <a name="BKMK_notes"></a>Notas

1. Para este liberar o RHEL/CentOS, funciona de marcação de VLAN, mas não os troncos VLAN.

2. Injeção de IP estática pode não funcionar se o Gerenciador de rede foi configurado para um adaptador de rede sintéticos na máquina virtual. Para o bom funcionamento do endereço IP estático injeção Certifique-se de que o Gerenciador de rede está desligado completamente ou foi desativado para um adaptador de rede específico por meio de seu arquivo ifcfg ethX.

3. No Windows Server 2012 R2, ao usar dispositivos virtuais de fibre channel, certifique-se de que o número de unidade lógica (LUN 0) de 0 foi preenchido. Se o LUN 0 não foi preenchido, uma máquina virtual Linux pode não ser capaz de montar dispositivos fibre channel nativamente.

4. Para LIS internos, o pacote de "daemons do Hyper-v" deve ser instalado para essa funcionalidade.

5. Se houver identificadores de arquivos abertos durante uma operação de backup de máquina virtual ao vivo e, em seguida, em alguns casos, os VHDs de backup talvez precise passar por uma verificação de consistência de sistema de arquivos (fsck) na restauração. Operações de backup ao vivo poderá falhar, silenciosamente, se a máquina virtual tem um dispositivo iSCSI anexado ou armazenamento anexado direto (também conhecido como um disco de passagem).

6. Enquanto o download do Integration Services do Linux é preferencial, live suporte de backup para o RHEL/CentOS 5.9 – 5.11/6.4/6.5 também está disponível por meio [conceitos básicos de Backup do Hyper-V para Linux](https://github.com/LIS/backupessentials/tree/1.0).

7. Suporte de memória dinâmica só está disponível em máquinas virtuais de 64 bits.

8. Suporte a quente não está habilitado por padrão na distribuição. Para habilitar o suporte a quente, você precisará adicionar uma regra de udev em /etc/udev/rules.d/ da seguinte maneira:

   1. Crie um arquivo **/etc/udev/rules.d/100-balloon.rules**. Você pode usar qualquer nome desejado para o arquivo.

   2. Adicione o seguinte conteúdo ao arquivo: `SUBSYSTEM=="memory", ACTION=="add", ATTR{state}="online"`

   3. Reinicialize o sistema para habilitar o suporte a quente.

   Enquanto o download do Integration Services do Linux cria essa regra na instalação, a regra também será removida quando LIS é desinstalado, portanto, a regra deve ser recriada caso a memória dinâmica é necessária após a desinstalação.

9. Operações de memória dinâmica podem falhar se o sistema operacional convidado é com muito pouco memória. A seguir estão algumas práticas recomendadas:

   * Memória de inicialização e um mínimo de memória devem ser igual ou maior que a quantidade de memória que recomenda o fornecedor de distribuição.

   * Aplicativos que tendem a consumir toda memória disponível em um sistema estão limitados a consumir até 80 por cento de RAM disponível.

10. Se você estiver usando a memória dinâmica em um sistema operacional Windows Server 2016 ou Windows Server 2012 R2, especifique **memória de inicialização**, **memória mínima**, e **memória máxima** parâmetros em múltiplos de 128 megabytes (MB). Falha ao fazer isso pode levar a inclusão automática de falhas e talvez você não veja nenhuma memória aumentam em um sistema operacional convidado.

11. Determinadas distribuições, incluindo aqueles que usam a LIS 4.0 e 4.1, somente dão suporte a Inchamento e não dão suporte a quente. Nesse cenário, o recurso de memória dinâmica pode ser usado definindo o parâmetro de memória de inicialização para um valor que é igual ao parâmetro de máximo de memória. Isso resulta em toda a memória necessária que está sendo adicionado quente para a máquina virtual em tempo de inicialização e, em seguida, dependendo dos requisitos de memória do host, do Hyper-V pode livremente alocar ou desalocar a memória do convidado usando Inchamento. Configure **memória de inicialização** e **memória mínima** ou acima do valor recomendado para a distribuição.

12. Para habilitar a infraestrutura de par (KVP) de chave/valor, instale o pacote rpm hypervkvpd ou daemons do Hyper-v de seu ISO RHEL. Como alternativa, o pacote pode ser instalado diretamente de repositórios do RHEL.

13. A infraestrutura de chave/valor par (KVP) pode não funcionar corretamente sem uma atualização de software do Linux. Entre em contato com seu fornecedor de distribuição para obter a atualização de software caso você veja problemas com esse recurso.

14. No Windows Server 2012 R2 geração 2 máquinas virtuais têm a inicialização segura habilitada por padrão e algumas máquinas virtuais do Linux não será inicializado, a menos que a opção de inicialização segura está desabilitada. Você pode desabilitar a inicialização segura na **Firmware** seção das configurações para a máquina virtual na **Gerenciador do Hyper-V** ou você pode desabilitá-lo usando o Powershell:

   ```Powershell
   Set-VMFirmware -VMName "VMname" -EnableSecureBoot Off

   ```

   O download do Integration Services do Linux pode ser aplicado a VMs da geração 2 existente, mas não é demonstrar o recurso de geração 2.

15. No Red Hat Enterprise Linux ou CentOS 5.2, 5.3 e 5.4 a funcionalidade congelamento de sistema de arquivos não está disponível, portanto, o Backup da máquina Virtual ao vivo também não está disponível.

Consulte também

* [Set-VMFirmware](https://technet.microsoft.com/library/dn464287.aspx)

* [Máquinas virtuais do Debian com suporte no Hyper-V](Supported-Debian-virtual-machines-on-Hyper-V.md)

* [Máquinas de virtuais do Oracle Linux com suporte no Hyper-V](Supported-Oracle-Linux-virtual-machines-on-Hyper-V.md)

* [De máquinas virtuais SUSE com suporte no Hyper-V](Supported-SUSE-virtual-machines-on-Hyper-V.md)

* [Máquinas virtuais do Ubuntu com suporte no Hyper-V](Supported-Ubuntu-virtual-machines-on-Hyper-V.md)

* [Máquinas de virtuais FreeBSD com suporte no Hyper-V](Supported-FreeBSD-virtual-machines-on-Hyper-V.md)

* [Descrições de recursos para máquinas virtuais de Linux e FreeBSD no Hyper-V](Feature-Descriptions-for-Linux-and-FreeBSD-virtual-machines-on-Hyper-V.md)

* [Práticas recomendadas para executar o Linux no Hyper-V](Best-Practices-for-running-Linux-on-Hyper-V.md)

* [Certificação de Hardware do Red Hat](https://hardware.redhat.com/&quicksearch=Microsoft)
