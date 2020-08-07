---
title: Ajuste do gerenciamento de energia do processador (PPM) para o plano de energia balanceado do Windows Server
description: Ajuste do gerenciamento de energia do processador (PPM) para o plano de energia balanceado do Windows Server
ms.topic: conceptual
ms.author: qizha;tristanb
author: phstee
ms.date: 10/16/2017
ms.openlocfilehash: 9b45ad16981667eff626278daadbe8b39f5cc5c8
ms.sourcegitcommit: 53d526bfeddb89d28af44210a23ba417f6ce0ecf
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/06/2020
ms.locfileid: "87896718"
---
# <a name="processor-power-management-ppm-tuning-for-the-windows-server-balanced-power-plan"></a>Ajuste do gerenciamento de energia do processador (PPM) para o plano de energia balanceado do Windows Server

A partir do Windows Server 2008, o Windows Server fornece três planos de energia: **equilibrado**, **alto desempenho**e **economia de energia**. O plano de energia **equilibrado** é a opção padrão que visa fornecer a melhor eficiência de energia para um conjunto de cargas de trabalho de servidor típicas. Este tópico descreve as cargas de trabalho que foram usadas para determinar as configurações padrão do esquema **equilibrado** para as várias versões anteriores do Windows.

Se você executar um sistema de servidor que tenha características de carga de trabalho ou requisitos de desempenho e de energia drasticamente diferentes das cargas de trabalho, convém considerar o ajuste das configurações de energia padrão (ou seja, criar um plano de energia personalizado). Uma fonte de informações de ajuste úteis é as [considerações de energia de hardware do servidor](../power.md). Como alternativa, você pode decidir que o plano de energia de **alto desempenho** é a escolha certa para seu ambiente, reconhecendo que você provavelmente terá um impacto de energia significativo no Exchange para algum nível de maior capacidade de resposta.

> [!IMPORTANT]
> Você deve aproveitar as políticas de energia incluídas no Windows Server, a menos que tenha uma necessidade específica de criar um personalizado e ter uma compreensão muito boa de que os resultados variam dependendo das características da sua carga de trabalho.

## <a name="windows-processor-power-tuning-methodology"></a>Metodologia de otimização de energia do processador do Windows

### <a name="tested-workloads"></a>Cargas de trabalho testadas

As cargas de trabalho são selecionadas para abranger um conjunto de melhores esforços de cargas de trabalho "típicas" do Windows Server. Obviamente, esse conjunto não deve ser representativo de toda a amplitude de ambientes de servidor do mundo real.

O ajuste em cada política de energia são os dados controlados pelas cinco cargas de trabalho a seguir:

- **Carga de trabalho do servidor Web do IIS**

    Um parâmetro de comparação interno da Microsoft chamado conceitos básicos da Web é usado para otimizar a eficiência de energia das plataformas que executam o servidor Web IIS. A instalação contém um servidor Web e vários clientes que simulam o tráfego de acesso à Web. A distribuição de páginas da Web dinâmicas, estáticas e insuficientes (na memória) e estática inativa (acesso ao disco necessário) baseia-se em estudos estatísticos de servidores de produção. Para empurrar os núcleos de CPU do servidor para a utilização total (uma extremidade do espectro testado), a instalação precisa de recursos de rede e disco suficientemente rápidos.

- **Carga de trabalho do banco de dados SQL Server**

    O parâmetro de comparação de [TPC-E](http://www.tpc.org/tpce/default.asp) é um benchmark popular para análise de desempenho de banco de dados. Ele é usado para gerar uma carga de trabalho OLTP para otimizações de ajuste de PPM. Essa carga de trabalho tem e/s de disco significativa e, portanto, tem um requisito de alto desempenho para o sistema de armazenamento e o tamanho da memória.

- **Carga de trabalho do servidor de arquivos**

    Um parâmetro de comparação desenvolvido pela Microsoft chamado [FSCT](http://www.snia.org/sites/default/files2/sdc_archives/2009_presentations/tuesday/BartoszNyczkowski-JianYan_FileServerCapacityTool.pdf) é usado para gerar uma carga de trabalho do servidor de arquivos SMB. Ele cria um grande conjunto de arquivos no servidor e usa muitos sistemas cliente (reais ou virtualizados) para gerar operações de abertura de arquivo, fechamento, leitura e gravação. A operação Mix é baseada em estudos estatísticos de servidores de produção. Ele enfatiza a CPU, o disco e os recursos de rede.

- **SPECpower – carga de trabalho JAVA**

    [SPECpower \_ ssj2008](http://spec.org/power_ssj2008/) é o primeiro benchmark de especificação padrão do setor que avalia em conjunto as características de energia e desempenho. É uma carga de trabalho Java do lado do servidor com vários níveis de carga de CPU. Ele não requer muitos recursos de disco ou rede, mas tem determinados requisitos de largura de banda de memória. Quase toda a atividade de CPU é executada no modo de usuário; a atividade no modo kernel não tem muito impacto sobre as características de potência e desempenho de benchmarks, exceto para as decisões de gerenciamento de energia.

- **Carga de trabalho do servidor de aplicativos**

    O parâmetro de comparação [SAP-SD](http://global.sap.com/campaigns/benchmark/index.epx) é usado para gerar uma carga de trabalho do servidor de aplicativos. Uma configuração de duas camadas é usada, com o banco de dados e o servidor de aplicativos no mesmo host do servidor. Essa carga de trabalho também utiliza o tempo de resposta como uma métrica de desempenho, que difere de outras cargas de trabalho testadas. Portanto, ele é usado para verificar o impacto dos parâmetros PPM na capacidade de resposta. No entanto, não se destina a ser representativo de todas as cargas de trabalho de produção sensíveis à latência.

Todos os benchmarks, exceto os SPECpower, foram originalmente projetados para análise de desempenho e, portanto, foram criados para serem executados em níveis de carga de pico. No entanto, os níveis de carga de médio a claro são mais comuns para servidores de produção reais e são mais interessantes para otimizações de plano **equilibrado** . Nós executamos intencionalmente os parâmetros de comparação em diferentes níveis de carga de 100% a 10% (em etapas de 10%) usando vários métodos de limitação (por exemplo, reduzindo o número de usuários/clientes ativos).

### <a name="hardware-configurations"></a>Configuração de hardware

Para cada versão do Windows, os servidores de produção mais atuais são usados no processo de análise e otimização do plano de energia. Em alguns casos, os testes foram executados em sistemas de pré-produção cuja agenda de lançamento correspondeu à versão seguinte do Windows.

Considerando que a maioria dos servidores é vendida com 1 a 4 soquetes de processador, e como os servidores de expansão têm menos probabilidade de ter eficiência de energia como uma preocupação principal, os testes de otimização do plano de energia são executados principalmente em sistemas de 2 e 4 soquetes. A quantidade de RAM, disco e recursos de rede para cada teste são escolhidos para permitir que cada sistema seja executado até sua capacidade total, levando em conta as restrições de custo que normalmente seriam em vigor para ambientes de servidor reais, como manter as configurações razoáveis.

> [!IMPORTANT]
> Embora o sistema possa ser executado com seu pico de carga, normalmente otimizamos para níveis de carga mais baixos, já que os servidores que são executados consistentemente em seus níveis de carga de pico seriam bem aconselhados a usar o plano de energia de **alto desempenho** , a menos que a eficiência de energia seja uma prioridade alta.

### <a name="metrics"></a>Métricas

Todos os benchmarks testados usam a taxa de transferência como a métrica de desempenho. O tempo de resposta é considerado um requisito de SLA para essas cargas de trabalho (exceto para SAP, onde é uma métrica primária). Por exemplo, uma execução de parâmetro de comparação é considerada "válida" se a média ou o tempo de resposta máximo for menor que determinado valor.

Portanto, a análise de ajuste PPM também usa a taxa de transferência como sua métrica de desempenho.  No nível de carga mais alto (100% de utilização da CPU), nosso objetivo é que a taxa de transferência não deve diminuir mais do que alguns por cento devido a otimizações de gerenciamento de energia. Mas a principal consideração é maximizar a eficiência de energia (conforme definido abaixo) em níveis médios e baixos de carga.

![fórmula de eficiência de energia](../../media/serverperf-ppm-formula.jpg)

A execução dos núcleos de CPU em frequências menores reduz o consumo de energia. No entanto, frequências menores normalmente diminuem a taxa de transferência e aumentam o tempo de resposta Para o plano de energia **equilibrado** , há uma compensação intencional de capacidade de resposta e eficiência de energia. Os testes de carga de trabalho do SAP, bem como os SLAs de tempo de resposta em outras cargas de trabalho, certifique-se de que o aumento do tempo de resposta não exceda determinado limite (5% como exemplo) para essas cargas de trabalho específicas.

> [!NOTE]
> Se a carga de trabalho usar o tempo de resposta como a métrica de desempenho, o sistema deverá mudar para o plano de energia de **alto desempenho** ou alterar o plano de energia com **balanceamento** conforme sugerido em [parâmetros de plano de energia balanceados recomendados para tempo de resposta rápido](recommended-balanced-plan-parameters.md).

### <a name="tuning-results"></a>Resultados de ajuste

A partir do Windows Server 2008, a Microsoft trabalhou com a Intel e a AMD para otimizar os parâmetros PPM para os processadores de servidor mais atualizados para cada versão do Windows. Um grande número de combinações de parâmetro PPM foi testado em cada uma das cargas de trabalho discutidas anteriormente para encontrar a melhor eficiência de energia em diferentes níveis de carga. Como os algoritmos de software foram refinados e as arquiteturas de energia de hardware evoluíram, cada novo Windows Server sempre tinha uma eficiência de energia melhor ou igual à de suas versões anteriores em toda a variedade de cargas de trabalho testadas.

A figura a seguir fornece um exemplo da eficiência de energia em diferentes níveis de carga de TPC-E em um servidor de produção de 4 soquetes que executa o Windows Server 2008 R2. Ele mostra uma melhoria de 8% em níveis de carga média em comparação com o Windows Server 2008.

![comparação de eficiência de energia](../../media/serverperf-ppm-figure1.jpg)

## <a name="customized-tuning-suggestions"></a>Sugestões de ajuste personalizadas

Se suas características de carga de trabalho primárias diferirem significativamente das cinco cargas de trabalho usadas para o ajuste do plano de energia **equilibrado** padrão, você poderá experimentar alterando um ou mais parâmetros ppm para encontrar o melhor ajuste para o seu ambiente.

Devido ao número e à complexidade dos parâmetros, essa pode ser uma tarefa desafiadora, mas se você estiver procurando o melhor equilíbrio entre o consumo de energia e a eficácia da carga de trabalho para seu ambiente específico, pode valer o esforço.

 O conjunto completo de parâmetros ajustáveis pode ser encontrado no [ajuste de gerenciamento de energia do processador](https://msdn.microsoft.com/windows/hardware/gg566941.aspx). Alguns dos parâmetros de energia mais simples para começar com o podem ser:

-   Aumento do **desempenho do processador e desempenho do processador aumento do tempo** – os valores maiores lentam a resposta de desempenho para a maior atividade

-   **Limite de redução de desempenho do processador** – valores grandes que aceleram a resposta de energia para períodos ociosos

-   **Tempo de diminuição do desempenho do processador** – valores maiores diminuem gradualmente o desempenho durante períodos ociosos

-   **Política de aumento de desempenho do processador** – a política "única" retarda a resposta de desempenho para a atividade aumentada e sustentada; a política "Rocket" reage rapidamente à atividade aumentada

-   **Política de diminuição de desempenho do processador** – a política "única" diminui gradualmente o desempenho em períodos de ociosidade maiores; a política "Rocket" elimina a energia muito rapidamente ao entrar em um período ocioso

>[!Important]
> Antes de iniciar qualquer experimento, você deve primeiro compreender suas cargas de trabalho, o que o ajudará a fazer as opções de parâmetro PPM corretas e a reduzir o esforço de ajuste.

### <a name="understand-high-level-performance-and-power-requirements"></a>Entenda os requisitos de desempenho e de energia de alto nível

Se sua carga de trabalho for "tempo real" (por exemplo, suscetível a problemas ou outros impactos visíveis do usuário final) ou tiver um requisito de capacidade de resposta muito rígido (por exemplo, uma corretora de ações) e se o consumo de energia não for um critério principal para o seu ambiente, você provavelmente deve simplesmente mudar para o plano de energia de **alto desempenho** . Caso contrário, você deve compreender os requisitos de tempo de resposta de suas cargas de trabalho e ajustar os parâmetros PPM para obter a melhor eficiência de energia possível que ainda atenda a esses requisitos.

### <a name="understand-underlying-workload-characteristics"></a>Entender as características de carga de trabalho subjacente

Você deve saber suas cargas de trabalho e projetar os conjuntos de parâmetros de teste para ajuste. Por exemplo, se as frequências dos núcleos de CPU precisarem ser aumentadas muito rapidamente (talvez você tenha uma carga de trabalho intermitente com períodos ociosos significativos, mas você precisa de uma capacidade de resposta muito rápida quando uma nova transação é exibida. em seguida, a política de aumento de desempenho do processador pode precisar ser definida como "Rocket" (que, como o nome sugere, emite a frequência de núcleo da CPU para seu valor máximo em vez de resumir em um período de tempo).

Se a carga de trabalho for muito intermitente, o intervalo de verificação PPM poderá ser reduzido para fazer com que a frequência de CPU comece a ser reativada mais cedo após a chegada de uma intermitência. Se sua carga de trabalho não tiver alta simultaneidade de thread, o estacionamento principal poderá ser habilitado para forçar a execução da carga de trabalho em um número menor de núcleos, o que também pode melhorar as taxas de acertos do cache do processador.

Se você quiser apenas aumentar as frequências de CPU em níveis médios de utilização (ou seja, não níveis de carga de trabalho leves), o desempenho do processador aumentar/diminuir os limites pode ser ajustado para não reagir até que determinados níveis de atividade sejam observados.

### <a name="understand-periodic-behaviors"></a>Entender comportamentos periódicos

Pode haver diferentes requisitos de desempenho para o dia e a noite, ou para os fins de semana, ou pode haver diferentes cargas de trabalho executadas em momentos diferentes. Nesse caso, um conjunto de parâmetros PPM pode não ser o ideal para todos os períodos de tempo. Como vários planos de energia personalizados podem ser elaborados, é possível até mesmo se ajustar a diferentes períodos de tempo e alternar entre planos de energia por meio de scripts ou outros meios de configuração dinâmica do sistema.

Mais uma vez, isso aumenta a complexidade do processo de otimização, portanto, é uma questão de quanto valor será obtido desse tipo de ajuste, o que provavelmente precisará ser repetido quando houver atualizações significativas de carga de trabalho ou alterações de Workload.

É por isso que o Windows fornece um plano de energia **equilibrado** em primeiro lugar, porque em muitos casos provavelmente não vale o esforço de ajuste manual para uma carga de trabalho específica em um servidor específico.

## <a name="see-also"></a>Consulte Também
- [Considerações sobre o desempenho de hardware](../index.md)
- [Server Hardware Power Considerations](../power.md) (Considerações de energia de hardware do servidor)
- [Power and Performance Tuning](power-performance-tuning.md) (Energia e ajuste de desempenho)
- [Processor Power Management Tuning](processor-power-management-tuning.md) (Ajuste de gerenciamento de energia do processador)
- [Parâmetros de plano balanceado recomendados](recommended-balanced-plan-parameters.md)
