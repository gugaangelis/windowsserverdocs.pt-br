---
title: Suporte para máquinas virtuais do Debian no Hyper-V
description: Lista os serviços de integração do Linux e os recursos incluídos em cada versão
ms.prod: windows-server-threshold
ms.service: na
manager: dongill
ms.technology: compute-hyper-v
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 3cc62c10-02a3-4633-960c-23bf91a45bd5
author: shirgall
ms.author: kathydav
ms.date: 10/03/2016
ms.openlocfilehash: 6ec089f501a0999a4460501dbc4d03428d36af40
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59863827"
---
# <a name="supported-debian-virtual-machines-on-hyper-v"></a>Suporte para máquinas virtuais do Debian no Hyper-V

>Aplica-se a: Windows Server 2016, Hyper-V Server 2016, Windows Server 2012 R2, Hyper-V Server 2012 R2, Windows Server 2012, do Hyper-V Server 2012, Windows Server 2008 R2, Windows 10, Windows 8.1, Windows 8, Windows 7.1, Windows 7

O mapa de distribuição de recurso a seguir indica os recursos que estão presentes em cada versão. Os problemas conhecidos e soluções alternativas para cada distribuição são listadas após a tabela.

## <a name="table-legend"></a>Legenda da tabela

* **Criado** -LIS são incluídos como parte de sua distribuição Linux. O pacote de download do LIS fornecidas pela Microsoft não funciona para essa distribuição, portanto, não as instale. Os números de versão do módulo de kernel para a LIS interna (conforme mostrado pelas **lsmod**, por exemplo) são diferentes do número da versão do pacote de download do LIS fornecidas pela Microsoft. Uma incompatibilidade não indica a LIS interna está desatualizada.

* &#10004;-Recurso disponível

* (*em branco*)-recurso não disponível

|**Recurso**|**Versão do sistema operacional Windows Server**|**9.0-9.6 (extensão)**|**8.0-8.11 (jessie)**|**7.0-7.11 (wheezy)**|
|-|-|-|-|-|
|**Disponibilidade**||Criado|Criado|Interno (nota 6)|
|**[Core](Feature-Descriptions-for-Linux-and-FreeBSD-virtual-machines-on-Hyper-V.md#BKMK_core)**|2019, 2016, 2012 R2, 2012, 2008 R2|&#10004;|&#10004;|&#10004;|
|Hora precisa do Windows Server 2016|2019, 2016|&#10004;Observação 8||
|**[Sistema de rede](Feature-Descriptions-for-Linux-and-FreeBSD-virtual-machines-on-Hyper-V.md#BKMK_Networking)**|
|Quadros jumbo|2019, 2016, 2012 R2, 2012, 2008 R2|&#10004;|&#10004;|&#10004;|
|Marcação de VLAN e entroncamento|2019, 2016, 2012 R2, 2012, 2008 R2|&#10004;|&#10004;|&#10004;|
|Migração ao vivo|2019, 2016, 2012 R2, 2012, 2008 R2|&#10004;|&#10004;|&#10004;|
|Injeção de endereço IP estático|2019, 2016, 2012 R2, 2012|||
|vRSS|2019, 2016, 2012 R2|&#10004;Observação 8|||
|Descarregamento de soma de verificação e de segmentação TCP|2019, 2016, 2012 R2, 2012, 2008 R2|&#10004;Observação 8|||
|SR-IOV|2019, 2016|&#10004;Observação 8||
|**[Armazenamento](Feature-Descriptions-for-Linux-and-FreeBSD-virtual-machines-on-Hyper-V.md#BKMK_Storage)**|
|Redimensionamento VHDX|2019, 2016, 2012 R2|&#10004;Observação 1|&#10004;Observação 1|&#10004;Observação 1|
|Fibre Channel Virtual|2019, 2016, 2012 R2|||
|Backup de máquina virtual ao vivo|2019, 2016, 2012 R2|&#10004;Observação 4,5|&#10004;Observação 4,5|&#10004;Observação 4|
|Suporte de CORTE|2019, 2016, 2012 R2|&#10004;Observação 8|||
|SCSI WWN|2019, 2016, 2012 R2|&#10004;Observação 8||
|**[Memória](Feature-Descriptions-for-Linux-and-FreeBSD-virtual-machines-on-Hyper-V.md#BKMK_Memory)**|
|Suporte do Kernel PAE|2019, 2016, 2012 R2, 2012, 2008 R2|&#10004;|&#10004;|&#10004;|
|Configuração de lacuna MMIO|2019, 2016, 2012 R2|&#10004;|&#10004;|&#10004;|
|Memória dinâmica - quente|2019, 2016, 2012 R2, 2012|&#10004;Observação 8|||
|Memória dinâmica - inflação|2019, 2016, 2012 R2, 2012|&#10004;Observação 8|||
|Redimensionamento de memória de tempo de execução|2019, 2016|&#10004;Observação 8|||
|**[Vídeo](Feature-Descriptions-for-Linux-and-FreeBSD-virtual-machines-on-Hyper-V.md#BKMK_Video)**|
|Dispositivo de vídeo específico do Hyper-V|2019, 2016, 2012 R2, 2012, 2008 R2|&#10004;|&#10004;||
|**[Diversos](Feature-Descriptions-for-Linux-and-FreeBSD-virtual-machines-on-Hyper-V.md#BKMK_Misc)**|
|Par chave-valor|2019, 2016, 2012 R2, 2012, 2008 R2|&#10004;Observação 4|&#10004;Observação 4||
|Interrupção não mascarável|2019, 2016, 2012 R2|&#10004;|&#10004;|
|Cópia do arquivo do host para a convidada|2019, 2016, 2012 R2|&#10004;Observação 4|&#10004;Observação 4||
|comando lsvmbus|2019, 2016, 2012 R2, 2012, 2008 R2|||
|Soquetes do Hyper-V|2019, 2016|&#10004;Observação 8|||
|Passagem/DDA de PCI|2019, 2016|&#10004;Observação 8|||
|**[Máquinas virtuais de geração 2](Feature-Descriptions-for-Linux-and-FreeBSD-virtual-machines-on-Hyper-V.md#BKMK_gen2)**|
|Inicialização usando UEFI|2019, 2016, 2012 R2|&#10004;Observação 7|&#10004;Observação 7||
|Inicialização segura|2019, 2016|||

## <a name="BKMK_notes"></a>Notas

1. Não há suporte para a criação de sistemas de arquivos maiores que 2TB em VHDs.

2. No Windows Server 2008 R2 SCSI discos criar 8 entradas diferentes no/DES/sd *.

3. Windows Server 2012 R2 uma VM com 8 núcleos ou mais terá todas as interrupções roteadas para uma única vCPU.

4. A partir do Debian 8.3 os pacote Debian instalados manualmente "Hyper-v-daemons" contém o par chave-valor, fcopy e daemons do VSS. No Debian 7. x e 8.0 8.2 o pacote de daemons do Hyper-v deve ser provenientes [backports Debian](https://wiki.debian.org/Backports).

5. Backup de máquina virtual ao vivo não funcionará com sistemas de arquivos ext2. O layout padrão criado pelo instalador Debian inclui sistemas de arquivos ext2, você deve personalizar o layout para não criar esse tipo de sistema de arquivos.

6. Enquanto o Debian 7. x está fora do suporte e usa um kernel mais antigo, o kernel incluído nos [backports Debian](https://wiki.debian.org/Backports) para Debian 7. x tem aprimorado de recursos do Hyper-V.

7. No Windows Server 2012 R2 geração 2 máquinas virtuais têm a inicialização segura habilitada por padrão e algumas máquinas virtuais do Linux não será inicializado, a menos que a opção de inicialização segura está desabilitada. Você pode desabilitar a inicialização segura na **Firmware** seção das configurações para a máquina virtual na **Gerenciador do Hyper-V** ou você pode desabilitá-lo usando o Powershell:

   ```Powershell
   Set-VMFirmware -VMName "VMname" -EnableSecureBoot Off

   ```
8. Os recursos mais recentes do kernel upstream só estão disponíveis usando o kernel incluído [backports Debian](https://wiki.debian.org/Backports).

Consulte também

* [Suporte para CentOS e Red Hat Enterprise Linux máquinas virtuais do Hyper-V](Supported-CentOS-and-Red-Hat-Enterprise-Linux-virtual-machines-on-Hyper-V.md)

* [Máquinas de virtuais do Oracle Linux com suporte no Hyper-V](Supported-Oracle-Linux-virtual-machines-on-Hyper-V.md)

* [De máquinas virtuais SUSE com suporte no Hyper-V](Supported-SUSE-virtual-machines-on-Hyper-V.md)

* [Máquinas virtuais do Ubuntu com suporte no Hyper-V](Supported-Ubuntu-virtual-machines-on-Hyper-V.md)

* [Máquinas de virtuais FreeBSD com suporte no Hyper-V](Supported-FreeBSD-virtual-machines-on-Hyper-V.md)

* [Descrições de recursos para máquinas virtuais de Linux e FreeBSD no Hyper-V](Feature-Descriptions-for-Linux-and-FreeBSD-virtual-machines-on-Hyper-V.md)

* [Práticas recomendadas para executar o Linux no Hyper-V](Best-Practices-for-running-Linux-on-Hyper-V.md)
