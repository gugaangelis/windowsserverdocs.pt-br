---
title: Máquinas virtuais Debian com suporte no Hyper-V
description: Lista os serviços e recursos de integração do Linux incluídos em cada versão
ms.prod: windows-server
ms.service: na
manager: dongill
ms.technology: compute-hyper-v
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 3cc62c10-02a3-4633-960c-23bf91a45bd5
author: shirgall
ms.author: kathydav
ms.date: 10/03/2016
ms.openlocfilehash: 7a717acf5c132d68d6ee041aeb5af5a430aa171b
ms.sourcegitcommit: 9f7cc76b8c9add44dcbbd97f77b4f881d5a2c073
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/02/2020
ms.locfileid: "80613002"
---
# <a name="supported-debian-virtual-machines-on-hyper-v"></a>Máquinas virtuais Debian com suporte no Hyper-V

>Aplica-se a: Windows Server 2016, Hyper-V Server 2016, Windows Server 2012 R2, Hyper-V Server 2012 R2, Windows Server 2012, Hyper-V Server 2012, Windows Server 2008 R2, Windows 10, Windows 8.1, Windows 8, Windows 7,1, Windows 7

O mapa de distribuição de recursos a seguir indica os recursos que estão presentes em cada versão. Os problemas conhecidos e as soluções alternativas para cada distribuição são listados após a tabela.

## <a name="table-legend"></a>Legenda da tabela

* A LIS **interna** é incluída como parte dessa distribuição do Linux. O pacote de download do LIS fornecido pela Microsoft não funciona para essa distribuição, portanto não o instale. Os números de versão do módulo do kernel para a LIS interna (conforme mostrado por **lsmod**, por exemplo) são diferentes do número de versão no pacote de download do LIS fornecido pela Microsoft. Uma incompatibilidade não indica que a LIS interna está desatualizada.

* &#10004;-Recurso disponível

* (*em branco*)-recurso não disponível

| **Recurso**                                                                                                                                  | **Versão do sistema operacional Windows Server** | **10.0-10.3 (Buster)** | **9.0-9.12 (Stretch)** | **8.0-8.11 (Jessie)** | **7.0-7.11 (Wheezy)** |
|----------------------------------------------------------------------------------------------------------------------------------------------|---------------------------------------------|-----------------------|-----------------------|-----------------------|-----------------------|
| **Disponibilidade**                                                                                                                             |                                             | Interno              | Interno              | Interno              | Interno (observação 6)     |
| **[Core](Feature-Descriptions-for-Linux-and-FreeBSD-virtual-machines-on-Hyper-V.md#core)**                                                   | 2019, 2016, 2012 R2, 2012, 2008 R2          | &#10004;              | &#10004;              | &#10004;              | &#10004;              |
| Tempo preciso do Windows Server 2016                                                                                                            | 2019, 2016                                  | &#10004;Nota 8       | &#10004;Nota 8       |                       |                       |
| **[Rede](Feature-Descriptions-for-Linux-and-FreeBSD-virtual-machines-on-Hyper-V.md#networking)**                                       |                                             |                       |                       |                       |                       |
| Quadros jumbo                                                                                                                                 | 2019, 2016, 2012 R2, 2012, 2008 R2          | &#10004;              | &#10004;              | &#10004;              | &#10004;              |
| Marcação e entroncamento de VLAN                                                                                                                    | 2019, 2016, 2012 R2, 2012, 2008 R2          | &#10004;              | &#10004;              | &#10004;              | &#10004;              |
| Migração ao vivo                                                                                                                               | 2019, 2016, 2012 R2, 2012, 2008 R2          | &#10004;              | &#10004;              | &#10004;              | &#10004;              |
| Injeção de IP estático                                                                                                                          | 2019, 2016, 2012 R2, 2012                   |                       |                       |                       |                       |
| vRSS                                                                                                                                         | 2019, 2016, 2012 R2                         | &#10004;Nota 8       | &#10004;Nota 8       |                       |                       |
| Segmentação de TCP e descarregamentos de soma de verificação                                                                                                       | 2019, 2016, 2012 R2, 2012, 2008 R2          | &#10004;Nota 8       | &#10004;Nota 8       |                       |                       |
| SR-IOV                                                                                                                                       | 2019, 2016                                  | &#10004;Nota 8       | &#10004;Nota 8       |                       |                       |
| **[Repositório](Feature-Descriptions-for-Linux-and-FreeBSD-virtual-machines-on-Hyper-V.md#storage)**                                             |                                             |                       |                       |                       |                       |
| Redimensionamento de VHDX                                                                                                                                  | 2019, 2016, 2012 R2                         | &#10004;Observação 1       | &#10004;Observação 1       | &#10004;Observação 1       | &#10004;Observação 1       |
| Fibre Channel Virtual                                                                                                                        | 2019, 2016, 2012 R2                         |                       |                       |                       |                       |
| Backup de máquina virtual ao vivo                                                                                                                  | 2019, 2016, 2012 R2                         | &#10004;Observação 4, 5     | &#10004;Observação 4, 5     | &#10004;Observação 4, 5     | &#10004;Observação 4       |
| Suporte a corte                                                                                                                                 | 2019, 2016, 2012 R2                         | &#10004;Nota 8       | &#10004;Nota 8       |                       |                       |
| WWN DO SCSI                                                                                                                                     | 2019, 2016, 2012 R2                         | &#10004;Nota 8       | &#10004;Nota 8       |                       |                       |
| **[Memória](Feature-Descriptions-for-Linux-and-FreeBSD-virtual-machines-on-Hyper-V.md#memory)**                                               |                                             |                       |                       |                       |                       |
| Suporte ao kernel de PAE                                                                                                                           | 2019, 2016, 2012 R2, 2012, 2008 R2          | &#10004;              | &#10004;              | &#10004;              | &#10004;              |
| Configuração da lacuna de MMIO                                                                                                                    | 2019, 2016, 2012 R2                         | &#10004;              | &#10004;              | &#10004;              | &#10004;              |
| Memória Dinâmica-adição a quente                                                                                                                     | 2019, 2016, 2012 R2, 2012                   | &#10004;Nota 8       | &#10004;Nota 8       |                       |                       |
| Memória Dinâmica-balões                                                                                                                  | 2019, 2016, 2012 R2, 2012                   | &#10004;Nota 8       | &#10004;Nota 8       |                       |                       |
| Redimensionamento de memória de Runtime                                                                                                                        | 2019, 2016                                  | &#10004;Nota 8       | &#10004;Nota 8       |                       |                       |
| **[Monitor](Feature-Descriptions-for-Linux-and-FreeBSD-virtual-machines-on-Hyper-V.md#video)**                                                 |                                             |                       |                       |                       |                       |
| Dispositivo de vídeo específico do Hyper-V                                                                                                                | 2019, 2016, 2012 R2, 2012, 2008 R2          | &#10004;              | &#10004;              | &#10004;              |                       |
| **[Várias](Feature-Descriptions-for-Linux-and-FreeBSD-virtual-machines-on-Hyper-V.md#miscellaneous)**                                 |                                             |                       |                       |                       |                       |
| Par chave-valor                                                                                                                               | 2019, 2016, 2012 R2, 2012, 2008 R2          | &#10004;Observação 4       | &#10004;Observação 4       | &#10004;Observação 4       |                       |
| Interrupção não mascarável                                                                                                                       | 2019, 2016, 2012 R2                         | &#10004;              | &#10004;              | &#10004;              |                       |
| Cópia de arquivo do host para o convidado                                                                                                                 | 2019, 2016, 2012 R2                         | &#10004;Observação 4       | &#10004;Observação 4       | &#10004;Observação 4       |                       |
| comando lsvmbus                                                                                                                              | 2019, 2016, 2012 R2, 2012, 2008 R2          |                       |                       |                       |                       |
| Soquetes do Hyper-V                                                                                                                              | 2019, 2016                                  | &#10004;Nota 8       | &#10004;Nota 8       |                       |                       |
| Passagem de PCI/DDA                                                                                                                          | 2019, 2016                                  | &#10004;Nota 8       | &#10004;Nota 8       |                       |                       |
| **[Máquinas virtuais de geração 2](Feature-Descriptions-for-Linux-and-FreeBSD-virtual-machines-on-Hyper-V.md#generation-2-virtual-machines)** |                                             |                       |                       |                       |                       |
| Inicializar usando UEFI                                                                                                                              | 2019, 2016, 2012 R2                         | &#10004;Observação 7       | &#10004;Observação 7       | &#10004;Observação 7       |                       |
| Inicialização segura                                                                                                                                  | 2019, 2016                                  | &#10004;              |                       |                       |                       |


## <a name="notes"></a><a name="BKMK_notes"></a>Registra

1. Não há suporte para a criação de sistemas de arquivos em VHDs maiores que 2TB.

2. Em discos SCSI do Windows Server 2008 R2, crie 8 entradas diferentes em/dev/SD *.

3. O Windows Server 2012 R2, uma VM com 8 núcleos ou mais, terá todas as interrupções roteadas para um único vCPU.

4. A partir do Debian 8,3, o pacote Debian instalado manualmente "HyperV-daemons" contém o par chave-valor, FCOPY e daemons VSS. No Debian 7. x e 8.0-8.2, o pacote HyperV-daemons deve vir de [backports de Debian](https://wiki.debian.org/Backports).

5. O backup de máquina virtual ao vivo não funcionará com sistemas de arquivos ext2. O layout padrão criado pelo instalador Debian inclui sistemas de arquivos de ext2, você deve personalizar o layout para não criar esse tipo de FileSystem.

6. Enquanto o Debian 7. x está sem suporte e usa um kernel mais antigo, o kernel incluído no Debian [backports](https://wiki.debian.org/Backports) para Debian 7. x aprimorou os recursos do Hyper-V.

7. Em máquinas virtuais do Windows Server 2012 R2 geração 2 têm inicialização segura habilitada por padrão e algumas máquinas virtuais do Linux não serão inicializadas a menos que a opção de inicialização segura esteja desabilitada. Você pode desabilitar a inicialização segura na seção **firmware** das configurações da máquina virtual no Gerenciador do **Hyper-V** ou pode desabilitá-la usando o PowerShell:

   ```Powershell
   Set-VMFirmware -VMName "VMname" -EnableSecureBoot Off

   ```
8. Os recursos mais recentes do kernel de upstream só estão disponíveis usando o kernel inclusos do [Debian](https://wiki.debian.org/Backports).

Veja também

* [Máquinas virtuais CentOS e Red Hat Enterprise Linux com suporte no Hyper-V](Supported-CentOS-and-Red-Hat-Enterprise-Linux-virtual-machines-on-Hyper-V.md)

* [Máquinas virtuais Oracle Linux com suporte no Hyper-V](Supported-Oracle-Linux-virtual-machines-on-Hyper-V.md)

* [Máquinas virtuais SUSE com suporte no Hyper-V](Supported-SUSE-virtual-machines-on-Hyper-V.md)

* [Máquinas virtuais Ubuntu com suporte no Hyper-V](Supported-Ubuntu-virtual-machines-on-Hyper-V.md)

* [Máquinas virtuais FreeBSD com suporte no Hyper-V](Supported-FreeBSD-virtual-machines-on-Hyper-V.md)

* [Descrições de recursos para máquinas virtuais Linux e FreeBSD no Hyper-V](Feature-Descriptions-for-Linux-and-FreeBSD-virtual-machines-on-Hyper-V.md)

* [Práticas recomendadas para executar o Linux no Hyper-V](Best-Practices-for-running-Linux-on-Hyper-V.md)
