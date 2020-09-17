---
title: Práticas recomendadas para executar o FreeBSD no Hyper-V
description: Fornece recomendações para executar o FreeBSD em máquinas virtuais
ms.topic: article
ms.assetid: 0c66f1c8-2606-43a3-b4cc-166acaaf2d2a
ms.author: benarm
author: BenjaminArmstrong
ms.date: 01/09/2017
ms.openlocfilehash: 09b6f532bac2b57fd8334556501c6197fa3036cc
ms.sourcegitcommit: dd1fbb5d7e71ba8cd1b5bfaf38e3123bca115572
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/17/2020
ms.locfileid: "90747151"
---
# <a name="best-practices-for-running-freebsd-on-hyper-v"></a>Práticas recomendadas para executar o FreeBSD no Hyper-V

>Aplica-se a: Windows Server 2019, Windows Server 2016, Hyper-V Server 2016, Windows Server 2012 R2, Hyper-V Server 2012 R2, Windows Server 2012, Hyper-V Server 2012, Windows Server 2008 R2, Windows 10, Windows 8.1, Windows 8, Windows 7,1, Windows 7

Este tópico contém uma lista de recomendações para executar o FreeBSD como um sistema operacional convidado em uma máquina virtual do Hyper-V.

## <a name="enable-carp-in-freebsd-102-on-hyper-v"></a>Habilitar CARP no FreeBSD 10,2 no Hyper-V

O CARP (protocolo de redundância de endereço comum) permite que vários hosts compartilhem o mesmo endereço IP e VHID (ID de host virtual) para ajudar a fornecer alta disponibilidade para um ou mais serviços. Se um ou mais hosts falharem, os outros hosts assumirão de modo transparente para que os usuários não percebam uma falha de serviço. Para usar o CARP no FreeBSD 10,2, siga as instruções no [manual do FreeBSD](https://www.freebsd.org/doc/en/books/handbook/carp.html) e faça o seguinte no Gerenciador do Hyper-V.

* Verifique se a máquina virtual tem um adaptador de rede e se ele foi atribuído a um comutador virtual. Selecione a máquina virtual e selecione **Actions**  >  **configurações**de ações.

![Captura de tela de configurações de máquina virtual com adaptador de rede selecionado](media/Hyper-V_Settings_NetworkAdapter.png)

* Habilite a falsificação de endereço MAC. Para fazer isso,

   1. Selecione a máquina virtual e selecione **Actions**  >  **configurações**de ações.

   2. Expanda **adaptador de rede** e selecione **recursos avançados**.

   3. Selecione **habilitar falsificação de endereço MAC**.

## <a name="create-labels-for-disk-devices"></a>Criar rótulos para dispositivos de disco

Durante a inicialização, os nós de dispositivo são criados à medida que novos dispositivos são descobertos. Isso pode significar que os nomes de dispositivos podem ser alterados quando novos dispositivos são adicionados. Se você receber um erro de montagem raiz durante a inicialização, deverá criar rótulos para cada partição IDE para evitar conflitos e alterações. Para saber como, consulte [rotulando dispositivos de disco](https://www.freebsd.org/doc/handbook/geom-glabel.html). Veja a seguir exemplos.

> [!IMPORTANT]
> Faça uma cópia de backup de seu fstab antes de fazer qualquer alteração.

1. Reinicialize o sistema no modo de usuário único. Isso pode ser feito selecionando a opção de menu de inicialização 2 para FreeBSD 10.3 + (opção 4 para FreeBSD 8. x) ou executando um "boot-s" do prompt de inicialização.

2. No modo de usuário único, crie rótulos de GEOM para cada uma das partições de disco IDE listadas em seu fstab (raiz e troca). Abaixo está um exemplo de FreeBSD 10,3.

   ```bash
   # cat  /etc/fstab
   # Device           Mountpoint      FStype  Options   Dump   Pass#
   /dev/da0p2         /               ufs     rw        1       1
   /dev/da0p3         none            swap    sw        0       0

   # glabel  label rootfs  /dev/da0p2
   # glabel  label swap   /dev/da0p3
   # exit
   ```

   Informações adicionais sobre rótulos GEOM podem ser encontradas em: [rotulando dispositivos de disco](https://www.freebsd.org/doc/handbook/geom-glabel.html).

3. O sistema continuará com a inicialização de vários usuários. Depois que a inicialização for concluída, edite o/etc/fstab e substitua os nomes de dispositivo convencionais por seus respectivos rótulos. O/etc/fstab final terá a seguinte aparência:

   ```
   # Device                Mountpoint      FStype  Options         Dump    Pass#
   /dev/label/rootfs       /               ufs     rw              1       1
   /dev/label/swap         none            swap    sw              0       0
   ```

4. O sistema agora pode ser reinicializado. Se tudo correr bem, ele surgirá normalmente e a montagem mostrará:

   ```
   # mount
   /dev/label/rootfs on / (ufs, local, journaled soft-updates)
   devfs on /dev (devfs, local, mutilabel)
   ```

## <a name="use-a-wireless-network-adapter-as-the-virtual-switch"></a>Usar um adaptador de rede sem fio como o comutador virtual

Se o comutador virtual no host for baseado no adaptador de rede sem fio, reduza o tempo de expiração do ARP para 60 segundos pelo comando a seguir. Caso contrário, a rede da VM pode parar de funcionar após um tempo.


```
   # sysctl net.link.ether.inet.max_age=60
```


Confira também

* [Máquinas virtuais FreeBSD com suporte no Hyper-V](Supported-FreeBSD-virtual-machines-on-Hyper-V.md)
