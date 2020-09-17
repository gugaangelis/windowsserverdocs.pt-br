---
title: Máquinas virtuais SUSE com suporte no Hyper-V
description: Lista os serviços e recursos de integração do Linux incluídos em cada versão
ms.topic: article
ms.assetid: 7ec0e14c-4498-4bd9-8fe6-b94260198efc
ms.author: benarm
author: BenjaminArmstrong
ms.date: 04/07/2020
ms.openlocfilehash: 92dd65669a537d619d9104378adae26c91878dca
ms.sourcegitcommit: dd1fbb5d7e71ba8cd1b5bfaf38e3123bca115572
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/17/2020
ms.locfileid: "90746731"
---
# <a name="supported-suse-virtual-machines-on-hyper-v"></a>Máquinas virtuais SUSE com suporte no Hyper-V

>Aplica-se a: Windows Server 2019, Hyper-V Server 2019, Windows Server 2016, Hyper-V Server 2016, Windows Server 2012 R2, Hyper-V Server 2012 R2, Windows 10, Windows 8.1

Veja a seguir um mapa de distribuição de recursos que indica os recursos em cada versão. Os problemas conhecidos e as soluções alternativas para cada distribuição são listados após a tabela.

Os drivers internos do SUSE Linux Enterprise Service para Hyper-V são certificados pelo SUSE. Uma configuração de exemplo pode ser exibida neste boletim: o [Boletim de certificação SuSE Yes](https://www.suse.com/nbswebapp/yesBulletin.jsp?bulletinNumber=144176).

## <a name="table-legend"></a>Legenda da tabela

* A LIS **interna** é incluída como parte dessa distribuição do Linux. O pacote de download do LIS fornecido pela Microsoft não funciona para essa distribuição, portanto, não o instale. Os números de versão do módulo do kernel para a LIS interna (conforme mostrado por **lsmod**, por exemplo) são diferentes do número de versão no pacote de download do LIS fornecido pela Microsoft. Uma incompatibilidade não indica que a LIS interna está desatualizada.

* &#10004;-recurso disponível

* (*em branco*)-recurso não disponível

SLES12 + é de apenas 64 bits.

|**Recurso**|**Versão do sistema operacional Windows Server**|**SLES 15 SP1**|**SLES 15**|**SLES 12 SP3-SP5**|**SLES 12 SP2**|**SLES 12 SP1**|**SLES 11 SP4**|**SLES 11 SP3**|
|-|-|-|-|-|-|-|-|-|
|**Disponibilidade**||Interno|Interno|Interno|Interno|Interno|Interno|Interno|
|**[Núcleo](Feature-Descriptions-for-Linux-and-FreeBSD-virtual-machines-on-Hyper-V.md#core)**|2019, 2016, 2012 R2|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;|
|Tempo preciso do Windows Server 2016|2019, 2016|&#10004;|&#10004;|&#10004;|&#10004;||||
|**[Rede](Feature-Descriptions-for-Linux-and-FreeBSD-virtual-machines-on-Hyper-V.md#networking)**|||||||||
|Quadros jumbo|2019, 2016, 2012 R2|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;|
|Marcação e entroncamento de VLAN|2019, 2016, 2012 R2|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;|
|Migração ao vivo|2019, 2016, 2012 R2|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;|
|Injeção de IP estático|2019, 2016, 2012 R2|Observação de &#10004;1|Observação de &#10004;1|Observação de &#10004;1|Observação de &#10004;1|Observação de &#10004;1|Observação de &#10004;1|Observação de &#10004;1|
|vRSS|2019, 2016, 2012 R2|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;|||
|Segmentação de TCP e descarregamentos de soma de verificação|2019, 2016, 2012 R2|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;||
|SR-IOV|2019, 2016|&#10004;|&#10004;|&#10004;|&#10004;||||
|**[Armazenamento](Feature-Descriptions-for-Linux-and-FreeBSD-virtual-machines-on-Hyper-V.md#storage)**|||||||||
|Redimensionamento de VHDX|2019, 2016, 2012 R2|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;|
|Fibre Channel Virtual|2019, 2016, 2012 R2|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;|
|Backup de máquina virtual ao vivo|2019, 2016, 2012 R2|&#10004; observação 2, 3, 8|&#10004;observação 2, 3, 8|&#10004; observação 2, 3, 8|&#10004; observação 2, 3, 8|&#10004; observação 2, 3, 8|&#10004; observação 2, 3, 8|&#10004; observação 2, 3, 8|
|Suporte a corte|2019, 2016, 2012 R2|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;||
|WWN DO SCSI|2019, 2016, 2012 R2|&#10004;|&#10004;|&#10004;|&#10004;||||
|**[Memória](Feature-Descriptions-for-Linux-and-FreeBSD-virtual-machines-on-Hyper-V.md#memory)**|||||||||
|Suporte ao kernel de PAE|2019, 2016, 2012 R2|N/D|N/D|N/D|N/D|N/D|&#10004;|&#10004;|
|Configuração da lacuna de MMIO|2019, 2016, 2012 R2|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;|
|Memória Dinâmica-adição a quente|2019, 2016, 2012 R2|&#10004; observação 6|&#10004;observação 6|&#10004; observação 6|&#10004; observação 6|&#10004; observação 6|&#10004; Observação 4, 5, 6|&#10004; Observação 4, 5, 6|
|Memória Dinâmica-balões|2019, 2016, 2012 R2|&#10004; observação 6|&#10004; observação 6|&#10004; observação 6|&#10004; observação 6|&#10004; observação 6|&#10004; Observação 4, 5, 6|&#10004; Observação 4, 5, 6|
|Redimensionamento de memória de Runtime|2019, 2016|&#10004; observação 6|&#10004; observação 6|&#10004; observação 6|&#10004; observação 6||||
|**[Vídeo](Feature-Descriptions-for-Linux-and-FreeBSD-virtual-machines-on-Hyper-V.md#video)**|||||||||
|Dispositivo de vídeo específico do Hyper-V|2019, 2016, 2012 R2|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;|
|**[Diversos](Feature-Descriptions-for-Linux-and-FreeBSD-virtual-machines-on-Hyper-V.md#miscellaneous)**|||||||||
|Pares chave/valor|2019, 2016, 2012 R2|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;|Observação de &#10004; 7|Observação de &#10004; 7|
|Interrupção não mascarável|2019, 2016, 2012 R2|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;|
|Cópia de arquivo do host para o convidado|2019, 2016, 2012 R2|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;||
|comando lsvmbus|2019, 2016, 2012 R2|&#10004;|&#10004;|&#10004;|&#10004;||||
|Soquetes do Hyper-V|2019, 2016|&#10004;|&#10004;|&#10004;|||||
|Passagem de PCI/DDA|2019, 2016|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;|||
|**[Máquinas virtuais de 2ª geração](Feature-Descriptions-for-Linux-and-FreeBSD-virtual-machines-on-Hyper-V.md#generation-2-virtual-machines)**|||||||||
|Inicializar usando UEFI|2019, 2016, 2012 R2|Nota de &#10004; 9|Nota de &#10004; 9|Nota de &#10004; 9|Nota de &#10004; 9|Nota de &#10004; 9|Nota de &#10004; 9||
|Inicialização Segura|2019, 2016|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;|||

## <a name="notes"></a><a name="BKMK_notes"></a>Observações

1. A injeção de IP estático poderá não funcionar se o **Gerenciador de rede** tiver sido configurado para um determinado adaptador de rede específico do Hyper-V na máquina virtual. Para garantir o funcionamento suave da injeção de IP estático, verifique se o Gerenciador de rede está desligado completamente ou se foi desligado para um adaptador de rede específico por meio de seu arquivo **ifcfg-ethX** .

2. Se houver identificadores de arquivos abertos durante uma operação de backup de máquina virtual ao vivo, em alguns casos de canto, os VHDs com backup poderão ter que passar por uma verificação de consistência do sistema de arquivos (fsck) na restauração.

3. As operações de backup dinâmico podem falhar silenciosamente se a máquina virtual tiver um dispositivo iSCSI conectado ou um armazenamento de conexão direta (também conhecido como um disco de passagem).

4. As operações de memória dinâmica podem falhar se o sistema operacional convidado estiver sendo executado com pouca memória. Veja a seguir algumas práticas recomendadas:

   * A memória de inicialização e a memória mínima devem ser iguais ou maiores que a quantidade de memória que o fornecedor de distribuição recomenda.

   * Os aplicativos que tendem a consumir toda a memória disponível em um sistema estão limitados a consumir até 80% da RAM disponível.

5. O suporte à memória dinâmica só está disponível em máquinas virtuais de 64 bits.

6. Se você estiver usando Memória Dinâmica nos sistemas operacionais Windows Server 2016 ou Windows Server 2012, especifique a **memória de inicialização**, a **memória mínima**e os parâmetros de **memória máxima** em múltiplos de 128 megabytes (MB). Não fazer isso pode levar a falhas de adição automática e talvez você não veja nenhum aumento de memória em um sistema operacional convidado.

7. No Windows Server 2016 ou no Windows Server 2012 R2, a infraestrutura de par chave/valor pode não funcionar corretamente sem uma atualização de software do Linux. Entre em contato com seu fornecedor de distribuição para obter a atualização de software caso você veja problemas com esse recurso.

8. O backup do VSS falhará se uma única partição for montada várias vezes.

9. No Windows Server 2012 R2, as máquinas virtuais de geração 2 têm inicialização segura habilitada por padrão e as máquinas virtuais Linux de geração 2 não serão inicializadas, a menos que a opção de inicialização segura esteja desabilitada. Você pode desabilitar a inicialização segura na seção **Firmware** das configurações da máquina virtual no Gerenciador do Hyper-V ou usando o Powershell:

   ```Powershell
   Set-VMFirmware -VMName "VMname" -EnableSecureBoot Off

   ```

## <a name="see-also"></a>Consulte Também

* [Set-VMFirmware](/powershell/module/hyper-v/set-vmfirmware?view=win10-ps)

* [Máquinas virtuais CentOS e Red Hat Enterprise Linux com suporte no Hyper-V](Supported-CentOS-and-Red-Hat-Enterprise-Linux-virtual-machines-on-Hyper-V.md)

* [Máquinas virtuais do Debian com suporte no Hyper-V](Supported-Debian-virtual-machines-on-Hyper-V.md)

* [Máquinas virtuais Oracle Linux com suporte no Hyper-V](Supported-Oracle-Linux-virtual-machines-on-Hyper-V.md)

* [Máquinas virtuais Ubuntu com suporte no Hyper-V](Supported-Ubuntu-virtual-machines-on-Hyper-V.md)

* [Máquinas virtuais FreeBSD com suporte no Hyper-V](Supported-FreeBSD-virtual-machines-on-Hyper-V.md)

* [Descrições de recursos para máquinas virtuais Linux e FreeBSD no Hyper-V](Feature-Descriptions-for-Linux-and-FreeBSD-virtual-machines-on-Hyper-V.md)

* [Práticas recomendadas para executar o Linux no Hyper-V](Best-Practices-for-running-Linux-on-Hyper-V.md)