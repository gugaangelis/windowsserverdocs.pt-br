---
title: De máquinas virtuais SUSE com suporte no Hyper-V
description: Lista os serviços de integração do Linux e os recursos incluídos em cada versão
ms.prod: windows-server-threshold
ms.service: na
manager: dongill
ms.technology: compute-hyper-v
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 7ec0e14c-4498-4bd9-8fe6-b94260198efc
author: shirgall
ms.author: kathydav
ms.date: 10/03/2016
ms.openlocfilehash: 3506c00651951aa2a62637cae6cc4989f9edf1fc
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59818997"
---
# <a name="supported-suse-virtual-machines-on-hyper-v"></a>De máquinas virtuais SUSE com suporte no Hyper-V

>Aplica-se a: Windows Server 2016, Hyper-V Server 2016, Windows Server 2012 R2, Hyper-V Server 2012 R2, Windows Server 2012, do Hyper-V Server 2012, Windows Server 2008 R2, Windows 10, Windows 8.1, Windows 8, Windows 7.1, Windows 7

O exemplo a seguir é um mapa de distribuição de recurso que indica os recursos em cada versão. Os problemas conhecidos e soluções alternativas para cada distribuição são listadas após a tabela.

Os drivers de serviço do SUSE Linux Enterprise internos do Hyper-V são certificados pelo SUSE. Um exemplo de configuração pode ser exibido neste boletim: [Boletim informativo de certificação do SUSE Sim](https://www.suse.com/nbswebapp/yesBulletin.jsp?bulletinNumber=144176).

## <a name="table-legend"></a>Legenda da tabela

* **Criado** -LIS são incluídos como parte de sua distribuição Linux. O pacote de download do LIS fornecidas pela Microsoft não funciona para essa distribuição, portanto, não as instale. Os números de versão do módulo de kernel para a LIS interna (conforme mostrado pelas **lsmod**, por exemplo) são diferentes do número da versão do pacote de download do LIS fornecidas pela Microsoft. Uma incompatibilidade não indica a LIS interna está desatualizada.

* &#10004;-Recurso disponível

* (*em branco*)-recurso não disponível

SLES12 + é de 64 bits apenas.

|**Recurso**|**Versão do sistema operacional Windows Server**|**SLES 15**|**SLES 12 SP3/SP4**|**SLES 12 SP2**|**SLES 12 SP1**|**SLES 11 SP4**|**SLES 11 SP3**|
|-|-|-|-|-|-|-|-|
|**Disponibilidade**||Internos|Internos|Internos|Internos|Internos|Internos|
|**[Core](Feature-Descriptions-for-Linux-and-FreeBSD-virtual-machines-on-Hyper-V.md#BKMK_core)**|2019, 2016, 2012 R2, 2012, 2008 R2|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;|
|Hora precisa do Windows Server 2016|2019, 2016|&#10004;|&#10004;|&#10004;||||
|**[Sistema de rede](Feature-Descriptions-for-Linux-and-FreeBSD-virtual-machines-on-Hyper-V.md#BKMK_Networking)**||||||||
|Quadros jumbo|2019, 2016, 2012 R2, 2012, 2008 R2|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;|
|Marcação de VLAN e entroncamento|2019, 2016, 2012 R2, 2012, 2008 R2|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;|
|Migração ao vivo|2019, 2016, 2012 R2, 2012, 2008 R2|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;|
|Injeção de endereço IP estático|2019, 2016, 2012 R2, 2012|&#10004;Observação 1|&#10004;Observação 1|&#10004;Observação 1|&#10004;Observação 1|&#10004;Observação 1|&#10004;Observação 1|
|vRSS|2019, 2016, 2012 R2|&#10004;|&#10004;|&#10004;|&#10004;|||
|Descarregamento de soma de verificação e de segmentação TCP|2019, 2016, 2012 R2, 2012, 2008 R2|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;||
|SR-IOV|2019, 2016|&#10004;|&#10004;|&#10004;||||
|**[Armazenamento](Feature-Descriptions-for-Linux-and-FreeBSD-virtual-machines-on-Hyper-V.md#BKMK_Storage)**||||||||
|Redimensionamento VHDX|2019, 2016, 2012 R2|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;|
|Fibre Channel Virtual|2019, 2016, 2012 R2|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;|
|Backup de máquina virtual ao vivo|2019, 2016, 2012 R2|&#10004;Observação 2, 3, 8|&#10004;Observação 2, 3, 8|&#10004;Observação 2, 3, 8|&#10004;Observação 2, 3, 8|&#10004;Observação 2, 3, 8|&#10004;Observação 2, 3, 8|
|Suporte de CORTE|2019, 2016, 2012 R2|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;||
|SCSI WWN|2019, 2016, 2012 R2|&#10004;|&#10004;|&#10004;||||
|**[Memória](Feature-Descriptions-for-Linux-and-FreeBSD-virtual-machines-on-Hyper-V.md#BKMK_Memory)**||||||||
|Suporte do Kernel PAE|2019, 2016, 2012 R2, 2012, 2008 R2|N/D|N/D|N/D|N/D|&#10004;|&#10004;|
|Configuração de lacuna MMIO|2019, 2016, 2012 R2|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;|
|Memória dinâmica - quente|2019, 2016, 2012 R2, 2012|&#10004;Observação 5, 6|&#10004;Observação 5, 6|&#10004;Observação 5, 6|&#10004;Observação 5, 6|&#10004;Observe a 4, 5, 6|&#10004;Observe a 4, 5, 6|
|Memória dinâmica - inflação|2019, 2016, 2012 R2, 2012|&#10004;Observação 5, 6|&#10004;Observação 5, 6|&#10004;Observação 5, 6|&#10004;Observação 5, 6|&#10004;Observe a 4, 5, 6|&#10004;Observe a 4, 5, 6|
|Redimensionamento de memória de tempo de execução|2019, 2016|&#10004;Observação 5, 6|&#10004;Observação 5, 6|&#10004;Observação 5, 6||||
|**[Vídeo](Feature-Descriptions-for-Linux-and-FreeBSD-virtual-machines-on-Hyper-V.md#BKMK_Video)**||||||||
|Dispositivo de vídeo específico do Hyper-V|2019, 2016, 2012 R2, 2012, 2008 R2|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;|
|**[Diversos](Feature-Descriptions-for-Linux-and-FreeBSD-virtual-machines-on-Hyper-V.md#BKMK_Misc)**||||||||
|Par chave/valor|2019, 2016, 2012 R2, 2012, 2008 R2|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;Observação 7|&#10004;Observação 7|
|Interrupção não mascarável|2019, 2016, 2012 R2|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;|
|Cópia do arquivo do host para a convidada|2019, 2016, 2012 R2|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;||
|comando lsvmbus|2019, 2016, 2012 R2, 2012, 2008 R2|&#10004;|&#10004;|&#10004;||||
|Soquetes do Hyper-V|2019, 2016|&#10004;|&#10004;|||||
|Passagem/DDA de PCI|2019, 2016|&#10004;|&#10004;|&#10004;|&#10004;|||
|**[Máquinas virtuais de geração 2](Feature-Descriptions-for-Linux-and-FreeBSD-virtual-machines-on-Hyper-V.md#BKMK_gen2)**||||||||
|Inicialização usando UEFI|2019, 2016, 2012 R2|&#10004;Observação 9|&#10004;Observação 9|&#10004;Observação 9|&#10004;Observação 9|&#10004;Observação 9||
|Inicialização segura|2019, 2016|&#10004;|&#10004;|&#10004;|&#10004;|||

## <a name="BKMK_notes"></a>Notas

1. Injeção de IP estática pode não funcionar se **Gerenciador de rede** foi configurado para um determinado adaptador de rede específico do Hyper-V na máquina virtual. Para garantir o bom funcionamento do endereço IP estático injeção Certifique-se de que o Gerenciador de rede está desativado completamente ou foi desativado para um adaptador de rede específico por meio de seu **ifcfg ethX** arquivo.

2. Se houver identificadores de arquivos abertos durante uma operação de backup de máquina virtual ao vivo e, em seguida, em alguns casos, os VHDs de backup talvez precise passar por uma verificação de consistência de sistema de arquivos (fsck) na restauração.

3. Operações de backup ao vivo poderá falhar, silenciosamente, se a máquina virtual tem um dispositivo iSCSI anexado ou armazenamento anexado direto (também conhecido como um disco de passagem).

4. Operações de memória dinâmica podem falhar se o sistema operacional convidado é com muito pouco memória. A seguir estão algumas práticas recomendadas:

   * Memória de inicialização e um mínimo de memória devem ser igual ou maior que a quantidade de memória que recomenda o fornecedor de distribuição.

   * Aplicativos que tendem a consumir toda memória disponível em um sistema estão limitados a consumir até 80 por cento de RAM disponível.

5. Suporte de memória dinâmica só está disponível em máquinas virtuais de 64 bits.

6. Se você estiver usando a memória dinâmica em sistemas operacionais Windows Server 2016 ou Windows Server 2012, especifique **memória de inicialização**, **memória mínima**, e **memória máxima** parâmetros em múltiplos de 128 megabytes (MB). Falha ao fazer isso pode levar a falhas quente e talvez você não veja nenhuma memória aumentam em um sistema operacional convidado.

7. No Windows Server 2016 ou Windows Server 2012 R2, a infraestrutura de par chave/valor pode não funcionar corretamente sem uma atualização de software do Linux. Entre em contato com seu fornecedor de distribuição para obter a atualização de software caso você veja problemas com esse recurso.

8. Backup VSS falhará se uma única partição estiver montada várias vezes.

9. No Windows Server 2012 R2, a geração 2 máquinas virtuais têm a inicialização segura habilitada por padrão e as máquinas virtuais de geração 2 Linux não será inicializado, a menos que a opção de inicialização segura está desabilitada. Você pode desabilitar a inicialização segura na seção **Firmware** das configurações da máquina virtual no Gerenciador do Hyper-V ou usando o Powershell:

   ```Powershell
   Set-VMFirmware -VMName "VMname" -EnableSecureBoot Off

   ```

## <a name="see-also"></a>Consulte também

* [Set-VMFirmware](https://technet.microsoft.com/library/dn464287.aspx)

* [Suporte para CentOS e Red Hat Enterprise Linux máquinas virtuais do Hyper-V](Supported-CentOS-and-Red-Hat-Enterprise-Linux-virtual-machines-on-Hyper-V.md)

* [Suporte para máquinas virtuais do Debian no Hyper-V](Supported-Debian-virtual-machines-on-Hyper-V.md)

* [Máquinas de virtuais do Oracle Linux com suporte no Hyper-V](Supported-Oracle-Linux-virtual-machines-on-Hyper-V.md)

* [Máquinas virtuais do Ubuntu com suporte no Hyper-V](Supported-Ubuntu-virtual-machines-on-Hyper-V.md)

* [Máquinas de virtuais FreeBSD com suporte no Hyper-V](Supported-FreeBSD-virtual-machines-on-Hyper-V.md)

* [Descrições de recursos para máquinas virtuais de Linux e FreeBSD no Hyper-V](Feature-Descriptions-for-Linux-and-FreeBSD-virtual-machines-on-Hyper-V.md)

* [Práticas recomendadas para executar o Linux no Hyper-V](Best-Practices-for-running-Linux-on-Hyper-V.md)
