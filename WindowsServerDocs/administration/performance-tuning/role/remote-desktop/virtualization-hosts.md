---
title: Ajuste de desempenho Área de Trabalho Remota hosts de virtualização
description: Ajuste de desempenho para hosts de virtualização Área de Trabalho Remota
ms.prod: windows-server
ms.technology: performance-tuning-guide
ms.topic: article
ms.author: hammadbu; vladmis; denisgun
author: phstee
ms.date: 10/22/2019
ms.openlocfilehash: 2a0db4d890a01df13c44a9bb7adfbd13bebbdde0
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80851699"
---
# <a name="performance-tuning-remote-desktop-virtualization-hosts"></a>Ajuste de desempenho Área de Trabalho Remota hosts de virtualização

Host de Virtualização de Área de Trabalho Remota (host de virtualização de área de trabalho remota) é um serviço de função que dá suporte a cenários de VDI (Virtual Desktop Infrastructure) e permite que vários usuários executem aplicativos baseados no Windows em máquinas virtuais hospedadas em um servidor que executa o Windows Server e o Hyper-V.

O Windows Server dá suporte a dois tipos de áreas de trabalho virtuais: áreas de trabalho virtuais pessoais e áreas de trabalho virtuais em pool.

## <a name="general-considerations"></a>Considerações gerais

### <a name="storage"></a>Armazenamento

O armazenamento é o afunilamento de desempenho mais provável, e é importante dimensionar o armazenamento para lidar corretamente com a carga de e/s gerada pelas alterações de estado da máquina virtual. Se um piloto ou uma simulação não for viável, uma boa diretriz é provisionar um eixo de disco para quatro máquinas virtuais ativas. Use configurações de disco com bom desempenho de gravação (como RAID 1 + 0).

Quando apropriado, use a eliminação de duplicação de disco e o cache para reduzir a carga de leitura de disco e para permitir que sua solução de armazenamento acelere o desempenho armazenando em cache uma parte significativa da imagem.

### <a name="data-deduplication-and-vdi"></a>Eliminação de duplicação de dados e VDI

Introduzido no Windows Server 2012 R2, a eliminação de duplicação de dados dá suporte à otimização de arquivos abertos. Para usar máquinas virtuais em execução em um volume com eliminação de duplicação, os arquivos de máquina virtual precisam ser armazenados em um host separado do host do Hyper-V. Se o Hyper-V e a eliminação de duplicação estiverem em execução no mesmo computador, os dois recursos irão competir com os recursos do sistema e impactar negativamente o desempenho geral.

O volume também deve ser configurado para usar o tipo de otimização de eliminação de duplicação de "Virtual Desktop Infrastructure (VDI)". Você pode configurar isso usando Gerenciador do Servidor (**serviços de arquivo e armazenamento** -&gt; **volumes** -&gt; **configurações de eliminação de duplicatas**) ou usando o seguinte comando do Windows PowerShell:

``` syntax
Enable-DedupVolume <volume> -UsageType HyperV
```

> [!NOTE]
> A otimização de eliminação de duplicação de dados de arquivos abertos tem suporte apenas para cenários de VDI com o Hyper-V usando o armazenamento remoto via SMB 3,0.

### <a name="memory"></a>Memória

O uso de memória do servidor é orientado por três fatores principais:

- Sobrecarga do sistema operacional

- Sobrecarga de serviço do Hyper-V por máquina virtual

- Memória alocada para cada máquina virtual

Para uma carga de trabalho típica de trabalhador de conhecimento, as máquinas virtuais convidadas que executam o x86 Window 8 ou Windows 8.1 devem receber aproximadamente 512 MB de memória como a linha de base. No entanto, Memória Dinâmica provavelmente aumentará a memória da máquina virtual convidada para cerca de 800 MB, dependendo da carga de trabalho. Para x64, vemos cerca de 800 MB, aumentando para 1024 MB.

Portanto, é importante fornecer memória de servidor suficiente para atender à memória exigida pelo número esperado de máquinas virtuais convidadas, além de permitir uma quantidade suficiente de memória para o servidor.

### <a name="cpu"></a>CPU

Quando você planeja a capacidade do servidor para um servidor de host de virtualização de área de trabalho remota, o número de máquinas virtuais por núcleo físico dependerá da natureza da carga de trabalho. Como ponto de partida, é razoável planejar 12 máquinas virtuais por núcleo físico e, em seguida, executar os cenários apropriados para validar o desempenho e a densidade. A maior densidade pode ser alcançada dependendo das especificidades da carga de trabalho.

É recomendável habilitar o Hyper-Threading, mas não se esqueça de calcular a taxa de assinatura em excesso com base no número de núcleos físicos e não no número de processadores lógicos. Isso garante o nível de desempenho esperado por CPU.

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

O clustering de failover no Windows Server 2012 e superior fornece cache em CSV (volumes compartilhados do cluster). Isso é extremamente benéfico para coleções de área de trabalho virtual em pool em que a maioria das e/SS de leitura são provenientes do sistema operacional de gerenciamento. O cache CSV fornece um desempenho mais alto por várias ordens de magnitude, pois ele armazena em cache os blocos que são lidos mais de uma vez e os entrega da memória do sistema, o que reduz a e/s. Para obter mais informações sobre o cache CSV, consulte [como habilitar o cache CSV](https://blogs.msdn.com/b/clustering/archive/2012/03/22/10286676.aspx).

### <a name="pooled-virtual-desktops"></a>Áreas de trabalho virtuais em pool

Por padrão, as áreas de trabalho virtuais em pool são revertidas para o estado original após o usuário sair, portanto, quaisquer alterações feitas no sistema operacional Windows desde a última entrada do usuário são abandonadas.

Embora seja possível desabilitar a reversão, ela ainda é uma condição temporária porque normalmente uma coleção de áreas de trabalho virtuais em pool é recriada devido a várias atualizações no modelo de área de trabalho virtual.

Faz sentido desativar os recursos e serviços do Windows que dependem do estado persistente. Além disso, faz sentido desativar os serviços que são basicamente para cenários não empresariais.

Cada serviço específico deve ser avaliado adequadamente antes de qualquer implantação ampla. Veja a seguir algumas coisas iniciais a serem consideradas:

| Service                                      | Por quê?                                                                                                                                                                                                      |
|----------------------------------------------|-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Atualização automática                                  | As áreas de trabalho virtuais em pool são atualizadas recriando o modelo de área de trabalho virtual.                                                                                                                          |
| Arquivos Offline                                | As áreas de trabalho virtuais estão sempre online e conectadas de um ponto de vista de rede.                                                                                                                         |
| Desfragmentação em segundo plano                            | As alterações do sistema de arquivos são descartadas depois que um usuário é desligado (devido a uma reversão para o estado original ou recriação do modelo de área de trabalho virtual, o que resulta na recriação de todas as áreas de trabalho virtuais em pool). |
| Hibernação ou suspensão                           | Não há tal conceito para VDI                                                                                                                                                                                   |
| Despejo de memória de verificação de bug                        | Não há tal conceito para áreas de trabalho virtuais em pool. Um bug-verificar a área de trabalho virtual em pool será iniciada no estado original.                                                                                       |
| Configuração automática de WLAN                              | Não há nenhuma interface de dispositivo WiFi para VDI                                                                                                                                                                 |
| Serviço de compartilhamento de rede do Windows Media Player | Serviço centrado no consumidor                                                                                                                                                                                  |
| Provedor de grupo base                          | Serviço centrado no consumidor                                                                                                                                                                                  |
| Compartilhamento de conexão com a Internet                  | Serviço centrado no consumidor                                                                                                                                                                                  |
| Serviços estendidos do Media Center               | Serviço centrado no consumidor                                                                                                                                                                                  |
> [!NOTE]
> Esta lista não deve ser uma lista completa, pois as alterações afetarão as metas e os cenários pretendidos. Para obter mais informações, consulte [Hot off the Presss, obter agora, o script de otimização do VDI do Windows 8, cortesia de PFE!](https://blogs.technet.com/b/jeff_stokes/archive/2013/04/09/hot-off-the-presses-get-it-now-the-windows-8-vdi-optimization-script-courtesy-of-pfe.aspx).


> [!NOTE]
> O SuperFetch no Windows 8 está habilitado por padrão. Ele reconhece VDI e não deve ser desabilitado. O SuperFetch pode reduzir ainda mais o consumo de memória por meio do compartilhamento de página de memória, o que é benéfico para VDI. Áreas de trabalho virtuais em pool que executam o Windows 7, o SuperFetch deve ser desabilitado, mas para desktops virtuais pessoais que executam o Windows 7, deve ser deixado em diante.
