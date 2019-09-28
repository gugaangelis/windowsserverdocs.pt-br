---
title: Considerações sobre o desempenho de hardware
description: Considerações sobre o desempenho de hardware do servidor Windows Server 2016
ms.prod: windows-server
ms.technology: performance-tuning-guide
ms.topic: landing-page
ms.author: phstee
author: phstee
ms.date: 01/08/2018
ms.openlocfilehash: 9c012711dff3746587b4a04b31d9c23ebb7de4cd
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71370547"
---
# <a name="server-hardware-performance-considerations"></a>Considerações sobre o desempenho de hardware

A seção a seguir lista os itens importantes que você deve considerar ao escolher o hardware do servidor. Ao seguir essas diretrizes, é possível eliminar os gargalos de desempenho que poderiam prejudicar o desempenho do servidor.

## <a name="processor-recommendations"></a>Recomendações de processador

Escolha os processadores de 64 bits para os servidores. Os processadores de 64 bits têm um número significativamente maior de espaços de endereço e são obrigatórios para o Windows Server 2016. Não será fornecida nenhuma edição de 32 bits do sistema operacional, mas os aplicativos de 32 bits serão executados no sistema operacional Windows Server 2016 de 64 bits.

Para aumentar os recursos de computação em um servidor, você pode usar um processador com núcleos de maior frequência ou aumentar o número de núcleos de processador. Se a CPU for o recurso limitador no sistema, um núcleo com a frequência de 2x normalmente oferecerá uma melhoria de desempenho maior do que dois núcleos com a frequência de 1x.

Não é esperado que vários núcleos forneçam uma escala linear perfeita. Além disso, o fator de escala poderá ser ainda menor se o Hyper-Threading estiver habilitado, porque o Hyper-Threading depende do compartilhamento de recursos do mesmo núcleo físico.


>[!Important]
> Corresponda e dimensione a memória e o subsistema de E/S com o desempenho da CPU e vice-versa.

Não compare as frequências de CPU entre fabricantes e gerações de processadores porque a comparação pode ser um indicador de velocidade enganoso.

Para o Hyper-V, verifique se o processador dá suporte para SLAT (Conversão de Endereços de Segundo Nível). Ele é implementado como EPT (Tabelas de Página Estendidas) pela Intel e como NPT (Tabelas de Página Aninhadas) pela AMD. Você pode verificar se esse recurso está presente usando o SystemInfo.exe no servidor.

## <a name="cache-recommendations"></a>Recomendações de cache

Escolha caches de processador L2 ou L3 grandes. Nas arquiteturas mais recentes, como Haswell ou Skylake, há um LLC (Cache de Último Nível) ou um L4. Os caches maiores geralmente oferecem um desempenho melhor e têm um papel mais importante do que a frequência bruta da CPU.

## <a name="memory-ram-and-paging-storage-recommendations"></a>Recomendações de memória (RAM) e de armazenamento de paginação

>[!Note] 
> Alguns sistemas podem apresentar um desempenho de armazenamento reduzido ao executar uma nova instalação do Windows Server 2016 em comparação com o Windows Server 2012 R2. Foram feitas várias alterações durante o desenvolvimento do Windows Server 2016 para melhorar a segurança e a confiabilidade da plataforma. Algumas dessas alterações, como a habilitação do Windows Defender por padrão, resultaram em caminhos de E/S mais longos que podem reduzir o desempenho de E/S em cargas de trabalho e padrões específicos. A Microsoft não recomenda que o Windows Defender seja desabilitado porque ele é uma camada importante de proteção para os sistemas. 

Aumente a RAM para corresponder às suas necessidades de memória.
Quando o computador fica com pouca memória e precisa de mais imediatamente, o Windows usa o espaço em disco rígido para complementar a RAM do sistema por meio de um procedimento chamado paginação. Uma quantidade excessiva de paginação degrada o desempenho geral do sistema.
Você pode otimizar a paginação usando as seguintes diretrizes para o posicionamento de arquivo de paginação:
- Isole o arquivo de paginação em seu próprio dispositivo de armazenamento ou pelo menos verifique se ele não compartilha os mesmos dispositivos de armazenamento que outros arquivos acessados com frequência. Por exemplo, coloque o arquivo de paginação e os arquivos do sistema operacional em unidades de disco físico separadas.

- Coloque o arquivo de paginação em uma unidade que não seja tolerante a falhas. Se o disco falhar, será muito provável que ocorra uma falha do sistema. Se você colocar o arquivo de paginação em uma unidade tolerante a falhas, lembre-se de que os sistemas tolerantes a falhas geralmente são mais lentos para gravar dados porque os gravam em vários locais.

- Use vários discos ou uma matriz de disco, se precisar de largura de banda de disco adicional para paginação. Não coloque vários arquivos de paginação em partições diferentes da mesma unidade de disco físico.

## <a name="peripheral-bus-recommendations"></a>Recomendações de barramento periférico
No Windows Server 2016, o armazenamento primário e os adaptadores de rede devem ser PCIe (PCI Express), portanto, os servidores com barramentos de PCIe são recomendados. Para evitar limitações de velocidade do barramento, use PCIe x8 e slots superiores para adaptadores de Ethernet de, no mínimo, 10 GB.

## <a name="disk-recommendations"></a>Recomendações de disco
Escolha discos com velocidades rotacionais maiores para reduzir os tempos de serviço de solicitação aleatória (cerca de 2 ms em média ao comparar as unidades de 7.200 e 15.000 RPM) e para aumentar a largura de banda das solicitações sequenciais. No entanto, há considerações de custo, energia e outras associadas aos discos que têm alta velocidade rotacional.

Os discos de 2,5 polegadas de classe empresarial podem atender a um número significativamente maior de solicitações aleatórias por segundo em comparação comparada às unidades de 3,5 polegadas equivalentes.

Armazene dados acessados com frequência, principalmente os dados acessados em sequência, no início de um disco, porque isso corresponde aproximadamente aos controles mais externos (mais rápidos).

A consolidação de pequenas unidades em menos unidades de alta capacidade pode reduzir o desempenho geral do armazenamento. Um número menor de eixos significa uma redução da simultaneidade do serviço de solicitação e, portanto, provavelmente uma taxa de transferência menor e tempos de resposta maiores (dependendo da intensidade da carga de trabalho).

O uso de SSD e de discos flash de alta velocidade é útil para ler principalmente os discos com altas taxas de E/S ou com E/S sensível à latência. Os discos de inicialização são bons candidatos para o uso de um SSD ou de discos flash de alta velocidade, pois eles podem melhorar significativamente os tempos de inicialização.

Os SSDs NVMe oferecem melhor desempenho com maior profundidade de fila de comando, processamento de interrupção mais eficiente e maior eficiência para os comandos de 4KB. Isso beneficia principalmente os cenários que exigem E/S simultâneas pesadas.


## <a name="network-and-storage-adapter-recommendations"></a>Recomendações de adaptador de rede e de armazenamento

A seção a seguir lista as características recomendadas para adaptadores de rede e armazenamento para servidores de alto desempenho. Essas configurações podem ajudar a impedir que o hardware de armazenamento ou de rede seja um gargalo quando estiverem sob carga pesada.

### <a name="certified-adapter-usage"></a>Uso de adaptador certificado
Use um adaptador que aprovado pelo conjunto de testes de Certificação de Hardware do Windows.

### <a name="64-bit-capability"></a>Capacidade de 64 bits
Os adaptadores compatíveis com 64 bits podem executar operações de DMA (acesso direto à memória) de/para locais de memória física grande (com mais de 4 GB). Se o driver não der suporte a um DMA com mais de 4 GB, o sistema armazenará em buffer duplo a E/S em um espaço de endereço físico com menos de 4 GB.

### <a name="copper-and-fiber-adapters"></a>Adaptadores de fibra e cobre
Os adaptadores de cobre geralmente têm o mesmo desempenho de suas contrapartes de fibra. As duas opções estão disponíveis em alguns adaptadores de Fibre Channel. Determinados ambientes são mais adequados para adaptadores de cobre, enquanto outros ambientes são mais adequados para adaptadores de fibra.

### <a name="dual--or-quad-port-adapters"></a>Adaptadores de porta dupla ou quádrupla
Os adaptadores de várias portas são úteis para servidores que têm um número limitado de slots PCI.

Para resolver as limitações de SCSI quanto ao número de discos que podem ser conectados a um barramento de SCSI, alguns adaptadores fornecem dois ou quatro barramentos de SCSI em uma única placa de adaptador. Os adaptadores de Fibre Channel geralmente não têm limites quanto ao número de discos conectados a um adaptador, a menos que eles estejam ocultos em uma interface SCSI.

Os adaptadores SAS (Serial Attached SCSI) e SATA (Serial ATA) também têm um número limitado de conexões devido à natureza serial dos protocolos, mas você pode anexar mais discos usando comutadores.

Os adaptadores de rede têm esse recurso para cenários de failover ou de balanceamento de carga. O uso de dois adaptadores de rede de porta única geralmente resulta em um desempenho melhor do que o uso de um único adaptador de rede de porta dupla para a mesma carga de trabalho.

A limitação de barramento de PCI pode ser um fator importante na limitação do desempenho para os adaptadores de várias portas. Portanto, é importante considerar que elas sejam colocadas em um slot de PCIe de alto desempenho que forneça largura de banda suficiente.

### <a name="interrupt-moderation"></a>Moderação de interrupção
Alguns adaptadores podem moderar a frequência com que interrompem os processadores do host para indicar a atividade ou sua conclusão. A moderação de interrupções geralmente pode resultar na redução da carga de CPU no host, mas, se a moderação de interrupção não for executada de forma inteligente, as economias de CPU poderão aumentar a latência.

### <a name="receive-side-scaling-rss-support"></a>Suporte a RSS (Receive Side Scaling)
O RSS permite que o processamento de recebimento de pacote seja dimensionado com o número de processadores do computador disponíveis. Isso é importante especificamente com a Ethernet de 10 GB ou mais rápidas.

### <a name="offload-capability-and-other-advanced-features-such-as-message-signaled-interrupt-msi-x"></a>Funcionalidade de descarregamento e outros recursos avançados, como a MSI-X (interrupção sinalizada de mensagem)
Os adaptadores com a funcionalidade de descarregamento oferecem economias de CPU que resultam na melhoria do desempenho.

### <a name="dynamic-interrupt-and-deferred-procedure-call-dpc-redirection"></a>Interrupção dinâmica e redirecionamento de DPC (chamada de procedimento deferida)
No Windows Server 2016, a E/S NUMA permite que os adaptadores de armazenamento de PCIe redirecionem as interrupções e as DPCs dinamicamente e pode ajudar qualquer sistema multiprocessador melhorando o particionamento de carga de trabalho, as taxas de ocorrências no cache e o uso de interconexão de hardware integrado para cargas de trabalho de E/S intensiva.

## <a name="see-also"></a>Consulte também
- [Server Hardware Power Considerations](power.md) (Considerações de energia de hardware do servidor)
- [Power and Performance Tuning](power/power-performance-tuning.md) (Energia e ajuste de desempenho)
- [Processor Power Management Tuning](power/processor-power-management-tuning.md) (Ajuste de gerenciamento de energia do processador)
- [Parâmetros de plano balanceado recomendados](power/recommended-balanced-plan-parameters.md)
