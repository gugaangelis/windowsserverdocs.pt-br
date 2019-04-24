---
title: Capacidade e desempenho de ajuste
description: Gerenciamento de energia do processador (PPM) de ajuste para o plano de energia Equilibrado do Windows Server
ms.prod: windows-server-threshold
ms.technology: performance-tuning-guide
ms.topic: article
ms.author: Qizha;TristanB
author: phstee
ms.date: 10/16/2017
ms.openlocfilehash: 91bc02e5edbdfbbbf3ccf600f3536a783e49eb79
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59814907"
---
# <a name="power-and-performance-tuning"></a>Energia e ajuste de desempenho

Eficiência de energia é cada vez mais importante em ambientes de centro de empresa e dados, e ele adiciona outro conjunto de vantagens e desvantagens na combinação de opções de configuração.

Windows Server 2016 é otimizado para eficiência de energia excelente com o impacto de desempenho mínimo em uma ampla variedade de cargas de trabalho do cliente. [Ajuste de gerenciamento de energia (PPM) do processador para o Windows Server com balanceamento de plano de energia](processor-power-management-tuning.md) descreve as cargas de trabalho usadas para ajustar os parâmetros padrão no Windows Server 2016 e fornece sugestões para tunings personalizado.

Esta seção expande as compensações de eficiência de energia para ajudá-lo a tomar decisões informadas, se você precisar ajustar as configurações de energia padrão em seu servidor. No entanto, a maioria das cargas de trabalho e hardware de servidor não deve exigir a otimização de energia administrador ao executar o Windows Server 2016.

## <a name="calculating-server-energy-efficiency"></a>Calculando a eficiência de energia do servidor

Quando você ajusta o servidor para economizar energia, você também deve considerar o desempenho. Ajuste afeta o desempenho e energia, às vezes, em valores desproporcional. Para cada ajuste possível, considere suas metas de desempenho e orçamento de energia para determinar se a compensação é aceitável.

Você pode calcular a taxa de eficiência de energia do seu servidor de uma métrica útil que incorpora informações de capacidade e desempenho. Eficiência de energia é a proporção do trabalho realizado para o de energia médio que é necessário durante um período de tempo especificado.

![fórmula de eficiência de energia](../../media/perftune-guide-power-formula.png)

Você pode usar essa métrica para definir os objetivos práticos que respeitam a compensação entre desempenho e energia. Em contraste, uma meta de 10 por cento de economia de energia no data center não pode capturar os efeitos correspondentes no desempenho e vice-versa.

Da mesma forma, se você ajustar o servidor para aumentar o desempenho por 5 por cento e que resulta em 10 por cento maior consumo de energia, o resultado total pode ou não pode ser aceitável para suas metas de negócios. Permite que a métrica de eficiência de energia para a tomada de decisão bem informada de métricas de desempenho ou de energia sozinhas.

## <a name="measuring-system-energy-consumption"></a>Medir o consumo de energia do sistema

Você deve estabelecer uma medida de alimentação de linha de base antes de você ajusta o servidor para a eficiência de energia.

Se o servidor tem o suporte necessário, você pode usar o poder de medição e orçamento de recursos no Windows Server 2016 para exibir o consumo de energia de nível de sistema usando o Monitor de desempenho.

É uma maneira de determinar se o servidor tem suporte para a medição e orçamento é examinar o [catálogo do Windows Server](http://www.windowsservercatalog.com). Se seu modelo de servidor se qualificar para a qualificação de gerenciamento de energia aprimorado de novo no programa de certificação de Hardware do Windows, é garantido que ele oferece suporte à funcionalidade de medição e orçamento.

Outra maneira de verificar se há suporte de medição é manualmente, procure os contadores no Monitor de desempenho. Abra o Monitor de desempenho, selecione **adicionar contadores**e, em seguida, localize o **medidor de energia** grupo de contadores.

Se as instâncias nomeadas do medidores de energia são exibidos na caixa rotulada **instâncias de objeto selecionado**, sua plataforma dá suporte à medição. O **energia** contador que mostra a potência em watts aparece no grupo de contador selecionado. A derivação exata do valor de dados de energia não está especificada. Por exemplo, é possível um consumo de energia instantânea ou um desenho de energia médio por algum intervalo de tempo.

Se sua plataforma de servidor não oferece suporte a medição, você pode usar um dispositivo de medição físico conectado à entrada de fornecimento de energia para medir o consumo de energia ou de desenho de energia do sistema.

Para estabelecer uma linha de base, você deve medir o necessário em vários pontos de carga do sistema, do estado ocioso a 100 por cento (taxa de transferência máxima) para gerar uma linha de carga de energia médio. A figura a seguir mostra linhas de carga para três exemplos de configurações:

![linhas de carga de exemplo](../../media/perftune-guide-sample-loadlines.png)

Você pode usar linhas de carga para avaliar e comparar o desempenho e consumo de energia de configurações em todos os pontos de carga. Nesse exemplo específico, é fácil ver o que é a melhor configuração. No entanto, com facilidade pode haver situações em que uma configuração funciona melhor para cargas de trabalho pesadas e funciona melhor para cargas de trabalho leves.

Você precisa compreender totalmente os requisitos de carga de trabalho para escolher uma configuração ideal. Não presuma que quando você encontrar uma configuração válida, ela sempre permanecerá ideal. Você deve medir o consumo de utilização de energia do sistema regularmente e depois das alterações em cargas de trabalho, os níveis de carga de trabalho ou hardware de servidor.

## <a name="diagnosing-energy-efficiency-issues"></a>Diagnosticando problemas de eficiência de energia

**PowerCfg.exe** dá suporte a uma opção de linha de comando que você pode usar para analisar a eficiência de energia ociosa do seu servidor. Quando você executa PowerCfg.exe com o **/energy** opção, a ferramenta executa um teste de 60 segundos para detectar problemas de eficiência de energia potencial. A ferramenta gera um relatório HTML simples no diretório atual.

>[!Important]
> Para garantir uma análise precisa, certifique-se de que todos os aplicativos locais estão fechados antes de executar **PowerCfg.exe**. 

Reduzido a taxas de tique do temporizador, drivers que falta suporte ao gerenciamento de energia e utilização excessiva da CPU são alguns dos problemas de comportamento que são detectados pelo **/energy powercfg** comando. Essa ferramenta fornece uma maneira simples de identificar e corrigir problemas de gerenciamento de energia, possivelmente resultando em economia de custo significativa em um datacenter grande.

Para obter mais informações sobre PowerCfg.exe, consulte [usando a PowerCfg para avaliar a eficiência de energia do sistema](https://msdn.microsoft.com/windows/hardware/gg463250.aspx).

## <a name="using-power-plans-in-windows-server"></a>Usando planos de energia no Windows Server

Windows Server 2016 tem três planos de energia internos, projetados para atender às diferentes conjuntos de necessidades de negócios. Esses planos fornecem uma maneira simple de personalizar um servidor para atender às metas de desempenho ou de energia. A tabela a seguir descreve os planos, lista os cenários comuns nos quais cada plano de usar e fornece alguns detalhes de implementação para cada plano.

| **Plano** | **Descrição** | **Cenários aplicáveis comuns** | **Destaques da implementação** |
|------------------------|--------------------------------------------------------------------------------------------------------------------------------------------------------|----------------------------------------------------------------------------------|-----------------------------------------------------------------------------------------------------------------------------------------------------------|
| Balanceamento (recomendado) | Configuração padrão. Eficiência de energia bons destinos com impacto mínimo no desempenho. | Geral de computação | Corresponde a capacidade à demanda. Recursos de economia de energia equilibrar o desempenho e energia. |
| Alto desempenho | Aumenta o desempenho às custas de consumo de energia alta. Energia e térmicas limitações, considerações sobre a confiabilidade e despesas operacionais se aplicam. | Aplicativos de baixa latência e o código do aplicativo que é sensível às alterações de desempenho do processador | Processadores sempre estão bloqueados no estado de desempenho mais alto (incluindo "turbo? frequências). Todos os núcleos são unparked. Saída térmica poderá ser significativa. |
| Economia de energia | Limita o desempenho de economizar energia e reduzir os custos operacionais. Não é recomendável sem um teste completo tornar-se de desempenho é adequado. | Implantações com orçamentos de energia limitado e restrições térmicas | Limita a frequência do processador em uma porcentagem de máximo (se houver suporte) e permite que outros recursos de economia de energia. |


Esses planos de energia existem no Windows corrente alternada (AC) e sistemas com fonte de alimentação de CC (corrente), mas vamos pressupor que servidores sempre estão usando uma fonte de alimentação de CA.

Para obter mais informações sobre planos de energia e configurações de política de energia, consulte [configuração de política de energia e a implantação no Windows](https://msdn.microsoft.com/windows/hardware/gg463243.aspx).

>[!Note]
> Alguns fabricantes de servidor têm suas próprias opções de gerenciamento de energia disponíveis por meio das configurações de BIOS. Se o sistema operacional não tem controle sobre o gerenciamento de energia, os planos de energia no Windows a alteração não afetará desempenho e energia do sistema.

## <a name="tuning-processor-power-management-parameters"></a>Ajuste os parâmetros de gerenciamento de energia do processador

Cada plano de energia representa uma combinação de vários parâmetros de gerenciamento de energia subjacente. Os planos internos são três conjuntos de configurações recomendadas que abrangem uma ampla variedade de cenários e cargas de trabalho. No entanto, reconhecemos que esses planos não atendem às necessidades de cada cliente.

As seções a seguir descrevem as maneiras de ajustar alguns parâmetros de gerenciamento de energia processador específico para atender às metas a não solucionadas pela três planos internos. Se você precisa entender uma gama de parâmetros de energia, consulte [configuração de política de energia e a implantação no Windows](https://msdn.microsoft.com/windows/hardware/gg463243.aspx).

## <a name="processor-performance-boost-mode"></a>Modo de aumento de desempenho do processador

Tecnologias Intel Turbo Boost e AMD Turbo CORE são recursos que permitem que os processadores alcançar desempenho adicionais quando ele é mais útil (ou seja, no sistema de alta carrega). No entanto, esse recurso aumenta o consumo de energia de núcleos de CPU, para que o Windows Server 2016 configura tecnologias Turbo com base na política de energia está em uso e a implementação de processador específico.

O Turbo está habilitado para planos de energia de alto desempenho em todos os processadores Intel e AMD e ele está desabilitado para planos de energia de economia de energia. Para planos de energia Equilibrado em sistemas que se baseiam no gerenciamento de frequência tradicional baseado em estado de P, Turbo é habilitado por padrão, somente se a plataforma oferece suporte o registro EPB.

>[!Note]
> O registro EPB só é suportado no Intel Westmere e processadores mais adiante.

Para os processadores Intel Nehalem e AMD, Turbo está desabilitado por padrão em plataformas com base no estado P. No entanto, se um sistema der suporte ao controle do desempenho de processador colaborativa (CPPC), que é um novo modo alternativo de comunicação de desempenho entre o sistema operacional e o hardware (definido na versão 5.0 do ACPI), Turbo pode ser ativado se a operação do Windows dinamicamente, o sistema solicita o hardware para fornecer os mais altos níveis de desempenho possível.

Para habilitar ou desabilitar o recurso Turbo Boost, o parâmetro de modo de aumento de desempenho do processador deve ser configurado pelo administrador ou pelas configurações de parâmetro padrão para o plano de energia escolhido. Modo de aumento de desempenho do processador tem cinco valores permitidos, conforme mostrado na tabela 5.

Para um controle com base no estado P, as opções são desabilitadas, habilitado (Turbo está disponível para o hardware, sempre que for solicitado a desempenho nominal) e eficiente (Turbo está disponível somente se o registro EPB é implementado).

Para um controle com base em CPPC, as opções são desabilitadas, habilitado eficiente (Windows Especifica o valor exato do Turbo para fornecer) e agressivo (Windows perguntará "máximo desempenho? Para habilitar Turbo).

No Windows Server 2016, o valor padrão para o modo de aumento é 3.

| **Nome** | **Comportamento com base no estado P** | **Comportamento CPPC** |
|--------------------------|------------------------|-------------------|
| 0 (desabilitado) | Desabilitada | Desabilitada |
| 1 (habilitado) | Enabled | Eficiente habilitado |
| 2 (agressiva) | Enabled | Agressiva |
| 3 (eficiente habilitado) | Eficiente | Eficiente habilitado |
| 4 (eficiente agressivo) | Eficiente | Agressiva |

 
Os comandos a seguir habilitar o modo de aumento de desempenho do processador no plano de energia atual (especificar a política por meio de um alias GUID):

``` syntax
Powercfg -setacvalueindex scheme_current sub_processor PERFBOOSTMODE 1
Powercfg -setactive scheme_current
```

>[!Important]  Você deve executar o **powercfg - setactive** comando para habilitar as novas configurações. Não é necessário reinicializar o servidor.

Para definir esse valor para planos de energia que não seja o plano selecionado no momento, você pode usar aliases como esquema\_máximo (economia de energia), o esquema de\_MIN (alto desempenho) e o esquema\_EQUILIBRADO (equilibrado) no lugar do esquema\_Atual. Substituir "esquema atual? nos comandos - setactive powercfg mostrados anteriormente com o alias desejado para habilitar esse plano de energia.

Por exemplo ajustar o modo de aumento no plano de economia de energia e fazer essa economia de energia é o plano atual, execute os seguintes comandos:

``` syntax
Powercfg -setacvalueindex scheme_max sub_processor PERFBOOSTMODE 1
Powercfg -setactive scheme_max
```

## <a name="minimum-and-maximum-processor-performance-state"></a>Mínimo e o estado de desempenho máximo do processador

Processadores mudam entre os estados de desempenho (P-states) muito rapidamente à fonte de correspondência para solicitar, fornecendo desempenho quando necessário e economia de energia quando possível. Se seu servidor tiver requisitos específicos de alto desempenho ou consumo de energia mínimo, você pode considerar a configuração do **estado de desempenho do processador mínimo** parâmetro ou o **máximo desempenho de processador Estado** parâmetro.

Os valores para o **estado de desempenho do processador mínimo** e **estado de desempenho do processador máximo** os parâmetros são expressos como uma porcentagem de frequência máxima do processador, com um valor no intervalo 0 – 100.

Se o servidor exigir latência extremamente baixa, frequência de CPU invariável (por exemplo, para testes repetíveis) ou os mais altos níveis de desempenho, você talvez não queira os processadores alternando para estados de desempenho inferior. Para o servidor, você pode limitar o estado de desempenho do processador mínimo 100 por cento, usando os comandos a seguir:

``` syntax
Powercfg -setacvalueindex scheme_current sub_processor PROCTHROTTLEMIN 100
Powercfg -setactive scheme_current
```

Se o servidor exigir menor consumo de energia, você talvez queira limitar o estado de desempenho do processador em uma porcentagem de máximo. Por exemplo, você pode restringir o processador de até 75% de sua frequência máxima, usando os comandos a seguir:

``` syntax
Powercfg -setacvalueindex scheme_current sub_processor PROCTHROTTLEMAX 75
Powercfg -setactive scheme_current
```

>[!Note]
> Limitar o desempenho do processador em uma porcentagem de máximo requer suporte do processador. Consulte a documentação do processador para determinar se esse suporte existe, ou exibir o contador de desempenho do sistema **% de frequência máxima** na **processador** grupo para ver se qualquer caps frequência foram aplicado.

## <a name="processor-performance-increase-and-decrease-of-thresholds-and-policies"></a>Desempenho do processador aumentam e diminuem de limites e políticas

A velocidade na qual um estado de desempenho do processador aumenta ou diminui é controlada por vários parâmetros. Os seguintes quatro parâmetros têm o impacto mais visível:

-   **Aumentar o limite de desempenho do processador** define o valor da utilização acima que aumentará o estado de desempenho do processador. Valores maiores diminuir a taxa de aumento para o estado de desempenho em resposta a atividades de aumento.

-   **Limite de diminuir o desempenho do processador** define o valor da utilização abaixo que diminuirá o estado de desempenho do processador. Valores maiores aumentam a taxa de redução para o estado de desempenho durante períodos ociosos.

-   **Política de aumentar o desempenho de processador e diminuir o desempenho do processador** política determinar qual estado de desempenho deve ser definido quando ocorre uma alteração. "Único? diretiva significa que ele escolhe o próximo estado. "Rocket? significa que o estado de desempenho de energia máximo ou mínimo. "Ideal? tenta encontrar um equilíbrio entre desempenho e energia.

Por exemplo, se o servidor exigir latência extremamente baixa, apesar de desejarem ainda se beneficiar de baixo consumo de energia durante períodos ociosos, poderia quicken o aumento de estado de desempenho para qualquer aumento de carga e lenta a diminuição quando carga fica inativo. Os comandos a seguir definem a diretiva de aumento como "Rocket? para um estado mais rápido, aumentar e definir a política de diminuição para "único?. Os limites de aumento e diminuição são definidos como 10 e 8, respectivamente.

``` syntax
Powercfg.exe -setacvalueindex scheme_current sub_processor PERFINCPOL 2
Powercfg.exe -setacvalueindex scheme_current sub_processor PERFDECPOL 1
Powercfg.exe -setacvalueindex scheme_current sub_processor PERFINCTHRESHOLD 10
Powercfg.exe -setacvalueindex scheme_current sub_processor PERFDECTHRESHOLD 8
Powercfg.exe /setactive scheme_current
```

## <a name="processor-performance-core-parking-maximum-and-minimum-cores"></a>Núcleo do processador desempenho mínimos e máximo de núcleos de estacionamento

Estacionamento do núcleo é um recurso que foi introduzido no Windows Server 2008 R2. O mecanismo PPM (gerenciamento) de energia do processador e o Agendador trabalham juntos para ajustar dinamicamente o número de núcleos que estão disponíveis para executar threads. O mecanismo de PPM escolhe um número mínimo de núcleos para os threads que serão agendados.

Núcleos estão estacionados geralmente não têm nenhum thread agendado, e eles descartará em estados de muito baixa energia quando eles não estão processando interrupções, DPCs ou outro trabalho estritamente afinidade. Os núcleos restantes são responsáveis pelo restante da carga de trabalho. Estacionamento do núcleo potencialmente pode aumentar a eficiência de energia durante o uso mais baixo.

Para a maioria dos servidores, o comportamento de estacionamento do núcleo padrão fornece um equilíbrio razoável de taxa de transferência e a eficiência energética. Em processadores onde estacionamento do núcleo pode não mostrar o máximo benefício em cargas de trabalho genéricas, ele pode ser desabilitado por padrão.

Se seu servidor tiver requisitos de estacionamento de núcleo específico, você pode controlar o número de núcleos que estão disponíveis para deixar usando o **desempenho Core estacionamento máximo núcleos de processador** parâmetro ou o **processador Desempenho Core estacionamento mínimo de núcleos de** parâmetro no Windows Server 2016.

Um cenário que estacionamento do núcleo nem sempre é ideal para é quando há um ou mais threads ativos relacionados a um subconjunto não trivial de CPUs em um nó NUMA (ou seja, mais de 1 CPU, mas menor do que o conjunto inteiro de CPUs no nó). Quando o algoritmo de estacionamento do núcleo é de separação de núcleos para unpark (supondo um aumento na intensidade da carga de trabalho ocorre), ele não pode escolher os núcleos dentro do subconjunto de afinidade ativo (ou subconjuntos) unpark sempre e, portanto, talvez acaba unparking núcleos que, na verdade, não será utilizado.

Os valores para esses parâmetros são percentuais no intervalo de 0 a 100. O **desempenho Core estacionamento máximo núcleos de processador** parâmetro controla o percentual máximo de núcleos que podem ser unparked (disponível para executar threads) a qualquer momento, enquanto o **principal de desempenho do processador de estacionamento Núcleos mínimos** parâmetro controla o percentual mínimo de núcleos que podem ser unparked. Para desativar estacionamento do núcleo, defina as **processador desempenho Core estacionamento mínimo de núcleos** parâmetro até 100 por cento, usando os comandos a seguir:

``` syntax
Powercfg -setacvalueindex scheme_current sub_processor CPMINCORES 100
Powercfg -setactive scheme_current
```

Para reduzir o número de núcleos agendáveis para 50% da contagem máxima, defina as **desempenho Core estacionamento máximo núcleos de processador** parâmetro como 50, da seguinte maneira:

``` syntax
Powercfg -setacvalueindex scheme_current sub_processor CPMAXCORES 50
Powercfg -setactive scheme_current
```

## <a name="processor-performance-core-parking-utility-distribution"></a>Núcleo de desempenho de processador, distribuição de utilitário de estacionamento

Utilitário de distribuição é uma otimização algorítmica no Windows Server 2016 que foi projetado para melhorar a eficiência de energia para algumas cargas de trabalho. Ele rastreia imóvel atividade da CPU (ou seja, DPCs, interrupções ou estritamente afinidade de threads), e ele prevê o trabalho futuro em cada processador com base na suposição de que qualquer trabalho móvel pode ser distribuído igualmente entre todos os núcleos unparked.

Utilitário de distribuição é habilitado por padrão para o plano de energia Equilibrado para alguns processadores. Ela pode reduzir o consumo de energia do processador, reduzindo as frequências de CPU solicitadas de cargas de trabalho que estão em um estado razoavelmente estável. No entanto, o utilitário de distribuição não é necessariamente uma boa opção algorítmica para cargas de trabalho que estão sujeitos a picos de alta atividade ou para programas em que a carga de trabalho rápida e aleatoriamente desloca entre processadores.

Para essas cargas de trabalho, é recomendável desabilitando a distribuição de utilitário, usando os comandos a seguir:

``` syntax
Powercfg -setacvalueindex scheme_current sub_processor DISTRIBUTEUTIL 0
Powercfg -setactive scheme_current
```

## <a name="see-also"></a>Consulte também
- [Considerações de desempenho de Hardware do servidor](../index.md)
- [Considerações de energia de Hardware do servidor](../power.md)
- [Ajuste de gerenciamento de energia do processador](processor-power-management-tuning.md)
- [Parâmetros de plano com balanceamento de recomendado](recommended-balanced-plan-parameters.md)
