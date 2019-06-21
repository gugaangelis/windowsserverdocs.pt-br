---
title: Práticas recomendadas para executar o Linux no Hyper-V
description: Fornece recomendações para executar o Linux em uma máquina virtual
ms.prod: windows-server-threshold
ms.service: na
manager: dongill
ms.technology: compute-hyper-v
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: a08648eb-eea0-4e2b-87fb-52bfe8953491
author: shirgall
ms.author: kathydav
ms.date: 3/1/2019
ms.openlocfilehash: a24e2b1a1d79d52c1cc16f9e7c1b253d9b477aae
ms.sourcegitcommit: afb0602767de64a76aaf9ce6a60d2f0e78efb78b
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/20/2019
ms.locfileid: "67284448"
---
# <a name="best-practices-for-running-linux-on-hyper-v"></a>Práticas recomendadas para executar o Linux no Hyper-V

>Aplica-se a: Windows Server 2019, Windows Server 2016, Hyper-V Server 2016, Windows Server 2012 R2, Hyper-V Server 2012 R2, Windows Server 2012, do Hyper-V Server 2012, Windows Server 2008 R2, Windows 10, Windows 8.1, Windows 8, Windows 7.1, Windows 7

Este tópico contém uma lista de recomendações para executar a máquina virtual do Linux no Hyper-V.

## <a name="tuning-linux-file-systems-on-dynamic-vhdx-files"></a>Sistemas de arquivos do Linux em arquivos VHDX dinâmicos de ajuste

Alguns sistemas de arquivos do Linux podem consumir uma quantidade significativa de espaço em disco real, mesmo quando o sistema de arquivos está basicamente vazio. Para reduzir a quantidade de uso do espaço em disco real dos arquivos VHDX dinâmicos, considere as seguintes recomendações:

* Ao criar o VHDX, use BlockSizeBytes de 1 MB (do padrão de 32MB) no PowerShell, por exemplo:

```Powershell
PS > New-VHD -Path C:\MyVHDs\test.vhdx -SizeBytes 127GB -Dynamic -BlockSizeBytes 1MB
```

* O formato ext4 é preferencial para ext3 porque ext4 é a mais de espaço eficiente que ext3 quando usado com arquivos VHDX dinâmicos.

* Quando criar o sistema de arquivos Especifica o número de grupos sejam 4096, por exemplo:

```bash
# mkfs.ext4 -G 4096 /dev/sdX1

```

## <a name="grub-menu-timeout-on-generation-2-virtual-machines"></a>Tempo limite do Menu GRUB em máquinas virtuais de geração 2

Devido ao hardware herdado que está sendo removido da emulação em máquinas virtuais de geração 2, o temporizador de contagem regressiva de menu grub contagem regressiva muito rapidamente para o menu grub a serem exibidos, carregando imediatamente a entrada padrão. Até que o grub é fixo para usar o timer de suporte para EFI, modifique **/boot/grub/grub.conf.** , /**padrão/etc/grub**, ou equivalente a ter "tempo limite = 100000" em vez do padrão "tempo limite = 5".

## <a name="pxe-boot-on-generation-2-virtual-machines"></a>Inicialização de PxE em máquinas virtuais de geração 2

Porque o temporizador PIT não está presente em máquinas virtuais de geração 2, conexões de rede para o servidor TFTP PxE podem ser encerradas prematuramente e impedir que o carregador de inicialização de ler a configuração do Grub e carregando um kernel do servidor.

No RHEL 6.x, o carregador de inicialização grub herdados v0.97 EFI pode ser usado em vez de grub2, conforme descrito aqui: [https://access.redhat.com/documentation/Red_Hat_Enterprise_Linux/6/html/Installation_Guide/s1-netboot-pxe-config-efi.html](https://access.redhat.com/documentation/Red_Hat_Enterprise_Linux/6/html/Installation_Guide/s1-netboot-pxe-config-efi.html)

Em distribuições de Linux que não seja o RHEL 6. x, etapas semelhantes podem ser seguidas para configurar v0.97 grub para carregar os kernels do Linux de um servidor PxE.

Além disso, em RHEL/CentOS 6.6 de teclado e mouse entrada não funcionará com o kernel de pré-instalação que impede especificando opções de instalação no menu. Um console serial deve ser configurado para permitir que escolher opções de instalação.

* No **efidefault** arquivo no servidor PxE, adicione o seguinte parâmetro de kernel **"console = ttyS1"**

* Na VM do Hyper-V, configurar uma porta COM o uso desse cmdlet do PowerShell:

```Powershell
Set-VMComPort -VMName <Name> -Number 2 -Path \\.\pipe\dbg1

```

Especificar um arquivo de início rápido para o kernel de pré-instalação também evita a necessidade de entrada durante a instalação de mouse e teclado.

## <a name="use-static-mac-addresses-with-failover-clustering"></a>Use endereços MAC estáticos com clustering de failover

Máquinas virtuais do Linux que serão implantadas usando clustering de failover deve ser configuradas com um endereço MAC (controle) acesso à mídia estático para cada adaptador de rede virtual. Em algumas versões do Linux, a configuração de rede pode ser perdida após o failover porque um novo endereço MAC é atribuído ao adaptador de rede virtual. Para evitar perder a configuração de rede, certifique-se de que cada adaptador de rede virtual tem um endereço MAC estático. Você pode configurar o endereço MAC, editando as configurações da máquina virtual no Gerenciador Hyper-V ou o Gerenciador de Cluster de Failover.

## <a name="use-hyper-v-specific-network-adapters-not-the-legacy-network-adapter"></a>Use adaptadores de rede específico do Hyper-V, não o adaptador de rede herdado

Configurar e usar o adaptador de Ethernet virtual, que é uma placa de rede específico do Hyper-V com desempenho aprimorado. Se herdados e adaptadores de rede específico do Hyper-V são anexados a uma máquina virtual, rede nomes na saída do **ifconfig - a** podem mostrar valores aleatórios, como **_tmp12000801310**. Para evitar esse problema, remova todos os adaptadores de rede herdados ao usar adaptadores de rede específico do Hyper-V em uma máquina virtual Linux.

## <a name="use-io-scheduler-noop-for-better-disk-io-performance"></a>Usar o Agendador de e/s NOOP para melhor desempenho de e/s do disco

O kernel do Linux tem quatro diferentes agendadores de e/s para reordenar as solicitações com algoritmos diferentes. NOOP é uma fila de primeiro a entrar primeiro a sair que passa a decisão de agenda a ser feita pelo hipervisor. É recomendável usar NOOP como o Agendador ao executar a máquina virtual do Linux no Hyper-V. Para alterar o Agendador para um dispositivo específico, na configuração do carregador de inicialização (/ etc/grub.conf, por exemplo), adicione **elevador = noop** aos parâmetros de kernel e reinicie.

## <a name="numa"></a>NUMA

Versões de kernel do Linux anteriores 2.6.37 não dão suporte a no Hyper-V com tamanhos maiores de VM. Esse problema afeta principalmente distribuições mais antigas usando upstream kernel Red Hat 2.6.32 e foi corrigido no Red Hat Enterprise Linux (RHEL) 6.6 (kernel-2.6.32-504). Sistemas que executam kernels personalizados anteriores a 2.6.37 ou com base em RHEL kernels mais antigos, que 2.6.32-504 devem definir o parâmetro de inicialização `numa=off` na linha de comando do kernel em GRUB. Para obter mais informações, consulte [Red Hat KB 436883](https://access.redhat.com/solutions/436883).

## <a name="reserve-more-memory-for-kdump"></a>Reservar mais memória para kdump

No caso do kernel de captura de despejo acaba com uma pane na inicialização, reserve mais memória para o kernel. Por exemplo, altere o parâmetro **crashkernel = 384M-:128M** à **crashkernel = 384M-:256M** no arquivo de configuração do grub Ubuntu.

## <a name="shrinking-vhdx-or-expanding-vhd-and-vhdx-files-can-result-in-erroneous-gpt-partition-tables"></a>Expandir arquivos VHD e VHDX ou reduzir VHDX pode resultar em tabelas de partição GPT incorretas

Hyper-V permite a redução de arquivos de disco virtual (VHDX) sem levar em consideração qualquer partição, volume ou estruturas de dados do sistema de arquivos que podem existir no disco. Se o VHDX é reduzido para onde o final do VHDX vem antes do final de uma partição, dados podem ser perdidos, que a partição pode se tornar dados corrompidos ou inválidos pode ser retornado quando a partição é lido.

Depois de redimensionar um VHD ou VHDX, os administradores devem usar um utilitário como fdisk ou divididas para atualizar a partição, volume e estruturas de sistema de arquivos para refletir a alteração no tamanho do disco. Reduzir ou expandir o tamanho de um VHD ou VHDX que tem uma tabela de partição GUID (GPT) fará com que um aviso quando uma ferramenta de gerenciamento de partição é usada para verificar o layout da partição e o administrador será avisado para corrigir os cabeçalhos GPT primeiro e o secundários. Essa etapa manual é segura executar sem perda de dados.

## <a name="see-also"></a>Consulte também

* [Linux e FreeBSD máquinas virtuais com suporte para Hyper-V no Windows](Supported-Linux-and-FreeBSD-virtual-machines-for-Hyper-V-on-Windows.md)

* [Práticas recomendadas para executar o FreeBSD no Hyper-V](Best-practices-for-running-FreeBSD-on-Hyper-V.md)

* [Implantar um Cluster do Hyper-V](https://technet.microsoft.com/library/jj863389.aspx)

* [Criar imagens do Linux para o Azure](https://docs.microsoft.com/azure/virtual-machines/linux/create-upload-generic)

* [Otimizar sua VM Linux no Azure](https://docs.microsoft.com/azure/virtual-machines/linux/optimization)
