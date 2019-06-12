---
title: Máquinas virtuais do Ubuntu com suporte no Hyper-V
description: Lista os serviços de integração do Linux e os recursos incluídos em cada versão
ms.prod: windows-server-threshold
ms.service: na
manager: dongill
ms.technology: compute-hyper-v
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 95ea5f7c-25c6-494b-8ffd-2a77f631ee94
author: shirgall
ms.author: shirgall
ms.date: 11/19/2018
ms.openlocfilehash: 662541658fe6e7b99e66fe31344450e0a1cbd201
ms.sourcegitcommit: eaf071249b6eb6b1a758b38579a2d87710abfb54
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 05/31/2019
ms.locfileid: "66447833"
---
# <a name="supported-ubuntu-virtual-machines-on-hyper-v"></a>Máquinas virtuais do Ubuntu com suporte no Hyper-V

>Aplica-se a: Windows Server 2019, 2016, Hyper-V Server 2019, 2016, Windows Server 2012 R2, Hyper-V Server 2012 R2, Windows Server 2012, do Hyper-V Server 2012, Windows Server 2008 R2, Windows 10, Windows 8.1, Windows 8, Windows 7.1, Windows 7

Começando com o Ubuntu 12.04, carregar o pacote "linux-virtual" instala um kernel adequado para uso como uma máquina virtual convidada. Este pacote sempre depende da imagem mais recente do kernel genérico mínima e os cabeçalhos usados para máquinas virtuais. Embora seu uso seja opcional, o kernel do linux virtual carregará menos drivers e pode de inicialização mais rápido e tem menos sobrecarga de memória do que uma imagem genérica.

Para obter o uso total do Hyper-V, instale as ferramentas apropriadas de linux e pacotes de ferramentas de nuvem do linux para instalar as ferramentas e daemons para uso com máquinas virtuais. Ao usar o kernel do linux-virtual, de carga virtual de ferramentas do linux e linux-cloud-tools-virtual.

O mapa de distribuição de recurso a seguir indica os recursos em cada versão. Os problemas conhecidos e soluções alternativas para cada distribuição são listadas após a tabela.

## <a name="table-legend"></a>Legenda da tabela

* **Criado** -LIS são incluídos como parte de sua distribuição Linux. O pacote de download do LIS fornecidas pela Microsoft não funciona para essa distribuição, portanto, não instalá-lo. Os números de versão do módulo de kernel para a LIS interna (conforme mostrado pelas **lsmod**, por exemplo) são diferentes do número da versão do pacote de download do LIS fornecidas pela Microsoft. Uma incompatibilidade não indica a LIS interna está desatualizada.

* &#10004;-Recurso disponível

* (*em branco*)-recurso não disponível

|**Recurso**|**Versão do sistema operacional Windows Server**|**18.10**|**18.04 LTS**|**16.04 LTS**|**14.04 LTS**|**12.04 LTS**|
|-|-|-|-|-|-|-|
|**Disponibilidade**||Internos|Internos|Internos|Internos|Internos|
|**[Core](Feature-Descriptions-for-Linux-and-FreeBSD-virtual-machines-on-Hyper-V.md#core)**|2019, 2016, 2012 R2, 2012, 2008 R2|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;|
|Hora precisa do Windows Server 2016|2019, 2016|&#10004;|&#10004;|&#10004;|||
|**[Sistema de rede](Feature-Descriptions-for-Linux-and-FreeBSD-virtual-machines-on-Hyper-V.md#networking)**|||||||
|Quadros jumbo|2019, 2016, 2012 R2, 2012, 2008 R2|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;|
|Marcação de VLAN e entroncamento|2019, 2016, 2012 R2, 2012, 2008 R2|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;|
|Migração ao vivo|2019, 2016, 2012 R2, 2012, 2008 R2|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;|
|Injeção de endereço IP estático|2019, 2016, 2012 R2, 2012|&#10004;Observação 1|&#10004;Observação 1|&#10004;Observação 1|&#10004;Observação 1|&#10004;Observação 1|
|vRSS|2019, 2016, 2012 R2|&#10004;|&#10004;|&#10004;|&#10004;||
|Descarregamento de soma de verificação e de segmentação TCP|2019, 2016, 2012 R2, 2012, 2008 R2|&#10004;|&#10004;|&#10004;|&#10004;||
|SR-IOV|2019, 2016|&#10004;|&#10004;|&#10004;|||
|**[Armazenamento](Feature-Descriptions-for-Linux-and-FreeBSD-virtual-machines-on-Hyper-V.md#storage)**||||||
|Redimensionamento VHDX|2019, 2016, 2012 R2|&#10004;|&#10004;|&#10004;|&#10004;||
|Fibre Channel Virtual|2019, 2016, 2012 R2|&#10004;Observação 2|&#10004;Observação 2|&#10004;Observação 2|&#10004;Observação 2||
|Backup de máquina virtual ao vivo|2019, 2016, 2012 R2|&#10004;Observe a 3, 4, 6|&#10004;Observe a 3, 4, 5|&#10004;Observe a 3, 4, 5|&#10004;Observe a 3, 4, 5||
|Suporte de CORTE|2019, 2016, 2012 R2|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;|
|SCSI WWN|2019, 2016, 2012 R2|&#10004;|&#10004;|&#10004;|&#10004;||
|**[Memória](Feature-Descriptions-for-Linux-and-FreeBSD-virtual-machines-on-Hyper-V.md#memory)**||||||
|Suporte do Kernel PAE|2019, 2016, 2012 R2, 2012, 2008 R2|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;|
|Configuração de lacuna MMIO|2019, 2016, 2012 R2|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;|
|Memória dinâmica - quente|2019, 2016, 2012 R2, 2012|&#10004;Observe a 7, 8, 9|&#10004;Observe a 7, 8, 9|&#10004;Observe a 7, 8, 9|&#10004;Observe a 7, 8, 9||
|Memória dinâmica - inflação|2019, 2016, 2012 R2, 2012|&#10004;Observe a 7, 8, 9|&#10004;Observe a 7, 8, 9|&#10004;Observe a 7, 8, 9|&#10004;Observe a 7, 8, 9||
|Redimensionamento de memória de tempo de execução|2019, 2016|&#10004;|&#10004;|&#10004;|&#10004;||
|**[Vídeo](Feature-Descriptions-for-Linux-and-FreeBSD-virtual-machines-on-Hyper-V.md#video)**|||||||
|Dispositivo de vídeo específico do Hyper-V|2019, 2016, 2012 R2, 2012, 2008 R2|&#10004;|&#10004;|&#10004;|&#10004;||
|**[Diversos](Feature-Descriptions-for-Linux-and-FreeBSD-virtual-machines-on-Hyper-V.md#miscellaneous)**||||||
|Par chave/valor|2019, 2016, 2012 R2, 2012, 2008 R2|&#10004;Nota 6, 10|&#10004;Observação 5, 10|&#10004;Observação 5, 10|&#10004;Observação 5, 10|&#10004;Observação 5, 10|
|Interrupção não mascarável|2019, 2016, 2012 R2|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;|
|Cópia do arquivo do host para a convidada|2019, 2016, 2012 R2|&#10004;|&#10004;|&#10004;|&#10004;||
|comando lsvmbus|2019, 2016, 2012 R2, 2012, 2008 R2|&#10004;|&#10004;|&#10004;|&#10004;||
|Soquetes do Hyper-V|2019, 2016||||||
|Passagem/DDA de PCI|2019, 2016|&#10004;|&#10004;|&#10004;|&#10004;||
|**[Máquinas virtuais de geração 2](Feature-Descriptions-for-Linux-and-FreeBSD-virtual-machines-on-Hyper-V.md#generation-2-virtual-machines)**||||||
|Inicialização usando UEFI|2019, 2016, 2012 R2|&#10004;Observação 11, 12|&#10004;Observação 11, 12|&#10004;Observação 11, 12|&#10004;Observação 11, 12||
|Inicialização segura|2019, 2016|&#10004;|&#10004;|&#10004;|&#10004;||

## <a name="notes"></a>Observações

1. Injeção de IP estática pode não funcionar se **Gerenciador de rede** foi configurado para um determinado adaptador de rede específico do Hyper-V na máquina virtual. Para garantir o bom funcionamento do endereço IP estático injeção Certifique-se de que o Gerenciador de rede está desativado completamente ou foi desativado para um adaptador de rede específico por meio de seu **ifcfg ethX** arquivo.

2. Ao usar dispositivos de canal de fibra virtual, certifique-se de que o número de unidade lógica (LUN 0) de 0 foi preenchido. Se o LUN 0 não foi preenchido, uma máquina virtual Linux não pode ser capaz de montar dispositivos de canal de fibra nativamente.

3. Se não houver abrir identificadores de arquivo durante uma operação de backup de máquina virtual ao vivo, em seguida, em alguns casos de canto, os VHDs de backup talvez precise passar por uma verificação de consistência do sistema de arquivos (`fsck`) na restauração.

4. Operações de backup ao vivo poderá falhar, silenciosamente, se a máquina virtual tem um dispositivo iSCSI anexado ou armazenamento anexado direto (também conhecido como um disco de passagem).

5. Sobre o suporte de longo prazo versões (LTS) usam o mais recente de kernel de habilitação de Hardware (HWE) virtuais para o Linux Integration Services atualizados.

   Para instalar o kernel do Azure ajustados em 14.04, 16.04 e 18.04, execute os seguintes comandos como raiz (ou sudo):

   ```bash
   # apt-get update
   # apt-get install linux-azure
   ```

   12.04 não tem um kernel virtual separado. Para instalar o kernel HWE genérico em 12.04, execute os seguintes comandos como raiz (ou sudo):

   ```bash
   # apt-get update
   # apt-get install linux-generic-lts-trusty
   ```

   Ubuntu 12.04 os seguintes daemons do Hyper-V estão em um pacote instalado separadamente:

   * **O daemon do instantâneo do VSS** – esse daemon é necessário para criar backups de máquina virtual Linux em tempo real.
   * **O daemon KVP** – esse daemon permite definir e consultar pares chave-valor intrínsecos e extrínsecos.
   * **o daemon fcopy** – esse daemon implementa um serviço entre o host e convidado de cópia de arquivos.

   Para instalar o daemon KVP 12.04, execute os seguintes comandos como raiz (ou sudo).

   ```bash
   # apt-get install hv-kvp-daemon-init linux-tools-lts-trusty linux-cloud-tools-generic-lts-trusty
   ```

   Sempre que o kernel é atualizado, a máquina virtual deve ser reinicializada para usá-lo.

6. No Ubuntu 18.10, use o kernel mais recente do virtual para recursos do Hyper-V atualizados.

   Para instalar o kernel virtuais 18.10, execute os seguintes comandos como raiz (ou sudo):

   ```bash
   # apt-get update
   # apt-get install linux-azure
   ```

   Sempre que o kernel é atualizado, a máquina virtual deve ser reinicializada para usá-lo.

7. Suporte de memória dinâmica só está disponível em máquinas virtuais de 64 bits.

8. Operações de memória dinâmicas podem falhar se o sistema operacional convidado é com muito pouco memória. A seguir estão algumas práticas recomendadas:

   * Memória de inicialização e um mínimo de memória devem ser igual ou maior que a quantidade de memória que recomenda o fornecedor de distribuição.

   * Aplicativos que tendem a consumir toda memória disponível em um sistema estão limitados a consumir até 80 por cento de RAM disponível.

9. Se você estiver usando a memória dinâmica no Windows Server 2019, Windows Server 2016 ou sistemas operacionais Windows Server 2012/2012 R2, especifique **memória de inicialização**, **memória mínima**, e **máximo memória** parâmetros em múltiplos de 128 megabytes (MB). Falha ao fazer isso pode levar a falhas quente e talvez você não veja nenhuma memória aumentar em um sistema operacional convidado.

10. No Windows Server 2019, Windows Server 2016 ou Windows Server 2012 R2, a infraestrutura de par chave/valor pode não funcionar corretamente sem uma atualização de software do Linux. Entre em contato com seu fornecedor de distribuição para obter a atualização de software caso você veja problemas com esse recurso.

11. No Windows Server 2012 R2, as máquinas virtuais de geração 2 têm inicialização segura habilitada por padrão e algumas Linux máquinas virtuais não será inicializado, a menos que a opção de inicialização segura está desabilitada. Você pode desabilitar a inicialização segura na **Firmware** seção das configurações para a máquina virtual na **Gerenciador do Hyper-V** ou você pode desabilitá-lo usando o Powershell:

    ```Powershell
    Set-VMFirmware -VMName "VMname" -EnableSecureBoot Off
    ```

12. Antes de tentar copiar o VHD de uma máquina virtual de geração 2 VHD existente para criar novas máquinas virtuais de geração 2, siga estas etapas:

    1. Faça logon máquina de virtual de geração 2 existente.

    2. Altere o diretório para o diretório de inicialização EFI:

       ```bash
       # cd /boot/efi/EFI
       ```

    3. Copie o diretório de ubuntu para um novo diretório chamado inicialização:

       ```bash
       # sudo cp -r ubuntu/ boot
       ```

    4. Altere o diretório para o diretório de inicialização recém-criada:

       ```bash
       # cd boot
       ```

    5. Renomear o arquivo shimx64.efi:

       ```bash
       # sudo mv shimx64.efi bootx64.efi
       ```

## <a name="see-also"></a>Consulte também

* [Suporte para CentOS e Red Hat Enterprise Linux máquinas virtuais do Hyper-V](Supported-CentOS-and-Red-Hat-Enterprise-Linux-virtual-machines-on-Hyper-V.md)

* [Máquinas virtuais do Debian com suporte no Hyper-V](Supported-Debian-virtual-machines-on-Hyper-V.md)

* [Máquinas de virtuais do Oracle Linux com suporte no Hyper-V](Supported-Oracle-Linux-virtual-machines-on-Hyper-V.md)

* [De máquinas virtuais SUSE com suporte no Hyper-V](Supported-SUSE-virtual-machines-on-Hyper-V.md)

* [Descrições de recursos para máquinas virtuais de Linux e FreeBSD no Hyper-V](Feature-Descriptions-for-Linux-and-FreeBSD-virtual-machines-on-Hyper-V.md)

* [Práticas recomendadas para executar o Linux no Hyper-V](Best-Practices-for-running-Linux-on-Hyper-V.md)

* [Set-VMFirmware](https://technet.microsoft.com/library/dn464287.aspx)

* [Ubuntu 14.04 em uma geração 2 VM – Blog de virtualização Ben Armstrong](https://blogs.msdn.com/b/virtual_pc_guy/archive/2014/06/09/ubuntu-14-04-in-a-generation-2-vm.aspx)
