---
title: Máquinas de virtuais FreeBSD com suporte no Hyper-V
description: Lista os serviços de integração do Linux e os recursos incluídos em cada versão
ms.prod: windows-server-threshold
ms.service: na
manager: dongill
ms.technology: compute-hyper-v
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 930e758f-bd50-46b4-a3a4-9857110f17b4
author: shirgall
ms.author: kathydav
ms.date: 08/30/2017
ms.openlocfilehash: 013328953321bc66b3fd30759e5be321eea32dde
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59824227"
---
# <a name="supported-freebsd-virtual-machines-on-hyper-v"></a>Máquinas de virtuais FreeBSD com suporte no Hyper-V

>Aplica-se a: Windows Server 2016, Hyper-V Server 2016, Windows Server 2012 R2, Hyper-V Server 2012 R2, Windows Server 2012, do Hyper-V Server 2012, Windows Server 2008 R2, Windows 10, Windows 8.1, Windows 8, Windows 7.1, Windows 7

O mapa de distribuição de recurso a seguir indica os recursos em cada versão. Os problemas conhecidos e soluções alternativas para cada distribuição são listadas após a tabela.

## <a name="table-legend"></a>Legenda da tabela

* **Criado** -BIS (serviço de integração do FreeBSD) são incluídos como parte desta versão do FreeBSD.

* &#10004;-Recurso disponível

* (*em branco*)-recurso não disponível

|**Recurso**|**Versão do sistema operacional Windows Server**|**11.1/11.2**|**11.0**|**10.3**|**10.2**|**10.0 - 10.1**|**9.1 - 9.3, 8.4**|
|-|-|-|-|-|-|-|-|
|**Disponibilidade**||Criado|Criado|Criado|Criado|Criado|[Portas](https://svnweb.freebsd.org/ports/branches/2015Q1/emulators/hyperv-is/) |
|**[Core](Feature-Descriptions-for-Linux-and-FreeBSD-virtual-machines-on-Hyper-V.md#BKMK_core)**|2016, 2012 R2, 2012, 2008 R2|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;|&#10004; |
|Hora precisa do Windows Server 2016|2016|&#10004;||||||
|**[Sistema de rede](Feature-Descriptions-for-Linux-and-FreeBSD-virtual-machines-on-Hyper-V.md#BKMK_Networking)**||||||||
|Quadros jumbo|2016, 2012 R2, 2012, 2008 R2|&#10004;Observação 3|&#10004;Observação 3|&#10004;Observação 3|&#10004;Observação 3|&#10004;Observação 3|&#10004;Observação 3|
|Marcação de VLAN e entroncamento|2016, 2012 R2, 2012, 2008 R2|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;|
|Migração ao vivo|2016, 2012 R2, 2012, 2008 R2|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;|
|Injeção de endereço IP estático|2016, 2012 R2, 2012|&#10004;Observação 4|&#10004;Observação 4|&#10004;Observação 4|&#10004;Observação 4|&#10004;Observação 4|&#10004;|
|vRSS|2016, 2012 R2|&#10004;|&#10004;|||||
|Descarregamento de soma de verificação e de segmentação TCP|2016, 2012 R2, 2012, 2008 R2|&#10004;|&#10004;|&#10004;|&#10004;|||
|Grande receber descarregamento (LRO)|2016, 2012 R2, 2012, 2008 R2|&#10004;|&#10004;|&#10004;||||
|SR-IOV|2016|||||||
|**[Armazenamento](Feature-Descriptions-for-Linux-and-FreeBSD-virtual-machines-on-Hyper-V.md#BKMK_Storage)**||Observação 1|Observação 1|Observação 1|Observação 1|Observação 1,2|Observação 1,2|
|Redimensionamento VHDX|2016, 2012 R2|&#10004;Observação 7|&#10004;Observação 7|||||
|Fibre Channel Virtual|2016, 2012 R2|||||||
|Backup de máquina virtual ao vivo|2016, 2012 R2|&#10004;||||||
|Suporte de CORTE|2016, 2012 R2|&#10004;||||||
|SCSI WWN|2016, 2012 R2|||||||
|**[Memória](Feature-Descriptions-for-Linux-and-FreeBSD-virtual-machines-on-Hyper-V.md#BKMK_Memory)**||||||||
|Suporte do Kernel PAE|2016, 2012 R2, 2012, 2008 R2|||||||
|Configuração de lacuna MMIO|2016, 2012 R2|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;|
|Memória dinâmica - quente|2016, 2012 R2, 2012|||||||
|Memória dinâmica - inflação|2016, 2012 R2, 2012|||||||
|Redimensionamento de memória de tempo de execução|2016|||||||
|**[Vídeo](Feature-Descriptions-for-Linux-and-FreeBSD-virtual-machines-on-Hyper-V.md#BKMK_Video)**||||||||
|Dispositivo de vídeo específico do Hyper-V|2016, 2012 R2, 2012, 2008 R2|||||||
|**[Diversos](Feature-Descriptions-for-Linux-and-FreeBSD-virtual-machines-on-Hyper-V.md#BKMK_Misc)**||||||||
|Par chave/valor|2016, 2012 R2, 2012, 2008 R2|&#10004;|&#10004;|&#10004;|&#10004;Nota 6|&#10004;Observação 5, 6|&#10004;Nota 6|
|Interrupção não mascarável|2016, 2012 R2|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;|
|Cópia do arquivo do host para a convidada|2016, 2012 R2|||||||
|comando lsvmbus|2016, 2012 R2, 2012, 2008 R2|||||||
|Soquetes do Hyper-V|2016|||||||
|Passagem/DDA de PCI|2016|&#10004;||||||
|**[Máquinas virtuais de geração 2](Feature-Descriptions-for-Linux-and-FreeBSD-virtual-machines-on-Hyper-V.md#BKMK_gen2)**||||||||
|Inicialização usando UEFI|2016, 2012 R2|&#10004;||||||
|Inicialização segura|2016|||||||

## <a name="BKMK_notes"></a>Notas

1. Sugerir [dispositivos de disco de rótulo]( https://www.freebsd.org/doc/handbook/geom-glabel.html) para evitar o erro de montagem de raiz durante a inicialização.

2. Unidade de DVD virtual pode não ser reconhecida quando drivers BIS são carregados em FreeBSD 8.x e 9.x, a menos que você habilitar o driver herdado do ATA por meio do comando a seguir.
    ```sh
    # echo ‘hw.ata.disk_enable=1’ >> /boot/loader.conf
    # shutdown -r now
    ```

3. 9126 é o que tamanho MTU o máximo com suporte.

4. Em um cenário de failover, é possível definir um endereço IPv6 estático no servidor de réplica. Use um endereço IPv4.

5. KVP é fornecido por portas no FreeBSD 10.0. Consulte a [10.0 FreeBSD portas](https://svnweb.freebsd.org/ports/branches/2015Q1/emulators/hyperv-is/) em FreeBSD.org para obter mais informações.

6. KVP pode não funcionar no Windows Server 2008 R2.

7. Para fazer o trabalho de redimensionamento online de VHDX corretamente no FreeBSD 11.0, uma etapa manual especial é necessária para contornar um bug de GEOM que foi corrigido no 11.0 +, depois que o host redimensiona o disco VHDX - abrir o disco para gravação e execute "recover gpart" da seguinte maneira.
    ```sh
    # dd if=/dev/da1 of=/dev/da1 count=0
    # gpart recover da1
    ```
**Observações adicionais**: A matriz de recurso de estável 10 e 11 estável é a mesma versão do FreeBSD 11.1. Além disso, FreeBSD 10.2 e versões anteriores (10.1, 10.0, 9, 8. x) são o fim da vida útil. Consulte [aqui](https://security.freebsd.org/) para obter uma lista atualizada das versões com suporte e os comunicados de segurança mais recentes.

**Observações adicionais**: A matriz de recurso de estável 10 e 11 estável é a mesma versão do FreeBSD 11.1. Além disso, FreeBSD 10.2 e versões anteriores (10.1, 10.0, 9, 8. x) são o fim da vida útil. Consulte [aqui](https://security.freebsd.org/) para obter uma lista atualizada das versões com suporte e os comunicados de segurança mais recentes.

## <a name="see-also"></a>Consulte também

* [Descrições de recursos para máquinas virtuais de Linux e FreeBSD no Hyper-V](Feature-Descriptions-for-Linux-and-FreeBSD-virtual-machines-on-Hyper-V.md)
* [Práticas recomendadas para executar o FreeBSD no Hyper-V](Best-practices-for-running-FreeBSD-on-Hyper-V.md)
