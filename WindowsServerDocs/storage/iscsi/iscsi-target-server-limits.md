---
title: Limites de escalabilidade do servidor de destino iSCSI
TOCTitle: iSCSI Target Server Scalability Limits
ms.prod: windows-server
ms.technology: storage-iscsi
ms.topic: article
author: JasonGerend
manager: dougkim
ms.author: jgerend
ms.date: 09/11/2018
ms.openlocfilehash: 6799e0e3b47d6cc98cbb42407ffbed1a9578675a
ms.sourcegitcommit: 771db070a3a924c8265944e21bf9bd85350dd93c
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/27/2020
ms.locfileid: "85473433"
---
# <a name="iscsi-target-server-scalability-limits"></a>Limites de escalabilidade do servidor de destino iSCSI

Aplica-se a: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Este tópico fornece os limites de servidor de destino iSCSI da Microsoft com suporte e testados no Windows Server. As tabelas a seguir exibem os limites de suporte testados e, quando aplicável, se os limites são impostos.

## <a name="general-limits"></a>Limites gerais

<table>
<colgroup>
<col style="width: 25%" />
<col style="width: 25%" />
<col style="width: 25%" />
<col style="width: 25%" />
</colgroup>
<thead>
<tr class="header">
<th><p>Item</p></th>
<th><p>Limite de suporte</p></th>
<th><p>Imposta?</p></th>
<th><p>Comentário</p></th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td><p>instâncias de destino iSCSI por servidor de destino iSCSI</p></td>
<td><p>256</p></td>
<td><p>No</p></td>
<td></td>
</tr>
<tr class="even">
<td><p>unidades lógicas (Lu) iSCSI ou discos virtuais por servidor de destino iSCSI</p></td>
<td><p>512</p></td>
<td><p>No</p></td>
<td><p>Configurações de teste incluídas: 8 Lu por instância de destino com uma média de mais de 64 destinos e 256 instâncias de destino com uma LU por destino.</p></td>
</tr>
<tr class="odd">
<td><p>Lu iSCSI ou discos virtuais por instância de destino iSCSI</p></td>
<td><p>256 (128 no Windows Server 2012)</p></td>
<td><p>Yes</p></td>
<td></td>
</tr>
<tr class="even">
<td><p>Sessões que podem se conectar simultaneamente a uma instância de destino iSCSI</p></td>
<td><p>544 (512 no Windows Server 2012)</p></td>
<td><p>Yes</p></td>
<td></td>
</tr>
<tr class="odd">
<td><p>Instantâneos por LU</p></td>
<td><p>512</p></td>
<td><p>Yes</p></td>
<td><p>Há um limite de 512 instantâneos por volume de aplicativo iSCSI independente.</p></td>
</tr>
<tr class="even">
<td><p>Discos virtuais ou instantâneos montados localmente por dispositivo de armazenamento</p></td>
<td><p>32</p></td>
<td><p>Yes</p></td>
<td><p>Discos virtuais montados localmente Don&#39;t oferecem qualquer funcionalidade específica de iSCSI e foram preteridos – para obter mais informações, consulte <a href="https://technet.microsoft.com/library/dn303411.aspx">recursos removidos ou preteridos no Windows Server 2012 R2</a>.</p></td>
</tr>
</tbody>
</table>

## <a name="fault-tolerance-limits"></a>Limites de tolerância a falhas

<table>
<colgroup>
<col style="width: 25%" />
<col style="width: 25%" />
<col style="width: 25%" />
<col style="width: 25%" />
</colgroup>
<thead>
<tr class="header">
<th><p>Item</p></th>
<th><p>Limite de suporte</p></th>
<th><p>Imposta?</p></th>
<th><p>Comentário</p></th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td><p>Nós de cluster de failover</p></td>
<td><p>8 (5 no Windows Server 2012)</p></td>
<td><p>No</p></td>
<td></td>
</tr>
<tr class="even">
<td><p>Vários nós de cluster ativos</p></td>
<td><p>Com suporte</p></td>
<td>
<p>N/D</p></td>
<td><p>Cada nó ativo no cluster de failover possui uma instância de cluster de servidor de destino iSCSI diferente com outros nós que atuam como possíveis nós de proprietário.</p></td>
</tr>
<tr class="odd">
<td><p>Nível de recuperação de erro (ERL)</p></td>
<td><p>0</p></td>
<td><p>Yes</p></td>
<td></td>
</tr>
<tr class="even">
<td><p>Conexões por sessão</p></td>
<td><p>1</p></td>
<td><p>Sim</p></td>
<td></td>
</tr>
<tr class="odd">
<td><p>Sessões que podem se conectar simultaneamente a uma instância de destino iSCSI</p></td>
<td><p>544 (512 no Windows Server 2012)</p></td>
<td><p>No</p></td>
<td></td>
</tr>
<tr class="even">
<td><p>Entrada/saída de vários caminhos (MPIO)</p></td>
<td><p>Com suporte</p></td>
<td><p>N/D</p></td>
<td></td>
</tr>
<tr class="odd">
<td><p>Caminhos de MPIO</p></td>
<td><p>4</p></td>
<td><p>Não</p></td>
<td></td>
</tr>
<tr class="even">
<td><p>Convertendo um servidor de destino iSCSI autônomo em um servidor de destino iSCSI clusterizado ou vice-versa</p></td>
<td><p>Sem suporte</p></td>
<td><p>No</p></td>
<td><p>A instância de destino iSCSI e os dados de configuração de disco virtual, incluindo metadados de instantâneo, são perdidos durante a conversão.</p></td>
</tr>
</tbody>
</table>

## <a name="network-limits"></a>Limites de rede

<table>
<colgroup>
<col style="width: 25%" />
<col style="width: 25%" />
<col style="width: 25%" />
<col style="width: 25%" />
</colgroup>
<thead>
<tr class="header">
<th><p>Item</p></th>
<th><p>Limite de suporte</p></th>
<th><p>Imposta?</p></th>
<th><p>Comentário</p></th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td><p>Número máximo de adaptadores de rede ativos</p></td>
<td><p>8</p></td>
<td><p>No</p></td>
<td><p>Aplica-se a adaptadores de rede dedicados ao tráfego iSCSI, em vez do número total de adaptadores de rede no dispositivo.</p></td>
</tr>
<tr class="even">
<td><p>Portal (endereços IP) com suporte</p></td>
<td><p>64</p></td>
<td><p>Yes</p></td>
<td></td>
</tr>
<tr class="odd">
<td><p>Velocidade da porta de rede</p></td>
<td><p>1 Gbps, 10 Gbps, 40Gbps, 56 Gbps (Windows Server 2012 R2 e mais recente somente)</p></td>
<td><p>No</p></td>
<td></td>
</tr>
<tr class="even">
<td><p>IPv4</p></td>
<td><p>Com suporte</p></td>
<td><p>N/D</p></td>
<td></td>
</tr>
<tr class="odd">
<td><p>IPv6</p></td>
<td><p>Com suporte</p></td>
<td><p>N/D</p></td>
<td></td>
</tr>
<tr class="even">
<td><p>Descarregamento de TCP</p></td>
<td><p>Com suporte</p></td>
<td><p>N/D</p></td>
<td><p>Aproveitar envio grande (segmentação), soma de verificação, moderação de interrupção e descarregamento de RSS</p></td>
</tr>
<tr class="odd">
<td><p>descarregamento de iSCSI</p></td>
<td><p>Sem suporte</p></td>
<td><br/><p>N/D</p></td>
<td></td>
</tr>
<tr class="even">
<td><p>Quadros jumbo</p></td>
<td><p>Com suporte</p></td>
<td><p>N/D</p></td>
<td></td>
</tr>
<tr class="odd">
<td><p>IPsec</p></td>
<td><p>Com suporte</p></td>
<td><p>N/D</p></td>
<td></td>
</tr>
<tr class="even">
<td><p>Descarregamento de CRC</p></td>
<td><p>Com suporte</p></td>
<td><p>N/D</p></td>
<td></td>
</tr>
</tbody>
</table>

## <a name="iscsi-virtual-disk-limits"></a>limites de disco virtual iSCSI

<table>
<colgroup>
<col style="width: 25%" />
<col style="width: 25%" />
<col style="width: 25%" />
<col style="width: 25%" />
</colgroup>
<thead>
<tr class="header">
<th><p>Item</p></th>
<th><p>Limite de suporte</p></th>
<th><p>Imposta?</p></th>
<th><p>Comentário</p></th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td><p>De um iniciador iSCSI convertendo o disco virtual de um disco básico em um disco dinâmico </p></td>
<td><p>Sim</p></td>
<td><p>Não</p></td>
<td></td>
</tr>
<tr class="even">
<td><p>Formato de disco rígido virtual</p></td>
<td><p>. vhdx (somente Windows Server 2012 R2 e mais recente)</p>
<p>.vhd</p></td>
<td></td>
<td></td>
</tr>
<tr class="odd">
<td><p>Tamanho mínimo do formato do VHD</p></td>
<td><p>. vhdx: 3 MB</p>
<p>. vhd: 8 MB</p></td>
<td><p>Yes</p></td>
<td><p>Aplica-se a todos os tipos de VHD com suporte: pai, diferencial e fixo.</p></td>
</tr>
<tr class="even">
<td><p>Tamanho máximo do VHD pai</p></td>
<td><p>. vhdx: 64 TB</p>
<p>. vhd: 2 TB</p></td>
<td><p>Yes</p></td>
<td></td>
</tr>
<tr class="odd">
<td><p>Tamanho máximo de VHD fixo</p></td>
<td><p>. vhdx: 64 TB</p>
<p>. vhd: 16 TB</p></td>
<td><p>Yes</p></td>
<td></td>
</tr>
<tr class="even">
<td><p>Tamanho máximo de VHD diferencial</p></td>
<td><p>. vhdx: 64 TB</p>
<p>. vhd: 2 TB</p></td>
<td><p>Yes</p></td>
<td></td>
</tr>
<tr class="odd">
<td><p>Formato de VHD fixo</p></td>
<td><p>Com suporte</p></td>
<td><p>No</p></td>
<td></td>
</tr>
<tr class="even">
<td><p>Formato diferencial do VHD</p></td>
<td><p>Com suporte</p></td>
<td><p>No</p></td>
<td><p>Os instantâneos não podem ser obtidos de discos virtuais iSCSI baseados em VHD diferenciais.</p></td>
</tr>
<tr class="odd">
<td><p>Número de VHDs diferenciais por VHD pai</p></td>
<td><p>256</p></td>
<td><p>Não (sim no Windows Server 2012)</p></td>
<td><p>Dois níveis de profundidade (arquivos netos. vhdx) é o máximo para arquivos. vhdx; um nível de profundidade (arquivos filho. vhd) é o máximo para arquivos. vhd.</p></td>
</tr>
<tr class="even">
<td><p>Formato dinâmico do VHD</p></td>
<td><p>. vhdx: Sim</p>
<p>. vhd: Sim (não no Windows Server 2012)</p></td>
<td><p>Yes</p></td>
<td><p>Não há suporte para desmapeador&#39;t.</p></td>
</tr>
<tr class="odd">
<td><p>exFAT/FAT32/FAT (volume de hospedagem do VHD)</p></td>
<td><p>Sem suporte</p></td>
<td><p>Sim</p></td>
<td></td>
</tr>
<tr class="even">
<td><p>CSV v2</p></td>
<td><p>Sem suporte</p></td>
<td><p>Sim</p></td>
<td></td>
</tr>
<tr class="odd">
<td><p>ReFS</p></td>
<td><p>Com suporte</p></td>
<td><p>N/D</p></td>
<td></td>
</tr>
<tr class="even">
<td><p>NTFS</p></td>
<td><p>Com suporte</p></td>
<td><p>N/D</p></td>
<td></td>
</tr>
<tr class="odd">
<td><p>CFS não Microsoft</p></td>
<td><p>Sem suporte</p></td>
<td><p>Sim</p></td>
<td></td>
</tr>
<tr class="even">
<td><p>Provisionamento dinâmico</p></td>
<td><p>Não</p></td>
<td><p>N/D</p></td>
<td><p>Há suporte para VHDs dinâmicos, mas não há suporte para desmapeador nem&#39;t.</p></td>
</tr>
<tr class="odd">
<td><p>Redução de unidade lógica</p></td>
<td><p>Sim (somente Windows Server 2012 R2 e mais recente)</p></td>
<td><p>N/D</p></td>
<td><p>Use <a href="https://docs.microsoft.com/powershell/module/iscsitarget/resize-iscsivirtualdisk">redimension-iSCSIVirtualDisk</a> para reduzir um LUN.</p></td>
</tr>
<tr class="even">
<td><p>Clonagem de unidade lógica</p></td>
<td><p>Sem suporte</p></td>
<td><p>N/D</p></td>
<td><p>Você pode clonar rapidamente os dados do disco usando VHDs diferenciais.</p></td>
</tr>
</tbody>
</table>

## <a name="snapshot-limits"></a>Limites de instantâneo

<table>
<colgroup>
<col style="width: 33%" />
<col style="width: 33%" />
<col style="width: 33%" />
</colgroup>
<thead>
<tr class="header">
<th><p>Item</p></th>
<th><p>Limite de suporte</p></th>
<th><p>Comentário</p></th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td><p>Criação de instantâneo</p></td>
<td><p>Com suporte</p></td>
<td></td>
</tr>
<tr class="even">
<td><p>Restauração de instantâneo</p></td>
<td><p>Com suporte</p></td>
<td></td>
</tr>
<tr class="odd">
<td><p>Instantâneos graváveis</p></td>
<td><p>Sem suporte</p></td>
<td></td>
</tr>
<tr class="even">
<td><p>Instantâneo – converter para completo</p></td>
<td><p>Sem suporte</p></td>
<td></td>
</tr>
<tr class="odd">
<td><p>Instantâneo – reversão online</p></td>
<td><p>Sem suporte</p></td>
<td></td>
</tr>
<tr class="even">
<td><p>Instantâneo – converter para gravável</p></td>
<td><p>Sem suporte</p></td>
<td></td>
</tr>
<tr class="odd">
<td><p>Redirecionamento de instantâneo</p></td>
<td><p>Sem suporte</p></td>
<td></td>
</tr>
<tr class="even">
<td><p>Fixação de instantâneo</p></td>
<td><p>Sem suporte</p></td>
<td></td>
</tr>
<tr class="odd">
<td><p>Montagem local</p></td>
<td><p>Com suporte</p></td>
<td><p>Discos virtuais iSCSI montados localmente são preteridos-para obter mais informações, consulte <a href="https://technet.microsoft.com/library/dn303411.aspx">recursos removidos ou preteridos no Windows Server 2012 R2</a>. Instantâneos de disco dinâmico não podem ser montados localmente.</p></td>
</tr>
</tbody>
</table>

## <a name="iscsi-target-server-manageability-and-backup"></a>gerenciamento e backup do servidor de destino iSCSI

Se você quiser criar cópias de sombra de volume (instantâneos de arquivos abertos do VSS) de dados em discos virtuais iSCSI de um servidor de aplicativos, ou você deseja gerenciar discos virtuais iSCSI com um aplicativo mais antigo (como o comando DiskRAID) que requer um provedor de hardware VDS (serviço de disco virtual), instale o provedor de armazenamento de destino iSCSI no servidor do qual você deseja tirar um instantâneo ou usar um aplicativo de gerenciamento VDS.

O provedor de armazenamento de destino iSCSI é um serviço de função no Windows Server 2016, no Windows Server 2012 R2 e no Windows Server 2012; Você também pode baixar e instalar [provedores de armazenamento de destino iSCSI (VDS/VSS) para servidores de aplicativos de nível inferior](https://www.microsoft.com/download/details.aspx?id=34759) nos seguintes sistemas operacionais, contanto que o servidor de destino iSCSI esteja em execução no Windows Server 2012:

  - Windows Storage Server 2008 R2

  - Windows Server 2008 R2

  - Windows HPC Server 2008 R2

  - Windows HPC Server 2008

Observe que, se o servidor de destino iSCSI for hospedado por um servidor que executa o Windows Server 2012 R2 ou mais recente e você quiser usar o VSS ou VDS de um servidor remoto, o servidor remoto também deverá executar a mesma versão do Windows Server e ter o serviço de função do provedor de armazenamento de destino iSCSI instalado. Observe também que, em todas as versões do Windows, você deve instalar apenas uma versão do serviço de função do provedor de armazenamento de destino iSCSI.

Para obter mais informações sobre o provedor de armazenamento de destino iSCSI, consulte [provedor de armazenamento de destino iSCSI (VDS/VSS)](https://blogs.technet.com/b/filecab/archive/2012/10/08/iscsi-target-storage-vds-vss-provider.aspx).

## <a name="tested-compatibility-with-iscsi-initiators"></a>Compatibilidade testada com iniciadores iSCSI

Testamos o software do servidor de destino iSCSI com os seguintes iniciadores iSCSI:

<table>
<colgroup>
<col style="width: 25%" />
<col style="width: 25%" />
<col style="width: 25%" />
<col style="width: 25%" />
</colgroup>
<tbody>
<tr class="odd">
<td><p>Initiator (iniciador)</p></td>
<td><p>Windows Server 2012 R2</p></td>
<td><p>Windows Server 2012</p></td>
<td><p>Comentários</p></td>
</tr>
<tr class="even">
<td><p>Windows Server 2012 R2</p></td>
<td><p>Validado</p></td>
<td></td>
<td></td>
</tr>
<tr class="odd">
<td><p>Windows Server 2012, Windows Server 2008 R2, Windows Server 2008, Windows Server 2003</p></td>
<td><p>Validado</p></td>
<td><p>Validado</p></td>
<td></td>
</tr>
<tr class="even">
<td><p>VMWare vSphere 5</p></td>
<td></td>
<td><p>Validado</p></td>
<td></td>
</tr>
<tr class="odd">
<td><p>VMWare ESXi 5,0</p></td>
<td><p>Validado</p></td>
<td></td>
<td></td>
</tr>
<tr class="even">
<td><p>VMWare ESX 4,1</p></td>
<td><p>Validado</p></td>
<td></td>
<td></td>
</tr>
<tr class="odd">
<td><p>CentOS 6.x</p></td>
<td><p>Validado</p></td>
<td></td>
<td><p>É necessário fazer logoff de uma sessão e fazer logon novamente para detectar um disco virtual redimensionado.</p></td>
</tr>
<tr class="even">
<td><p>Red Hat Enterprise Linux 6</p></td>
<td><p>Validado</p></td>
<td></td>
<td></td>
</tr>
<tr class="odd">
<td><p>RedHat Enterprise Linux 5 e 5</p></td>
<td><p>Validado</p></td>
<td><p>Validado</p></td>
<td></td>
</tr>
<tr class="even">
<td><p>SUSE Linux Enterprise Server 10</p></td>
<td></td>
<td><p>Validado</p></td>
<td></td>
</tr>
<tr class="odd">
<td><p>Oracle Solaris 11. x</p></td>
<td><p>Validado</p></td>
<td></td>
<td></td>
</tr>
</tbody>
</table>

Também testamos os seguintes iniciadores iSCSI executando uma inicialização sem disco de discos virtuais hospedados pelo servidor de destino iSCSI:

  - Windows Server 2012 R2

  - Windows Server 2012

  - NIC PCIe com iPXE

  - CD ou disco USB com iPXE

## <a name="additional-references"></a>Referências adicionais

A lista a seguir fornece recursos adicionais sobre o Servidor de Destino iSCSI e as tecnologias relacionadas.

- [Visão geral do Armazenamento em bloco de destino iSCSI](iscsi-target-server.md)

- [iSCSI Target Boot Overview](iscsi-boot-overview.md)

- [Armazenamento no Windows Server](../storage.yml)

