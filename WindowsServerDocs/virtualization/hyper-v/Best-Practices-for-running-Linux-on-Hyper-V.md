---
title: Práticas recomendadas para executar o Linux no Hyper-V
description: Fornece recomendações para executar o Linux em uma máquina virtual
ms.prod: windows-server
ms.service: na
manager: dongill
ms.technology: compute-hyper-v
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: a08648eb-eea0-4e2b-87fb-52bfe8953491
author: shirgall
ms.author: kathydav
ms.date: 3/1/2019
ms.openlocfilehash: 3488bbc1e295a68befc7044b83379bd65a5f28df
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71365571"
---
# <a name="best-practices-for-running-linux-on-hyper-v"></a>Práticas recomendadas para executar o Linux no Hyper-V

>Aplica-se a: Windows Server 2019, Windows Server 2016, Hyper-V Server 2016, Windows Server 2012 R2, Hyper-V Server 2012 R2, Windows Server 2012, Hyper-V Server 2012, Windows Server 2008 R2, Windows 10, Windows 8.1, Windows 8, Windows 7,1, Windows 7

Este tópico contém uma lista de recomendações para executar a máquina virtual do Linux no Hyper-V.

## <a name="tuning-linux-file-systems-on-dynamic-vhdx-files"></a>Ajustando sistemas de arquivos do Linux em arquivos VHDX dinâmicos

Alguns sistemas de arquivos do Linux podem consumir quantidades significativas de espaço em disco real, mesmo quando o sistema de arquivos está basicamente vazio. Para reduzir a quantidade de uso de espaço em disco real de arquivos VHDX dinâmicos, considere as seguintes recomendações:

* Ao criar o VHDX, use 1MB BlockSizeBytes (do padrão de 32MB) no PowerShell, por exemplo:

```Powershell
PS > New-VHD -Path C:\MyVHDs\test.vhdx -SizeBytes 127GB -Dynamic -BlockSizeBytes 1MB
```

* O formato ext4 é preferencial para ext3 porque o ext4 é mais eficiente do que o ext3 quando usado com arquivos VHDX dinâmicos.

* Ao criar o sistema de arquivos, especifique o número de grupos a serem 4096, por exemplo:

```bash
# mkfs.ext4 -G 4096 /dev/sdX1

```

## <a name="grub-menu-timeout-on-generation-2-virtual-machines"></a>Tempo limite do menu grub em máquinas virtuais de geração 2

Devido ao hardware herdado ser removido da emulação em máquinas virtuais de geração 2, o temporizador de contagem regressiva do menu grub conta muito rapidamente para que o menu grub seja exibido, carregando imediatamente a entrada padrão. Até que o grub seja corrigido para usar o temporizador compatível com EFI, modifique **/boot/grub/grub.conf**,**etc/default/grub**, ou equivalente para ter "Timeout = 100.000" em vez do "Timeout = 5" padrão.

## <a name="pxe-boot-on-generation-2-virtual-machines"></a>Inicialização PxE em máquinas virtuais de geração 2

Como o temporizador PIT não está presente nas máquinas virtuais de geração 2, as conexões de rede com o servidor de TFTP PxE podem ser encerradas prematuramente e impedir que o carregador de carga Leia a configuração do grub e carregue um kernel do servidor.

No RHEL 6. x, o carregador de logon EFI v 0.97 do grub herdado pode ser usado em vez de Grub2, conforme descrito aqui: [https://access.redhat.com/documentation/Red_Hat_Enterprise_Linux/6/html/Installation_Guide/s1-netboot-pxe-config-efi.html](https://access.redhat.com/documentation/Red_Hat_Enterprise_Linux/6/html/Installation_Guide/s1-netboot-pxe-config-efi.html)

Em distribuições Linux diferentes do RHEL 6. x, etapas semelhantes podem ser seguidas para configurar o grub v 0.97 para carregar kernels do Linux de um servidor PxE.

Além disso, o teclado RHEL/CentOS 6,6 e a entrada do mouse não funcionarão com o kernel de pré-instalação, que impede a especificação das opções de instalação no menu. Um console serial deve ser configurado para permitir a escolha de opções de instalação.

* No arquivo **efidefault** no servidor PxE, adicione o seguinte parâmetro de kernel **"console = ttyS1"**

* Na VM no Hyper-V, configure uma porta COM usando este cmdlet do PowerShell:

```Powershell
Set-VMComPort -VMName <Name> -Number 2 -Path \\.\pipe\dbg1

```

A especificação de um arquivo início rápido para o kernel de pré-instalação também evitaria a necessidade de entrada de teclado e mouse durante a instalação.

## <a name="use-static-mac-addresses-with-failover-clustering"></a>Usar endereços MAC estáticos com clustering de failover

As máquinas virtuais do Linux que serão implantadas usando o clustering de failover devem ser configuradas com um endereço MAC (controle de acesso à mídia) estático para cada adaptador de rede virtual. Em algumas versões do Linux, a configuração de rede pode ser perdida após o failover porque um novo endereço MAC é atribuído ao adaptador de rede virtual. Para evitar a perda da configuração de rede, verifique se cada adaptador de rede virtual tem um endereço MAC estático. Você pode configurar o endereço MAC editando as configurações da máquina virtual no Gerenciador do Hyper-V ou Gerenciador de Cluster de Failover.

## <a name="use-hyper-v-specific-network-adapters-not-the-legacy-network-adapter"></a>Usar adaptadores de rede específicos do Hyper-V, não o adaptador de rede herdado

Configure e use o adaptador Ethernet virtual, que é uma placa de rede específica do Hyper-V com desempenho aprimorado. Se os adaptadores de rede herdados e do Hyper-V forem anexados a uma máquina virtual, os nomes de rede na saída de **ifconfig-a** poderão mostrar valores aleatórios, como **_tmp12000801310**. Para evitar esse problema, remova todos os adaptadores de rede herdados ao usar adaptadores de rede específicos do Hyper-V em uma máquina virtual Linux.

## <a name="use-io-scheduler-noop-for-better-disk-io-performance"></a>Usar NOOP do Agendador de e/s para melhorar O desempenho de e/s de disco

O kernel do Linux tem quatro agendadores de e/s diferentes para reordenar solicitações com algoritmos diferentes. NOOP é uma fila de primeiro a entrar, que passa a decisão de agendamento para ser feita pelo hipervisor. É recomendável usar NOOP como o Agendador ao executar a máquina virtual do Linux no Hyper-V. Para alterar o Agendador de um dispositivo específico, na configuração do carregador de inicialização (/etc/grub.conf, por exemplo), adicione **elevador = NOOP** aos parâmetros do kernel e reinicie o.

## <a name="numa"></a>NUMA

As versões do kernel do Linux anteriores ao 2.6.37 não dão suporte a NUMA no Hyper-V com tamanhos de VM maiores. Esse problema afeta principalmente as distribuições mais antigas usando o kernel upstream Red Hat 2.6.32 e foi corrigido em Red Hat Enterprise Linux (RHEL) 6,6 (kernel-2.6.32-504). Sistemas que executam kernels personalizados com mais de 2.6.37 ou kernels baseados em RHEL mais antigos que 2.6.32-504 devem definir o parâmetro de inicialização `numa=off` na linha de comando do kernel em grub. conf. Para obter mais informações, consulte [Red Hat KB 436883](https://access.redhat.com/solutions/436883).

## <a name="reserve-more-memory-for-kdump"></a>Reserve mais memória para kdump

Caso o kernel de captura de despejo acabe com um pane na inicialização, Reserve mais memória para o kernel. Por exemplo, altere o parâmetro **crashkernel = 384M-: 128M** para **crashkernel = 384M-: 256M** no arquivo de configuração grub do Ubuntu.

## <a name="shrinking-vhdx-or-expanding-vhd-and-vhdx-files-can-result-in-erroneous-gpt-partition-tables"></a>A redução de VHDX ou a expansão de arquivos VHD e VHDX pode resultar em tabelas de partição GPT erradas

O Hyper-V permite a redução de arquivos de VHDX (disco virtual) sem considerar qualquer partição, volume ou estrutura de dados do sistema de arquivos que possa existir no disco. Se o VHDX for reduzido para onde o final do VHDX vier antes do final de uma partição, os dados poderão ser perdidos, essa partição poderá se tornar corrompida ou dados inválidos poderão ser retornados quando a partição for lida.

Depois de redimensionar um VHD ou VHDX, os administradores devem usar um utilitário como o fdisk ou parcialmente para atualizar a partição, o volume e as estruturas do sistema de arquivos para refletir a alteração no tamanho do disco. Reduzir ou expandir o tamanho de um VHD ou VHDX que tenha uma tabela de partição GUID (GPT) causará um aviso quando uma ferramenta de gerenciamento de partição for usada para verificar o layout da partição, e o administrador será avisado para corrigir os cabeçalhos GPT primeiros e secundários. Essa etapa manual é segura para ser executada sem perda de dados.

## <a name="see-also"></a>Consulte também

* [Máquinas virtuais Linux e FreeBSD com suporte para Hyper-V no Windows](Supported-Linux-and-FreeBSD-virtual-machines-for-Hyper-V-on-Windows.md)

* [Práticas recomendadas para executar o FreeBSD no Hyper-V](Best-practices-for-running-FreeBSD-on-Hyper-V.md)

* [Implantar um cluster Hyper-V](https://technet.microsoft.com/library/jj863389.aspx)

* [Criar imagens do Linux para o Azure](https://docs.microsoft.com/azure/virtual-machines/linux/create-upload-generic)

* [Otimizar sua VM do Linux no Azure](https://docs.microsoft.com/azure/virtual-machines/linux/optimization)
