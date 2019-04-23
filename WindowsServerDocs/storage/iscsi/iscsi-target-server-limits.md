---
title: Limites de escalabilidade do servidor de destino de iSCSI
TOCTitle: iSCSI Target Server Scalability Limits
ms.prod: windows-server-threshold
ms.technology: storage-iscsi
ms.topic: article
author: JasonGerend
manager: dougkim
ms.author: jgerend
ms.date: 09/11/2018
ms.openlocfilehash: 9514392da133911c900f68fc8f1be260b6c91138
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59873027"
---
# <a name="iscsi-target-server-scalability-limits"></a>Limites de escalabilidade do servidor de destino de iSCSI

Aplica-se a: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Este tópico fornece os limites do servidor de destino Microsoft iSCSI testado e com suporte no Windows Server. As tabelas a seguir exibem os limites de suporte testado e, quando aplicável, se os limites são impostos.

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
<th><p>Imposto?</p></th>
<th><p>Comentário</p></th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td><p>instâncias de destino de iSCSI por servidor de destino iSCSI</p></td>
<td><p>256</p></td>
<td><p>Não</p></td>
<td></td>
</tr>
<tr class="even">
<td><p>unidades lógicas (LUs) de iSCSI ou discos virtuais por servidor de destino iSCSI</p></td>
<td><p>512</p></td>
<td><p>Não</p></td>
<td><p>Testando as configurações incluídas: 8 LUs por instância de destino com uma média destinos mais de 64 e 256 instâncias de destino com uma LU por destino.</p></td>
</tr>
<tr class="odd">
<td><p>instância de destino iSCSI LUs ou discos virtuais por iSCSI</p></td>
<td><p>256 (128 no Windows Server 2012)</p></td>
<td><p>Sim</p></td>
<td></td>
</tr>
<tr class="even">
<td><p>Sessões que podem se conectar simultaneamente a uma instância de destino iSCSI</p></td>
<td><p>544 (512 no Windows Server 2012)</p></td>
<td><p>Sim</p></td>
<td></td>
</tr>
<tr class="odd">
<td><p>Instantâneos por LU</p></td>
<td><p>512</p></td>
<td><p>Sim</p></td>
<td><p>Há um limite de 512 instantâneos por volume de iSCSI independente do aplicativo.</p></td>
</tr>
<tr class="even">
<td><p>Discos virtuais montados localmente ou instantâneos por dispositivo de armazenamento</p></td>
<td><p>32</p></td>
<td><p>Sim</p></td>
<td><p>Discos virtuais montados localmente não oferecem qualquer funcionalidade específica do iSCSI e são preteridas – para obter mais informações, consulte <a href="https://technet.microsoft.com/library/dn303411.aspx">recursos removidos ou preteridos no Windows Server 2012 R2</a>.</p></td>
</tr>
</tbody>
</table>

## <a name="fault-tolerance-limits"></a>Limita a tolerância a falhas

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
<th><p>Imposto?</p></th>
<th><p>Comentário</p></th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td><p>Nós de cluster de failover</p></td>
<td><p>8 (5 no Windows Server 2012)</p></td>
<td><p>Não</p></td>
<td></td>
</tr>
<tr class="even">
<td><p>Vários nós de cluster ativo</p></td>
<td><p>Com suporte</p></td>
<td> 
<p>N/D</p></td>
<td><p>Cada nó ativo no cluster de failover é o proprietário de uma instância clusterizada do servidor de destino de iSCSI diferentes com outros nós que atua como possíveis nós do proprietário.</p></td>
</tr>
<tr class="odd">
<td><p>Nível de recuperação de erro (ERL)</p></td>
<td><p>0</p></td>
<td><p>Sim</p></td>
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
<td><p>Não</p></td>
<td></td>
</tr>
<tr class="even">
<td><p>Entrada/saída de vários caminhos (MPIO)</p></td>
<td><p>Com suporte</p></td>
<td><p>N/D</p></td>
<td></td>
</tr>
<tr class="odd">
<td><p>Caminhos MPIO</p></td>
<td><p>4</p></td>
<td><p>Não</p></td>
<td></td>
</tr>
<tr class="even">
<td><p>Convertendo um servidor de destino de iSCSI autônomo em um cluster iSCSI servidor de destino ou vice-versa</p></td>
<td><p>Sem suporte</p></td>
<td><p>Não</p></td>
<td><p>A instância de destino iSCSI e o disco virtual dados de configuração, incluindo metadados de instantâneo for perdidos durante a conversão.</p></td>
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
<th><p>Imposto?</p></th>
<th><p>Comentário</p></th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td><p>Número máximo de adaptadores de rede do Active Directory</p></td>
<td><p>8</p></td>
<td><p>Não</p></td>
<td><p>Aplica-se aos adaptadores de rede dedicados para tráfego iSCSI, em vez do número total de adaptadores de rede no dispositivo.</p></td>
</tr>
<tr class="even">
<td><p>Portal (endereços IP) com suporte</p></td>
<td><p>64</p></td>
<td><p>Sim</p></td>
<td></td>
</tr>
<tr class="odd">
<td><p>Velocidade da porta de rede</p></td>
<td><p>1 Gbps, 10 Gbps, 40Gbps, 56 Gbps (Windows Server 2012 R2 e mais recente somente)</p></td>
<td><p>Não</p></td>
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
<td><p>Descarregamento TCP</p></td>
<td><p>Com suporte</p></td>
<td><p>N/D</p></td>
<td><p>Descarregamento de soma de verificação, aproveite enviar grandes (segmentação), moderação de interrupção e RSS</p></td>
</tr>
<tr class="odd">
<td><p>Descarregamento de iSCSI</p></td>
<td><p>Sem suporte</p></td>
<td>              
<p>N/D</p></td>
<td></td>
</tr>
<tr class="even">
<td><p>Quadros jumbo</p></td>
<td><p>Com suporte</p></td>
<td><p>N/D</p></td>
<td></td>
</tr>
<tr class="odd">
<td><p>IPSec</p></td>
<td><p>Com suporte</p></td>
<td><p>N/D</p></td>
<td></td>
</tr>
<tr class="even">
<td><p>Descarregamento CRC</p></td>
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
<th><p>Imposto?</p></th>
<th><p>Comentário</p></th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td><p>De um iniciador iSCSI convertendo o disco virtual de um disco básico para um disco dinâmico </p></td>
<td><p>Sim</p></td>
<td><p>Não</p></td>
<td></td>
</tr>
<tr class="even">
<td><p>Formato de disco rígido virtual</p></td>
<td><p>. vhdx (Windows Server 2012 R2 e mais recente somente)</p>
<p>.vhd</p></td>
<td></td>
<td></td>
</tr>
<tr class="odd">
<td><p>Tamanho mínimo do formato VHD</p></td>
<td><p>.vhdx: 3 MB</p>
<p>.vhd: 8 MB</p></td>
<td><p>Sim</p></td>
<td><p>Aplica-se a todos os tipos com suporte do VHD: pai, diferenciais e corrigido.</p></td>
</tr>
<tr class="even">
<td><p>Tamanho máximo do VHD pai</p></td>
<td><p>.vhdx: 64 TB</p>
<p>.vhd: 2 TB</p></td>
<td><p>Sim</p></td>
<td></td>
</tr>
<tr class="odd">
<td><p>Tamanho máximo de VHD fixo</p></td>
<td><p>.vhdx: 64 TB</p>
<p>.vhd: 16 TB</p></td>
<td><p>Sim</p></td>
<td></td>
</tr>
<tr class="even">
<td><p>Tamanho máximo do VHD de diferenciação</p></td>
<td><p>.vhdx: 64 TB</p>
<p>.vhd: 2 TB</p></td>
<td><p>Sim</p></td>
<td></td>
</tr>
<tr class="odd">
<td><p>VHD de formato fixo</p></td>
<td><p>Com suporte</p></td>
<td><p>Não</p></td>
<td></td>
</tr>
<tr class="even">
<td><p>Formato VHD diferencial</p></td>
<td><p>Com suporte</p></td>
<td><p>Não</p></td>
<td><p>Instantâneos não podem ser criados de discos virtuais iSCSI com base no VHD de diferenciação.</p></td>
</tr>
<tr class="odd">
<td><p>Número de VHDs diferenciais por VHD pai</p></td>
<td><p>256</p></td>
<td><p>Não (Sim, no Windows Server 2012)</p></td>
<td><p>Dois níveis de profundidade (arquivos. vhdx do netos) é o máximo dos arquivos. vhdx. um nível de profundidade (arquivos. vhd do filho) é o máximo para arquivos. vhd.</p></td>
</tr>
<tr class="even">
<td><p>Formato VHD dinâmico</p></td>
<td><p>.vhdx: Sim</p>
<p>.vhd: Sim (não no Windows Server 2012)</p></td>
<td><p>Sim</p></td>
<td><p>Cancelar o mapeamento não é suportado.</p></td>
</tr>
<tr class="odd">
<td><p>exFAT/FAT32/FAT (que hospeda o volume do VHD)</p></td>
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
<td><p>CFS não - Microsoft</p></td>
<td><p>Sem suporte</p></td>
<td><p>Sim</p></td>
<td></td>
</tr>
<tr class="even">
<td><p>Provisionamento dinâmico</p></td>
<td><p>Não</p></td>
<td><p>N/D</p></td>
<td><p>VHDs dinâmicos têm suporte, mas não há suporte para desmapeamento.</p></td>
</tr>
<tr class="odd">
<td><p>Redução de unidade lógica</p></td>
<td><p>Sim (Windows Server 2012 R2 e mais recente somente)</p></td>
<td><p>N/D</p></td>
<td><p>Use <a href="https://docs.microsoft.com/powershell/module/iscsitarget/resize-iscsivirtualdisk">Resize-iSCSIVirtualDisk</a> para reduzir um LUN.</p></td>
</tr>
<tr class="even">
<td><p>Unidade lógica de clonagem</p></td>
<td><p>Sem suporte</p></td>
<td><p>N/D</p></td>
<td><p>Rapidamente, você pode clonar dados de disco usando VHDs diferenciais.</p></td>
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
<td><p>Criar instantâneo</p></td>
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
<td><p>Instantâneo – converter em completa</p></td>
<td><p>Sem suporte</p></td>
<td></td>
</tr>
<tr class="odd">
<td><p>Instantâneo – rollback on-line</p></td>
<td><p>Sem suporte</p></td>
<td></td>
</tr>
<tr class="even">
<td><p>Instantâneo – converter em gravável</p></td>
<td><p>Sem suporte</p></td>
<td></td>
</tr>
<tr class="odd">
<td><p>Instantâneo – redirecionamento</p></td>
<td><p>Sem suporte</p></td>
<td></td>
</tr>
<tr class="even">
<td><p>O instantâneo - a fixação</p></td>
<td><p>Sem suporte</p></td>
<td></td>
</tr>
<tr class="odd">
<td><p>Montagem local</p></td>
<td><p>Com suporte</p></td>
<td><p>Discos virtuais iSCSI montado localmente são preteridos - para obter mais informações, consulte <a href="https://technet.microsoft.com/library/dn303411.aspx">recursos removidos ou preteridos no Windows Server 2012 R2</a>. Instantâneos de disco dinâmico não podem ser montados localmente.</p></td>
</tr>
</tbody>
</table>

## <a name="iscsi-target-server-manageability-and-backup"></a>backup e capacidade de gerenciamento de servidor de destino iSCSI

Se você deseja criar cópias de sombra (instantâneos do VSS arquivos abertos) de dados de volume em discos virtuais iSCSI a partir de um servidor de aplicativos, ou se você deseja gerenciar discos virtuais iSCSI com um aplicativo mais antigo (por exemplo, o comando Diskraid) que requer um hardware Virtual Disk Service (VDS) o provedor, instale o provedor de armazenamento de destino iSCSI no servidor do qual você deseja fazer um instantâneo ou usar um aplicativo de gerenciamento do VDS.

O provedor de armazenamento de destino iSCSI é um serviço de função no Windows Server 2016, Windows Server 2012 R2 e Windows Server 2012; Você também pode baixar e instalar [iSCSI provedores de armazenamento de destino (VDS/VSS) para servidores de aplicativos de nível inferior](http://www.microsoft.com/download/details.aspx?id=34759) em sistemas operacionais a seguir, desde que o servidor de destino iSCSI está em execução no Windows Server 2012:

  - Windows Storage Server 2008 R2

  - Windows Server 2008 R2

  - Windows HPC Server 2008 R2

  - Windows HPC Server 2008

Observe que, se o servidor de destino iSCSI está hospedado por um servidor executando o Windows Server 2012 R2 ou mais recente e você quiser usar o VSS ou VDS de um servidor remoto, o servidor remoto tem também executar a mesma versão do Windows Server e ter a função de provedor de armazenamento de destino iSCSI serviço e instalado. Observe também que, em todas as versões do Windows, você deve instalar apenas uma versão do serviço de função do provedor de armazenamento de destino iSCSI.

Para obter mais informações sobre o provedor de armazenamento de destino iSCSI, consulte [provedor de armazenamento de destino (VDS/VSS) iSCSI](http://blogs.technet.com/b/filecab/archive/2012/10/08/iscsi-target-storage-vds-vss-provider.aspx).

## <a name="tested-compatibility-with-iscsi-initiators"></a>Compatibilidade testada com iniciadores iSCSI

Testamos o iSCSI software de servidor de destino com os seguinte iniciadores iSCSI:

<table>
<colgroup>
<col style="width: 25%" />
<col style="width: 25%" />
<col style="width: 25%" />
<col style="width: 25%" />
</colgroup>
<tbody>
<tr class="odd">
<td><p>Iniciador</p></td>
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
<td><p>VMWare ESXi 5.0</p></td>
<td><p>Validado</p></td>
<td></td>
<td></td>
</tr>
<tr class="even">
<td><p>VMWare ESX 4.1</p></td>
<td><p>Validado</p></td>
<td></td>
<td></td>
</tr>
<tr class="odd">
<td><p>CentOS 6.x</p></td>
<td><p>Validado</p></td>
<td></td>
<td><p>Deve fazer logoff de uma sessão e logon novamente para detectar um disco virtual redimensionado.</p></td>
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
<td><p>Oracle Solaris 11.x</p></td>
<td><p>Validado</p></td>
<td></td>
<td></td>
</tr>
</tbody>
</table>

Testamos também os iniciadores iSCSI seguir efetuar uma inicialização sem disco de discos virtuais hospedados pelo servidor de destino iSCSI:

  - Windows Server 2012 R2

  - Windows Server 2012

  - PCIe NIC com iPXE

  - Disco de CD ou USB com iPXE

## <a name="see-also"></a>Consulte também

A lista a seguir fornece recursos adicionais sobre o Servidor de Destino iSCSI e as tecnologias relacionadas.

  - [Visão geral de armazenamento de bloco de destino iSCSI](iscsi-target-server.md)

  - [Visão geral de inicialização de destino iSCSI](iscsi-boot-overview.md)

  - [Armazenamento no Windows Server](..\storage.md)

