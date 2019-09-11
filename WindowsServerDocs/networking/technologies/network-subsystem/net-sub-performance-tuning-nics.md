---
title: Ajuste de desempenho de adaptadores de rede
description: Este tópico faz parte do guia de ajuste de desempenho do subsistema de rede para o Windows Server 2016.
ms.prod: windows-server-threshold
ms.technology: networking
ms.topic: article
ms.assetid: 0b9b0f80-415c-4f5e-8377-c09b51d9c5dd
manager: brianlic
ms.author: pashort
author: shortpatti
ms.openlocfilehash: a0d87f2e8c9b3ca7581d20009cd9c0e030241270
ms.sourcegitcommit: f6490192d686f0a1e0c2ebe471f98e30105c0844
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/10/2019
ms.locfileid: "70871886"
---
# <a name="performance-tuning-network-adapters"></a>Ajuste de desempenho de adaptadores de rede

>Aplica-se a: Windows Server (Canal Semestral), Windows Server 2016

Você pode usar este tópico para ajustar o desempenho de adaptadores de rede que estão instalados em computadores que executam o Windows Server 2016.

Determinar as corretas configurações de ajuste para seu adaptador de rede depende das seguintes variáveis:

- O adaptador de rede e seu conjunto de recursos  

- O tipo de carga de trabalho executado pelo servidor  

- Os recursos de hardware e software do servidor  

- Seus objetivos de desempenho para o servidor  

Se seu adaptador de rede fornecer opções de ajuste, você poderá otimizar a capacidade da rede e o uso de recursos para obter a taxa de transferência ideal de acordo com os parâmetros descritos acima.  

As seções a seguir descrevem algumas de suas opções de ajuste de desempenho.  

##  <a name="bkmk_offload"></a>Habilitando recursos de descarregamento

O ajuste nos recursos de descarregamento do adaptador de rede normalmente é benéfico. Às vezes, no entanto, o adaptador de rede não está potente o suficiente para lidar com os recursos de descarregamento com alta taxa de transferência.

>[!IMPORTANT]
>Não use os recursos de descarregamento Descarregamento de **tarefa IPSec** ou **descarregamento de Chimney TCP**. Essas tecnologias são preteridas no Windows Server 2016 e podem afetar negativamente o desempenho do servidor e da rede. Além disso, essas tecnologias podem não ter suporte da Microsoft no futuro.

Por exemplo, habilitar o descarregamento de segmentação pode reduzir a taxa de transferência sustentável máxima em alguns adaptadores de rede, devido aos recursos de hardware limitados. Porém, se a taxa de transferência reduzida não representar uma limitação, você deverá habilitar os recursos de descarregamento, mesmo para esse tipo de adaptador de rede.

>[!NOTE]
> Alguns adaptadores de rede requerem que os recursos de descarregamento sejam habilitados independentemente para enviar e receber caminhos.

##  <a name="bkmk_rss_web"></a>Habilitando o RSS (recebimento de recepção lateral) para servidores Web

O RSS pode melhorar a escalabilidade e o desempenho da Web quando há menos adaptadores de rede do que processadores lógicos no servidor. Quando todo o tráfego está passando pelos adaptadores de rede habilitados para RSS, as solicitações da Web que entram de diferentes conexões podem ser processadas simultaneamente em diferentes CPUs.

É importante observar que, devido à lógica em RSS e http \(\) do protocolo de transferência de hipertexto para distribuição de carga, o desempenho poderá ser seriamente degradado se um adaptador de rede compatível com RSS não aceitar o tráfego da Web em um servidor que tenha um ou mais adaptadores de rede compatíveis com RSS. Nesse caso, você deve usar adaptadores de rede habilitados para RSS ou desabilitar RSS na guia **Propriedades Avançadas** das propriedades do adaptador de rede. Para determinar se um adaptador de rede está habilitado para RSS, você pode visualizar as informações RSS na guia **Propriedades Avançadas** das propriedades do adaptador de rede.

### <a name="rss-profiles-and-rss-queues"></a>Perfis RSS e filas RSS

O perfil padrão RSS predefinido é estático NUMA, que altera o comportamento padrão de versões anteriores do sistema operacional. Para iniciar com Perfis RSS, você pode examinar os perfis disponíveis para entender quando eles são benéficos e como eles se aplicam ao seu ambiente de rede e hardware.

Por exemplo, se você abrir o Gerenciador de Tarefas e examinar os processadores lógicos em seu servidor, e eles parecerem subutilizados para receber tráfego, você pode tentar aumentar o número de filas RSS do padrão de 2 para o máximo a que seu adaptador de rede oferce suporte. O adaptador de rede pode ter opções para alterar o número de filas RSS como parte do driver.

##  <a name="bkmk_resources"></a>Aumentando os recursos do adaptador de rede

Para adaptadores de rede que permitem configuração manual dos recursos, nos buffers de envio e recebimento, você pode aumentar os recursos alocados. 

Alguns adaptadores de rede configuram seus buffers de recebimento como baixos para preservar a memória alocada do host. O valor baixo resulta em pacotes descartados e desempenho reduzido. Portanto, para cenários de recebimento intenso, recomendamos aumentar o valor do buffer de recebimento para o máximo.

>[!NOTE]
>Se um adaptador de rede não expõe a configuração de recurso manual, ele configura os recursos dinamicamente, ou os recursos são definidos para um valor fixo que não pode ser alterado.

### <a name="enabling-interrupt-moderation"></a>Habilitar moderação de interrupção

Para controlar a moderação de interrupção, alguns adaptadores de rede expõem diferentes níveis de moderação de interrupção, parâmetros de conciliação de buffer (às vezes separadamente para buffers de envio e recebimento) ou ambos.

Você deve considerar moderação de interrupção para cargas de trabalho ligadas a CPU, e ponderar entre economias de CPU do host e latência ou o aumento de economias de CPU do host, devido a mais interrupções e menos latência. Se o adaptador de rede não executa a moderação de interrupções, mas isso expõe a conciliação de buffer, aumentar o número de buffers conciliados permite mais buffers por envio ou recebimento, o que melhora o desempenho.

##  <a name="bkmk_low"></a>Ajuste de desempenho para processamento de pacotes de baixa latência

Muitos adaptadores de rede fornecem opções para otimizar a latência induzida pelo sistema operacional. A latência é o tempo decorrido entre o momento em que o driver de rede processa um pacote recebido e o driver de rede envia o pacote de volta. Esse tempo geralmente é medido em microssegundos. Para comparação, o tempo de transmissão para transmissões de pacotes por longas distâncias geralmente é medido em milissegundos \(uma ordem de magnitude maior.\) Esse ajuste não reduzirá o tempo que um pacote gasta em trânsito.

A seguir estão algumas sugestões de ajuste no desempenho para redes sensíveis a microssegundo.

- Defina o BIOS do computador para **Alto Desempenho**, com estados C desabilitados. Entretanto, observe que isso depende do sistema e do BIOS, alguns sistemas fornecerão maior desempenho se o sistema operacional controlar o gerenciamento de energia. Você pode verificar e ajustar suas configurações de gerenciamento de energia de **configurações** ou usando o comando **powercfg** . Para obter mais informações, consulte [Opções de linha de comando powercfg](https://technet.microsoft.com/library/cc748940.aspx)

- Defina o perfil de gerenciamento de energia do sistema operacional para **Sistema de alto desempenho**. Observe que isso não funcionará corretamente se o BIOS do sistema estiver definido para desabilitar o controle de gerenciamento de energia do sistema operacional.

- Habilite os Descarregamentos estáticos, por exemplo, somas de verificação UDP, somas de verificação TCP e LSO (Descarregamento de Envio Grande).

- Habilite RSS se o tráfego for multistream, como de recebimento de alto volume de multicast.

-   Desabilite a configuração **Moderação de interrupção** dos drivers da placa de rede que requerem a menor latência possível. Lembre-se de que isso pode usar mais tempo de CPU e representar um comprometimento.

- Manipule as interrupções do adaptador de rede e DPCs em um processador de núcleo que compartilha cache de CPU com o núcleo que está sendo usado pelo programa (thread de usuário) que está lidando com o pacote. O ajuste na afinidade da CPU pode ser usado para direcionar um processo para determinados processadores lógicos em conjunto com a configuração de RSS com essa finalidade. O uso do mesmo núcleo para interrupção, DPC e thread de modo de usuário revela piora no desempenho na medida em que a carga aumenta, porque o ISR, DPC e thread disputam o uso do núcleo.

##  <a name="bkmk_smi"></a>Interrupções de gerenciamento do sistema

Muitos sistemas de hardware usam o gerenciamento do \(sistema\) interrompe o SMI para uma variedade de funções de manutenção, incluindo relatórios \(de\) erros de memória ECC de código de correção de erros, compatibilidade com USB herdada, ventilador controle e gerenciamento de energia controlado por BIOS. 

O SMI é a interrupção de mais alta prioridade no sistema e deixa a CPU em um modo de gerenciamento que impede todas as outras atividades enquanto executa uma rotina de serviço de interrupção, normalmente contida no BIOS.

Infelizmente, isso pode resultar em picos de latência de 100 microssegundos ou mais. 

Se for necessário obter a menor latência, você deve solicitar uma versão do BIOS, junto a seu fornecedor de hardware, que reduza as SMIs para o menor nível possível. Isso normalmente é referido como "BIOS de baixa latência” ou “BIOS livre de SMI”. Em alguns casos, não é possível para uma plataforma de hardware eliminar a atividade de SMI completamente, ela é usada para controlar funções essenciais (por exemplo, ventiladores de refrigeração).

>[!NOTE]
>O sistema operacional pode não exercer nenhum controle sobre as SMIs, porque o processador lógico está executando em um modo de manutenção especial, que impede a intervenção do sistema operacional.

##  <a name="bkmk_tcp"></a>Ajuste de desempenho TCP

 Você pode ajustar o desempenho de TCP usando os itens a seguir.

###  <a name="bkmk_tcp_params"></a>Ajuste automático da janela de recepção TCP

Antes do Windows Server 2008, a pilha de rede usava uma janela do lado de recebimento de tamanho fixo (65.535 bytes) que limitou a taxa de transferência potencial geral para conexões. Uma das mudanças mais significativas para a pilha TCP é o ajuste automático da janela de recebimento TCP. 

Você pode calcular a taxa de transferência total de uma única conexão ao usar uma janela de recebimento de TCP de tamanho fixo como:

**Taxa de transferência atingível total em bytes = tamanho da janela de \* recepção TCP em bytes (1/latência de conexão em segundos)**

Por exemplo, a taxa de transferência atingível total é de apenas 51 Mbps em uma conexão com \(uma latência de 10 ms um valor razoável para\)uma grande infraestrutura de rede corporativa. 

Com o ajuste automático, porém, a janela do lado de recebimento é ajustável, e pode aumentar para atender às demandas do emissor. É possível que uma conexão obtenha uma taxa de linha completa de uma conexão de 1 Gbps. Os cenários de uso da rede que, no passado podiam limitar a conexão por meio de taxa de transferência possível total em conexões TCP, agora podem usar a rede integralmente.

#### <a name="deprecated-tcp-parameters"></a>Parâmetros TCP preteridos

As seguintes configurações do registro do Windows Server 2003 não são mais suportadas e são ignoradas em versões posteriores.

Todas essas configurações tinham o seguinte local de registro:

    ```  
    HKEY_LOCAL_MACHINE\System\CurrentControlSet\Services\Tcpip\Parameters  
    ```  

- TcpWindowSize

- NumTcbTablePartitions  

- MaxHashTableSize  



###  <a name="bkmk_wfp"></a>Plataforma de filtragem do Windows

A WFP (Windows Filtering Platform) introduzida no Windows Vista e no Windows Server 2008 fornece APIs para ISVs (fornecedores independentes de software) que não são da Microsoft para criar filtros de processamento de pacotes. Exemplos incluem software de firewall e antivírus.

>[!NOTE]
>Um filtro WFP mal escrito pode diminuir significativamente o desempenho de rede de um servidor. Para obter mais informações, consulte [portando drivers de processamento de pacotes e aplicativos para o WFP](https://msdn.microsoft.com/windows/hardware/gg463267.aspx) no centro de desenvolvimento do Windows.


Para obter links para todos os tópicos deste guia, consulte [ajuste de desempenho do subsistema de rede](net-sub-performance-top.md).
