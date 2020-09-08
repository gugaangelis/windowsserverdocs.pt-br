---
title: Máquinas virtuais Ubuntu com suporte no Hyper-V
description: Lista os serviços e recursos de integração do Linux incluídos em cada versão
manager: dongill
ms.topic: article
ms.assetid: 95ea5f7c-25c6-494b-8ffd-2a77f631ee94
author: shirgall
ms.author: shirgall
ms.date: 08/29/2020
ms.openlocfilehash: 5bd5f7a129cbc5c69bc6b909e292c096a3812af1
ms.sourcegitcommit: 34f9577ef32cbdc7ef96040caabc9d83517f9b79
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/08/2020
ms.locfileid: "89554549"
---
# <a name="supported-ubuntu-virtual-machines-on-hyper-v"></a>Máquinas virtuais Ubuntu com suporte no Hyper-V

>Aplica-se a: Windows Server 2019, Hyper-V Server 2019, Windows Server 2016, Hyper-V Server 2016, Windows Server 2012 R2, Hyper-V Server 2012 R2, Windows 10, Windows 8.1

O mapa de distribuição de recursos a seguir indica os recursos em cada versão. Os problemas conhecidos e as soluções alternativas para cada distribuição são listados após a tabela.

## <a name="table-legend"></a>Legenda da tabela

* A LIS **interna** é incluída como parte dessa distribuição do Linux. O pacote de download do LIS fornecido pela Microsoft não funciona para essa distribuição, portanto, não o instale. Os números de versão do módulo do kernel para a LIS interna (conforme mostrado por **lsmod**, por exemplo) são diferentes do número de versão no pacote de download do LIS fornecido pela Microsoft. Uma incompatibilidade não indica que a LIS interna está desatualizada.

* &#10004;-recurso disponível

* (*em branco*)-recurso não disponível

|**Recurso**|**Versão do sistema operacional Windows Server**|**20, 4 LTS**|**18.04 LTS**|**16.04 LTS**|**14.04 LTS**|
|-|-|-|-|-|-|
|**Disponibilidade**||Interno|Interno|Interno|Interno|
|**[Core](Feature-Descriptions-for-Linux-and-FreeBSD-virtual-machines-on-Hyper-V.md#core)**|2019, 2016, 2012 R2|&#10004;|&#10004;|&#10004;|&#10004;|
|Tempo preciso do Windows Server 2016|2019, 2016|&#10004;|&#10004;|&#10004;||
|**[Rede](Feature-Descriptions-for-Linux-and-FreeBSD-virtual-machines-on-Hyper-V.md#networking)**||||||
|Quadros jumbo|2019, 2016, 2012 R2|&#10004;|&#10004;|&#10004;|&#10004;|
|Marcação e entroncamento de VLAN|2019, 2016, 2012 R2|&#10004;|&#10004;|&#10004;|&#10004;|
|Migração ao vivo|2019, 2016, 2012 R2|&#10004;|&#10004;|&#10004;|&#10004;|
|Injeção de IP estático|2019, 2016, 2012 R2|Observação de &#10004; 1|Observação de &#10004; 1|Observação de &#10004; 1|Observação de &#10004; 1|
|vRSS|2019, 2016, 2012 R2|&#10004;|&#10004;|&#10004;|&#10004;|
|Segmentação de TCP e descarregamentos de soma de verificação|2019, 2016, 2012 R2|&#10004;|&#10004;|&#10004;|&#10004;|
|SR-IOV|2019, 2016|&#10004;|&#10004;|&#10004;||
|**[Armazenamento](Feature-Descriptions-for-Linux-and-FreeBSD-virtual-machines-on-Hyper-V.md#storage)**|||||
|Redimensionamento de VHDX|2019, 2016, 2012 R2|&#10004;|&#10004;|&#10004;|&#10004;|
|Fibre Channel Virtual|2019, 2016, 2012 R2|Observação de &#10004; 2|Observação de &#10004; 2|Observação de &#10004; 2|Observação de &#10004; 2|
|Backup de máquina virtual ao vivo|2019, 2016, 2012 R2|&#10004; observação 3, 4, 5|&#10004; observação 3, 4, 5|&#10004; observação 3, 4, 5|&#10004; observação 3, 4, 5|
|Suporte a corte|2019, 2016, 2012 R2|&#10004;|&#10004;|&#10004;|&#10004;|
|WWN DO SCSI|2019, 2016, 2012 R2|&#10004;|&#10004;|&#10004;|&#10004;|
|**[Memória](Feature-Descriptions-for-Linux-and-FreeBSD-virtual-machines-on-Hyper-V.md#memory)**|||||
|Suporte ao kernel de PAE|2019, 2016, 2012 R2|&#10004;|&#10004;|&#10004;|&#10004;|
|Configuração da lacuna de MMIO|2019, 2016, 2012 R2|&#10004;|&#10004;|&#10004;|&#10004;|
|Memória Dinâmica-adição a quente|2019, 2016, 2012 R2|Nota de &#10004; 6, 7, 8|Nota de &#10004; 6, 7, 8|Nota de &#10004; 6, 7, 8|Nota de &#10004; 6, 7, 8|
|Memória Dinâmica-balões|2019, 2016, 2012 R2|Nota de &#10004; 6, 7, 8|Nota de &#10004; 6, 7, 8|Nota de &#10004; 6, 7, 8|Nota de &#10004; 6, 7, 8|
|Redimensionamento de memória de Runtime|2019, 2016|&#10004;|&#10004;|&#10004;|&#10004;|
|**[Vídeo](Feature-Descriptions-for-Linux-and-FreeBSD-virtual-machines-on-Hyper-V.md#video)**||||||
|Dispositivo de vídeo específico do Hyper-V|2019, 2016, 2012 R2|&#10004;|&#10004;|&#10004;|&#10004;|
|**[Diversos](Feature-Descriptions-for-Linux-and-FreeBSD-virtual-machines-on-Hyper-V.md#miscellaneous)**|||||
|Pares chave/valor|2019, 2016, 2012 R2|&#10004; observação 5, 9|&#10004; observação 5, 9|&#10004; observação 5, 9|&#10004; observação 5, 9|
|Interrupção não mascarável|2019, 2016, 2012 R2|&#10004;|&#10004;|&#10004;|&#10004;|
|Cópia de arquivo do host para o convidado|2019, 2016, 2012 R2|&#10004;|&#10004;|&#10004;|&#10004;|
|comando lsvmbus|2019, 2016, 2012 R2|&#10004;|&#10004;|&#10004;|&#10004;|
|Soquetes do Hyper-V|2019, 2016|&#10004;|&#10004;|&#10004;|&#10004;|
|Passagem de PCI/DDA|2019, 2016|&#10004;|&#10004;|&#10004;|&#10004;|
|**[Máquinas virtuais de 2ª geração](Feature-Descriptions-for-Linux-and-FreeBSD-virtual-machines-on-Hyper-V.md#generation-2-virtual-machines)**|||||
|Inicializar usando UEFI|2019, 2016, 2012 R2|Observação de &#10004; 10, 11|Observação de &#10004; 10, 11|Observação de &#10004; 10, 11|Observação de &#10004; 10, 11|
|Inicialização Segura|2019, 2016|&#10004;|&#10004;|&#10004;|&#10004;|

## <a name="notes"></a>Observações

1. A injeção de IP estático poderá não funcionar se o **Gerenciador de rede** tiver sido configurado para um determinado adaptador de rede específico do Hyper-V na máquina virtual. Para garantir o funcionamento suave da injeção de IP estático, verifique se o Gerenciador de rede está desligado completamente ou se foi desligado para um adaptador de rede específico por meio de seu arquivo **ifcfg-ethX** .

2. Ao usar dispositivos de Fiber Channel virtual, verifique se o número de unidade lógica 0 (LUN 0) foi populado. Se o LUN 0 não tiver sido populado, uma máquina virtual Linux poderá não conseguir montar dispositivos de Fiber Channel nativamente.

3. Se houver identificadores de arquivos abertos durante uma operação de backup de máquina virtual em tempo real, em alguns casos de canto, os VHDs com backup poderão ter que passar por uma verificação de consistência do sistema de arquivos ( `fsck` ) na restauração.

4. As operações de backup dinâmico podem falhar silenciosamente se a máquina virtual tiver um dispositivo iSCSI conectado ou um armazenamento de conexão direta (também conhecido como um disco de passagem).

5. Em versões de suporte a longo prazo (LTS), use o kernel de habilitação de hardware virtual (HWE) mais recente para Integration Services Linux atualizada.

   Para instalar o kernel ajustado do Azure em 16, 4, 18, 4 e 20, 4, execute os seguintes comandos como root (ou sudo):

   ```bash
   # apt-get update
   # apt-get install linux-azure
   ```
6. O suporte à memória dinâmica só está disponível em máquinas virtuais de 64 bits.

7. Memória Dinâmica operações poderão falhar se o sistema operacional convidado estiver em execução muito baixo na memória. Veja a seguir algumas práticas recomendadas:

   * A memória de inicialização e a memória mínima devem ser iguais ou maiores que a quantidade de memória que o fornecedor de distribuição recomenda.

   * Os aplicativos que tendem a consumir toda a memória disponível em um sistema estão limitados a consumir até 80% da RAM disponível.

8. Se você estiver usando Memória Dinâmica nos sistemas operacionais Windows Server 2019, Windows Server 2016 ou Windows Server 2012/2012 R2, especifique a **memória de inicialização**, a **memória mínima**e os parâmetros de **memória máxima** em múltiplos de 128 megabytes (MB). Não fazer isso pode levar a falhas de adição automática e talvez você não veja nenhum aumento de memória em um sistema operacional convidado.

9. No Windows Server 2019, Windows Server 2016 ou Windows Server 2012 R2, a infraestrutura de par chave/valor pode não funcionar corretamente sem uma atualização de software do Linux. Entre em contato com seu fornecedor de distribuição para obter a atualização de software caso você veja problemas com esse recurso.

10. No Windows Server 2012 R2, as máquinas virtuais de geração 2 têm inicialização segura habilitada por padrão e algumas máquinas virtuais do Linux não serão inicializadas a menos que a opção de inicialização segura esteja desabilitada. Você pode desabilitar a inicialização segura na seção **firmware** das configurações da máquina virtual no Gerenciador do **Hyper-V** ou pode desabilitá-la usando o PowerShell:

    ```Powershell
    Set-VMFirmware -VMName "VMname" -EnableSecureBoot Off
    ```

11. Antes de tentar copiar o VHD de uma máquina virtual de VHD de geração 2 existente para criar novas máquinas virtuais de geração 2, siga estas etapas:

    1. Faça logon na máquina virtual de geração 2 existente.

    2. Altere o diretório para o diretório EFI de inicialização:

       ```bash
       # cd /boot/efi/EFI
       ```

    3. Copie o diretório do Ubuntu no para um novo diretório chamado Boot:

       ```bash
       # sudo cp -r ubuntu/ boot
       ```

    4. Altere o diretório para o diretório de inicialização criado recentemente:

       ```bash
       # cd boot
       ```

    5. Renomeie o arquivo shimx64. efi:

       ```bash
       # sudo mv shimx64.efi bootx64.efi
       ```

## <a name="see-also"></a>Consulte Também

* [Máquinas virtuais CentOS e Red Hat Enterprise Linux com suporte no Hyper-V](Supported-CentOS-and-Red-Hat-Enterprise-Linux-virtual-machines-on-Hyper-V.md)

* [Máquinas virtuais do Debian com suporte no Hyper-V](Supported-Debian-virtual-machines-on-Hyper-V.md)

* [Máquinas virtuais Oracle Linux com suporte no Hyper-V](Supported-Oracle-Linux-virtual-machines-on-Hyper-V.md)

* [Máquinas virtuais SUSE com suporte no Hyper-V](Supported-SUSE-virtual-machines-on-Hyper-V.md)

* [Descrições de recursos para máquinas virtuais Linux e FreeBSD no Hyper-V](Feature-Descriptions-for-Linux-and-FreeBSD-virtual-machines-on-Hyper-V.md)

* [Práticas recomendadas para executar o Linux no Hyper-V](Best-Practices-for-running-Linux-on-Hyper-V.md)

* [Set-VMFirmware](/powershell/module/hyper-v/set-vmfirmware?view=win10-ps)

* [Ubuntu 14, 4 em uma VM de geração 2-blog de virtualização de Ben Armstrong](/archive/blogs/virtual_pc_guy/ubuntu-14-04-in-a-generation-2-vm)
