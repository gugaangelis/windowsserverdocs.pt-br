---
title: Gerenciamento de energia do processador (PPM) de ajuste para o plano de energia Equilibrado do Windows Server
description: Gerenciamento de energia do processador (PPM) de ajuste para o plano de energia Equilibrado do Windows Server
ms.prod: windows-server-threshold
ms.technology: performance-tuning-guide
ms.topic: article
ms.author: Qizha;TristanB
author: phstee
ms.date: 10/16/2017
ms.openlocfilehash: 9b8af89992f01712e16d0ef503c8cbbac915df1d
ms.sourcegitcommit: 6ef4986391607bb28593852d06cc6645e548a4b3
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/07/2019
ms.locfileid: "66811584"
---
# <a name="processor-power-management-ppm-tuning-for-the-windows-server-balanced-power-plan"></a>Gerenciamento de energia do processador (PPM) de ajuste para o plano de energia Equilibrado do Windows Server

Começando com o Windows Server 2008, Windows Server fornece três planos de energia: **Balanceamento**, **alto desempenho**, e **economia de energia**. O **equilibrado** plano de energia é a opção padrão que tem como objetivo fornecer a melhor eficiência de energia para um conjunto de cargas de trabalho típica do servidor. Este tópico descreve as cargas de trabalho que foram usadas para determinar as configurações padrão para o **equilibrado** esquema para os últimos lançamentos do Windows.

Se você executar um sistema de servidor que tem requisitos de energia que essas cargas de trabalho ou de desempenho e de características de carga de trabalho drasticamente diferente, talvez queira considerar o ajuste as configurações de energia padrão (ou seja, criar um plano de energia personalizado). Uma fonte de informações úteis de ajustes é o [considerações de energia de Hardware de servidor](../power.md). Como alternativa, você pode decidir que o **de alto desempenho** plano de energia é a escolha certa para seu ambiente, reconhecendo que você provavelmente levará uma quantidade significativa de energia de ocorrências em troca de algum nível de maior capacidade de resposta.

> [!IMPORTANT]
> Você deve aproveitar as políticas de energia que são incluídas com o Windows Server, a menos que você tenha uma necessidade específica para criar um personalizado e ter uma compreensão muito boa que os resultados vão variar dependendo das características da carga de trabalho.

## <a name="windows-processor-power-tuning-methodology"></a>Metodologia de ajuste do Windows processador Power


### <a name="tested-workloads"></a>Cargas de trabalho testadas

Cargas de trabalho são selecionadas para abranger um conjunto de melhor esforço "típica" cargas de trabalho do Windows Server. Obviamente, esse conjunto não se destina para representar a largura inteira de ambientes de servidor do mundo real.

O ajuste de cada política de energia é conduzidos pelas seguintes cinco cargas de trabalho de dados:

-   **Carga de trabalho do servidor Web do IIS**

    Uma comparação internos da Microsoft chamada conceitos básicos da Web é usada para otimizar a eficiência energética de plataformas que executam o servidor Web do IIS. A instalação contém um servidor web e vários clientes que simulam o tráfego de acesso da web. A distribuição de dinâmico e estático quente (na memória) e frio estático (acesso a disco necessário) as páginas da web é com base em estudos de estatísticos de servidores de produção. Para enviar por push de núcleos de CPU do servidor para a utilização total (uma extremidade do espectro testada), a instalação precisa de recursos de rede e disco suficientemente rápidos.

-   **Carga de trabalho de banco de dados do SQL Server**

    O [TPC E](http://www.tpc.org/tpce/default.asp) parâmetro de comparação é um parâmetro de comparação popular para análise de desempenho do banco de dados. Ele é usado para gerar uma carga de trabalho OLTP para otimizações de ajuste do PPM. Essa carga de trabalho tem e/s de disco significativo e, portanto, tem um requisito de alto desempenho para o tamanho de memória e o sistema de armazenamento.

-   **Carga de trabalho de servidor de arquivos**

    Chamado de um parâmetro de comparação com a Microsoft desenvolveu [FSCT](http://www.snia.org/sites/default/files2/sdc_archives/2009_presentations/tuesday/BartoszNyczkowski-JianYan_FileServerCapacityTool.pdf) é usado para gerar uma carga de trabalho do servidor de arquivos SMB. Ele cria um arquivo grande definida no servidor e usa muitos sistemas de cliente (reais ou virtualizados) para gerar o arquivo abrir, fechar, ler e gravar operações. A combinação de operação baseia-se em estudos de estatísticos de servidores de produção. Ele enfatiza a CPU, disco e recursos de rede.

-   **SPECpower – carga de trabalho do JAVA**

    [SPECpower\_ssj2008](http://spec.org/power_ssj2008/) é o primeiro benchmark de especificações de padrão da indústria que avalia em conjunto de características de desempenho e energia. É uma carga de trabalho do Java do lado do servidor com diferentes níveis de carga de CPU. Ele não requer muitos recursos de disco ou rede, mas tem certos requisitos de largura de banda de memória. Quase todas as atividades de CPU é executada no modo de usuário; atividade de modo kernel não ter muito impacto na energia dos parâmetros de comparação e características de desempenho, exceto as decisões de gerenciamento de energia.

-   **Carga de trabalho de servidor de aplicativo**

    O [SAP SD](http://global.sap.com/campaigns/benchmark/index.epx) benchmark é usado para gerar uma carga de trabalho do servidor de aplicativo. Uma configuração de duas camadas é usada com o banco de dados e o servidor de aplicativos no mesmo host do servidor. Essa carga de trabalho também utiliza o tempo de resposta como uma métrica de desempenho, que é diferente de outras cargas de trabalho testadas. Portanto, ele é usado para verificar o impacto dos parâmetros do PPM na capacidade de resposta. No entanto, ele não pretende ser representativos de todas as cargas de trabalho de produção sensíveis à latência.

Todos os parâmetros de comparação, exceto SPECpower foram originalmente criados para análise de desempenho e, portanto, foram criados para serem executados em níveis de carga de pico. No entanto, os níveis de médio a carga leve estão mais comum para servidores de produção do mundo real e são mais interessante para **equilibrado** planejar otimizações. Intencionalmente executamos os parâmetros de comparação em vários níveis de carga de 100% para baixo até 10% (nas etapas de 10%) usando vários métodos de limitação (por exemplo, reduzindo o número de clientes/usuários do Active Directory).

### <a name="hardware-configurations"></a>Configurações de hardware

Para cada versão do Windows, servidores de produção mais recentes são usados no processo análise e otimização do plano de energia. Em alguns casos, os testes foram executados em sistemas de pré-produção cuja cronograma de lançamento combinou da próxima versão do Windows.

Considerando que a maioria dos servidores são vendidas com soquetes de processador de 1 a 4, e como os servidores de expansão são menos propensos a eficiência de energia como uma preocupação principal, os testes de otimização do plano de energia principalmente são executados em sistemas de soquete de 2 e 4 soquetes. A quantidade de RAM, disco e recursos de rede para cada teste são escolhidos para permitir que cada sistema ser executado até sua capacidade total, levando em consideração as restrições de custo que normalmente seriam em vigor para ambientes de servidor do mundo real, como manter o configurações de razoáveis.

> [!IMPORTANT]
> Mesmo que o sistema pode executar em sua carga de pico, normalmente otimizamos para níveis inferiores de carga, uma vez que os servidores que executam consistentemente em seus níveis de carga de pico, seria bem aconselhados a usar o **de alto desempenho** , a menos que o plano de energia energia eficiência é uma prioridade alta.

### <a name="metrics"></a>metrics

Todos os parâmetros de comparação testados usar taxa de transferência como a métrica de desempenho. Tempo de resposta é considerado como um requisito do SLA para essas cargas de trabalho (exceto para SAP, onde ele é uma métrica principal). Por exemplo, uma execução de parâmetro de comparação é considerada "válida" se o tempo de resposta máximo ou média for menor que o valor determinado.

Portanto, o PPM análise de ajuste também usa taxa de transferência como sua métrica de desempenho.  No nível de carga mais alto (100% de utilização da CPU), nosso objetivo é que a taxa de transferência não deva ser reduzido mais de um pequeno percentual devido às otimizações de gerenciamento de energia. Mas a principal consideração é maximizar a eficiência de energia (conforme definido abaixo) nos níveis de carga média e baixa.

![fórmula de eficiência de energia](../../media/serverperf-ppm-formula.jpg)

Executar os núcleos de CPU a frequências baixas reduz o consumo de energia. No entanto, frequências inferiores geralmente diminuir a taxa de transferência e aumentam o tempo de resposta. Para o **equilibrado** plano de energia, há uma compensação intencional de eficiência de energia e de capacidade de resposta. Os testes de carga de trabalho do SAP, bem como o tempo de resposta SLAs nas outras cargas de trabalho, certifique-se de que o aumento do tempo de resposta não excede determinado limite (% 5 como um exemplo) para essas cargas de trabalho específicas.

> [!NOTE]
> Se a carga de trabalho usa o tempo de resposta como a métrica de desempenho, o sistema deve pode alterar para o **de alto desempenho** plano de energia ou alteração **equilibrado** plano de energia conforme sugerido no [ Recomendado parâmetros de plano de energia Equilibrado para tempo de resposta rápido](recommended-balanced-plan-parameters.md).

### <a name="tuning-results"></a>Resultados do ajuste

Começando com o Windows Server 2008, a Microsoft trabalhou com a Intel e AMD para otimizar os parâmetros PPM para os processadores de servidor mais recentes para cada versão do Windows. Um grande número de combinações de parâmetro PPM foram testado em cada uma das cargas de trabalho discutidas anteriormente para encontrar a melhor eficiência de energia em níveis diferentes de carga. Como o software algoritmos foram refinados e como arquiteturas de energia de hardware evolved, cada novo do Windows Server sempre teve melhor ou igual a eficiência de energia que suas versões anteriores em uma série de cargas de trabalho testadas.

A figura a seguir fornece um exemplo da eficiência de energia em diferentes níveis de carga do TPC-eletrônico em um servidor de produção de 4 soquetes, executando o Windows Server 2008 R2. Ele mostra uma melhoria de 8% nos níveis de carga média em comparação comparada o Windows Server 2008.

![comparação de eficiência de energia](../../media/serverperf-ppm-figure1.jpg)

## <a name="customized-tuning-suggestions"></a>Personalizado sugestões de ajuste

Se suas características de carga de trabalho primária diferem significativamente das cinco cargas de trabalho usadas para o padrão **equilibrado** plano de energia PPM ajuste, você pode fazer experiências alterando um ou mais parâmetros PPM para encontrar a melhor opção para seu ambiente.

Devido ao número e a complexidade de parâmetros, isso pode ser uma tarefa desafiadora, mas se você estiver procurando a melhor relação entre a eficácia de carga de trabalho e o consumo de energia para seu ambiente específico, pode ser que vale a pena o esforço.

 O conjunto completo de parâmetros ajustáveis do PPM pode ser encontrado no [ajuste de gerenciamento de energia do processador](https://msdn.microsoft.com/windows/hardware/gg566941.aspx). Alguns dos parâmetros de energia mais simples para começar poderia ser:

-   **Aumentar o limite de desempenho do processador e a hora de aumentar o desempenho de processador** – valores maiores de perf resposta lenta a maior atividade

-   **Limite de diminuir o desempenho do processador** – valores grandes quicken a resposta de energia para períodos de ociosidade

-   **Tempo de diminuir o desempenho de processador** – valores maiores mais gradualmente diminuem o desempenho durante períodos ociosos

-   **Política de aumentar o desempenho de processador** – a política "Única" reduz a resposta de desempenho para aumento e sustentada atividade; a política "Rocket" reage rapidamente para maior atividade

-   **Política de diminuir o desempenho de processador** – a política "Única" diminui mais gradualmente perf por períodos mais ociosos; a política "Rocket" descarta power muito rapidamente ao inserir um período ocioso

>[!Important]
> Antes de iniciar qualquer experimentos, você deve primeiro compreender suas cargas de trabalho, o que ajudarão você a fazer as escolhas de parâmetro PPM corretas e reduzir os esforços de ajuste.

### <a name="understand-high-level-performance-and-power-requirements"></a>Entender o desempenho de alto nível e requisitos de energia

Se sua carga de trabalho é "em tempo real" (por exemplo, suscetível a problemas na imagem ou de outros visíveis ao usuário final afeta) ou tem o requisito de capacidade de resposta muito forte (por exemplo, um corretagem de ações), e se o consumo de energia não é um critério principal para o seu ambiente, você provavelmente deve simplesmente alternar para o **de alto desempenho** plano de energia. Caso contrário, você deve entender os requisitos de tempo de resposta de suas cargas de trabalho e, em seguida, ajustar os parâmetros PPM para a melhor eficiência possível de energia que atende a esses requisitos ainda.

### <a name="understand-underlying-workload-characteristics"></a>Entender as características de carga de trabalho subjacente

Você deve conhecer suas cargas de trabalho e os conjuntos de parâmetros de teste para o ajuste de design. Por exemplo, se as frequências dos núcleos da CPU precisam ser aumentada muito fast (talvez você tenha uma carga de trabalho de intermitente com períodos de inatividade significativos, mas precisar de capacidade de resposta muito rápida quando uma nova transação acompanha) e, em seguida, o desempenho do processador aumenta política Talvez seja necessário a ser definido como "rocket" (que, como o nome implica, Atira a frequência de núcleo de CPU para seu valor máximo em vez de progredirem durante um período de tempo).

Se sua carga de trabalho for muito intermitente, o intervalo de verificação do PPM pode ser reduzido para fazer com que a frequência da CPU inicie progredirem mais cedo depois que chega um disparo. Se sua carga de trabalho não tiver a simultaneidade de threads alta, e estacionamento do núcleo pode ser ativado para forçar a carga de trabalho para executar em um número menor de núcleos, que também pode melhorar de acertos do cache do processador proporções.

Se você quiser aumentar frequências de CPU em níveis de utilização média (ou seja, níveis de carga de trabalho não leve), os limites de aumentar/diminuir de desempenho do processador podem ser ajustados para reagir não até que determinados níveis de atividade são observados.

### <a name="understand-periodic-behaviors"></a>Entender os comportamentos periódicos

Pode haver requisitos de desempenho diferente para a hora do dia e hora da noite ou nos finais de semana de, ou pode haver diferentes cargas de trabalho que são executados em momentos diferentes. Nesse caso, um conjunto de parâmetros do PPM pode não ser o ideal para todos os períodos de tempo. Uma vez que vários planos de energia personalizado podem ser projetados, é possível até mesmo ajustar para períodos de tempo diferentes e mudar entre planos de energia por meio de scripts ou outros meios de configuração do sistema dinâmico.

Novamente, isso aumenta a complexidade do processo de otimização, portanto, é uma questão de quanto o valor será obtido deste tipo de ajuste, que provavelmente precisarão ser repetidas quando há atualizações significativas de hardware ou alterações de carga de trabalho.

Isso ocorre porque o Windows fornece um **equilibrado** plano de energia em primeiro lugar, porque em muitos casos é provavelmente não vale a pena o esforço de mão-de ajuste uma carga de trabalho específico em um servidor específico.

## <a name="see-also"></a>Consulte também
- [Considerações de desempenho de Hardware do servidor](../index.md)
- [Server Hardware Power Considerations](../power.md) (Considerações de energia de hardware do servidor)
- [Power and Performance Tuning](power-performance-tuning.md) (Energia e ajuste de desempenho)
- [Processor Power Management Tuning](processor-power-management-tuning.md) (Ajuste de gerenciamento de energia do processador)
- [Parâmetros de plano balanceado recomendados](recommended-balanced-plan-parameters.md)
