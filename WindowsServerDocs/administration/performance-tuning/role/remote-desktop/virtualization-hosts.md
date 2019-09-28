---
title: Ajuste de desempenho Área de Trabalho Remota hosts de virtualização
description: Ajuste de desempenho para hosts de virtualização Área de Trabalho Remota
ms.prod: windows-server
ms.technology: performance-tuning-guide
ms.topic: article
ms.author: HammadBu; VladmiS
author: phstee
ms.date: 10/16/2017
ms.openlocfilehash: 6aad1560fa9f9429af94426487d9a33369137ded
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71370034"
---
# <a name="performance-tuning-remote-desktop-virtualization-hosts"></a>Ajuste de desempenho Área de Trabalho Remota hosts de virtualização


Host de Virtualização de Área de Trabalho Remota (host de Virtualização RD) é um serviço de função que dá suporte a cenários de VDI (Virtual Desktop Infrastructure) e permite que vários usuários simultâneos executem aplicativos baseados no Windows em máquinas virtuais hospedadas em um servidor que executa o Windows Server 2016 e Hyper-V.

O Windows Server 2016 dá suporte a dois tipos de áreas de trabalho virtuais, áreas de trabalho virtuais pessoais e áreas de trabalho virtuais em pool.

**Neste tópico:**

-   [Considerações gerais](#general-considerations)

-   [Otimizações de desempenho](#performance-optimizations)

## <a name="general-considerations"></a>Considerações gerais


### <a name="storage"></a>Armazenamento

O armazenamento é o afunilamento de desempenho mais provável, e é importante dimensionar o armazenamento para lidar corretamente com a carga de e/s gerada pelas alterações de estado da máquina virtual. Se um piloto ou uma simulação não for viável, uma boa diretriz é provisionar um eixo de disco para quatro máquinas virtuais ativas. Use configurações de disco com bom desempenho de gravação (como RAID 1 + 0).

Quando apropriado, use a eliminação de duplicação de disco e o cache para reduzir a carga de leitura de disco e para permitir que sua solução de armazenamento acelere o desempenho armazenando em cache uma parte significativa da imagem.

### <a name="data-deduplication-and-vdi"></a>Eliminação de duplicação de dados e VDI

Introduzido no Windows Server 2012 R2, a eliminação de duplicação de dados dá suporte à otimização de arquivos abertos. Para usar máquinas virtuais em execução em um volume com eliminação de duplicação, os arquivos de máquina virtual precisam ser armazenados em um host separado do host do Hyper-V. Se o Hyper-V e a eliminação de duplicação estiverem em execução no mesmo computador, os dois recursos irão competir com os recursos do sistema e impactar negativamente o desempenho geral.

O volume também deve ser configurado para usar o tipo de otimização de eliminação de duplicação de "Virtual Desktop Infrastructure (VDI)". Você pode configurar isso usando Gerenciador do servidor ( **configurações de eliminação de duplicatas**de **volumes**  -  - &gt; &gt; **de serviços de arquivo e armazenamento** ) ou usando o seguinte comando do Windows PowerShell:

``` syntax
Enable-DedupVolume <volume> -UsageType HyperV
```

> [!NOTE]
> A otimização de eliminação de duplicação de dados de arquivos abertos tem suporte apenas para cenários de VDI com o Hyper-V usando o armazenamento remoto via SMB 3,0.

### <a name="memory"></a>Memória

O uso de memória do servidor é orientado por três fatores principais:

-   Sobrecarga do sistema operacional

-   Sobrecarga de serviço do Hyper-V por máquina virtual

-   Memória alocada para cada máquina virtual

Para uma carga de trabalho típica de trabalhador de conhecimento, as máquinas virtuais convidadas que executam o x86 Window 8 ou Windows 8.1 devem receber aproximadamente 512 MB de memória como a linha de base. No entanto, Memória Dinâmica provavelmente aumentará a memória da máquina virtual convidada para cerca de 800 MB, dependendo da carga de trabalho. Para x64, vemos cerca de 800 MB, aumentando para 1024 MB.

Portanto, é importante fornecer memória de servidor suficiente para atender à memória exigida pelo número esperado de máquinas virtuais convidadas, além de permitir uma quantidade suficiente de memória para o servidor.

### <a name="cpu"></a>CPU

Quando você planeja a capacidade do servidor para um servidor de host de virtualização de área de trabalho remota, o número de máquinas virtuais por núcleo físico dependerá da natureza da carga de trabalho. Como ponto de partida, é razoável planejar 12 máquinas virtuais por núcleo físico e, em seguida, executar os cenários apropriados para validar o desempenho e a densidade. A maior densidade pode ser alcançada dependendo das especificidades da carga de trabalho.

É recomendável habilitar o Hyper-Threading, mas não se esqueça de calcular a taxa de assinatura em excesso com base no número de núcleos físicos e não no número de processadores lógicos. Isso garante o nível de desempenho esperado por CPU.

### <a name="virtual-gpu"></a>GPU virtual

O Microsoft RemoteFX para o host de virtualização de área de trabalho remota oferece uma rica experiência gráfica para VDI (Virtual Desktop Infrastructure) por meio de comunicação remota do lado do host, um pipeline de processamento de captura e alta eficiência, com base no cliente atividade e uma GPU virtual habilitada para DirectX. RemoteFX para Host de Virtualização de Área de Trabalho Remota atualiza a GPU virtual de DirectX9 para DirectX11. Ele também melhora a experiência do usuário ao dar suporte a mais monitores em resoluções mais altas.

A experiência DirectX11 do RemoteFX está disponível sem uma GPU de hardware, por meio de um driver emulado por software. Embora essa GPU de software ofereça uma boa experiência, a VGPU (unidade de processamento gráfico virtual) do RemoteFX adiciona uma experiência acelerada de hardware a áreas de trabalho virtuais.

Para aproveitar a experiência de VGPU do RemoteFX em um servidor que executa o Windows Server 2016, você precisa de um driver de GPU (como DirectX 11.1 ou WDDM 1,2) no servidor host. Para obter mais informações sobre as ofertas de GPU a serem usadas com o RemoteFX para Host de Virtualização de Área de Trabalho Remota, entre em contato com seu provedor de GPU.

Se você usar a GPU virtual do RemoteFX em sua implantação do VDI, a capacidade de implantação variará com base nos cenários de uso e na configuração de hardware. Ao planejar sua implantação, considere o seguinte:

-   Número de GPUs em seu sistema

-   Capacidade de memória de vídeo nas GPUs

-   Recursos de processador e hardware no seu sistema

### <a name="remotefx-server-system-memory"></a>Memória do sistema do servidor RemoteFX

Para cada área de trabalho virtual habilitada com uma GPU virtual, o RemoteFX usa a memória do sistema no sistema operacional convidado e no servidor habilitado para RemoteFX. O hipervisor garante a disponibilidade da memória do sistema para um sistema operacional convidado. No servidor, cada área de trabalho virtual habilitada para GPU virtual precisa anunciar seu requisito de memória do sistema para o hipervisor. Quando a área de trabalho virtual habilitada para a GPU virtual está iniciando, o hipervisor reserva memória do sistema adicional no servidor habilitado para RemoteFX para a área de trabalho virtual habilitada para VGPU.

O requisito de memória para o servidor habilitado para RemoteFX é dinâmico porque a quantidade de memória consumida no servidor habilitado para RemoteFX depende do número de monitores associados às áreas de trabalho virtuais habilitadas para VGPU e à resolução máxima de esses monitores.

### <a name="remotefx-server-gpu-video-memory"></a>Memória de vídeo da GPU do servidor RemoteFX

Cada área de trabalho virtual habilitada para GPU virtual usa a memória de vídeo no hardware de GPU no servidor de host para renderizar a área de trabalho. Além da renderização, a memória de vídeo é usada por um codec para compactar a tela renderizada. A quantidade de memória necessária é diretamente baseada na quantidade de monitores provisionados para a máquina virtual.

A memória de vídeo reservada varia de acordo com o número de monitores e a resolução da tela do sistema. Alguns usuários podem exigir uma resolução de tela mais alta para tarefas específicas. Haverá maior escalabilidade com configurações de resolução mais baixa se todas as outras configurações permanecerem constantes.

### <a name="remotefx-processor"></a>Processador RemoteFX

O hipervisor agenda o servidor habilitado para RemoteFX e as áreas de trabalho virtuais habilitadas para a GPU virtual na CPU. Diferentemente da memória do sistema, não há informações relacionadas a recursos adicionais que o RemoteFX precisa compartilhar com o hipervisor. A sobrecarga de CPU adicional que o RemoteFX traz para a área de trabalho virtual habilitada para a GPU virtual está relacionada à execução do driver de GPU virtual e a uma pilha de protocolo RDP de modo de usuário.

No servidor habilitado para RemoteFX, a sobrecarga é aumentada, pois o sistema executa um processo adicional (rdvgm. exe) por área de trabalho virtual habilitada para GPU virtual. Esse processo usa o driver de dispositivo de gráficos para executar comandos na GPU. O codec também usa as CPUs para compactar os dados da tela que precisam ser enviados de volta ao cliente.

Mais processadores virtuais significam uma melhor experiência do usuário. É recomendável alocar pelo menos duas CPUs virtuais por área de trabalho virtual habilitada para GPU virtual. Também é recomendável usar a arquitetura x64 para áreas de trabalho virtuais habilitadas para GPU virtual porque o desempenho em máquinas virtuais x64 é melhor em comparação com as máquinas virtuais x86.

### <a name="remotefx-gpu-processing-power"></a>Potência de processamento de GPU do RemoteFX

Para cada área de trabalho virtual habilitada para GPU virtual, há um processo do DirectX correspondente em execução no servidor habilitado para RemoteFX. Esse processo repete todos os comandos gráficos que ele recebe da área de trabalho virtual do RemoteFX para a GPU física. Para a GPU física, é equivalente a executar simultaneamente vários aplicativos do DirectX.

Normalmente, os drivers e dispositivos gráficos são ajustados para executar alguns aplicativos na área de trabalho. O RemoteFX amplia as GPUs a serem usadas de maneira exclusiva. Para medir como a GPU está sendo executada em um servidor RemoteFX, os contadores de desempenho foram adicionados para medir a resposta de GPU às solicitações do RemoteFX.

Geralmente, quando um recurso de GPU está com poucos recursos, as operações de leitura e gravação na GPU levam muito tempo para serem concluídas. Usando contadores de desempenho, os administradores podem tomar ações preventivas, eliminando a possibilidade de qualquer tempo de inatividade para seus usuários finais.

Os seguintes contadores de desempenho estão disponíveis no servidor do RemoteFX para medir o desempenho da GPU virtual:

**Elementos gráficos do RemoteFX**

-   **Quadros ignorados/segundo-recursos insuficientes do cliente** Número de quadros ignorados por segundo devido a recursos de cliente insuficientes

-   **Taxa de compactação de gráficos** Taxa do número de bytes codificados para o número de bytes de entrada

**Gerenciamento de GPU raiz do RemoteFX**

-   **Os TDRs em GPUs** de servidor número total de vezes que o TdR atinge o tempo limite na GPU no servidor

-   **Os Máquinas virtuais executando o** número total de máquinas virtuais do RemoteFX que têm o adaptador de vídeo 3D RemoteFX instalado

-   **VRAM MB disponíveis por valor** de GPU de memória de vídeo dedicada que não está sendo usada

-   **VRAM % Reservada por GPU @ no__t-0% da memória de vídeo dedicada que foi reservada para o RemoteFX

**Software do RemoteFX**

-   **Taxa de captura para o monitor** \[1-4\] exibe a taxa de captura do RemoteFX para monitores 1-4

-   **Taxa de compactação** Preterido no Windows 8 e substituído pela **taxa de compactação de gráficos**

-   **Quadros atrasados/s** Número de quadros por segundo em que os dados gráficos não foram enviados dentro de um determinado período de tempo

-   **Tempo de resposta de GPU da captura** Latência medida na captura do RemoteFX (em microssegundos) para que as operações de GPU sejam concluídas

-   **Tempo de resposta da GPU do processamento** Latência medida dentro da renderização do RemoteFX (em microssegundos) para que as operações de GPU sejam concluídas

-   **Bytes de saída** Número total de bytes de saída do RemoteFX

-   **Aguardando contagem de cliente/s** Preterido no Windows 8 e substituído por **quadros ignorados/segundo-recursos insuficientes do cliente**

**Gerenciamento de vGPU do RemoteFX**

-   **Os TDRs local para máquinas** virtuais número total de TDRs que ocorreram nesta máquina virtual (TDRs que o servidor propagado para as máquinas virtuais não estão incluídos)

-   **Os TDRs propagado pelo** servidor número total de TDRs que ocorreram no servidor e que foram propagadas para a máquina virtual

**Desempenho de vGPU de máquina virtual do RemoteFX**

-   **Dado Número total de presentes** /s invocados (em segundos) de operações presentes a serem processadas para a área de trabalho da máquina virtual por segundo

-   **Dado Número total de presentes de** saída/s de operações atuais enviadas pela máquina virtual para a GPU do servidor por segundo

-   **Dado Bytes de leitura/** s-número total de bytes de leitura do servidor habilitado para RemoteFX por segundo

-   **Dado Bytes de envio/** s-número total de bytes enviados à GPU do servidor habilitada para RemoteFX por segundo

-   **DMA Média de buffers de comunicação (s** ) tempo médio (em segundos) gasto nos buffers de comunicação

-   **DMA Tempo de latência do buffer de** DMA (s) (em segundos) desde quando o DMA é enviado até ser concluído

-   **DMA Comprimento da** fila DMA de comprimento da fila para um adaptador de vídeo 3D RemoteFX

-   **Os Tempos limite de TDR por** contagem de GPU de tempos limite de TDR que ocorreram por GPU na máquina virtual

-   **Os Tempos limite de TDR por contagem** de mecanismo de GPU de tempos limite de TDR que ocorreram por mecanismo de GPU na máquina virtual

Além dos contadores de desempenho de GPU virtual do RemoteFX, você também pode medir a utilização de GPU usando o Process Explorer, que mostra o uso de memória de vídeo e a utilização de GPU.

## <a name="performance-optimizations"></a>Otimizações de desempenho

### <a name="dynamic-memory"></a>Memória Dinâmica

O Memória Dinâmica permite uma utilização mais eficiente dos recursos de memória do servidor que executa o Hyper-V, equilibrando como a memória é distribuída entre as máquinas virtuais em execução. A memória pode ser realocada dinamicamente entre máquinas virtuais em resposta às suas cargas de trabalho em alteração.

Memória Dinâmica permite aumentar a densidade da máquina virtual com os recursos que você já tem sem sacrificar o desempenho ou a escalabilidade. O resultado é um uso mais eficiente de recursos de hardware de servidor caros, que podem ser convertidos em um gerenciamento mais fácil e custos menores.

Em sistemas operacionais convidados que executam o Windows 8 e superior com processadores virtuais que abrangem vários processadores lógicos, considere a compensação entre executar com Memória Dinâmica para ajudar a minimizar o uso de memória e desabilitar o Memória Dinâmica para melhorar o desempenho de um aplicativo que reconhece a topologia de computador. Esse aplicativo pode aproveitar as informações de topologia para tomar decisões de agendamento e alocação de memória.

### <a name="tiered-storage"></a>Armazenamento em camadas

O host de Virtualização RD dá suporte ao armazenamento em camadas para pools de área de trabalho virtual O computador físico compartilhado por todas as áreas de trabalho virtuais em pool em uma coleção pode usar uma solução de armazenamento de alto desempenho, de pequeno porte, como uma SSD (unidade de estado sólido) espelhada. As áreas de trabalho virtuais em pool podem ser colocadas em armazenamento tradicional e mais barato, como RAID 1 + 0.

O computador físico deve ser colocado em um SSD porque a maioria das e/SS de leitura de áreas de trabalho virtuais em pool vai para o sistema operacional de gerenciamento. Portanto, o armazenamento usado pelo computador físico deve sustentar um e/SS de leitura muito maior por segundo.

Essa configuração de implantação garante um desempenho econômico em que o desempenho é necessário. O SSD fornece um desempenho mais alto em um disco de tamanho menor (cerca de 20 GB por coleção, dependendo da configuração). O armazenamento tradicional para áreas de trabalho virtuais em pool (RAID 1 + 0) usa cerca de 3 GB por máquina virtual.

### <a name="csv-cache"></a>Cache CSV

O clustering de failover no Windows Server 2012 e superior fornece cache em CSV (volumes compartilhados do cluster). Isso é extremamente benéfico para coleções de área de trabalho virtual em pool em que a maioria das e/SS de leitura são provenientes do sistema operacional de gerenciamento. O cache CSV fornece um desempenho mais alto por várias ordens de magnitude, pois ele armazena em cache os blocos que são lidos mais de uma vez e os entrega da memória do sistema, o que reduz a e/s. Para obter mais informações sobre o cache CSV, consulte [como habilitar o cache CSV](http://blogs.msdn.com/b/clustering/archive/2012/03/22/10286676.aspx).

### <a name="pooled-virtual-desktops"></a>Áreas de trabalho virtuais em pool

Por padrão, as áreas de trabalho virtuais em pool são revertidas para o estado original após o usuário sair, portanto, quaisquer alterações feitas no sistema operacional Windows desde a última entrada do usuário são abandonadas.

Embora seja possível desabilitar a reversão, ela ainda é uma condição temporária porque normalmente uma coleção de áreas de trabalho virtuais em pool é recriada devido a várias atualizações no modelo de área de trabalho virtual.

Faz sentido desativar os recursos e serviços do Windows que dependem do estado persistente. Além disso, faz sentido desativar os serviços que são basicamente para cenários não empresariais.

Cada serviço específico deve ser avaliado adequadamente antes de qualquer implantação ampla. Veja a seguir algumas coisas iniciais a serem consideradas:

| Serviço                                      | Por quê?                                                                                                                                                                                                      |
|----------------------------------------------|-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Atualização automática                                  | As áreas de trabalho virtuais em pool são atualizadas recriando o modelo de área de trabalho virtual.                                                                                                                          |
| Arquivos offline                                | As áreas de trabalho virtuais estão sempre online e conectadas de um ponto de vista de rede.                                                                                                                         |
| Desfragmentação em segundo plano                            | As alterações do sistema de arquivos são descartadas depois que um usuário é desligado (devido a uma reversão para o estado original ou recriação do modelo de área de trabalho virtual, o que resulta na recriação de todas as áreas de trabalho virtuais em pool). |
| Hibernação ou suspensão                           | Não há tal conceito para VDI                                                                                                                                                                                   |
| Despejo de memória de verificação de bug                        | Não há tal conceito para áreas de trabalho virtuais em pool. Um bug-verificar a área de trabalho virtual em pool será iniciada no estado original.                                                                                       |
| Configuração automática de WLAN                              | Não há nenhuma interface de dispositivo WiFi para VDI                                                                                                                                                                 |
| Serviço de compartilhamento de rede do Windows Media Player | Serviço centrado no consumidor                                                                                                                                                                                  |
| Provedor de grupo base                          | Serviço centrado no consumidor                                                                                                                                                                                  |
| Compartilhamento de conexão com a Internet                  | Serviço centrado no consumidor                                                                                                                                                                                  |
| Serviços estendidos do Media Center               | Serviço centrado no consumidor                                                                                                                                                                                  |
> [!NOTE]
> Esta lista não deve ser uma lista completa, pois as alterações afetarão as metas e os cenários pretendidos. Para obter mais informações, consulte [Hot off the Presss, obter agora, o script de otimização do VDI do Windows 8, cortesia de PFE!](http://blogs.technet.com/b/jeff_stokes/archive/2013/04/09/hot-off-the-presses-get-it-now-the-windows-8-vdi-optimization-script-courtesy-of-pfe.aspx).

 
> [!NOTE]
> O SuperFetch no Windows 8 está habilitado por padrão. Ele reconhece VDI e não deve ser desabilitado. O SuperFetch pode reduzir ainda mais o consumo de memória por meio do compartilhamento de página de memória, o que é benéfico para VDI. Áreas de trabalho virtuais em pool que executam o Windows 7, o SuperFetch deve ser desabilitado, mas para desktops virtuais pessoais que executam o Windows 7, deve ser deixado em diante.

 
