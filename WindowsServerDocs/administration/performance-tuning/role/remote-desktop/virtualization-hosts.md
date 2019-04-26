---
title: Hosts de virtualização de área de trabalho remota de ajuste de desempenho
description: Ajuste de desempenho para Hosts de virtualização de área de trabalho remota
ms.prod: windows-server-threshold
ms.technology: performance-tuning-guide
ms.topic: article
ms.author: HammadBu; VladmiS
author: phstee
ms.date: 10/16/2017
ms.openlocfilehash: 7ef1eee97e40b9d3d131cef01722a0d42660b609
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59838057"
---
# <a name="performance-tuning-remote-desktop-virtualization-hosts"></a>Hosts de virtualização de área de trabalho remota de ajuste de desempenho


Host de virtualização de área de trabalho remota (Host de virtualização de área de trabalho remota) é um serviço de função que dá suporte a cenários de Virtual Desktop Infrastructure (VDI) e permite que vários usuários simultâneos execute aplicativos baseados em Windows em máquinas virtuais que são hospedadas em um servidor executando Windows Server 2016 e do Hyper-V.

Windows Server 2016 dá suporte a dois tipos de áreas de trabalho virtuais, áreas de trabalho virtuais pessoais e áreas de trabalho virtuais.

**Neste tópico:**

-   [Considerações gerais](#general)

-   [Otimizações de desempenho](#perfopt)

## <a href="" id="general"></a>Considerações gerais


### <a name="storage"></a>Armazenamento

O armazenamento é o gargalo de desempenho mais provável, e é importante dimensionar o armazenamento para lidar adequadamente com a carga de e/s gerada por alterações de estado da máquina virtual. Se um piloto ou uma simulação não for viável, uma boa diretriz é provisionar um eixo de disco para quatro máquinas virtuais ativas. Use as configurações de disco com bom desempenho de gravação (como RAID 1 + 0).

Quando apropriado, use eliminação de duplicação de disco e armazenamento em cache para reduzir o carga de leitura de disco e habilitar sua solução de armazenamento acelerar o desempenho armazenando em cache uma parte significativa da imagem.

### <a name="data-deduplication-and-vdi"></a>VDI e eliminação de duplicação de dados

Introduzido no Windows Server 2012 R2, a eliminação de duplicação dá suporte à otimização de arquivos abertos. Para usar máquinas virtuais em execução em um volume com eliminação de duplicação, os arquivos de máquina virtual precisam ser armazenado em um host separado do host do Hyper-V. Se estiver executando o Hyper-V e a eliminação de duplicação no mesmo computador, os dois recursos serão disputam os recursos do sistema e afetar negativamente o desempenho geral.

O volume também deve ser configurado para usar o "Virtual Desktop Infrastructure (VDI)? tipo de otimização da eliminação de duplicação. Você pode configurar isso usando o Gerenciador do servidor (**serviços de arquivo e armazenamento**  - &gt; **Volumes**  - &gt; **deconfiguraçõesdeeliminaçãodeduplicação**) ou, usando o Windows PowerShell a seguir de comando:

``` syntax
Enable-DedupVolume <volume> -UsageType HyperV
```

**Observação**    otimização de eliminação de duplicação de dados de arquivos abertos tem suporte somente para cenários VDI com Hyper-V usando armazenamento remoto em SMB 3.0.


### <a name="memory"></a>Memória

Uso de memória do servidor é orientado por três fatores principais:

-   Sobrecarga do sistema operacional

-   Serviço do Hyper-V sobrecarga por máquina virtual

-   Memória alocada para cada máquina virtual

Uma carga de trabalho de trabalhador de Conhecimento típico, o convidado virtual máquinas x86 executando Windows 8 ou Windows 8.1 deve ser dada ~ 512 MB de memória como a linha de base. No entanto, a memória dinâmica provavelmente aumentará a memória da máquina virtual convidada para aproximadamente 800 MB, dependendo da carga de trabalho. Para x64, podemos ver sobre 800 MB iniciando, aumentando a 1024 MB.

Portanto, é importante fornecer memória suficiente server para satisfazer a memória necessária para o número esperado de máquinas virtuais convidadas, além de permitir uma quantidade suficiente de memória para o servidor.

### <a name="cpu"></a>CPU

Quando você planejar a capacidade do servidor para um servidor de Host de virtualização de área de trabalho remota, o número de máquinas virtuais por núcleo físico dependerá da natureza da carga de trabalho. Como ponto de partida, é razoável planejar 12 máquinas virtuais por núcleo físico e, em seguida, execute os cenários apropriados para validar o desempenho e a densidade. Maior densidade pode ser alcançada dependendo das especificações da carga de trabalho.

É recomendável habilitar o hyper-threading, mas não se esqueça de calcular a taxa de excesso de assinatura com base no número de núcleos físicos e não o número de processadores lógicos. Isso garante que o nível esperado de desempenho em uma base por CPU.

### <a name="virtual-gpu"></a>Virtual GPU

Microsoft RemoteFX para Host de virtualização de área de trabalho remota oferece uma experiência de gráficos sofisticados para codificar Virtual Desktop Infrastructure (VDI) por meio de comunicação remota do lado do host, um pipeline de renderização-captura-codificação, altamente eficiente baseada em GPU, a limitação com base no cliente atividade e uma GPU virtual habilitada para DirectX. RemoteFX para o Host de virtualização de área de trabalho remota atualiza a GPU virtual de DirectX9 para DirectX11. Ele também melhora a experiência do usuário, oferecendo suporte a mais monitores em resoluções mais altas.

A experiência do RemoteFX DirectX11 está disponível sem um hardware GPU, por meio de um driver emuladas pelo software. Embora esse software GPU fornece uma boa experiência, a unidade de processamento de gráficos virtual do RemoteFX (VGPU) adiciona uma experiência de aceleradas por hardware para áreas de trabalho virtuais.

Para tirar proveito da experiência do VGPU do RemoteFX em um servidor executando o Windows Server 2016, você precisa de um driver da GPU (como DirectX11.1 ou WDDM 1.2) no servidor host. Para obter mais informações sobre as ofertas de GPU a ser usado com o RemoteFX para o Host de virtualização de área de trabalho remota, entre em contato com seu provedor GPU.

Se você usar a GPU virtual do RemoteFX em sua implantação de VDI, a capacidade de implantação variará com base em cenários de uso e configuração de hardware. Ao planejar sua implantação, considere o seguinte:

-   Número de GPUs no seu sistema

-   Capacidade de memória de vídeo sobre as GPUs

-   Recursos de processador e de hardware em seu sistema

### <a name="remotefx-server-system-memory"></a>Memória do sistema de servidor RemoteFX

Para cada área de trabalho virtual habilitada com uma GPU virtual, o RemoteFX usa memória do sistema no sistema operacional convidado e no servidor habilitadas para RemoteFX. O hipervisor garante a disponibilidade de memória do sistema para um sistema operacional convidado. No servidor, cada virtual habilitada para GPU de área de trabalho virtual precisa anunciar seu requisito de memória do sistema para o hipervisor. Quando habilitadas para GPU virtual área de trabalho virtual é iniciado, o hipervisor reserva memória adicional do sistema no servidor para o desktop virtual habilitado para VGPU habilitadas para RemoteFX.

O requisito de memória para o servidor habilitadas para RemoteFX é dinâmico porque a quantidade de memória consumida no servidor habilitadas para RemoteFX é dependente do número de monitores que estão associados com habilitado para VGPU áreas de trabalho virtuais e a resolução máxima para esses monitores.

### <a name="remotefx-server-gpu-video-memory"></a>Servidor RemoteFX memória de vídeo de GPU

Cada virtual habilitada para GPU de área de trabalho virtual usa a memória de vídeo no hardware da GPU no servidor de host para renderizar a área de trabalho. Além de renderização, a memória de vídeo é usada por um codec para compactar a tela renderizada. A quantidade de memória necessária é diretamente com base na quantidade de monitores que são provisionados para a máquina virtual.

A memória de vídeo é reservada varia de acordo com o número de monitores e a resolução de tela do sistema. Alguns usuários podem exigir uma tela com resolução superior para tarefas específicas. Há maior escalabilidade com as configurações de resolução mais baixos se todas as outras configurações permanecem constantes.

### <a name="remotefx-processor"></a>Processador do RemoteFX

O hipervisor agenda habilitadas para GPU virtuais áreas de trabalho virtuais na CPU e de servidor habilitadas para RemoteFX. Ao contrário de memória do sistema, não existe informações relacionadas aos recursos adicionais que precisa do RemoteFX para compartilhar com o hipervisor. A adicional sobrecarga da CPU que traz os RemoteFX para virtual habilitada para GPU de área de trabalho virtual relacionada à execução do driver GPU virtual e uma pilha de protocolo de área de trabalho remota do modo de usuário.

No servidor habilitadas para RemoteFX, a sobrecarga é aumentada, pois o sistema é executado em um processo adicional (rdvgm.exe) por virtual habilitada para GPU de área de trabalho virtual. Esse processo usa o driver de dispositivo de gráficos para executar comandos na GPU. O codec também usa as CPUs para compactar os dados da tela que precisa ser enviada de volta ao cliente.

Mais processadores virtuais significam uma melhor experiência do usuário. É recomendável alocar pelo menos duas CPUs virtuais por virtual habilitada para GPU de área de trabalho virtual. Também recomendamos o uso de x64 arquitetura áreas de trabalho virtuais habilitadas para GPU virtual porque o desempenho em máquinas virtuais é melhor em comparação com x86 de x64 máquinas virtuais.

### <a name="remotefx-gpu-processing-power"></a>Capacidade de processamento da GPU RemoteFX

Para cada virtual habilitada para GPU de área de trabalho virtual, há um processo correspondente do DirectX em execução no servidor habilitadas para RemoteFX. Esse processo repete todos os comandos de elementos gráficos que ele recebe do RemoteFX área de trabalho virtual em física GPU. Para a GPU física, é equivalente a executar simultaneamente vários aplicativos de DirectX.

Normalmente, os drivers e dispositivos de gráficos são ajustados para executar alguns aplicativos na área de trabalho. RemoteFX alonga as GPUs a ser usado de maneira exclusiva. Para medir o desempenho do GPU em um servidor RemoteFX, contadores de desempenho foram adicionados para medir a resposta GPU para solicitações do RemoteFX.

Normalmente, quando um recurso GPU é pouco recursos, leia e operações de gravação das leva GPU muito tempo para ser concluída. Usando contadores de desempenho, os administradores podem assumir medidas preventivas, eliminando a possibilidade de qualquer tempo de inatividade para seus usuários finais.

Os seguintes contadores de desempenho estão disponíveis no servidor RemoteFX para medir o desempenho de GPU virtual:

**Elementos gráficos do RemoteFX**

-   **Quadros ignorados/segundo - recursos insuficientes do cliente** número de quadros ignorados por segundo devido a recursos insuficientes do cliente

-   **Taxa de compactação de elementos gráficos** proporção do número de bytes codificado para o número de bytes de entrada

**Raiz do RemoteFX gerenciamento de GPU**

-   **Recursos: TDRs em GPUs Server** Número Total de vezes que a TDR expira a GPU no servidor

-   **Recursos: Máquinas virtuais em execução RemoteFX** Número Total de máquinas virtuais que têm o adaptador de vídeo 3D RemoteFX instalado

-   **VRAM: MB disponível por GPU** quantidade de memória de vídeo dedicada que não está sendo usada

-   **VRAM: % Reservada por GPU** porcentagem de memória de vídeo dedicada que foi reservada para o RemoteFX

**Software do RemoteFX**

-   **Taxa de captura para o monitor** \[1-4\] exibe a taxa de captura do RemoteFX para monitores de 1 a 4

-   **Taxa de compactação** preteridos no Windows 8 e substituída por **taxa de compactação de gráficos**

-   **Quadros por segundo de atraso** número de quadros por segundo em que os dados de gráficos não foi enviados dentro de um determinado período de tempo

-   **Tempo de resposta GPU de captura** latência medido de captura do RemoteFX (em microssegundos) para a conclusão das operações de GPU

-   **Tempo de resposta GPU de renderização** latência medida dentro de renderização do RemoteFX (em microssegundos) para a conclusão das operações de GPU

-   **Bytes de saída** bytes de saída do número Total de RemoteFX

-   **Aguardando a contagem de cliente/s** preteridos no Windows 8 e substituída por **quadros ignorados/segundo - recursos insuficientes do cliente**

**Gerenciamento de vGPU do RemoteFX**

-   **Recursos: TDRs locais às máquinas virtuais** Número Total de TDRs que ocorreram nessa máquina virtual (TDRs servidor propagado para as máquinas virtuais não estão incluídos)

-   **Recursos: TDRs propagados pelo servidor** Número Total de TDRs que ocorreu no servidor e que foram propagadas para a máquina virtual

**Desempenho de vGPU do RemoteFX máquina virtual**

-   **Dados: Invocado apresenta/s** Número Total (em segundos) de operações presentes a ser renderizado para a área de trabalho da máquina virtual por segundo

-   **Dados: Apresenta/s de saída** Número Total de operações presentes enviados pela máquina virtual para o servidor de GPU por segundo

-   **Dados: Bytes lidos/s** Número Total de bytes lidos do servidor habilitadas para RemoteFX por segundo

-   **Dados: Bytes/s de envio** Número Total de bytes enviados para o servidor habilitadas para RemoteFX GPU por segundo

-   **DMA: Latência (s) de média de buffers de comunicação** quantidade média de tempo (em segundos) gasto nos buffers de comunicação

-   **DMA: Latência de buffer DMA (s)** período de tempo (em segundos) entre quando o DMA é enviado até concluída

-   **DMA: Comprimento da fila** comprimento da fila de DMA para um adaptador de vídeo 3D RemoteFX

-   **Recursos: Tempos limite de TDR por GPU** tempos limite de contagem de TDR que ocorreu por GPU na máquina virtual

-   **Recursos: Tempos limite de TDR por mecanismo de GPU** do mecanismo de tempos limite de contagem de TDR que ocorreu por GPU na máquina virtual

Além dos RemoteFX virtual GPU contadores de desempenho também é possível medir a utilização de GPU usando o Process Explorer, que mostra o uso de memória de vídeo e a utilização de GPU.

## <a href="" id="perfopt"></a>Otimizações de desempenho


### <a name="dynamic-memory"></a>Memória Dinâmica

Memória dinâmica com mais eficiência permite a utilização dos recursos de memória do servidor que executa o Hyper-V, equilibrando como a memória é distribuída entre máquinas virtuais em execução. Memória pode ser dinamicamente realocada entre máquinas virtuais em resposta às suas cargas de trabalho de alteração.

Memória dinâmica permite aumentar a densidade de máquina virtual com os recursos que você já tiver sem sacrificar o desempenho e escalabilidade. O resultado é um uso mais eficiente de recursos de hardware de servidor caros, que podem ser traduzidos em gerenciamento mais fácil e reduzir os custos.

Em sistemas operacionais convidados executando o Windows 8 e acima com processadores virtuais que se estendem por vários processadores lógicos, considere a compensação entre executando com memória dinâmica para ajudar a minimizar o uso de memória e desabilitar a memória dinâmica para melhorar o desempenho de um aplicativo que está ciente de topologia computadores. Esse aplicativo pode aproveitar as informações de topologia para tornar o agendamento e a memória decisões de alocação.

### <a name="tiered-storage"></a>Armazenamento em camadas

Host de virtualização de área de trabalho remota oferece suporte a armazenamento em camadas para pools de área de trabalho virtual. O computador físico que é compartilhado por todas as áreas de trabalho virtuais dentro de uma coleção pode usar uma solução de armazenamento de alto desempenho, de tamanho pequeno, como uma unidade de estado sólida (SSD) espelhada. As áreas de trabalho virtuais podem ser colocados em armazenamento mais barato, tradicional como RAID 1 + 0.

O computador físico deve ser colocado em um SSD é porque a maioria do leitura-eu/sistema operacional de áreas de trabalho virtuais vai para o sistema operacional de gerenciamento. Portanto, o armazenamento que é usado pelo computador físico deve manter muito mais altos de leitura e/Ss por segundo.

Essa configuração de implantação garante desempenho econômico, em que o desempenho é necessário. O SSD proporciona um desempenho superior em um disco de tamanho menor (aproximadamente 20 GB por coleção, dependendo da configuração). Armazenamento tradicional para áreas de trabalho virtuais (RAID 1 + 0) usa cerca de 3 GB por máquina virtual.

### <a name="csv-cache"></a>Cache do CSV

Clustering de failover no Windows Server 2012 e superior fornece o armazenamento em cache no Cluster Shared Volumes (CSV). Isso é extremamente benéfico para coleções de área de trabalho virtuais em pool em que a maioria da leitura e/SS provenientes de sistema operacional de gerenciamento. O cache do CSV fornece um melhor desempenho em várias ordens de magnitude porque ele armazena em cache os blocos que são lidas por mais de uma vez e os entrega da memória do sistema, o que reduz a e/s. Para obter mais informações sobre o cache do CSV, consulte [como habilitar o Cache de CSV](http://blogs.msdn.com/b/clustering/archive/2012/03/22/10286676.aspx).

### <a name="pooled-virtual-desktops"></a>Áreas de trabalho virtuais

Por padrão, áreas de trabalho virtuais serão revertidas para o estado original depois que um usuário sai, portanto, todas as alterações feitas ao sistema operacional Windows, desde a última entrada do usuário são abandonados.

Embora seja possível desabilitar a reversão, ainda é uma condição temporária porque normalmente uma coleção de área de trabalho virtuais em pool é recriada devido a várias atualizações para o modelo de área de trabalho virtual.

Faz sentido para desativar recursos do Windows e serviços que dependem do estado persistente. Além disso, faz sentido para desativar os serviços que são principalmente para cenários não empresariais.

Cada serviço específico deve ser avaliado adequadamente antes de qualquer implantação ampla. Estes são alguns pontos iniciais a serem considerados:

| Fornecer manutenção                                      | Por quê?                                                                                                                                                                                                      |
|----------------------------------------------|-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Atualização automática                                  | Áreas de trabalho virtuais são atualizadas, recriando o modelo de área de trabalho virtual.                                                                                                                          |
| Arquivos offline                                | Áreas de trabalho virtuais estão sempre online e conectado de um ponto de vista rede.                                                                                                                         |
| Desfragmentação de plano de fundo                            | Alterações no sistema de arquivos são descartadas depois que um usuário faz (devido a uma reversão para o estado original ou para recriar o modelo de área de trabalho virtual, o que resulta na recriação de todas as áreas de trabalho virtuais). |
| Modo de hibernação ou suspensão                           | Sem esse conceito para VDI                                                                                                                                                                                   |
| Despejo de memória de verificação de bug                        | Sem esse conceito para áreas de trabalho virtuais. Um desktop virtual em pool de verificação de bug será desde o estado original.                                                                                       |
| Configuração automática de WLAN                              | Não há nenhuma interface de dispositivo de Wi-Fi para VDI                                                                                                                                                                 |
| Serviço de compartilhamento de rede do Windows Media Player | Centralizado no serviço do consumidor                                                                                                                                                                                  |
| Provedor do grupo doméstico                          | Centralizado no serviço do consumidor                                                                                                                                                                                  |
| Compartilhamento de conexão com a Internet                  | Centralizado no serviço do consumidor                                                                                                                                                                                  |
| Media Center serviços estendidos               | Centralizado no serviço do consumidor                                                                                                                                                                                  |

 

**Observação**    essa lista não deve ser uma lista completa, pois qualquer alteração afetará as metas pretendidas e os cenários. Para obter mais informações, consulte [quente desativar o pressiona obtê-lo agora, o script de otimização do Windows 8 VDI, cortesia do PFE!](http://blogs.technet.com/b/jeff_stokes/archive/2013/04/09/hot-off-the-presses-get-it-now-the-windows-8-vdi-optimization-script-courtesy-of-pfe.aspx).

 

**Observação**    SuperFetch no Windows 8 é habilitado por padrão. Ele está ciente de VDI e não deve ser desabilitado. O superFetch pode reduzir ainda mais o consumo de memória por meio do compartilhamento de página de memória, que é muito bom para VDI. Áreas de trabalho virtuais que executam o Windows 7, o SuperFetch deve ser desabilitado, mas para áreas de trabalho virtuais que executam o Windows 7, ele deve ser deixado no.

 
