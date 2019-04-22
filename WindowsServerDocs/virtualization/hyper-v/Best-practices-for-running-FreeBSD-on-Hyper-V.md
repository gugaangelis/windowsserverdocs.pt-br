---
title: Práticas recomendadas para executar o FreeBSD no Hyper-V
description: Fornece recomendações para executar o FreeBSD em máquinas virtuais
ms.prod: windows-server-threshold
ms.service: na
manager: dongill
ms.technology: compute-hyper-v
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 0c66f1c8-2606-43a3-b4cc-166acaaf2d2a
author: shirgall
ms.author: kathydav
ms.date: 01/09/2017
ms.openlocfilehash: 1a8efb4a991169258d6a11999513ce97d98130eb
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59816947"
---
# <a name="best-practices-for-running-freebsd-on-hyper-v"></a>Práticas recomendadas para executar o FreeBSD no Hyper-V

>Aplica-se a: Windows Server 2016, Hyper-V Server 2016, Windows Server 2012 R2, Hyper-V Server 2012 R2, Windows Server 2012, do Hyper-V Server 2012, Windows Server 2008 R2, Windows 10, Windows 8.1, Windows 8, Windows 7.1, Windows 7

Este tópico contém uma lista de recomendações para executar o FreeBSD como um sistema operacional convidado em uma máquina virtual de Hyper-V.

## <a name="enable-carp-in-freebsd-102-on-hyper-v"></a>Habilitar CARP no FreeBSD 10.2 no Hyper-V

O protocolo de redundância de endereço comum (CARP) permite que vários hosts para compartilhar o mesmo endereço IP e a ID de Host Virtual (VHID) para ajudar a fornecer alta disponibilidade para um ou mais serviços. Se um ou mais hosts falharem, outros hosts será assumir transparentemente para que os usuários não perceberão uma falha de serviço. Para usar CARP no FreeBSD 10.2, siga as instruções de [manual do FreeBSD](https://www.freebsd.org/doc/en/books/handbook/carp.html) e faça o seguinte no Gerenciador do Hyper-V.

* Verifique se a máquina virtual tem um adaptador de rede e ele foi atribuído a um comutador virtual. Selecione a máquina virtual e selecione **ações** > **configurações**.

![Captura de tela de configurações da máquina virtual com o adaptador de rede selecionado](media/Hyper-V_Settings_NetworkAdapter.png)

* Habilite a falsificação de endereço MAC. Para fazer isso,

   1. Selecione a máquina virtual e selecione **ações** > **configurações**.

   2. Expandir **adaptador de rede** e selecione **recursos avançados**.

   3. Selecione **falsificação de endereço de MAC habilitar**.

## <a name="create-labels-for-disk-devices"></a>Criar rótulos para os dispositivos de disco

Durante a inicialização, nós de dispositivo são criadas conforme novos dispositivos são descobertos. Isso pode significar que os nomes de dispositivo podem alterar quando novos dispositivos são adicionados. Se você receber um erro de montagem de raiz durante a inicialização, você deve criar rótulos para cada partição IDE para evitar conflitos e alterações. Para saber como, consulte [rotulagem dispositivos de disco](https://www.freebsd.org/doc/handbook/geom-glabel.html). A seguir estão exemplos. 

> [!IMPORTANT]
> Faça uma cópia de backup de seu fstab antes de fazer alterações.

1. Reinicie o sistema no modo de usuário único. Isso pode ser feito selecionando a opção de menu de inicialização 2 para FreeBSD 10.3 + (opção 4 para FreeBSD 8.x), ou executar o '-s de inicialização' do prompt de inicialização.

2. No modo de usuário único, crie rótulos GEOM para cada uma das partições de disco IDE listadas na sua fstab (raiz e troca). Abaixo está um exemplo do FreeBSD 10.3.

   ```bash
   # cat  /etc/fstab
   # Device           Mountpoint      FStype  Options   Dump   Pass#
   /dev/da0p2         /               ufs     rw        1       1
   /dev/da0p3         none            swap    sw        0       0

   # glabel  label rootfs  /dev/da0p2
   # glabel  label swap   /dev/da0p3
   # exit
   ```

   Informações adicionais nos rótulos GEOM podem ser encontradas em: [Dispositivos de disco de rotulagem](https://www.freebsd.org/doc/handbook/geom-glabel.html).

3. O sistema continuará com a inicialização de vários usuário. Após a inicialização, editar o /etc/fstab e substitua os nomes de dispositivo convencionais, com seus respectivos rótulos. O /etc/fstab final terá esta aparência:

   ```
   # Device                Mountpoint      FStype  Options         Dump    Pass#
   /dev/label/rootfs       /               ufs     rw              1       1
   /dev/label/swap         none            swap    sw              0       0

   ```

4.  O sistema agora pode ser reinicializado. Caso tudo tenha corrido bem, ele aparecerá normalmente e montagem mostrará:
   
   ```
   # mount
   /dev/label/rootfs on / (ufs, local, journaled soft-updates)
   devfs on /dev (devfs, local, mutilabel)
   ```

## <a name="use-a-wireless-network-adapter-as-the-virtual-switch"></a>Usar um adaptador de rede sem fio como o comutador virtual

Se o comutador virtual no host é baseado no adaptador de rede sem fio, reduza o tempo de expiração de ARP como 60 segundos, o comando a seguir. Caso contrário, o sistema de rede da VM pode parar de funcionar após alguns instantes.


```
   # sysctl net.link.ether.inet.max_age=60
```


Consulte também

* [Máquinas de virtuais FreeBSD com suporte no Hyper-V](Supported-FreeBSD-virtual-machines-on-Hyper-V.md)
