---
title: Máquinas virtuais do Debian com suporte no Hyper-V
description: Lista os serviços e recursos de integração do Linux incluídos em cada versão
ms.topic: article
ms.assetid: 3cc62c10-02a3-4633-960c-23bf91a45bd5
ms.author: benarm
author: BenjaminArmstrong
ms.date: 04/07/2020
ms.openlocfilehash: c0ea0a8e9a030c8d35bf3042b16108523753b36b
ms.sourcegitcommit: dd1fbb5d7e71ba8cd1b5bfaf38e3123bca115572
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/17/2020
ms.locfileid: "90746541"
---
# <a name="supported-debian-virtual-machines-on-hyper-v"></a>Máquinas virtuais do Debian com suporte no Hyper-V

>Aplica-se a: Windows Server 2019, Hyper-V Server 2019, Windows Server 2016, Hyper-V Server 2016, Windows Server 2012 R2, Hyper-V Server 2012 R2, Windows 10, Windows 8.1

O mapa de distribuição de recursos a seguir indica os recursos que estão presentes em cada versão. Os problemas conhecidos e as soluções alternativas para cada distribuição são listados após a tabela.

## <a name="table-legend"></a>Legenda da tabela

* A LIS **interna** é incluída como parte dessa distribuição do Linux. O pacote de download do LIS fornecido pela Microsoft não funciona para essa distribuição, portanto não o instale. Os números de versão do módulo do kernel para a LIS interna (conforme mostrado por **lsmod**, por exemplo) são diferentes do número de versão no pacote de download do LIS fornecido pela Microsoft. Uma incompatibilidade não indica que a LIS interna está desatualizada.

* &#10004;-recurso disponível

* (*em branco*)-recurso não disponível

| **Recurso**                                                                                                                                  | **Versão do sistema operacional Windows Server** | **10.0-10.3 (Buster)** | **9.0-9.12 (Stretch)** | **8.0-8.11 (Jessie)** | **7.0-7.11 (Wheezy)** |
|----------------------------------------------------------------------------------------------------------------------------------------------|---------------------------------------------|-----------------------|-----------------------|-----------------------|-----------------------|
| **Disponibilidade**                                                                                                                             |                                             | Interno              | Interno              | Interno              | Interno (Observação 5)     |
| **[Núcleo](Feature-Descriptions-for-Linux-and-FreeBSD-virtual-machines-on-Hyper-V.md#core)**                                                   | 2019, 2016, 2012 R2          | &#10004;              | &#10004;              | &#10004;              | &#10004;              |
| Tempo preciso do Windows Server 2016                                                                                                            | 2019, 2016                                  | &#10004; Observação 4       | &#10004; Observação 4       |                       |                       |
| **[Rede](Feature-Descriptions-for-Linux-and-FreeBSD-virtual-machines-on-Hyper-V.md#networking)**                                       |                                             |                       |                       |                       |                       |
| Quadros jumbo                                                                                                                                 | 2019, 2016, 2012 R2          | &#10004;              | &#10004;              | &#10004;              | &#10004;              |
| Marcação e entroncamento de VLAN                                                                                                                    | 2019, 2016, 2012 R2          | &#10004;              | &#10004;              | &#10004;              | &#10004;              |
| Migração dinâmica                                                                                                                               | 2019, 2016, 2012 R2          | &#10004;              | &#10004;              | &#10004;              | &#10004;              |
| Injeção de IP estático                                                                                                                          | 2019, 2016, 2012 R2                   |                       |                       |                       |                       |
| vRSS                                                                                                                                         | 2019, 2016, 2012 R2                         | &#10004; Observação 4       | &#10004; Observação 4       |                       |                       |
| Segmentação de TCP e descarregamentos de soma de verificação                                                                                                       | 2019, 2016, 2012 R2          | &#10004; Observação 4       | &#10004; Observação 4       |                       |                       |
| SR-IOV                                                                                                                                       | 2019, 2016                                  | &#10004; Observação 4       | &#10004; Observação 4       |                       |                       |
| **[Armazenamento](Feature-Descriptions-for-Linux-and-FreeBSD-virtual-machines-on-Hyper-V.md#storage)**                                             |                                             |                       |                       |                       |                       |
| Redimensionamento de VHDX                                                                                                                                  | 2019, 2016, 2012 R2                         | Observação de &#10004; 1       | Observação de &#10004; 1       | Observação de &#10004; 1       | Observação de &#10004; 1       |
| Fibre Channel Virtual                                                                                                                        | 2019, 2016, 2012 R2                         |                       |                       |                       |                       |
| Backup de máquina virtual ao vivo                                                                                                                  | 2019, 2016, 2012 R2                         | &#10004; NOTE2 | &#10004; NOTE2 | &#10004; NOTE2 | &#10004; NOTE2 |
| Suporte a corte                                                                                                                                 | 2019, 2016, 2012 R2                         | &#10004; Observação 4       | &#10004; Observação 4       |                       |                       |
| WWN DO SCSI                                                                                                                                     | 2019, 2016, 2012 R2                         | &#10004; Observação 4       | &#10004; Observação 4       |                       |                       |
| **[Memória](Feature-Descriptions-for-Linux-and-FreeBSD-virtual-machines-on-Hyper-V.md#memory)**                                               |                                             |                       |                       |                       |                       |
| Suporte ao kernel de PAE                                                                                                                           | 2019, 2016, 2012 R2          | &#10004;              | &#10004;              | &#10004;              | &#10004;              |
| Configuração da lacuna de MMIO                                                                                                                    | 2019, 2016, 2012 R2                         | &#10004;              | &#10004;              | &#10004;              | &#10004;              |
| Memória Dinâmica-adição a quente                                                                                                                     | 2019, 2016, 2012 R2                   | &#10004; Observação 4       | &#10004; Observação 4       |                       |                       |
| Memória Dinâmica-balões                                                                                                                  | 2019, 2016, 2012 R2                   | &#10004; Observação 4       | &#10004; Observação 4       |                       |                       |
| Redimensionamento de memória de Runtime                                                                                                                        | 2019, 2016                                  | &#10004; Observação 4       | &#10004; Observação 4       |                       |                       |
| **[Vídeo](Feature-Descriptions-for-Linux-and-FreeBSD-virtual-machines-on-Hyper-V.md#video)**                                                 |                                             |                       |                       |                       |                       |
| Dispositivo de vídeo específico do Hyper-V                                                                                                                | 2019, 2016, 2012 R2          | &#10004;              | &#10004;              | &#10004;              |                       |
| **[Diversos](Feature-Descriptions-for-Linux-and-FreeBSD-virtual-machines-on-Hyper-V.md#miscellaneous)**                                 |                                             |                       |                       |                       |                       |
| Par chave-valor                                                                                                                               | 2019, 2016, 2012 R2          | Observação de &#10004; 2       | Observação de &#10004; 2       | Observação de &#10004; 2       |                       |
| Interrupção não mascarável                                                                                                                       | 2019, 2016, 2012 R2                         | &#10004;              | &#10004;              | &#10004;              |                       |
| Cópia de arquivo do host para o convidado                                                                                                                 | 2019, 2016, 2012 R2                         | Observação de &#10004; 2       | Observação de &#10004; 2       | Observação de &#10004; 2       |                       |
| comando lsvmbus                                                                                                                              | 2019, 2016, 2012 R2          |                       |                       |                       |                       |
| Soquetes do Hyper-V                                                                                                                              | 2019, 2016                                  | &#10004; Observação 4       | &#10004; Observação 4       |                       |                       |
| Passagem de PCI/DDA                                                                                                                          | 2019, 2016                                  | &#10004; Observação 4       | &#10004; Observação 4       |                       |                       |
| **[Máquinas virtuais de 2ª geração](Feature-Descriptions-for-Linux-and-FreeBSD-virtual-machines-on-Hyper-V.md#generation-2-virtual-machines)** |                                             |                       |                       |                       |                       |
| Inicializar usando UEFI                                                                                                                              | 2019, 2016, 2012 R2                         | Observação de &#10004; 3       | Observação de &#10004; 3       | Observação de &#10004; 3       |                       |
| Inicialização Segura                                                                                                                                  | 2019, 2016                                  | &#10004;              |                       |                       |                       |


## <a name="notes"></a>Observações

1. Não há suporte para a criação de sistemas de arquivos em VHDs maiores que 2TB.

2. A partir do Debian 8,3, o pacote Debian instalado manualmente "HyperV-daemons" contém o par chave-valor, FCOPY e daemons VSS. No Debian 7. x e 8.0-8.2, o pacote HyperV-daemons deve vir de [backports de Debian](https://wiki.debian.org/Backports).

3. Em máquinas virtuais do Windows Server 2012 R2 geração 2 têm inicialização segura habilitada por padrão e algumas máquinas virtuais do Linux não serão inicializadas a menos que a opção de inicialização segura esteja desabilitada. Você pode desabilitar a inicialização segura na seção **firmware** das configurações da máquina virtual no Gerenciador do **Hyper-V** ou pode desabilitá-la usando o PowerShell:

   ```Powershell
   Set-VMFirmware -VMName "VMname" -EnableSecureBoot Off
   ```
4. Os recursos mais recentes do kernel de upstream só estão disponíveis usando o kernel inclusos do [Debian](https://wiki.debian.org/Backports).

5. Enquanto o Debian 7. x está sem suporte e usa um kernel mais antigo, o kernel incluído no Debian backports para Debian 7. x aprimorou os recursos do Hyper-V.

Consulte Também

* [Máquinas virtuais CentOS e Red Hat Enterprise Linux com suporte no Hyper-V](Supported-CentOS-and-Red-Hat-Enterprise-Linux-virtual-machines-on-Hyper-V.md)

* [Máquinas virtuais Oracle Linux com suporte no Hyper-V](Supported-Oracle-Linux-virtual-machines-on-Hyper-V.md)

* [Máquinas virtuais SUSE com suporte no Hyper-V](Supported-SUSE-virtual-machines-on-Hyper-V.md)

* [Máquinas virtuais Ubuntu com suporte no Hyper-V](Supported-Ubuntu-virtual-machines-on-Hyper-V.md)

* [Máquinas virtuais FreeBSD com suporte no Hyper-V](Supported-FreeBSD-virtual-machines-on-Hyper-V.md)

* [Descrições de recursos para máquinas virtuais Linux e FreeBSD no Hyper-V](Feature-Descriptions-for-Linux-and-FreeBSD-virtual-machines-on-Hyper-V.md)

* [Práticas recomendadas para executar o Linux no Hyper-V](Best-Practices-for-running-Linux-on-Hyper-V.md)
