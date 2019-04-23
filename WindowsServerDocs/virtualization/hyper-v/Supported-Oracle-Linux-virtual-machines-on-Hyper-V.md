---
title: Máquinas de virtuais do Oracle Linux com suporte no Hyper-V
description: Lista os serviços de integração do Linux e os recursos incluídos em cada versão
ms.prod: windows-server-threshold
ms.service: na
manager: dongill
ms.technology: compute-hyper-v
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: c02fdb5b-62f3-43cb-a190-ab74b3ebcf77
author: shirgall
ms.author: kathydav
ms.date: 06/01/2017
ms.openlocfilehash: a1569a30c0c5657de14c6df936b3b4596dceb7e5
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59838697"
---
# <a name="supported-oracle-linux-virtual-machines-on-hyper-v"></a>Máquinas de virtuais do Oracle Linux com suporte no Hyper-V

>Aplica-se a: Windows Server 2016, Hyper-V Server 2016, Windows Server 2012 R2, Hyper-V Server 2012 R2, Windows Server 2012, do Hyper-V Server 2012, Windows Server 2008 R2, Windows 10, Windows 8.1, Windows 8, Windows 7.1, Windows 7

O mapa de distribuição de recurso a seguir indica os recursos que estão presentes em cada versão. Os problemas conhecidos e soluções alternativas para cada distribuição são listadas após a tabela.

Nesta seção:

* [Série de Kernel compatível com Red Hat](Supported-Oracle-Linux-virtual-machines-on-Hyper-V.md#BKMK_rhc)

* [Unbreakable Enterprise Kernel série](Supported-Oracle-Linux-virtual-machines-on-Hyper-V.md#BKMK_uek)

* [Notas](Supported-Oracle-Linux-virtual-machines-on-Hyper-V.md#BKMK_notes)

## <a name="table-legend"></a>Legenda da tabela

* **Criado** -LIS são incluídos como parte de sua distribuição Linux. Os números de versão do módulo de kernel para a LIS interna (conforme mostrado pelas **lsmod**, por exemplo) são diferentes do número da versão do pacote de download do LIS fornecidas pela Microsoft. Uma incompatibilidade não indica a LIS interna está desatualizada.

* &#10004;-Recurso disponível

* (*em branco*)-recurso não disponível

* **R UEK\*x QU\*y** -Unbreakable Enterprise Kernel (UEK) em que *x* é o número de versão e *y* é a atualização trimestral.

## <a name="BKMK_rhc"></a>Série de Kernel compatível com Red Hat

O kernel de 32 bits para a série de 6. x é PAE habilitada. Não há nenhum suporte interno do LIS para rhck c do Oracle Linux 6.0-6.3. Kernels do Oracle Linux 7.x são 64 bits apenas.

| **Recurso**                                                                                                              | **Versão do Windows server**   | **7.5-7.6**         | **7.4**             | **6.4 6.8 e 7.0 7.3**                                             | **6.4 6.8 e 7.0 7.2**                                             | **RHCK 7.0-7.2**         | **RHCK 6.8**             | **RHCK 6.6, 6.7**        | **RHCK 6.5**              | **RHCK6.4**               |
|--------------------------------------------------------------------------------------------------------------------------|------------------------------|---------------------|---------------------|---------------------------------------------------------------------|---------------------------------------------------------------------|--------------------------|--------------------------|--------------------------|---------------------------|---------------------------|
| **Disponibilidade**                                                                                                         |                              | Criado            | Criado            | [LIS 4.2](https://www.microsoft.com/download/details.aspx?id=55106) | [LIS 4.1](https://www.microsoft.com/download/details.aspx?id=51612) | Criado                 | Criado                 | Criado                 | Criado                  | Criado                  |
| **[Core](Feature-Descriptions-for-Linux-and-FreeBSD-virtual-machines-on-Hyper-V.md#BKMK_core)**                          | 2016, 2012 R2, 2012, 2008 R2 | &#10004;            | &#10004;            | &#10004;                                                            | &#10004;                                                            | &#10004;                 | &#10004;                 | &#10004;                 | &#10004;                  |                           |
| Hora precisa do Windows Server 2016                                                                                        | 2016                         |                     |                     |                                                                     |                                                                     |                          |                          |                          |                           |                           |
| **[Sistema de rede](Feature-Descriptions-for-Linux-and-FreeBSD-virtual-machines-on-Hyper-V.md#BKMK_Networking)**              |                              |                     |                     |                                                                     |                                                                     |                          |                          |                          |                           |                           |
| Quadros jumbo                                                                                                             | 2016, 2012 R2, 2012, 2008 R2 | &#10004;            | &#10004;            | &#10004;                                                            | &#10004;                                                            | &#10004;                 | &#10004;                 | &#10004;                 | &#10004;                  | &#10004;                  |
| Marcação de VLAN e entroncamento                                                                                                | 2016, 2012 R2, 2012, 2008 R2 | &#10004;            | &#10004;            | &#10004;(Observação 1 para 6.4 6,8)                                       | &#10004;(Observação 1 para 6.4 6,8)                                       | &#10004;                 | &#10004;Observação 1          | &#10004;Observação 1          | &#10004;Observação 1           | &#10004;Observação 1           |
| Migração ao vivo                                                                                                           | 2016, 2012 R2, 2012, 2008 R2 | &#10004;            | &#10004;            | &#10004;                                                            | &#10004;                                                            | &#10004;                 | &#10004;                 | &#10004;                 | &#10004;                  | &#10004;                  |
| Injeção de endereço IP estático                                                                                                      | 2016, 2012 R2, 2012          | &#10004;Observação 14    | &#10004;Observação 14    | &#10004;                                                            | &#10004;                                                            | &#10004;                 | &#10004;                 | &#10004;                 | &#10004;                  | &#10004;                  |
| vRSS                                                                                                                     | 2016, 2012 R2                | &#10004;            | &#10004;            | &#10004;                                                            | &#10004;                                                            | &#10004;                 | &#10004;                 | &#10004;                 |                           |                           |
| Descarregamento de soma de verificação e de segmentação TCP                                                                                   | 2016, 2012 R2, 2012, 2008 R2 | &#10004;            | &#10004;            | &#10004;                                                            | &#10004;                                                            | &#10004;                 | &#10004;                 | &#10004;                 |                           |                           |
| SR-IOV                                                                                                                   | 2016                         | &#10004;            | &#10004;            |                                                                     |                                                                     |                          |                          |                          |                           |                           |
| **[Armazenamento](Feature-Descriptions-for-Linux-and-FreeBSD-virtual-machines-on-Hyper-V.md#BKMK_Storage)**                    |                              |                     |                     |                                                                     |                                                                     |                          |                          |                          |                           |                           |
| Redimensionamento VHDX                                                                                                              | 2016, 2012 R2                | &#10004;            | &#10004;            | &#10004;                                                            | &#10004;                                                            | &#10004;                 | &#10004;                 | &#10004;                 | &#10004;                  |                           |
| Fibre Channel Virtual                                                                                                    | 2016, 2012 R2                | &#10004;Observação 2     | &#10004;Observação 2     | &#10004;Observação 2                                                     | &#10004;Observação 2                                                     | &#10004;Observação 2          | &#10004;Observação 2          | &#10004;Observação 2          | &#10004;Observação 2           |                           |
| Backup de máquina virtual ao vivo                                                                                              | 2016, 2012 R2                | &#10004;Observação 11,3  | &#10004;Observação 11, 3 | &#10004;Observação 3, 4                                                  | &#10004;Observação 3, 4                                                  | &#10004;Observe a 3, 4, 11   | &#10004;Observe a 3, 4, 11   | &#10004;Observe a 3, 4, 11   | &#10004;Observe a 3, 4, 5, 11 | &#10004;Observe a 3, 4, 5, 11 |
| Suporte de CORTE                                                                                                             | 2016, 2012 R2                | &#10004;            | &#10004;            | &#10004;                                                            | &#10004;                                                            | &#10004;                 | &#10004;                 |                          |                           |                           |
| SCSI WWN                                                                                                                 | 2016, 2012 R2                | &#10004;            |                     | &#10004;                                                            | &#10004;                                                            |                          |                          |                          |                           |                           |
| **[Memória](Feature-Descriptions-for-Linux-and-FreeBSD-virtual-machines-on-Hyper-V.md#BKMK_Memory)**                      |                              |                     |                     |                                                                     |                                                                     |                          |                          |                          |                           |                           |
| Suporte do Kernel PAE                                                                                                       | 2016, 2012 R2, 2012, 2008 R2 | N/D                 | N/D                 | &#10004;(6. x somente)                                                 | &#10004;(6. x somente)                                                 | N/D                      | &#10004;                 | &#10004;                 | &#10004;                  | &#10004;                  |
| Configuração de lacuna MMIO                                                                                                | 2016, 2012 R2                | &#10004;            | &#10004;            | &#10004;                                                            | &#10004;                                                            | &#10004;                 | &#10004;                 | &#10004;                 | &#10004;                  | &#10004;                  |
| Memória dinâmica - quente                                                                                                 | 2016, 2012 R2, 2012          | &#10004;Observação 8, 9  | &#10004;Observação 8, 9  | &#10004;Observe a 7, 8, 9, 10 (Observe a 6 para 6.4 6.7)                      | &#10004;Observe a 7, 8, 9, 10 (Observe a 6 para 6.4 6.7)                      | &#10004;Observe a 6, 7, 8, 9 | &#10004;Observe a 6, 7, 8, 9 | &#10004;Observe a 6, 7, 8, 9 | &#10004;Observe a 6, 7, 8, 9  |                           |
| Memória dinâmica - inflação                                                                                              | 2016, 2012 R2, 2012          | &#10004;Observação 8, 9  | &#10004;Observação 8, 9  | &#10004;Observe a 7, 9, 10 (Observe a 6 para 6.4 6.7)                         | &#10004;Observe a 7, 9, 10 (Observe a 6 para 6.4 6.7)                         | &#10004;Observe a 6, 8, 9    | &#10004;Observe a 6, 8, 9    | &#10004;Observe a 6, 8, 9    | &#10004;Observe a 6, 8, 9     | &#10004;Observe a 6, 8, 9, 10 |
| Redimensionamento de memória de tempo de execução                                                                                                    | 2016                         |                     |                     |                                                                     |                                                                     |                          |                          |                          |                           |                           |
| **[Vídeo](Feature-Descriptions-for-Linux-and-FreeBSD-virtual-machines-on-Hyper-V.md#BKMK_Video)**                        |                              |                     |                     |                                                                     |                                                                     |                          |                          |                          |                           |                           |
| Dispositivo de vídeo específico do Hyper-V                                                                                            | 2016,2012 R2, 2012, 2008 R2  | &#10004;            | &#10004;            | &#10004;                                                            | &#10004;                                                            | &#10004;                 | &#10004;                 | &#10004;                 | &#10004;                  |                           |
| **[Diversos](Feature-Descriptions-for-Linux-and-FreeBSD-virtual-machines-on-Hyper-V.md#BKMK_Misc)**                 |                              |                     |                     |                                                                     |                                                                     |                          |                          |                          |                           |                           |
| Par chave-valor                                                                                                           | 2016, 2012 R2, 2012, 2008 R2 | &#10004;            | &#10004;            | &#10004;                                                            | &#10004;                                                            | &#10004;Observação 12         | &#10004;Observação 12         | &#10004;Observação 12         | &#10004;Observação 12          | &#10004;Observação 12          |
| Interrupção não mascarável                                                                                                   | 2016, 2012 R2                | &#10004;            | &#10004;            | &#10004;                                                            | &#10004;                                                            | &#10004;                 | &#10004;                 | &#10004;                 | &#10004;                  | &#10004;                  |
| Cópia do arquivo do host para a convidada                                                                                             | 2016, 2012 R2                | &#10004;            | &#10004;            | &#10004;                                                            | &#10004;                                                            |                          | &#10004;                 |                          |                           |                           |
| comando lsvmbus                                                                                                          | 2016, 2012 R2, 2012, 2008 R2 |                     |                     | &#10004;                                                            | &#10004;                                                            |                          |                          |                          |                           |                           |
| Soquetes do Hyper-V                                                                                                          | 2016                         |                     |                     | &#10004;                                                            | &#10004;                                                            |                          |                          |                          |                           |                           |
| Passagem/DDA de PCI                                                                                                      | 2016                         | &#10004;            | &#10004;            |                                                                     |                                                                     |                          |                          |                          |                           |                           |
| **[Máquinas virtuais de geração 2](Feature-Descriptions-for-Linux-and-FreeBSD-virtual-machines-on-Hyper-V.md#BKMK_gen2)** |                              |                     |                     |                                                                     |                                                                     |                          |                          |                          |                           |                           |
| Inicialização usando UEFI                                                                                                          | 2016, 2012 R2                | &#10004;Observação 13    | &#10004;Observação 13    | &#10004;Observação 13                                                    | &#10004;Observação 13                                                    | &#10004;Observação 13         | &#10004;Observação 13         |                          |                           |                           |
| Inicialização segura                                                                                                              | 2016                         | &#10004;            | &#10004;            |                                                                     |                                                                     |                          |                          |                          |                           |                           |


## <a name="BKMK_uek"></a>Unbreakable Enterprise Kernel série

O Oracle Linux Unbreakable Enterprise Kernel (UEK) tem apenas 64 bits e tem o LIS dão suporte interno.

|**Recurso**|**Versão do Windows server**|**UEK R4**|**UEK R3 QU3**|**UEK R3 QU2**|**UEK R3 QU1**|
|-|-|-|-|-|-|
|**Disponibilidade**||Criado|Criado|Criado|Criado|
|**[Core](Feature-Descriptions-for-Linux-and-FreeBSD-virtual-machines-on-Hyper-V.md#BKMK_core)**|2016, 2012 R2, 2012, 2008 R2|&#10004;|&#10004;|&#10004;|&#10004;|
|Hora precisa do Windows Server 2016|2016|||||
|**[Sistema de rede](Feature-Descriptions-for-Linux-and-FreeBSD-virtual-machines-on-Hyper-V.md#BKMK_Networking)**||||||
|Quadros jumbo|2016, 2012 R2, 2012, 2008 R2|&#10004;|&#10004;|&#10004;|&#10004;|
|Marcação de VLAN e entroncamento|2016, 2012 R2, 2012, 2008 R2|&#10004;|&#10004;|&#10004;|&#10004;|
|Migração ao vivo|2016, 2012 R2, 2012, 2008 R2|&#10004;|&#10004;|&#10004;|&#10004;|
|Injeção de endereço IP estático|2016, 2012 R2, 2012|&#10004;|&#10004;|&#10004;||
|vRSS|2016, 2012 R2|&#10004;||||
|Descarregamento de soma de verificação e de segmentação TCP|2016, 2012 R2, 2012, 2008 R2|&#10004;||||
|SR-IOV|2016|||||
|**[Armazenamento](Feature-Descriptions-for-Linux-and-FreeBSD-virtual-machines-on-Hyper-V.md#BKMK_Storage)**||||||
|Redimensionamento VHDX|2016, 2012 R2|&#10004;|&#10004;|&#10004;||
|Fibre Channel Virtual|2016, 2012 R2|&#10004;|&#10004;|&#10004;||
|Backup de máquina virtual ao vivo|2016, 2012 R2|&#10004;Observe a 3, 4, 5, 11|&#10004;Observe a 3, 4, 5, 11|&#10004;Observe a 3, 4, 5, 11||
|Suporte de CORTE|2016, 2012 R2|&#10004;||||
|SCSI WWN|2016, 2012 R2|||||
|**[Memória](Feature-Descriptions-for-Linux-and-FreeBSD-virtual-machines-on-Hyper-V.md#BKMK_Memory)**|
|Suporte do Kernel PAE|2016, 2012 R2, 2012, 2008 R2|N/D|N/D|N/D|N/D|
|Configuração de lacuna MMIO|2016, 2012 R2|&#10004;|&#10004;|&#10004;|&#10004;|
|Memória dinâmica - quente|2016, 2012 R2, 2012|&#10004;||||
|Memória dinâmica - inflação|2016, 2012 R2, 2012|&#10004;||||
|Redimensionamento de memória de tempo de execução|2016|||||
|**[Vídeo](Feature-Descriptions-for-Linux-and-FreeBSD-virtual-machines-on-Hyper-V.md#BKMK_Video)**||||||
|Dispositivo de vídeo específico do Hyper-V|2016, 2012 R2, 2012, 2008 R2|&#10004;|&#10004;|&#10004;||
|**[Diversos](Feature-Descriptions-for-Linux-and-FreeBSD-virtual-machines-on-Hyper-V.md#BKMK_Misc)**||||||
|Par chave-valor|2016, 2012 R2, 2012, 2008 R2|&#10004;Observação 11, 12|&#10004;Observação 11, 12|&#10004;Observação 11, 12|&#10004;Observação 11, 12|
|Interrupção não mascarável|2016, 2012 R2|&#10004;|&#10004;|&#10004;|&#10004;|
|Cópia do arquivo do host para a convidada|2016, 2012 R2|&#10004;Observação 11|&#10004;Observação 11|&#10004;Observação 11|&#10004;Observação 11|
|comando lsvmbus|2016, 2012 R2, 2012, 2008 R2|||||
|Soquetes do Hyper-V|2016|||||
|Passagem/DDA de PCI|2016|||||
|**[Máquinas virtuais de geração 2](Feature-Descriptions-for-Linux-and-FreeBSD-virtual-machines-on-Hyper-V.md#BKMK_gen2)**|
|Inicialização usando UEFI|2016, 2012 R2|&#10004;||||
|Inicialização segura|2016|&#10004;||||

## <a name="BKMK_notes"></a>Notas

1. Para esta versão do Oracle Linux, funciona, mas troncos VLAN de marcação de VLAN, não.

2. Ao usar dispositivos virtuais de fibre channel, certifique-se de que o número de unidade lógica (LUN 0) de 0 foi preenchido. Se o LUN 0 não foi preenchido, uma máquina virtual Linux pode não ser capaz de montar dispositivos fibre channel nativamente.

3. Se houver identificadores de arquivos abertos durante uma operação de backup de máquina virtual ao vivo e, em seguida, em alguns casos, os VHDs de backup talvez precise passar por uma verificação de consistência de sistema de arquivos (fsck) na restauração.

4. Operações de backup ao vivo poderá falhar, silenciosamente, se a máquina virtual tem um dispositivo iSCSI anexado ou armazenamento anexado direto (também conhecido como um disco de passagem).

5. Live suporte de backup para Oracle Linux 6.4/6.5/UEKR3 QU2 e QU3 estão disponíveis por meio [conceitos básicos de Backup do Hyper-V para Linux](https://github.com/LIS/backupessentials/tree/1.0).

6. Suporte de memória dinâmica só está disponível em máquinas virtuais de 64 bits.

7. Suporte a quente não está habilitado por padrão na distribuição. Para habilitar o suporte a quente, você precisará adicionar uma regra de udev em /etc/udev/rules.d/ da seguinte maneira:

   1. Crie um arquivo **/etc/udev/rules.d/100-balloon.rules**. Você pode usar qualquer nome desejado para o arquivo.

   2. Adicione o seguinte conteúdo ao arquivo: `SUBSYSTEM=="memory", ACTION=="add", ATTR{state}="online"`

   3. Reinicialize o sistema para habilitar o suporte a quente.

   Enquanto o download do Integration Services do Linux cria essa regra na instalação, a regra também será removida quando LIS é desinstalado, portanto, a regra deve ser recriada caso a memória dinâmica é necessária após a desinstalação.

8. Operações de memória dinâmica podem falhar se o sistema operacional convidado é com muito pouco memória. A seguir estão algumas práticas recomendadas:

   * Memória de inicialização e um mínimo de memória devem ser igual ou maior que a quantidade de memória que recomenda o fornecedor de distribuição.

   * Aplicativos que tendem a consumir toda memória disponível em um sistema estão limitados a consumir até 80 por cento de RAM disponível.

9. Se você estiver usando a memória dinâmica em um sistema operacional Windows Server 2016 ou Windows Server 2012 R2, especifique **memória de inicialização**, **memória mínima**, e **memória máxima** parâmetros em múltiplos de 128 megabytes (MB). Falha ao fazer isso pode levar a inclusão automática de falhas e talvez você não veja nenhuma memória aumentam em um sistema operacional convidado.

10. Determinadas distribuições, incluindo aqueles que usam a LIS 3.5 ou 4.0 LIS, somente dão suporte a Inchamento e não dão suporte a quente. Nesse cenário, o recurso de memória dinâmica pode ser usado definindo o parâmetro de memória de inicialização para um valor que é igual ao parâmetro de máximo de memória. Isso resulta em toda a memória necessária que está sendo adicionado quente para a máquina virtual em tempo de inicialização e, em seguida, dependendo dos requisitos de memória do host, do Hyper-V pode livremente alocar ou desalocar a memória do convidado usando Inchamento. Configure **memória de inicialização** e **memória mínima** ou acima do valor recomendado para a distribuição.

11. Daemons do Oracle Linux Hyper-V não são instalados por padrão. Para usar esses daemons, instale o pacote de daemons do Hyper-v. Este está em conflito com pacote baixado Linux Integration Services e não deve ser instalado em sistemas com LIS baixado.

12. A infraestrutura de chave/valor par (KVP) pode não funcionar corretamente sem uma atualização de software do Linux. Entre em contato com seu fornecedor de distribuição para obter a atualização de software caso você veja problemas com esse recurso.

13. As máquinas virtuais das plataformas Windows Server 2012 R2Generation 2 tem inicialização segura habilitada por padrão e algumas máquinas virtuais do Linux não será inicializado, a menos que a opção de inicialização segura está desabilitada. Você pode desabilitar a inicialização segura na **Firmware** seção das configurações para a máquina virtual na **Gerenciador do Hyper-V** ou você pode desabilitá-lo usando o Powershell:

   ```Powershell
   Set-VMFirmware -VMName "VMname" -EnableSecureBoot Off

   ```

    O download do Integration Services do Linux pode ser aplicado a VMs da geração 2 existente, mas não é demonstrar o recurso de geração 2.

14. Injeção de IP estática pode não funcionar se o Gerenciador de rede foi configurado para um adaptador de rede sintéticos na máquina virtual. Para o bom funcionamento do endereço IP estático injeção Certifique-se de que o Gerenciador de rede está desligado completamente ou foi desativado para um adaptador de rede específico por meio de seu arquivo ifcfg ethX.


Consulte também

* [Set-VMFirmware](https://technet.microsoft.com/library/dn464287.aspx)

* [Suporte para CentOS e Red Hat Enterprise Linux máquinas virtuais do Hyper-V](Supported-CentOS-and-Red-Hat-Enterprise-Linux-virtual-machines-on-Hyper-V.md)

* [Suporte para máquinas virtuais do Debian no Hyper-V](Supported-Debian-virtual-machines-on-Hyper-V.md)

* [De máquinas virtuais SUSE com suporte no Hyper-V](Supported-SUSE-virtual-machines-on-Hyper-V.md)

* [Máquinas virtuais do Ubuntu com suporte no Hyper-V](Supported-Ubuntu-virtual-machines-on-Hyper-V.md)

* [Máquinas de virtuais FreeBSD com suporte no Hyper-V](Supported-FreeBSD-virtual-machines-on-Hyper-V.md)

* [Descrições de recursos para máquinas virtuais de Linux e FreeBSD no Hyper-V](Feature-Descriptions-for-Linux-and-FreeBSD-virtual-machines-on-Hyper-V.md)

* [Práticas recomendadas para executar o Linux no Hyper-V](Best-Practices-for-running-Linux-on-Hyper-V.md)
