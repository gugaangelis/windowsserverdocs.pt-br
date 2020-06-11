---
title: Máquinas virtuais Oracle Linux com suporte no Hyper-V
description: Lista os serviços e recursos de integração do Linux incluídos em cada versão
ms.prod: windows-server
manager: dongill
ms.technology: compute-hyper-v
ms.topic: article
ms.assetid: c02fdb5b-62f3-43cb-a190-ab74b3ebcf77
author: shirgall
ms.author: kathydav
ms.date: 06/05/2020
ms.openlocfilehash: 67f38d11c032e9eb0b98da14c25e01a5f67cabae
ms.sourcegitcommit: 76a3b5f66e47e08e8235e2d152185b304d03b68b
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/10/2020
ms.locfileid: "84663169"
---
# <a name="supported-oracle-linux-virtual-machines-on-hyper-v"></a>Máquinas virtuais Oracle Linux com suporte no Hyper-V

>Aplica-se a: Windows Server 2019, Windows Server 2016, Hyper-V Server 2016, Windows Server 2012 R2, Hyper-V Server 2012 R2, Windows 10, Windows 8.1

O mapa de distribuição de recursos a seguir indica os recursos que estão presentes em cada versão. Os problemas conhecidos e as soluções alternativas para cada distribuição são listados após a tabela.

Nesta seção:

* [Série Oracle Linux 8. x](#oracle-linux-8x-series)
* [Série Oracle Linux 7. x](#oracle-linux-7x-series)
* [Série Oracle Linux 6. x](#oracle-linux-6x-series)
 
   
## <a name="table-legend"></a>Legenda da tabela

* A LIS **interna** é incluída como parte dessa distribuição do Linux. Os números de versão do módulo do kernel para a LIS interna (conforme mostrado por **lsmod**, por exemplo) são diferentes do número de versão no pacote de download do LIS fornecido pela Microsoft. Uma incompatibilidade não indica que a LIS interna está desatualizada.

* &#10004;-recurso disponível
* (*em branco*)-recurso não disponível
* Kernel **RHCK** -Red Hat compatível
* **UEK** -UEK (inquebrable Enterprise kernel) 
   * UEK4 na versão 4.1.12 do kernel upstream do Linux
   * UEK5 na versão de kernel do Linux upstream 4,14
   * UEK6 na versão de kernel do Linux upstream 5,4

## <a name="oracle-linux-8x-series"></a>Série Oracle Linux 8. x

|       **Recurso**     |       **Versão do Windows Server**      |       **8.0-8.1 (RHCK)** |
|-----------------------|---------------------------------------|-------------------|
|       **Disponibilidade**        |   |
|       **[Núcleo](Feature-Descriptions-for-Linux-and-FreeBSD-virtual-machines-on-Hyper-V.md#core)**      | 2019, 2016, 2012 R2 | &#10004; | 
|       Tempo preciso do Windows Server 2016       | 2019, 2016 | &#10004; | 
|       **[Rede](Feature-Descriptions-for-Linux-and-FreeBSD-virtual-machines-on-Hyper-V.md#networking)**      |   | 
|       Quadros jumbo        | 2019, 2016, 2012 R2 | &#10004; | 
|       Marcação e entroncamento de VLAN       | 2019, 2016, 2012 R2 | &#10004;  | 
|       Migração dinâmica      | 2019, 2016, 2012 R2 | &#10004; |
|       Injeção de IP estático     |  2019, 2016, 2012 R2 | Observação de &#10004; 2 | 
|       vRSS     | 2019, 2016, 2012 R2 | &#10004; |
|       Segmentação de TCP e descarregamentos de soma de verificação | 2019, 2016, 2012 R2 | &#10004;|
|       SR-IOV  | 2019, 2016 |  &#10004;   |
|       **[Armazenamento](Feature-Descriptions-for-Linux-and-FreeBSD-virtual-machines-on-Hyper-V.md#storage)** |  | 
|       Redimensionamento de VHDX  | 2019, 2016, 2012 R2 | &#10004; |
|       Fibre Channel Virtual | 2019, 2016, 2012 R2 | Observação de &#10004; 3  |
|       Backup de máquina virtual ao vivo  | 2019, 2016, 2012 R2 | Observação de &#10004; 5 |
|       Suporte a corte | 2019, 2016, 2012 R2 | &#10004;  |
|       WWN DO SCSI | 2019, 2016, 2012 R2 | &#10004;  |
|       **[Memória](Feature-Descriptions-for-Linux-and-FreeBSD-virtual-machines-on-Hyper-V.md#memory)** | |
|       Suporte ao kernel de PAE  | 2019, 2016, 2012 R2 |  N/D |
|       Configuração da lacuna de MMIO  | 2019, 2016, 2012 R2 | &#10004; | 
|       Memória Dinâmica-adição a quente | 2019, 2016, 2012 R2  | Observação de &#10004; 7, 8, 9 |
|       Memória Dinâmica-balões | 2019, 2016, 2012 R2 | Observação de &#10004; 7, 8, 9 |
|       Redimensionamento de memória de Runtime | 2019, 2016  | &#10004;  |
|       **[Vídeo](Feature-Descriptions-for-Linux-and-FreeBSD-virtual-machines-on-Hyper-V.md#video)** | |
|       Dispositivo de vídeo específico do Hyper-V | 2019, 2016, 2012 R2 | &#10004;   | 
|       **[Diversos](Feature-Descriptions-for-Linux-and-FreeBSD-virtual-machines-on-Hyper-V.md#miscellaneous)** | |
|       Par chave-valor  | 2019, 2016, 2012 R2 | &#10004;   | 
|       Interrupção não mascarável | 2019, 2016, 2012 R2 | &#10004;  | 
|       Cópia de arquivo do host para o convidado | 2019, 2016, 2012 R2 | &#10004;  | 
|       comando lsvmbus | 2019, 2016, 2012 R2 | &#10004;  | 
|       Soquetes do Hyper-V | 2019, 2016 | &#10004;  | 
|       Passagem de PCI/DDA | 2019, 2016 | &#10004; | 
| **[Máquinas virtuais de 2ª geração](Feature-Descriptions-for-Linux-and-FreeBSD-virtual-machines-on-Hyper-V.md#generation-2-virtual-machines)** | |  |
|       Inicializar usando UEFI | 2019, 2016, 2012 R2 |  Nota de &#10004; 12  |   
|       Inicialização segura | 2019, 2016 |  &#10004; | 

## <a name="oracle-linux-7x-series"></a>Série Oracle Linux 7. x

Esta série tem apenas kernels de 64 bits.

<table width="100%">
<tr height="50px">
<td width="20%" rowspan="2">

Recurso
</td>
<td width="20%" rowspan="2">

Versão do Windows Server
</td>
<td width="30%" colspan="3">

7.5-7.8
</td>
<td width="30%" colspan="3">

7.3-7.4
</td>
</tr>
<tr>
<td width="20%" colspan="2">

RHCK
</td>
<td width="10%">

UEK5
</td>
<td width="20%" colspan="2">

RHCK
</td>
<td width="10%">

UEK4
</td>
</tr>
<tr>
<td width="20%">

Disponibilidade
</td>
<td width="20%">


</td>
<td width="10%">

LIS 4,3
</td>
<td width="10%">

Interno
</td>
<td width="10%">

Interno
</td>
<td width="10%">

LIS 4,3
</td>
<td width="10%">

Interno
</td>
<td width="10%">

Interno
</td>
</tr>
<tr height="50px">
<td width="20%">

**[Núcleo](Feature-Descriptions-for-Linux-and-FreeBSD-virtual-machines-on-Hyper-V.md#core)**
</td>
<td width="20%">

2019, 2016, 2012 R2
</td>
<td width="10%">

&#10004;
</td>
<td width="10%">

&#10004;
</td>
<td width="10%">

&#10004;
</td>
<td width="10%">

&#10004;
</td>
<td width="10%">

&#10004;
</td>
<td width="10%">

&#10004;
</td>
</tr>
<tr height="50px">
<td width="20%">

Tempo preciso do Windows Server 2016
</td>
<td width="20%">

2019, 2016
</td>
<td width="10%">

&#10004;
</td>
<td width="10%">

</td>
<td width="10%">

</td>
<td width="10%">

&#10004;
</td>
<td width="10%">

</td>
<td width="10%">

</td>
</tr>
<tr height="50px">
<td width="20%">

 **[Rede](Feature-Descriptions-for-Linux-and-FreeBSD-virtual-machines-on-Hyper-V.md#networking)** 
</td>
<td width="20%">

</td>
<td width="10%">

</td>
<td width="10%">

</td>
<td width="10%">

</td>
<td width="10%">

</td>
<td width="10%">

</td>
<td width="10%">

</td>
</tr>
<tr height="50px">
<td width="20%">

Quadros jumbo
</td>
<td width="20%">

2019, 2016, 2012 R2
</td>
<td width="10%">

&#10004;
</td>
<td width="10%">

&#10004;
</td>
<td width="10%">

&#10004;
</td>
<td width="10%">

&#10004;
</td>
<td width="10%">

&#10004;
</td>
<td width="10%">

&#10004;
</td>
</tr>
<tr height="50px">
<td width="20%">
Marcação e entroncamento de VLAN
</td>
<td width="20%">
2019, 2016, 2012 R2
</td>
<td width="10%">

&#10004;
</td>
<td width="10%">

&#10004;
</td>
<td width="10%">

&#10004;
</td>
<td width="10%">

&#10004;
</td>
<td width="10%">

&#10004;
</td>
<td width="10%">

&#10004;
</td>
</tr>
<tr height="50px">
<td width="20%">

Migração dinâmica
</td>
<td width="20%">

2019, 2016, 2012 R2
</td>
<td width="10%">

&#10004;
</td>
<td width="10%">

&#10004;
</td>
<td width="10%">

&#10004;
</td>
<td width="10%">

&#10004;
</td>
<td width="10%">

&#10004;
</td>
<td width="10%">

&#10004;
</td>
</tr>
<tr height="50px">
<td width="20%">

Injeção de IP estático
</td>
<td width="20%">

2019, 2016, 2012 R2
</td>
<td width="10%">

Observação de &#10004; 2
</td>
<td width="10%">

Observação de &#10004; 2
</td>
<td width="10%">

Observação de &#10004; 2
</td>
<td width="10%">

Observação de &#10004; 2
</td>
<td width="10%">

Observação de &#10004; 2
</td>
<td width="10%">

Observação de &#10004; 2
</td>
</tr>
<tr height="50px">
<td width="20%">

vRSS
</td>
<td width="20%">

2019, 2016, 2012 R2
</td>
<td width="10%">

&#10004;
</td>
<td width="10%">

&#10004;
</td>
<td width="10%">

&#10004;
</td>
<td width="10%">

&#10004;
</td>
<td width="10%">

&#10004;
</td>
<td width="10%">

&#10004;
</td>
</tr>
<tr height="50px">
<td width="20%">

Segmentação de TCP e descarregamentos de soma de verificação
</td>
<td width="20%">

2019, 2016, 2012 R2
</td>
<td width="10%">

&#10004;
</td>
<td width="10%">

&#10004;
</td>
<td width="10%">

&#10004;
</td>
<td width="10%">

&#10004;
</td>
<td width="10%">

&#10004;
</td>
<td width="10%">

&#10004;
</td>
</tr>
<tr height="50px">
<td width="20%">

SR-IOV
</td>
<td width="20%">

2019, 2016
</td>
<td width="10%">

&#10004;
</td>
<td width="10%">

&#10004;
</td>
<td width="10%">

&#10004;
</td>
<td width="10%">

&#10004;
</td>
<td width="10%">

&#10004;
</td>
<td width="10%">

&#10004;
</td>
</tr>
<tr height="50px">
<td width="20%">

**[Armazenamento](Feature-Descriptions-for-Linux-and-FreeBSD-virtual-machines-on-Hyper-V.md#storage)**
</td>
<td width="20%">

</td>
<td width="10%">

</td>
<td width="10%">

</td>
<td width="10%">

</td>
<td width="10%">

</td>
<td width="10%">

</td>
<td width="10%">

</td>
</tr>
<tr height="50px">
<td width="20%">

Redimensionamento de VHDX
</td>
<td width="20%">

2019, 2016, 2012 R2
</td>
<td width="10%">

&#10004;
</td>
<td width="10%">

&#10004;
</td>
<td width="10%">

&#10004;
</td>
<td width="10%">

&#10004;
</td>
<td width="10%">

&#10004;
</td>
<td width="10%">

&#10004;
</td>
</tr>
<tr height="50px">
<td width="20%">

Fibre Channel Virtual
</td>
<td width="20%">

2019, 2016, 2012 R2
</td>
<td width="10%">

Observação de &#10004; 3
</td>
<td width="10%">

Observação de &#10004; 3
</td>
<td width="10%">

Observação de &#10004; 3
</td>
<td width="10%">

Observação de &#10004; 3
</td>
<td width="10%">

Observação de &#10004; 3
</td>
<td width="10%">

Observação de &#10004; 3
</td>
</tr>
<tr height="50px">
<td width="20%">

Backup de máquina virtual ao vivo
</td>
<td width="20%">

2019, 2016, 2012 R2
</td>
<td width="10%">

Observação de &#10004; 5
</td>
<td width="10%">

&#10004; Observação 4, 5
</td>
<td width="10%">

Observação de &#10004; 5
</td>
<td width="10%">

Observação de &#10004; 5
</td>
<td width="10%">

&#10004; Observação 4, 5
</td>
<td width="10%">

Observação de &#10004; 5
</td>
</tr>
<tr height="50px">
<td width="20%">

Suporte a corte
</td>
<td width="20%">

2019, 2016, 2012 R2
</td>
<td width="10%">

&#10004;
</td>
<td width="10%">

&#10004;
</td>
<td width="10%">

&#10004;
</td>
<td width="10%">

&#10004;
</td>
<td width="10%">

&#10004;
</td>
<td width="10%">

&#10004;
</td>
</tr>
<tr height="50px">
<td width="20%">

WWN DO SCSI
</td>
<td width="20%">

2019, 2016, 2012 R2
</td>
<td width="10%">

&#10004;
</td>
<td width="10%">

&#10004;
</td>
<td width="10%">

&#10004;
</td>
<td width="10%">

&#10004;
</td>
<td width="10%">

&#10004;
</td>
<td width="10%">

</td>
</tr>
<tr height="50px">
<td width="20%">

**[Memória](Feature-Descriptions-for-Linux-and-FreeBSD-virtual-machines-on-Hyper-V.md#memory)**
</td>
<td width="20%">


</td>
<td width="10%">

</td>
<td width="10%">

</td>
<td width="10%">

</td>
<td width="10%">

</td>
<td width="10%">

</td>
<td width="10%">

</td>
</tr>
<tr height="50px">
<td width="20%">

Suporte ao kernel de PAE
</td>
<td width="20%">

2019, 2016, 2012 R2
</td>
<td width="10%">

N/D
</td>
<td width="10%">

N/D
</td>
<td width="10%">

N/D
</td>
<td width="10%">

N/D
</td>
<td width="10%">

N/D
</td>
<td width="10%">

N/D
</td>
</tr>
<tr height="50px">
<td width="20%">

Configuração da lacuna de MMIO
</td>
<td width="20%">

2019, 2016, 2012 R2
</td>
<td width="10%">

&#10004;
</td>
<td width="10%">

&#10004;
</td>
<td width="10%">

&#10004;
</td>
<td width="10%">

&#10004;
</td>
<td width="10%">

&#10004;
</td>
<td width="10%">

&#10004;
</td>
</tr>
<tr height="50px">
<td width="20%">

Adição automática de Memória Dinâmica
</td>
<td width="20%">

2019, 2016, 2012 R2
</td>
<td width="10%">

Observação de &#10004; 7, 8, 9
</td>
<td width="10%">

Nota &#10004; 8, 9
</td>
<td width="10%">

Nota &#10004; 8, 9
</td>
<td width="10%">

Nota &#10004; 8, 9
</td>
<td width="10%">

Nota &#10004; 8, 9
</td>
<td width="10%">

Nota &#10004; 8, 9
</td>
</tr>
<tr height="50px">
<td width="20%">

Memória Dinâmicando o balão
</td>
<td width="20%">

2019, 2016, 2012 R2
</td>
<td width="10%">

Observação de &#10004; 7, 8, 9
</td>
<td width="10%">

Nota &#10004; 8, 9
</td>
<td width="10%">

Nota &#10004; 8, 9
</td>
<td width="10%">

Nota &#10004; 8, 9
</td>
<td width="10%">

Nota &#10004; 8, 9
</td>
<td width="10%">

Nota &#10004; 8, 9
</td>
</tr>
<tr height="50px">
<td width="20%">

Redimensionamento de memória de Runtime
</td>
<td width="20%">

2019, 2016
</td>
<td width="10%">

&#10004;
</td>
<td width="10%">


</td>
<td width="10%">

&#10004;
</td>
<td width="10%">

&#10004;
</td>
<td width="10%">


</td>
<td width="10%">

</td>
</tr>
<tr height="50px">
<td width="20%">

**[Vídeo](Feature-Descriptions-for-Linux-and-FreeBSD-virtual-machines-on-Hyper-V.md#video)**
</td>
<td width="20%">

</td>
<td width="10%">

</td>
<td width="10%">

</td>
<td width="10%">

</td>
<td width="10%">

</td>
<td width="10%">

</td>
<td width="10%">

</td>
</tr>
<tr height="50px">
<td width="20%">

Vídeo específico do Hyper-V
</td>
<td width="20%">

2019, 2016, 2012 R2
</td>
<td width="10%">

&#10004;
</td>
<td width="10%">

&#10004;
</td>
<td width="10%">

&#10004;
</td>
<td width="10%">

&#10004;
</td>
<td width="10%">

&#10004;
</td>
<td width="10%">

&#10004;
</td>
</tr>
<tr height="50px">
<td width="20%">

**[Diversos](Feature-Descriptions-for-Linux-and-FreeBSD-virtual-machines-on-Hyper-V.md#miscellaneous)**
</td>
<td width="20%">

</td>
<td width="10%">

</td>
<td width="10%">

</td>
<td width="10%">

</td>
<td width="10%">

</td>
<td width="10%">

</td>
<td width="10%">

</td>
</tr>
<tr height="50px">
<td width="20%">

Par chave-valor
</td>
<td width="20%">

2019, 2016, 2012 R2
</td>
<td width="10%">

&#10004;
</td>
<td width="10%">

&#10004;
</td>
<td width="10%">

&#10004;
</td>
<td width="10%">

&#10004;
</td>
<td width="10%">

&#10004;
</td>
<td width="10%">

&#10004;
</td>
</tr>
<tr height="50px">
<td width="20%">

Interrupção não mascarável
</td>
<td width="20%">

2019, 2016, 2012 R2
</td>
<td width="10%">

&#10004;
</td>
<td width="10%">

&#10004;
</td>
<td width="10%">

&#10004;
</td>
<td width="10%">

&#10004;
</td>
<td width="10%">

&#10004;
</td>
<td width="10%">

&#10004;
</td>
</tr>
<tr height="50px">
<td width="20%">

Cópia de arquivo do host para o convidado
</td>
<td width="20%">

2019, 2016, 2012 R2
</td>
<td width="10%">

&#10004;
</td>
<td width="10%">

&#10004;
</td>
<td width="10%">

&#10004;
</td>
<td width="10%">

&#10004;
</td>
<td width="10%">

&#10004;
</td>
<td width="10%">

&#10004;
</td>
</tr>
<tr height="50px">
<td width="20%">

comando lsvmbus
</td>
<td width="20%">

2019, 2016, 2012 R2
</td>
<td width="10%">

&#10004;
</td>
<td width="10%">


</td>
<td width="10%">

&#10004;
</td>
<td width="10%">

&#10004;
</td>
<td width="10%">


</td>
<td width="10%">

&#10004;
</td>
</tr>
<tr height="50px">
<td width="20%">

Soquetes do Hyper-V
</td>
<td width="20%">

2019, 2016
</td>
<td width="10%">

&#10004;
</td>
<td width="10%">


</td>
<td width="10%">

&#10004;
</td>
<td width="10%">

&#10004;
</td>
<td width="10%">


</td>
<td width="10%">

&#10004;
</td>
</tr>
<tr height="50px">
<td width="20%">

Passagem de PCI/DDA
</td>
<td width="20%">

2019, 2016
</td>
<td width="10%">

&#10004;
</td>
<td width="10%">

&#10004;
</td>
<td width="10%">

&#10004;
</td>
<td width="10%">

&#10004;
</td>
<td width="10%">

&#10004;
</td>
<td width="10%">

&#10004;
</td>
</tr>
<tr height="50px">
<td width="20%">

**[Máquinas virtuais de 2ª geração](Feature-Descriptions-for-Linux-and-FreeBSD-virtual-machines-on-Hyper-V.md#generation-2-virtual-machines)**
</td>
<td width="20%">

</td>
<td width="10%">

</td>
<td width="10%">

</td>
<td width="10%">

</td>
<td width="10%">

</td>
<td width="10%">

</td>
<td width="10%">

</td>
</tr>
<tr height="50px">
<td width="20%">

Inicializar usando UEFI
</td>
<td width="20%">

2019, 2016, 2012 R2
</td>
<td width="10%">

Nota de &#10004; 12
</td>
<td width="10%">

Nota de &#10004; 12
</td>
<td width="10%">

Nota de &#10004; 12
</td>
<td width="10%">

Nota de &#10004; 12
</td>
<td width="10%">

Nota de &#10004; 12
</td>
<td width="10%">

Nota de &#10004; 12
</td>
</tr>
<tr height="50px">
<td width="20%">

Inicialização segura
</td>
<td width="20%">

2019, 2016, 2012 R2
</td>
<td width="10%">

&#10004;
</td>
<td width="10%">

&#10004;
</td>
<td width="10%">

&#10004;
</td>
<td width="10%">

&#10004;
</td>
<td width="10%">

&#10004;
</td>
<td width="10%">

&#10004;
</td>
</tr>
</table>


## <a name="oracle-linux-6x-series"></a>Série Oracle Linux 6. x

Esta série tem apenas kernels de 64 bits.

|       **Recurso**     |       **Versão do Windows Server**      |       **6.8-6.10 (RHCK)** |       **6.8-6.10 (UEK4)**     | 
|-----------------------|---------------------------------------|-------------------|-------------------|
|       **Disponibilidade**     |   | LIS 4,3  | Interno  |
|       **[Núcleo](Feature-Descriptions-for-Linux-and-FreeBSD-virtual-machines-on-Hyper-V.md#core)**      | 2019, 2016, 2012 R2 | &#10004; | &#10004;
|       Tempo preciso do Windows Server 2016       | 2019, 2016 | | 
|       **[Rede](Feature-Descriptions-for-Linux-and-FreeBSD-virtual-machines-on-Hyper-V.md#networking)**      |   |  |
|       Quadros jumbo        | 2019, 2016, 2012 R2 | &#10004; | &#10004;|
|       Marcação e entroncamento de VLAN       | 2019, 2016, 2012 R2 | Observação de &#10004; 1 | Observação de &#10004; 1 |
|       Migração dinâmica      | 2019, 2016, 2012 R2 | &#10004; | &#10004;|
|       Injeção de IP estático     |  2019, 2016, 2012 R2 | Observação de &#10004; 2 | &#10004;|
|       vRSS     | 2019, 2016, 2012 R2 | &#10004; | &#10004;|
|       Segmentação de TCP e descarregamentos de soma de verificação | 2019, 2016, 2012 R2 | &#10004;|  &#10004; |
|       SR-IOV  | 2019, 2016 |    |  |
|       **[Armazenamento](Feature-Descriptions-for-Linux-and-FreeBSD-virtual-machines-on-Hyper-V.md#storage)** |  |  |
|       Redimensionamento de VHDX  | 2019, 2016, 2012 R2 | &#10004; | &#10004; |
|       Fibre Channel Virtual | 2019, 2016, 2012 R2 | Observação de &#10004; 3  | Observação de &#10004; 3 |
|       Backup de máquina virtual ao vivo  | 2019, 2016, 2012 R2 | Observação de &#10004; 5 | Observação de &#10004; 5|
|       Suporte a corte | 2019, 2016, 2012 R2 | &#10004;  | &#10004; |
|       WWN DO SCSI | 2019, 2016, 2012 R2 | &#10004;  | &#10004; |
|       **[Memória](Feature-Descriptions-for-Linux-and-FreeBSD-virtual-machines-on-Hyper-V.md#memory)** | |  |
|       Suporte ao kernel de PAE  | 2019, 2016, 2012 R2 |  N/D | N/D
|       Configuração da lacuna de MMIO  | 2019, 2016, 2012 R2 | &#10004; | &#10004;  |
|       Memória Dinâmica-adição a quente | 2019, 2016, 2012 R2  | Nota de &#10004; 6, 8, 9 | Nota de &#10004; 6, 8, 9 |
|       Memória Dinâmica-balões | 2019, 2016, 2012 R2 | Nota de &#10004; 6, 8, 9 | Nota de &#10004; 6, 8, 9 |
|       Redimensionamento de memória de Runtime | 2019, 2016  |  | |
|       **[Vídeo](Feature-Descriptions-for-Linux-and-FreeBSD-virtual-machines-on-Hyper-V.md#video)** | | |
|       Dispositivo de vídeo específico do Hyper-V | 2019, 2016, 2012 R2 | &#10004;   | &#10004; |
|       **[Diversos](Feature-Descriptions-for-Linux-and-FreeBSD-virtual-machines-on-Hyper-V.md#miscellaneous)** | | |
|       Par chave-valor  | 2019, 2016, 2012 R2 | Observação de &#10004; 10, 11   | Observação de &#10004; 10, 11  |
|       Interrupção não mascarável | 2019, 2016, 2012 R2 | &#10004;  | &#10004; |
|       Cópia de arquivo do host para o convidado | 2019, 2016, 2012 R2 | &#10004;  | &#10004; |
|       comando lsvmbus | 2019, 2016, 2012 R2 | &#10004;  | &#10004; |
|       Soquetes do Hyper-V | 2019, 2016 | &#10004;  | &#10004; |
|       Passagem de PCI/DDA | 2019, 2016 | &#10004; | &#10004; |
| **[Máquinas virtuais de 2ª geração](Feature-Descriptions-for-Linux-and-FreeBSD-virtual-machines-on-Hyper-V.md#generation-2-virtual-machines)** | |  |
|       Inicializar usando UEFI | 2019, 2016, 2012 R2 |  Nota de &#10004; 12  | Nota de &#10004; 12   
|       Inicialização segura | 2019, 2016 |  |  |



## <a name="notes"></a><a name="BKMK_notes"></a>Observações

1. Para essa versão de Oracle Linux, a marcação de VLAN funciona, mas o entroncamento de VLAN não.

2. A injeção de IP estático poderá não funcionar se o Gerenciador de rede tiver sido configurado para um determinado adaptador de rede sintético na máquina virtual. Para um funcionamento suave da injeção de IP estático, verifique se o Gerenciador de rede está desligado completamente ou se foi desligado para um adaptador de rede específico por meio de seu arquivo ifcfg-ethX.

3.  No Windows Server 2012 R2, ao usar dispositivos de Fibre Channel virtual, verifique se o número de unidade lógica 0 (LUN 0) foi populado. Se o LUN 0 não tiver sido populado, uma máquina virtual Linux poderá não ser capaz de montar dispositivos Fibre Channel nativamente.

4. Para LIS interna, o pacote "HyperV-daemons" deve ser instalado para essa funcionalidade.

5.  Se houver identificadores de arquivos abertos durante uma operação de backup de máquina virtual ao vivo, em alguns casos de canto, os VHDs com backup poderão ter que passar por uma verificação de consistência do sistema de arquivos (fsck) na restauração. As operações de backup dinâmico podem falhar silenciosamente se a máquina virtual tiver um dispositivo iSCSI conectado ou um armazenamento de conexão direta (também conhecido como um disco de passagem).

6. O suporte à memória dinâmica só está disponível em máquinas virtuais de 64 bits.

7. O suporte de adição automática não está habilitado por padrão nesta distribuição. Para habilitar o suporte de adição automática, você precisa adicionar uma regra udev em/etc/udev/rules.d/da seguinte maneira:

   1. Crie um arquivo **/etc/udev/rules.d/100-Balloon.Rules**. Você pode usar qualquer outro nome desejado para o arquivo.

   2. Adicione o seguinte conteúdo ao arquivo:`SUBSYSTEM=="memory", ACTION=="add", ATTR{state}="online"`

   3. Reinicialize o sistema para habilitar o suporte a Hot-Add.

   Enquanto o download do Integration Services do Linux cria essa regra na instalação, a regra também é removida quando o LIS é desinstalado, portanto, a regra deve ser recriada se a memória dinâmica for necessária após a desinstalação.

8. As operações de memória dinâmica podem falhar se o sistema operacional convidado estiver sendo executado com pouca memória. Veja a seguir algumas práticas recomendadas:

   * A memória de inicialização e a memória mínima devem ser iguais ou maiores que a quantidade de memória que o fornecedor de distribuição recomenda.

   * Os aplicativos que tendem a consumir toda a memória disponível em um sistema estão limitados a consumir até 80% da RAM disponível.

9. Se você estiver usando Memória Dinâmica em um sistema operacional Windows Server 2016 ou Windows Server 2012 R2, especifique a **memória de inicialização**, **memória mínima**e parâmetros de **memória máxima** em múltiplos de 128 megabytes (MB). Não fazer isso pode levar a falhas de adição automática e talvez você não veja nenhum aumento de memória em um sistema operacional convidado.

10. Para habilitar a infraestrutura de par chave/valor (KVP), instale o pacote RPM do hypervkvpd ou do HyperV-daemons do seu Oracle Linux ISO. Como alternativa, o pacote pode ser instalado diretamente de repositórios Oracle Linux yum.

11. A infraestrutura do par chave/valor (KVP) pode não funcionar corretamente sem uma atualização de software do Linux. Entre em contato com seu fornecedor de distribuição para obter a atualização de software caso você veja problemas com esse recurso.

12. Em máquinas virtuais do Windows Server 2012 R2 geração 2 têm inicialização segura habilitada por padrão e algumas máquinas virtuais do Linux não serão inicializadas a menos que a opção de inicialização segura esteja desabilitada. Você pode desabilitar a inicialização segura na seção **firmware** das configurações da máquina virtual no Gerenciador do **Hyper-V** ou pode desabilitá-la usando o PowerShell:

    ```Powershell
    Set-VMFirmware -VMName "VMname" -EnableSecureBoot Off
    ```

    O download do Integration Services do Linux pode ser aplicado a VMs existentes de geração 2, mas não comunicar a funcionalidade de geração 2.


Consulte Também

* [Set-VMFirmware](https://technet.microsoft.com/library/dn464287.aspx)

* [Máquinas virtuais CentOS e Red Hat Enterprise Linux com suporte no Hyper-V](Supported-CentOS-and-Red-Hat-Enterprise-Linux-virtual-machines-on-Hyper-V.md)

* [Máquinas virtuais do Debian com suporte no Hyper-V](Supported-Debian-virtual-machines-on-Hyper-V.md)

* [Máquinas virtuais SUSE com suporte no Hyper-V](Supported-SUSE-virtual-machines-on-Hyper-V.md)

* [Máquinas virtuais Ubuntu com suporte no Hyper-V](Supported-Ubuntu-virtual-machines-on-Hyper-V.md)

* [Máquinas virtuais FreeBSD com suporte no Hyper-V](Supported-FreeBSD-virtual-machines-on-Hyper-V.md)

* [Descrições de recursos para máquinas virtuais Linux e FreeBSD no Hyper-V](Feature-Descriptions-for-Linux-and-FreeBSD-virtual-machines-on-Hyper-V.md)

* [Práticas recomendadas para executar o Linux no Hyper-V](Best-Practices-for-running-Linux-on-Hyper-V.md)
