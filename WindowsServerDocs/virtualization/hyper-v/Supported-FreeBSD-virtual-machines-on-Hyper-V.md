---
title: Máquinas virtuais FreeBSD com suporte no Hyper-V
description: Lista os serviços e recursos de integração do Linux incluídos em cada versão
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
ms.openlocfilehash: a6e9c6e3bec2001c73254ffd813954f04a37a714
ms.sourcegitcommit: 6f968368c12b9dd699c197afb3a3d13c2211f85b
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/26/2019
ms.locfileid: "68544718"
---
# <a name="supported-freebsd-virtual-machines-on-hyper-v"></a>Máquinas virtuais FreeBSD com suporte no Hyper-V

>Aplica-se a: Windows Server 2016, Hyper-V Server 2016, Windows Server 2012 R2, Hyper-V Server 2012 R2, Windows Server 2012, Hyper-V Server 2012, Windows Server 2008 R2, Windows 10, Windows 8.1, Windows 8, Windows 7,1, Windows 7

O mapa de distribuição de recursos a seguir indica os recursos em cada versão. Os problemas conhecidos e as soluções alternativas para cada distribuição são listados após a tabela.

## <a name="table-legend"></a>Legenda da tabela

* **Interno** – o bis (serviço de integração do FreeBSD) é incluído como parte desta versão do FreeBSD.

* &#10004;-Recurso disponível

* (*em branco*)-recurso não disponível

|**Recurso**|**Versão do sistema operacional Windows Server**|**11.1/11.2**|**11,0**|**10,3**|**10,2**|**10,0-10,1**|**9,1-9,3, 8,4**|
|-|-|-|-|-|-|-|-|
|**Disponibilidade**||Interno|Interno|Interno|Interno|Interno|[Porta](https://svnweb.freebsd.org/ports/branches/2015Q1/emulators/hyperv-is/) |
|**[Core](Feature-Descriptions-for-Linux-and-FreeBSD-virtual-machines-on-Hyper-V.md#core)**|2019, 2016, 2012 R2, 2012, 2008 R2|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;|&#10004; |
|Tempo preciso do Windows Server 2016|2019, 2016|&#10004;||||||
|**[Rede](Feature-Descriptions-for-Linux-and-FreeBSD-virtual-machines-on-Hyper-V.md#networking)**||||||||
|Quadros jumbo|2019, 2016, 2012 R2, 2012, 2008 R2|&#10004;Observação 3|&#10004;Observação 3|&#10004;Observação 3|&#10004;Observação 3|&#10004;Observação 3|&#10004;Observação 3|
|Marcação e entroncamento de VLAN|2019, 2016, 2012 R2, 2012, 2008 R2|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;|
|Migração ao vivo|2019, 2016, 2012 R2, 2012, 2008 R2|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;|
|Injeção de IP estático|2019, 2016, 2012 R2, 2012|&#10004;Observação 4|&#10004;Observação 4|&#10004;Observação 4|&#10004;Observação 4|&#10004;Observação 4|&#10004;|
|vRSS|2019, 2016, 2012 R2|&#10004;|&#10004;|||||
|Segmentação de TCP e descarregamentos de soma de verificação|2019, 2016, 2012 R2, 2012, 2008 R2|&#10004;|&#10004;|&#10004;|&#10004;|||
|Descarregamento de recebimento grande (LRO)|2019, 2016, 2012 R2, 2012, 2008 R2|&#10004;|&#10004;|&#10004;||||
|SR-IOV|2019, 2016|||||||
|**[Repositório](Feature-Descriptions-for-Linux-and-FreeBSD-virtual-machines-on-Hyper-V.md#storage)**||Observação 1|Observação 1|Observação 1|Observação 1|Observação 1, 2|Observação 1, 2|
|Redimensionamento de VHDX|2019, 2016, 2012 R2|&#10004;Observação 7|&#10004;Observação 7|||||
|Fibre Channel Virtual|2019, 2016, 2012 R2|||||||
|Backup de máquina virtual ao vivo|2019, 2016, 2012 R2|&#10004;||||||
|Suporte a corte|2019, 2016, 2012 R2|&#10004;||||||
|WWN DO SCSI|2019, 2016, 2012 R2|||||||
|**[Memória](Feature-Descriptions-for-Linux-and-FreeBSD-virtual-machines-on-Hyper-V.md#memory)**||||||||
|Suporte ao kernel de PAE|2019, 2016, 2012 R2, 2012, 2008 R2|||||||
|Configuração da lacuna de MMIO|2019, 2016, 2012 R2|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;|
|Memória Dinâmica-adição a quente|2019, 2016, 2012 R2, 2012|||||||
|Memória Dinâmica-balões|2019, 2016, 2012 R2, 2012|||||||
|Redimensionamento de memória de Runtime|2019, 2016|||||||
|**[Monitor](Feature-Descriptions-for-Linux-and-FreeBSD-virtual-machines-on-Hyper-V.md#video)**||||||||
|Dispositivo de vídeo específico do Hyper-V|2019, 2016, 2012 R2, 2012, 2008 R2|||||||
|**[Várias](Feature-Descriptions-for-Linux-and-FreeBSD-virtual-machines-on-Hyper-V.md#miscellaneous)**||||||||
|Par chave/valor|2019, 2016, 2012 R2, 2012, 2008 R2|&#10004;|&#10004;|&#10004;|&#10004;Nota 6|&#10004;Observação 5, 6|&#10004;Nota 6|
|Interrupção não mascarável|2019, 2016, 2012 R2|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;|
|Cópia de arquivo do host para o convidado|2019, 2016, 2012 R2|||||||
|comando lsvmbus|2019, 2016, 2012 R2, 2012, 2008 R2|||||||
|Soquetes do Hyper-V|2019, 2016|||||||
|Passagem de PCI/DDA|2019, 2016|&#10004;||||||
|**[Máquinas virtuais de geração 2](Feature-Descriptions-for-Linux-and-FreeBSD-virtual-machines-on-Hyper-V.md#generation-2-virtual-machines)**||||||||
|Inicializar usando UEFI|2019, 2016, 2012 R2|&#10004;||||||
|Inicialização segura|2019, 2016|||||||

## <a name="BKMK_notes"></a>Registra

1. Sugira para [rotular dispositivos de disco]( https://www.freebsd.org/doc/handbook/geom-glabel.html) para evitar erro de montagem raiz durante a inicialização.

2. A unidade de DVD virtual pode não ser reconhecida quando os drivers do BIS são carregados no FreeBSD 8. x e 9. x, a menos que você habilite o driver do ATA herdado por meio do comando a seguir.
    ```sh
    # echo ‘hw.ata.disk_enable=1’ >> /boot/loader.conf
    # shutdown -r now
    ```

3. 9126 é o tamanho máximo de MTU com suporte.

4. Em um cenário de failover, você não pode definir um endereço IPv6 estático no servidor de réplica. Em vez disso, use um endereço IPv4.

5. O KVP é fornecido por portas no FreeBSD 10,0. Consulte as [portas FreeBSD 10,0](https://svnweb.freebsd.org/ports/branches/2015Q1/emulators/hyperv-is/) no FreeBSD.org para obter mais informações.

6. KVP pode não funcionar no Windows Server 2008 R2.

7. Para que o redimensionamento online de VHDX funcione corretamente no FreeBSD 11,0, uma etapa manual especial é necessária para contornar um bug GEOM que é corrigido no 11.0 +, depois que o host redimensiona o disco VHDX, abra o disco para gravação e execute "Gpart Recover" da seguinte maneira.
    ```sh
    # dd if=/dev/da1 of=/dev/da1 count=0
    # gpart recover da1
    ```
   **Observações adicionais**: A matriz de recursos de 10 estável e 11 estável é a mesma que a versão FreeBSD 11,1. Além disso, o FreeBSD 10,2 e as versões anteriores (10,1, 10,0, 9. x, 8. x) são o fim da vida útil. Consulte [aqui](https://security.freebsd.org/) uma lista atualizada de versões com suporte e as mais recentes consultorias de segurança.

**Observações adicionais**: A matriz de recursos de 10 estável e 11 estável é a mesma que a versão FreeBSD 11,1. Além disso, o FreeBSD 10,2 e as versões anteriores (10,1, 10,0, 9. x, 8. x) são o fim da vida útil. Consulte [aqui](https://security.freebsd.org/) uma lista atualizada de versões com suporte e as mais recentes consultorias de segurança.

## <a name="see-also"></a>Consulte também

* [Descrições de recursos para máquinas virtuais Linux e FreeBSD no Hyper-V](Feature-Descriptions-for-Linux-and-FreeBSD-virtual-machines-on-Hyper-V.md)
* [Práticas recomendadas para executar o FreeBSD no Hyper-V](Best-practices-for-running-FreeBSD-on-Hyper-V.md)
