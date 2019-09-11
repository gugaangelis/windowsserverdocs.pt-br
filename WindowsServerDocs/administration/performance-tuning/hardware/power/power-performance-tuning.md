---
title: Ajuste de energia e desempenho
description: Ajuste do gerenciamento de energia do processador (PPM) para o plano de energia balanceado do Windows Server
ms.prod: windows-server-threshold
ms.technology: performance-tuning-guide
ms.topic: article
ms.author: Qizha;TristanB
author: phstee
ms.date: 10/16/2017
ms.openlocfilehash: 4ffa39d50d7b4c8429485e7db35bb2c863b7a995
ms.sourcegitcommit: f6490192d686f0a1e0c2ebe471f98e30105c0844
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/10/2019
ms.locfileid: "70866771"
---
# <a name="power-and-performance-tuning"></a>Ajuste de energia e desempenho

A eficiência energética é cada vez mais importante em ambientes corporativos e data center e adiciona outro conjunto de compensações à combinação de opções de configuração.

O Windows Server 2016 é otimizado para uma excelente eficiência de energia com impacto mínimo no desempenho em uma ampla gama de cargas de trabalho do cliente. [O ajuste do gerenciamento de energia do processador (ppm) para o plano de energia equilibrado do Windows Server](processor-power-management-tuning.md) descreve as cargas de trabalho usadas para ajustar os parâmetros padrão no windows Server 2016 e fornece sugestões para ajustes personalizados.

Esta seção expande as compensações de eficiência de energia para ajudá-lo a tomar decisões informadas se você precisar ajustar as configurações de energia padrão no servidor. No entanto, a maior parte do hardware e das cargas de trabalho do servidor não deve exigir o ajuste de energia do administrador ao executar o Windows Server 2016.

## <a name="calculating-server-energy-efficiency"></a>Calculando a eficiência de energia do servidor

Ao ajustar seu servidor para economia de energia, você também deve considerar o desempenho. O ajuste afeta o desempenho e a potência, às vezes em valores desproporcionais. Para cada possível ajuste, considere a alocação de energia e as metas de desempenho para determinar se a compensação é aceitável.

Você pode calcular a taxa de eficiência de energia do servidor para uma métrica útil que incorpora informações de energia e desempenho. A eficiência de energia é a taxa de trabalho feita para a média de energia necessária durante um período de tempo especificado.

![fórmula de eficiência de energia](../../media/perftune-guide-power-formula.png)

Você pode usar essa métrica para definir metas práticas que respeitam a compensação entre energia e desempenho. Por outro lado, uma meta de 10% de economia de energia no data center falha ao capturar os efeitos correspondentes no desempenho e vice-versa.

Da mesma forma, se você ajustar seu servidor para aumentar o desempenho em 5% e isso resultar em um consumo de energia superior a 10%, o resultado total poderá ou não ser aceitável para suas metas de negócios. A métrica de eficiência de energia permite uma tomada de decisão mais informada do que as métricas de energia ou de desempenho sozinhas.

## <a name="measuring-system-energy-consumption"></a>Medindo o consumo de energia do sistema

Você deve estabelecer uma medição de energia de linha de base antes de ajustar seu servidor para eficiência energética.

Se o seu servidor tiver o suporte necessário, você poderá usar os recursos de medição de energia e orçamento no Windows Server 2016 para exibir o consumo de energia no nível do sistema usando o monitor de desempenho.

Uma maneira de determinar se o seu servidor tem suporte para medição e orçamento é examinar o catálogo do [Windows Server](http://www.windowsservercatalog.com). Se o seu modelo de servidor se qualificar para a nova qualificação de gerenciamento de energia aprimorada no programa de certificação de hardware do Windows, será garantido que ele ofereça suporte à funcionalidade de medição e orçamento.

Outra maneira de verificar o suporte de medição é procurar manualmente os contadores no monitor de desempenho. Abra o monitor de desempenho, selecione **Adicionar contadores**e localize o grupo de contadores do **medidor de energia** .

Se as instâncias nomeadas de medidores de energia aparecerem na caixa rotulada **instâncias do objeto selecionado**, sua plataforma dará suporte à medição. O contador de **energia** que mostra a potência em watts aparece no grupo de contadores selecionado. A derivação exata do valor de dados de energia não foi especificada. Por exemplo, pode ser um desenho de energia instantânea ou uma potência média desenhada em um intervalo de tempo.

Se a sua plataforma de servidor não oferecer suporte à medição, você poderá usar um dispositivo de medição física conectado à entrada de fonte de alimentação para medir o consumo de energia ou energia do sistema.

Para estabelecer uma linha de base, você deve medir a potência média necessária em vários pontos de carga do sistema, de ocioso a 100 por cento (taxa de transferência máxima) para gerar uma linha de carga. A figura a seguir mostra as linhas de carga para três configurações de exemplo:

![linhas de carregamento de exemplo](../../media/perftune-guide-sample-loadlines.png)

Você pode usar linhas de carga para avaliar e comparar o consumo de energia e desempenho de configurações em todos os pontos de carga. Neste exemplo específico, é fácil ver qual é a melhor configuração. No entanto, pode haver cenários facilmente em que uma configuração funciona melhor para cargas de trabalho pesadas e uma funciona melhor para cargas de trabalho leves.

Você precisa compreender totalmente seus requisitos de carga de trabalho para escolher uma configuração ideal. Não presuma que, quando você encontrar uma boa configuração, ela sempre permanecerá ideal. Você deve medir a utilização do sistema e o consumo de energia regularmente e depois de alterações em cargas de trabalho, níveis de carga de trabalho ou hardware de servidor.

## <a name="diagnosing-energy-efficiency-issues"></a>Diagnosticando problemas de eficiência de energia

O **powercfg. exe** dá suporte a uma opção de linha de comando que você pode usar para analisar a eficiência de energia ociosa do seu servidor. Quando você executa o PowerCfg. exe com a opção **/Energy** , a ferramenta executa um teste de 60 segundos para detectar possíveis problemas de eficiência de energia. A ferramenta gera um relatório HTML simples no diretório atual.

> [!Important]
> Para garantir uma análise precisa, verifique se todos os aplicativos locais estão fechados antes de executar o **powercfg. exe**. 

As taxas de tiques do temporizador reduzidas, os drivers que não têm suporte ao gerenciamento de energia e a utilização excessiva da CPU são alguns dos problemas comportamentais detectados pelo comando **powercfg/Energy** . Essa ferramenta fornece uma maneira simples de identificar e corrigir problemas de gerenciamento de energia, potencialmente resultando em uma economia de custos significativa em um datacenter grande.

Para obter mais informações sobre o PowerCfg. exe, consulte [usando o Powercfg para avaliar a eficiência de energia do sistema](https://msdn.microsoft.com/windows/hardware/gg463250.aspx).

## <a name="using-power-plans-in-windows-server"></a>Usando planos de energia no Windows Server

O Windows Server 2016 tem três planos de energia internos projetados para atender a diferentes conjuntos de necessidades comerciais. Esses planos fornecem uma maneira simples de personalizar um servidor para atender às metas de energia ou desempenho. A tabela a seguir descreve os planos, lista os cenários comuns nos quais usar cada plano e fornece alguns detalhes de implementação para cada plano.

| **Plano** | **Descrição** | **Cenários comuns aplicáveis** | **Destaques da implementação** |
|------------------------|--------------------------------------------------------------------------------------------------------------------------------------------------------|----------------------------------------------------------------------------------|-----------------------------------------------------------------------------------------------------------------------------------------------------------|
| Equilibrado (recomendado) | Configuração padrão. Tem como alvo uma boa eficiência de energia com impacto mínimo no desempenho. | Computação geral | Corresponde à capacidade de demanda. Recursos de economia de energia equilibram energia e desempenho. |
| Alto Desempenho | Aumenta o desempenho ao custo de alto consumo de energia. As limitações de energia e térmica, as despesas operacionais e as considerações de confiabilidade se aplicam. | Aplicativos de baixa latência e código de aplicativo que são sensíveis a alterações de desempenho do processador | Os processadores são sempre bloqueados no estado de desempenho mais alto (incluindo frequências de "Turbo"). Todos os núcleos não são estacionados. A saída térmica pode ser significativa. |
| Economia de energia | Limita o desempenho para economizar energia e reduzir o custo operacional. Não recomendado sem testes completos para garantir que o desempenho seja adequado. | Implantações com orçamentos de energia e restrições térmicas limitadas | A frequência do processador em uma porcentagem de máximo (se houver suporte) e habilita outros recursos de economia de energia. |


Esses planos de energia existem no Windows para sistemas de corrente alternada (AC) e corrente direta (DC), mas vamos pressupor que os servidores estejam sempre usando uma fonte de energia CA.

Para obter mais informações sobre planos de energia e configurações de política de energia, consulte [configuração e implantação da política de energia no Windows](https://msdn.microsoft.com/windows/hardware/gg463243.aspx).

> [!Note]
> Alguns fabricantes de servidor têm suas próprias opções de gerenciamento de energia disponíveis por meio das configurações do BIOS. Se o sistema operacional não tiver controle sobre o gerenciamento de energia, alterar os planos de energia no Windows não afetará a energia e o desempenho do sistema.

## <a name="tuning-processor-power-management-parameters"></a>Ajustando parâmetros de gerenciamento de energia do processador

Cada plano de energia representa uma combinação de vários parâmetros de gerenciamento de energia subjacentes. Os planos internos são três coleções de configurações recomendadas que abrangem uma ampla variedade de cargas de trabalho e cenários. No entanto, reconhecemos que esses planos não atenderão às necessidades de cada cliente.

As seções a seguir descrevem maneiras de ajustar alguns parâmetros específicos de gerenciamento de energia do processador para atender às metas não abordadas pelos três planos internos. Se você precisar entender uma matriz mais ampla de parâmetros de energia, consulte [configuração e implantação da política de energia no Windows](https://msdn.microsoft.com/windows/hardware/gg463243.aspx).

## <a name="processor-performance-boost-mode"></a>Modo de aumento de desempenho do processador

As tecnologias Intel Turbo Boost e AMD Turbo CORE são recursos que permitem que os processadores alcancem o desempenho adicional quando ele é mais útil (ou seja, com altas cargas do sistema). No entanto, esse recurso aumenta o consumo de energia de núcleo da CPU, portanto, o Windows Server 2016 configura o Turbo Technologies com base na política de energia que está em uso e na implementação específica do processador.

O Turbo está habilitado para planos de energia de alto desempenho em todos os processadores Intel e AMD e está desabilitado para planos de energia de economia de energia. Para planos de energia equilibrados em sistemas que dependem do gerenciamento tradicional de frequência baseado em estado P, o Turbo será habilitado por padrão somente se a plataforma der suporte ao registro EPB.

> [!Note]
> O registro EPB tem suporte apenas nos processadores Intel Westmere e posteriores.

Para processadores Intel Nehalem e AMD, o Turbo está desabilitado por padrão nas plataformas baseadas em Estados P. No entanto, se um sistema oferecer suporte a CPPC (controle de desempenho de processador colaborativo), que é um novo modo alternativo de comunicação de desempenho entre o sistema operacional e o hardware (definido na ACPI 5,0), o Turbo poderá ser envolvido se o Windows Operating o sistema solicita dinamicamente o hardware para fornecer os níveis de desempenho mais altos possíveis.

Para habilitar ou desabilitar o recurso Turbo Boost, o parâmetro modo de aumento de desempenho do processador deve ser configurado pelo administrador ou pelas configurações de parâmetro padrão para o plano de energia escolhido. O modo de aumento de desempenho do processador tem cinco valores permitidos, conforme mostrado na tabela 5.

Para o controle baseado em estado P, as opções são desabilitadas, habilitadas (o Turbo está disponível para o hardware sempre que o desempenho nominal é solicitado) e eficiente (o Turbo estará disponível somente se o registro do EPB for implementado).

Para o controle baseado em CPPC, as opções são desabilitadas, eficientemente habilitadas (o Windows especifica a quantidade exata de Turbo a fornecer) e agressiva (o Windows solicita "desempenho máximo" para habilitar o Turbo).

No Windows Server 2016, o valor padrão para o modo Boost é 3.

| **Name** | **Comportamento baseado em estado P** | **Comportamento de CPPC** |
|--------------------------|------------------------|-------------------|
| 0 (desabilitado) | Desabilitado | Desabilitado |
| 1 (habilitado) | Enabled | Habilitado com eficiência |
| 2 (agressivo) | Enabled | Curtas |
| 3 (eficiente habilitado) | Eficiente | Habilitado com eficiência |
| 4 (agressivo eficiente) | Eficiente | Curtas |

 
Os comandos a seguir habilitam o modo de aumento de desempenho do processador no plano de energia atual (especifique a política usando um alias de GUID):

``` syntax
Powercfg -setacvalueindex scheme_current sub_processor PERFBOOSTMODE 1
Powercfg -setactive scheme_current
```

> [!Important]
> Você deve executar o comando **powercfg-SetActive** para habilitar as novas configurações. Você não precisa reinicializar o servidor.

Para definir esse valor para planos de energia diferentes do plano selecionado no momento, você pode usar aliases como o esquema\_máximo (economia de energia),\_o esquema mínimo (alto desempenho) e\_o esquema balanceado (equilibrado) no lugar do esquema\_Atual. Substitua "esquema atual" nos comandos powercfg-SetActive anteriormente mostrados com o alias desejado para habilitar esse plano de energia.

Por exemplo, para ajustar o modo de aumento no plano de economia de energia e fazer com que a economia de energia seja o plano atual, execute os seguintes comandos:

``` syntax
Powercfg -setacvalueindex scheme_max sub_processor PERFBOOSTMODE 1
Powercfg -setactive scheme_max
```

## <a name="minimum-and-maximum-processor-performance-state"></a>Estado mínimo e máximo de desempenho do processador

Os processadores mudam de acordo com os Estados de desempenho (P-States) rapidamente para corresponder à demanda, fornecendo desempenho quando necessário e economizando energia quando possível. Se o seu servidor tiver requisitos específicos de alto desempenho ou consumo mínimo de energia, considere a possibilidade de configurar o parâmetro **mínimo de desempenho do processador** ou o parâmetro de estado de **desempenho máximo do processador** .

Os valores para o **estado mínimo de desempenho do processador** e os parâmetros de **estado de desempenho máximo do processador** são expressos como um percentual da frequência máxima do processador, com um valor no intervalo de 0 a 100.

Se o servidor exigir latência ultra baixa, frequência de CPU invariável (por exemplo, para teste repetível) ou os níveis de desempenho mais altos, talvez você não queira que os processadores alternem para Estados de desempenho inferior. Para esse servidor, você pode limitar o estado mínimo de desempenho do processador em 100% usando os seguintes comandos:

``` syntax
Powercfg -setacvalueindex scheme_current sub_processor PROCTHROTTLEMIN 100
Powercfg -setactive scheme_current
```

Se o servidor exigir um consumo de energia menor, talvez você queira limitar o estado de desempenho do processador a um percentual de máximo. Por exemplo, você pode restringir o processador a 75% de sua frequência máxima usando os seguintes comandos:

``` syntax
Powercfg -setacvalueindex scheme_current sub_processor PROCTHROTTLEMAX 75
Powercfg -setactive scheme_current
```

> [!Note]
> A limitação do desempenho do processador a um percentual do máximo exige suporte do processador. Verifique a documentação do processador para determinar se esse suporte existe ou exiba o contador do monitor de desempenho **% de frequência máxima** no grupo de **processadores** para ver se as maiúsculas e minúsculas foram aplicadas.

## <a name="processor-performance-increase-and-decrease-of-thresholds-and-policies"></a>Aumento e redução de desempenho do processador de limites e políticas

A velocidade na qual um estado de desempenho do processador aumenta ou diminui é controlado por vários parâmetros. Os quatro parâmetros a seguir têm o impacto mais visível:

-   O **limite de aumento de desempenho do processador** define o valor de utilização acima do qual o estado de desempenho do processador aumentará. Valores maiores diminuem a taxa de aumento para o estado de desempenho em resposta às atividades aumentadas.

-   O **limite de redução de desempenho do processador** define o valor de utilização abaixo do qual o estado de desempenho do processador será reduzido. Valores maiores aumentam a taxa de redução para o estado de desempenho durante períodos ociosos.

-   **Aumento de desempenho do processador e redução de** desempenho do processador A política determina qual estado de desempenho deve ser definido quando ocorre uma alteração. A política "única" significa que ele escolhe o estado seguinte. "Rocket" significa o estado de desempenho máximo ou mínimo de energia. "Ideal" tenta encontrar um equilíbrio entre energia e desempenho.

Por exemplo, se o servidor exigir latência ultra baixa e ainda quiser se beneficiar de baixo consumo de energia durante períodos ociosos, você poderá acelerar o aumento do estado de desempenho para qualquer aumento na carga e reduzir a redução quando o carregamento falhar. Os comandos a seguir definem a política de aumento para "Rocket" para um aumento de estado mais rápido e definem a política de redução como "única". Os limites aumentar e diminuir são definidos como 10 e 8, respectivamente.

``` syntax
Powercfg.exe -setacvalueindex scheme_current sub_processor PERFINCPOL 2
Powercfg.exe -setacvalueindex scheme_current sub_processor PERFDECPOL 1
Powercfg.exe -setacvalueindex scheme_current sub_processor PERFINCTHRESHOLD 10
Powercfg.exe -setacvalueindex scheme_current sub_processor PERFDECTHRESHOLD 8
Powercfg.exe /setactive scheme_current
```

## <a name="processor-performance-core-parking-maximum-and-minimum-cores"></a>Núcleos máximos e mínimos de estacionamento do desempenho do processador

O estacionamento principal é um recurso que foi introduzido no Windows Server 2008 R2. O mecanismo de gerenciamento de energia do processador (PPM) e o Agendador trabalham juntos para ajustar dinamicamente o número de núcleos disponíveis para execução de threads. O mecanismo PPM escolhe um número mínimo de núcleos para os threads que serão agendados.

Os núcleos estacionados geralmente não têm nenhum thread agendado e serão descartados em Estados de energia muito baixos quando não estiverem processando interrupções, DPCs ou outro trabalho estritamente relacionados. Os núcleos restantes são responsáveis pelo restante da carga de trabalho. O estacionamento principal pode aumentar a eficiência de energia durante o uso mais baixo.

Para a maioria dos servidores, o comportamento padrão de estacionamento de núcleo fornece um equilíbrio razoável de produtividade e eficiência de energia. Em processadores em que o estacionamento principal pode não mostrar o máximo de benefícios em cargas de trabalho genéricas, ele pode ser desabilitado por padrão.

Se o seu servidor tiver requisitos de estacionamento de núcleo específicos, você poderá controlar o número de núcleos que estão disponíveis para estacionar usando o parâmetro núcleo de **desempenho do processador-máximo de núcleos de estacionamento** ou os **núcleos mínimos de estacionamento do núcleo de desempenho do processador** no Windows Server 2016.

Um cenário que o estacionamento principal nem sempre é ideal para o é quando há um ou mais threads ativos relacionados a um subconjunto não trivial de CPUs em um nó NUMA (ou seja, mais de 1 CPU, mas menos do que o conjunto inteiro de CPUs no nó). Quando o algoritmo de estacionamento principal está escolhendo núcleos para desestacionar (supondo que ocorra um aumento na intensidade da carga de trabalho), ele nem sempre pode escolher os núcleos dentro do subconjunto (ou subconjuntos) relacionados ativo para desfazer o estacionamento e, portanto, pode acabar com o estacionamento de núcleos que não serão realmente utilizados.

Os valores para esses parâmetros são porcentagens no intervalo de 0 a 100. O parâmetro de **núcleos de estacionamento máximo de núcleos de desempenho do processador** controla o percentual máximo de núcleos que podem ser desestacionados (disponíveis para executar threads) a qualquer momento, enquanto o parâmetro **núcleos mínimos do estacionamento do núcleo de desempenho do processador** controla o percentual mínimo de núcleos que podem ter o estacionamento anulado. Para desativar o estacionamento principal, defina o parâmetro núcleos **mínimos de estacionamento do núcleo de desempenho do processador** como 100 por cento usando os seguintes comandos:

``` syntax
Powercfg -setacvalueindex scheme_current sub_processor CPMINCORES 100
Powercfg -setactive scheme_current
```

Para reduzir o número de núcleos de agendáveis a 50% da contagem máxima, defina o parâmetro de **núcleos de estacionamento máximo de núcleos de desempenho do processador** como 50 da seguinte maneira:

``` syntax
Powercfg -setacvalueindex scheme_current sub_processor CPMAXCORES 50
Powercfg -setactive scheme_current
```

## <a name="processor-performance-core-parking-utility-distribution"></a>Distribuição do utilitário de estacionamento do núcleo de desempenho do processador

A distribuição de utilitários é uma otimização de algoritmo no Windows Server 2016 que foi projetada para melhorar a eficiência de energia para algumas cargas de trabalho. Ele acompanha a atividade de CPU não-móvel (ou seja, DPCs, interrupções ou rigorosamente relacionados threads) e prevê o trabalho futuro em cada processador com base na suposição de que qualquer trabalho móvel pode ser distribuído igualmente entre todos os núcleos sem estacionamento.

A distribuição do utilitário é habilitada por padrão para o plano de energia equilibrado para alguns processadores. Ele pode reduzir o consumo de energia do processador reduzindo as frequências de CPU solicitadas das cargas de trabalho que estão em um estado razoavelmente estável. No entanto, a distribuição do utilitário não é necessariamente uma boa opção de algoritmos para cargas de trabalho sujeitas a grandes picos de atividade ou para programas em que a carga de trabalho muda de forma rápida e aleatória entre os processadores.

Para essas cargas de trabalho, é recomendável desabilitar a distribuição do utilitário usando os seguintes comandos:

``` syntax
Powercfg -setacvalueindex scheme_current sub_processor DISTRIBUTEUTIL 0
Powercfg -setactive scheme_current
```

## <a name="see-also"></a>Consulte também
- [Considerações de desempenho de hardware do servidor](../index.md)
- [Server Hardware Power Considerations](../power.md) (Considerações de energia de hardware do servidor)
- [Processor Power Management Tuning](processor-power-management-tuning.md) (Ajuste de gerenciamento de energia do processador)
- [Parâmetros de plano balanceado recomendados](recommended-balanced-plan-parameters.md)
