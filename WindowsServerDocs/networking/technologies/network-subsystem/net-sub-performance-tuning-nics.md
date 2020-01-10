---
title: Adaptadores de rede para ajuste do desempenho
description: Este tópico faz parte do guia de ajuste de desempenho do subsistema de rede para o Windows Server 2016.
audience: Admin
ms.custom:
- CI ID 111485
- CSSTroubleshoot
ms.prod: windows-server
ms.technology: networking
ms.topic: article
ms.assetid: 0b9b0f80-415c-4f5e-8377-c09b51d9c5dd
manager: dcscontentpm
ms.author: pashort
author: Teresa-Motiv
ms.date: 12/23/2019
ms.openlocfilehash: 3feec719934fb16ca34cebe1e653768da5fb9eb7
ms.sourcegitcommit: 33c89b76ac902927490b9727f3cf92b374754699
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 01/08/2020
ms.locfileid: "75728427"
---
# <a name="performance-tuning-network-adapters"></a>Adaptadores de rede para ajuste do desempenho

> Aplica-se a: Windows Server 2019, Windows Server 2016, Windows Server (canal semestral)

Use as informações neste tópico para ajustar os adaptadores de rede de desempenho para computadores que executam o Windows Server 2016 e versões posteriores. Se os adaptadores de rede fornecerem opções de ajuste, você poderá usar essas opções para otimizar a taxa de transferência de rede e o uso de recursos.

As configurações de ajuste corretas para seus adaptadores de rede dependem das seguintes variáveis:

- O adaptador de rede e seu conjunto de recursos  
- O tipo de carga de trabalho que o servidor executa  
- Os recursos de hardware e software do servidor  
- Seus objetivos de desempenho para o servidor  

As seções a seguir descrevem algumas de suas opções de ajuste de desempenho.  

##  <a name="bkmk_offload"></a>Habilitando recursos de descarregamento

O ajuste nos recursos de descarregamento do adaptador de rede normalmente é benéfico. No entanto, o adaptador de rede pode não ser eficiente o suficiente para lidar com os recursos de descarregamento com alta taxa de transferência.

> [!IMPORTANT]
> Não use os recursos de descarregamento Descarregamento de **tarefa IPSec** ou **descarregamento de Chimney TCP**. Essas tecnologias são preteridas no Windows Server 2016 e podem afetar negativamente o desempenho do servidor e da rede. Além disso, essas tecnologias podem não ter suporte da Microsoft no futuro.

Por exemplo, considere um adaptador de rede que tenha recursos de hardware limitados.
Nesse caso, habilitar os recursos de descarregamento de segmentação pode reduzir a taxa de transferência máxima sustentável do adaptador. No entanto, se a taxa de transferência reduzida for aceitável, você deverá seguir um habilitar os recursos de descarga de segmentação.

> [!NOTE]  
> Alguns adaptadores de rede exigem que você habilite recursos de descarregamento independentemente para os caminhos de envio e recebimento.

##  <a name="bkmk_rss_web"></a>Habilitando o RSS (recebimento em escala) para servidores Web

O RSS pode melhorar a escalabilidade e o desempenho da Web quando há menos adaptadores de rede do que processadores lógicos no servidor. Quando todo o tráfego da Web está passando por adaptadores de rede compatíveis com RSS, o servidor pode processar solicitações da Web de entrada de diferentes conexões simultaneamente em diferentes CPUs.

> [!IMPORTANT]  
> Evite usar adaptadores de rede não RSS e adaptadores de rede compatíveis com RSS no mesmo servidor. Devido à lógica de distribuição de carga em RSS e HTTP (Hypertext Transfer Protocol), o desempenho poderá ser seriamente degradado se um adaptador de rede compatível com RSS aceitar o tráfego da Web em um servidor que tenha um ou mais adaptadores de rede compatíveis com RSS. Nesse caso, você deve usar adaptadores de rede habilitados para RSS ou desabilitar RSS na guia **Propriedades Avançadas** das propriedades do adaptador de rede.
>  
> Para determinar se um adaptador de rede está habilitado para RSS, você pode visualizar as informações RSS na guia **Propriedades Avançadas** das propriedades do adaptador de rede.

### <a name="rss-profiles-and-rss-queues"></a>Perfis RSS e filas RSS

O perfil RSS predefinido padrão é **NUMAStatic**, que difere do padrão que as versões anteriores do Windows usavam. Antes de começar a usar perfis RSS, examine os perfis disponíveis para entender quando eles são benéficos e como eles se aplicam ao seu ambiente de rede e ao hardware.

Por exemplo, se você abrir o Gerenciador de tarefas e examinar os processadores lógicos em seu servidor e eles parecerem estar subutilizados para receber tráfego, você poderá tentar aumentar o número de filas RSS do padrão de dois para o máximo ao qual o adaptador de rede dá suporte. Seu adaptador de rede pode ter opções para alterar o número de filas RSS como parte do driver.

##  <a name="bkmk_resources"></a>Aumentando os recursos do adaptador de rede

Para adaptadores de rede que permitem configurar manualmente recursos, como buffers de recebimento e envio, você deve aumentar os recursos alocados.  

Alguns adaptadores de rede configuram seus buffers de recebimento como baixos para preservar a memória alocada do host. O valor baixo resulta em pacotes descartados e desempenho reduzido. Portanto, para cenários de recebimento intenso, recomendamos aumentar o valor do buffer de recebimento para o máximo.

> [!NOTE]  
> Se um adaptador de rede não expõe a configuração manual de recursos, ele configura dinamicamente os recursos ou os recursos são definidos com um valor fixo que não pode ser alterado.

### <a name="enabling-interrupt-moderation"></a>Habilitando a moderação de interrupção

Para controlar a moderação da interrupção, alguns adaptadores de rede expõem diferentes níveis de moderação de interrupção, diferentes parâmetros de União de buffer (às vezes separadamente para buffers de envio e recebimento) ou ambos.

Você deve considerar a moderação da interrupção para cargas de trabalho vinculadas à CPU. Ao usar a moderação de interrupção, considere a compensação entre as economias e a latência da CPU do host versus a economia de CPU do host maior devido a mais interrupções e menos latência. Se o adaptador de rede não executar a moderação de interrupção, mas expor a União de buffer, você poderá melhorar o desempenho aumentando o número de buffers agrupados para permitir mais buffers por envio ou recebimento.

##  <a name="bkmk_low"></a>Ajuste de desempenho para processamento de pacotes de baixa latência

Muitos adaptadores de rede fornecem opções para otimizar a latência induzida pelo sistema operacional. A latência é o tempo decorrido entre o momento em que o driver de rede processa um pacote recebido e o driver de rede envia o pacote de volta. Esse tempo geralmente é medido em microssegundos. Para comparar, o tempo de transmissão nas transmissões de pacote em longas distâncias geralmente é medido em milissegundos (uma ordem de magnitude maior). Esse ajuste não reduzirá o tempo que um pacote gasta em trânsito.

A seguir estão algumas sugestões de ajuste no desempenho para redes sensíveis a microssegundo.

- Defina o BIOS do computador para **Alto Desempenho**, com estados C desabilitados. Entretanto, observe que isso depende do sistema e do BIOS, alguns sistemas fornecerão maior desempenho se o sistema operacional controlar o gerenciamento de energia. Você pode verificar e ajustar suas configurações de gerenciamento de energia de **configurações** ou usando o comando **powercfg** . Para obter mais informações, consulte [Opções de linha de comando powercfg](https://docs.microsoft.com/windows-hardware/design/device-experiences/powercfg-command-line-options).

- Defina o perfil de gerenciamento de energia do sistema operacional para **Sistema de alto desempenho**.  
   > [!NOTE]  
   > Essa configuração não funcionará corretamente se o BIOS do sistema tiver sido definido para desabilitar o controle do sistema operacional do gerenciamento de energia.

- Habilitar descarregamentos estáticos. Por exemplo, habilite as somas de verificação de UDP, somas de verificação de TCP e configurações de descarga grande (LSO).

- Se o tráfego for de vários fluxos, como ao receber o tráfego de multicast de alto volume, habilite o RSS.

- Desabilite a configuração **Moderação de interrupção** dos drivers da placa de rede que requerem a menor latência possível. Lembre-se de que essa configuração pode usar mais tempo de CPU e representa uma compensação.

- Manipule as interrupções do adaptador de rede e DPCs em um processador de núcleo que compartilha cache de CPU com o núcleo que está sendo usado pelo programa (thread de usuário) que está lidando com o pacote. O ajuste na afinidade da CPU pode ser usado para direcionar um processo para determinados processadores lógicos em conjunto com a configuração de RSS com essa finalidade. O uso do mesmo núcleo para interrupção, DPC e thread de modo de usuário revela piora no desempenho na medida em que a carga aumenta, porque o ISR, DPC e thread disputam o uso do núcleo.

##  <a name="bkmk_smi"></a>Interrupções de gerenciamento do sistema

Muitos sistemas de hardware usam o SMI (System Management interrupções) para uma variedade de funções de manutenção, como erros de memória ECC (código de correção de erro), mantendo a compatibilidade com USB herdada, controlando o ventilador e gerenciando a energia controlada pelo BIOS Configurações.

O SMI é a interrupção de prioridade mais alta no sistema e coloca a CPU em um modo de gerenciamento. Esse modo preempção de todas as outras atividades enquanto o SMI executa uma rotina de serviço de interrupção, normalmente contida no BIOS.

Infelizmente, esse comportamento pode resultar em picos de latência de 100 microssegundos ou mais.

Se for necessário obter a menor latência, você deve solicitar uma versão do BIOS, junto a seu fornecedor de hardware, que reduza as SMIs para o menor nível possível. Essas versões de BIOS são frequentemente chamadas de "BIOS de baixa latência" ou "BIOS gratuito do SMI". Em alguns casos, não é possível para uma plataforma de hardware eliminar a atividade de SMI completamente, ela é usada para controlar funções essenciais (por exemplo, ventiladores de refrigeração).

> [!NOTE]  
> O sistema operacional não pode controlar SMIs porque o processador lógico está sendo executado em um modo de manutenção especial, o que impede a intervenção do sistema operacional.

##  <a name="bkmk_tcp"></a>Ajuste de desempenho TCP

 Você pode usar os seguintes itens para ajustar o desempenho de TCP.

###  <a name="bkmk_tcp_params"></a>Ajuste automática da janela de recepção TCP

No Windows Vista, no Windows Server 2008 e em versões posteriores do Windows, a pilha de rede do Windows usa um recurso chamado *nível de ajuste da janela de recepção TCP* para negociar o tamanho da janela de recepção TCP. Esse recurso pode negociar um tamanho de janela de recebimento definido para cada comunicação TCP durante o handshake TCP.

Nas versões anteriores do Windows, a pilha de rede do Windows usava uma janela de recepção de tamanho fixo (65.535 bytes) que limitou a taxa de transferência potencial geral para conexões. A taxa de transferência atingível total de conexões TCP pode limitar os cenários de uso da rede. A janela de recepção TCP habilita esses cenários para usar totalmente a rede.

Para uma janela de recepção TCP que tem um tamanho específico, você pode usar a equação a seguir para calcular a taxa de transferência total de uma única conexão.

> *Taxa de transferência atingível total em bytes* = *tamanho da janela de recepção TCP em bytes* \* (1/ *latência de conexão em segundos*)

Por exemplo, para uma conexão que tem uma latência de 10 ms, a taxa de transferência atingível total é de apenas 51 Mbps. Esse valor é razoável para uma grande infraestrutura de rede corporativa. No entanto, usando a AutoAjuste para ajustar a janela de recebimento, a conexão pode atingir a taxa de linha completa de uma conexão de 1 Gbps.  

Alguns aplicativos definem o tamanho da janela de recepção TCP. Se o aplicativo não definir o tamanho da janela de recebimento, a velocidade do link determinará o tamanho da seguinte maneira:

- Menos de 1 megabit por segundo (Mbps): 8 quilobytes (KB)
- 1 Mbps a 100 Mbps: 17 KB
- 100 Mbps a 10 gigabits por segundo (Gbps): 64 KB
- 10 Gbps ou mais rápido: 128 KB

Por exemplo, em um computador que tem um adaptador de rede de 1 Gbps instalado, o tamanho da janela deve ser 64 KB.

Esse recurso também faz uso completo de outros recursos para melhorar o desempenho da rede. Esses recursos incluem o restante das opções de TCP que são definidas no [RFC 1323](https://tools.ietf.org/html/rfc1323). Usando esses recursos, os computadores baseados no Windows podem negociar tamanhos de janela de recepção TCP menores, mas são dimensionados em um valor definido, dependendo da configuração. Esse comportamento é mais fácil de lidar com dispositivos de rede.

> [!NOTE]  
> Você pode enfrentar um problema em que o dispositivo de rede não está em conformidade com a **opção de dimensionamento de janela TCP**, conforme definido no [RFC 1323](https://tools.ietf.org/html/rfc1323) e, portanto, não dá suporte ao fator de escala. Nesses casos, consulte este [KB 934430, a conectividade de rede falha quando você tenta usar o Windows Vista por trás de um dispositivo de firewall](https://support.microsoft.com/help/934430/network-connectivity-fails-when-you-try-to-use-windows-vista-behind-a) ou entrar em contato com a equipe de suporte para o fornecedor do dispositivo de rede.  

#### <a name="review-and-configure-tcp-receive-window-autotuning-level"></a>Examinar e configurar o nível de ajuste automática da janela de recepção TCP

Você pode usar comandos netsh ou cmdlets do Windows PowerShell para revisar ou modificar o nível de ajuste da janela de recepção TCP.

> [!NOTE]  
> Ao contrário das versões do Windows que são anteriores ao Windows 10 ou ao Windows Server 2019, você não pode mais usar o registro para configurar o tamanho da janela de recepção TCP. Para obter mais informações sobre as configurações preteridas, consulte [parâmetros TCP preteridos](#deprecated-tcp-parameters).

> [!NOTE]  
> Para obter informações detalhadas sobre os níveis de ajuste automática disponíveis, consulte [níveis de ajuste](#autotuning-levels)automática.

**Para usar o netsh para revisar ou modificar o nível de ajuste automática**

Para examinar as configurações atuais, abra uma janela de prompt de comando e execute o seguinte comando:

```cmd
netsh interface tcp show global
```

A saída desse comando deve ser semelhante ao seguinte:

```
Querying active state...

TCP Global Parameters  
-----
Receive-Side Scaling State : enabled
Chimney Offload State : disabled
Receive Window Auto-Tuning Level : normal
Add-On Congestion Control Provider : default
ECN Capability : disabled
RFC 1323 Timestamps : disabled
Initial RTO : 3000
Receive Segment Coalescing State : enabled
Non Sack Rtt Resiliency : disabled
Max SYN Retransmissions : 2
Fast Open : enabled
Fast Open Fallback : enabled
Pacing Profile : off
```

Para modificar a configuração, execute o seguinte comando no prompt de comando:

```cmd
netsh interface tcp set global autotuninglevel=<Value>
```

> [!NOTE]  
> No comando anterior, \<*valor*> representa o novo valor para o nível de ajuste automático.

Para obter mais informações sobre esse comando, consulte [comandos netsh para protocolo de controle de transmissão de interface](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/cc731258(v=ws.10)).

**Para usar o PowerShell para revisar ou modificar o nível de ajuste automática**

Para examinar as configurações atuais, abra uma janela do PowerShell e execute o cmdlet a seguir.

```PowerShell
Get-NetTCPSetting | Select SettingName,AutoTuningLevelLocal
```

A saída desse cmdlet deve ser semelhante ao seguinte.

```
SettingName           AutoTuningLevelLocal
-----------          --------------------
Automatic
InternetCustom       Normal
DatacenterCustom     Normal
Compat               Normal
Datacenter           Normal
Internet             Normal
```

Para modificar a configuração, execute o seguinte cmdlet no prompt de comando do PowerShell.

```PowerShell
Set-NetTCPSetting -AutoTuningLevelLocal <Value>
```

> [!NOTE]  
> No comando anterior, \<*valor*> representa o novo valor para o nível de ajuste automático.

Para obter mais informações sobre esses cmdlets, consulte os seguintes artigos:

- [Get-NetTCPSetting](https://docs.microsoft.com/powershell/module/nettcpip/get-nettcpsetting?view=win10-ps)
- [Set-NetTCPSetting](https://docs.microsoft.com/powershell/module/nettcpip/set-nettcpsetting?view=win10-ps)

#### <a name="autotuning-levels"></a>Níveis de ajuste automática

Você pode definir o ajuste automática da janela de recebimento para qualquer um dos cinco níveis. O nível padrão é **normal**. A tabela a seguir descreve os níveis.

|Nível |Valor hexadecimal |Comentários |
| --- | --- | --- |
|Normal (padrão) |0x8 (fator de escala de 8) |Defina a janela de recepção TCP para aumentar para acomodar quase todos os cenários. |
|Desabilitada |Nenhum fator de escala disponível |Defina a janela de recepção TCP com seu valor padrão. |
|Restricted (Restrito) |0x4 (fator de escala de 4) |Defina a janela de recepção TCP para aumentar além do valor padrão, mas limite esse crescimento em alguns cenários. |
|Altamente restrito |0x2 (fator de escala de 2) |Defina a janela de recepção TCP para aumentar além do valor padrão, mas faça isso de forma muito conservadora. |
|Experimental |0xE (fator de escala de 14) |Defina a janela de recepção TCP para aumentar para acomodar cenários extremos. |

Se você usar um aplicativo para capturar pacotes de rede, o aplicativo deverá relatar dados semelhantes aos seguintes para configurações de nível de ajuste automática de janela diferentes.

- Nível de ajuste automática: **normal (estado padrão)**

   ```
   Frame: Number = 492, Captured Frame Length = 66, MediaType = ETHERNET
   + Ethernet: Etype = Internet IP (IPv4),DestinationAddress:[D8-FE-E3-65-F3-FD],SourceAddress:[C8-5B-76-7D-FA-7F]
   + Ipv4: Src = 192.169.0.5, Dest = 192.169.0.4, Next Protocol = TCP, Packet ID = 2667, Total IP Length = 52
   - Tcp: [Bad CheckSum]Flags=......S., SrcPort=60975, DstPort=Microsoft-DS(445), PayloadLen=0, Seq=4075590425, Ack=0, Win=64240 ( Negotiating scale factor 0x8 ) = 64240
   SrcPort: 60975
   DstPort: Microsoft-DS(445)
   SequenceNumber: 4075590425 (0xF2EC9319)
   AcknowledgementNumber: 0 (0x0)
   + DataOffset: 128 (0x80)
   + Flags: ......S. ---------------------------------------------------------> SYN Flag set
   Window: 64240 ( Negotiating scale factor 0x8 ) = 64240 ---------> TCP Receive Window set as 64K as per NIC Link bitrate. Note it shows the 0x8 Scale Factor.
   Checksum: 0x8182, Bad
   UrgentPointer: 0 (0x0)
   - TCPOptions:
   + MaxSegmentSize: 1
   + NoOption:
   + WindowsScaleFactor: ShiftCount: 8 -----------------------------> Scale factor, defined by AutoTuningLevel
   + NoOption:
   + NoOption:
   + SACKPermitted:
   ```

- Nível de ajuste automática: **desabilitado**

   ```
   Frame: Number = 353, Captured Frame Length = 62, MediaType = ETHERNET
   + Ethernet: Etype = Internet IP (IPv4),DestinationAddress:[D8-FE-E3-65-F3-FD],SourceAddress:[C8-5B-76-7D-FA-7F]
   + Ipv4: Src = 192.169.0.5, Dest = 192.169.0.4, Next Protocol = TCP, Packet ID = 2576, Total IP Length = 48
   - Tcp: [Bad CheckSum]Flags=......S., SrcPort=60956, DstPort=Microsoft-DS(445), PayloadLen=0, Seq=2315885330, Ack=0, Win=64240 ( ) = 64240
   SrcPort: 60956
   DstPort: Microsoft-DS(445)
   SequenceNumber: 2315885330 (0x8A099B12)
   AcknowledgementNumber: 0 (0x0)
   + DataOffset: 112 (0x70)
   + Flags: ......S. ---------------------------------------------------------> SYN Flag set
   Window: 64240 ( ) = 64240 ----------------------------------------> TCP Receive Window set as 64K as per NIC Link bitrate. Note there is no Scale Factor defined. In this case, Scale factor is not being sent as a TCP Option, so it will not be used by Windows.
   Checksum: 0x817E, Bad
   UrgentPointer: 0 (0x0)
   - TCPOptions:
   + MaxSegmentSize: 1
   + NoOption:
   + NoOption:
   + SACKPermitted:
   ```

- Nível de ajuste automática: **restrito**

   ```
   Frame: Number = 3, Captured Frame Length = 66, MediaType = ETHERNET
   + Ethernet: Etype = Internet IP (IPv4),DestinationAddress:[D8-FE-E3-65-F3-FD],SourceAddress:[C8-5B-76-7D-FA-7F]
   + Ipv4: Src = 192.169.0.5, Dest = 192.169.0.4, Next Protocol = TCP, Packet ID = 2319, Total IP Length = 52
   - Tcp: [Bad CheckSum]Flags=......S., SrcPort=60890, DstPort=Microsoft-DS(445), PayloadLen=0, Seq=1966088568, Ack=0, Win=64240 ( Negotiating scale factor 0x4 ) = 64240
   SrcPort: 60890
   DstPort: Microsoft-DS(445)
   SequenceNumber: 1966088568 (0x75302178)
   AcknowledgementNumber: 0 (0x0)
   + DataOffset: 128 (0x80)
   + Flags: ......S. ---------------------------------------------------------> SYN Flag set
   Window: 64240 ( Negotiating scale factor 0x4 ) = 64240 ---------> TCP Receive Window set as 64K as per NIC Link bitrate. Note it shows the 0x4 Scale Factor.
   Checksum: 0x8182, Bad
   UrgentPointer: 0 (0x0)
   - TCPOptions:
   + MaxSegmentSize: 1
   + NoOption:
   + WindowsScaleFactor: ShiftCount: 4 -------------------------------> Scale factor, defined by AutoTuningLevel.
   + NoOption:
   + NoOption:
   + SACKPermitted:
   ```

- Nível de ajuste automática: **altamente restrito**

   ```
   Frame: Number = 115, Captured Frame Length = 66, MediaType = ETHERNET
   + Ethernet: Etype = Internet IP (IPv4),DestinationAddress:[D8-FE-E3-65-F3-FD],SourceAddress:[C8-5B-76-7D-FA-7F]
   + Ipv4: Src = 192.169.0.5, Dest = 192.169.0.4, Next Protocol = TCP, Packet ID = 2388, Total IP Length = 52
   - Tcp: [Bad CheckSum]Flags=......S., SrcPort=60903, DstPort=Microsoft-DS(445), PayloadLen=0, Seq=1463725706, Ack=0, Win=64240 ( Negotiating scale factor 0x2 ) = 64240
   SrcPort: 60903
   DstPort: Microsoft-DS(445)
   SequenceNumber: 1463725706 (0x573EAE8A)
   AcknowledgementNumber: 0 (0x0)
   + DataOffset: 128 (0x80)
   + Flags: ......S. ---------------------------------------------------------> SYN Flag set
   Window: 64240 ( Negotiating scale factor 0x2 ) = 64240 ---------> TCP Receive Window set as 64K as per NIC Link bitrate. Note it shows the 0x2 Scale Factor.
   Checksum: 0x8182, Bad
   UrgentPointer: 0 (0x0)
   - TCPOptions:
   + MaxSegmentSize: 1
   + NoOption:
   + WindowsScaleFactor: ShiftCount: 2 ------------------------------> Scale factor, defined by AutoTuningLevel
   + NoOption:
   + NoOption:
   + SACKPermitted:
   ```

- Nível de ajuste automática: **experimental**

   ```
   Frame: Number = 238, Captured Frame Length = 66, MediaType = ETHERNET
   + Ethernet: Etype = Internet IP (IPv4),DestinationAddress:[D8-FE-E3-65-F3-FD],SourceAddress:[C8-5B-76-7D-FA-7F]
   + Ipv4: Src = 192.169.0.5, Dest = 192.169.0.4, Next Protocol = TCP, Packet ID = 2490, Total IP Length = 52
   - Tcp: [Bad CheckSum]Flags=......S., SrcPort=60933, DstPort=Microsoft-DS(445), PayloadLen=0, Seq=2095111365, Ack=0, Win=64240 ( Negotiating scale factor 0xe ) = 64240
   SrcPort: 60933
   DstPort: Microsoft-DS(445)
   SequenceNumber: 2095111365 (0x7CE0DCC5)
   AcknowledgementNumber: 0 (0x0)
   + DataOffset: 128 (0x80)
   + Flags: ......S. ---------------------------------------------------------> SYN Flag set
   Window: 64240 ( Negotiating scale factor 0xe ) = 64240 ---------> TCP Receive Window set as 64K as per NIC Link bitrate. Note it shows the 0xe Scale Factor.
   Checksum: 0x8182, Bad
   UrgentPointer: 0 (0x0)
   - TCPOptions:
   + MaxSegmentSize: 1
   + NoOption:
   + WindowsScaleFactor: ShiftCount: 14 -----------------------------> Scale factor, defined by AutoTuningLevel
   + NoOption:
   + NoOption:
   + SACKPermitted:
   ```

#### <a name="deprecated-tcp-parameters"></a>Parâmetros TCP preteridos

As seguintes configurações do registro do Windows Server 2003 não são mais suportadas e são ignoradas em versões posteriores.

- **Valor TcpWindowSize**
- **NumTcbTablePartitions**  
- **MaxHashTableSize**  

Todas essas configurações foram localizadas na seguinte subchave do registro:

> **HKEY_LOCAL_MACHINE \System\CurrentControlSet\Services\Tcpip\Parameters**  

###  <a name="bkmk_wfp"></a>Plataforma de filtragem do Windows

O Windows Vista e o Windows Server 2008 introduziram a WFP (Windows Filtering Platform). A WFP fornece APIs para fornecedores de software independentes (ISVs) que não são da Microsoft para criar filtros de processamento de pacotes. Exemplos incluem software de firewall e antivírus.

> [!NOTE]  
> Um filtro WFP mal escrito pode diminuir significativamente o desempenho de rede de um servidor. Para obter mais informações, consulte [portando drivers de processamento de pacotes e aplicativos para o WFP](https://docs.microsoft.com/windows-hardware/drivers/network/porting-packet-processing-drivers-and-apps-to-wfp) no centro de desenvolvimento do Windows.

Para obter links para todos os tópicos deste guia, consulte [ajuste de desempenho do subsistema de rede](net-sub-performance-top.md).
