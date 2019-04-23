---
title: Recomendado parâmetros de plano de energia Equilibrado para tempos de resposta rápidos
description: Recomendado parâmetros de plano de energia Equilibrado para tempo de resposta rápida
ms.prod: windows-server-threshold
ms.technology: performance-tuning-guide
ms.topic: article
ms.author: Qizha;TristanB
author: phstee
ms.date: 10/16/2017
ms.openlocfilehash: 134e868e1400729f754039fc8120cea0c73945bf
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59878787"
---
# <a name="recommended-balanced-power-plan-parameters-for-workloads-requiring-quick-response-times"></a>Recomendado parâmetros de plano de energia Equilibrado para cargas de trabalho que exigem tempos de resposta rápidos

O padrão **equilibrado** usa do plano de energia **taxa de transferência** como a métrica de desempenho para o ajuste. Durante o estado estável, **taxa de transferência** não muda com as diferentes utilizações até que o sistema esteja totalmente sobrecarregado (~ 100% de utilização).  Como resultado, o **equilibrado** power bastante com minimizando a frequência do processador e maximizar a utilização favorece o plano de energia.

No entanto **tempo de resposta** poderia aumentar exponencialmente com aumentos de utilização. Hoje em dia, o requisito de tempo de resposta rápida foi intensamente aprimorado. Embora a Microsoft sugeridas para alternar para os usuários a **de alto desempenho** plano de energia quando eles precisarem de tempo de resposta rápido, alguns usuários não desejam perder o benefício de energia durante a luz para níveis de carga média. Portanto, a Microsoft fornece o seguinte conjunto de alterações de parâmetro sugeridos para as cargas de trabalho que exigem o tempo de resposta rápida.


| Parâmetro | Descrição | Valor padrão | Proposta de valor |
|------------------------|--------------------------------------------------------------------------------------------------------------------------------------------------------|----------------------------------------------------------------------------------|-----------------------------------------------------------------------------------------------------------------------------------------------------------|
| Limite de aumento de desempenho do processador | Limite de utilização acima que é aumentar a frequência | 90 | 60 |
| Limite de redução de desempenho do processador | Limite de utilização abaixo que a frequência é diminuir | 80 | 40 |
| Tempo de aumento de desempenho do processador | Número de janelas de seleção PPM antes que a frequência é aumentar | 3 | 1 |
| Política de aumento de desempenho do processador | Quão rápido a frequência é aumentar | Único | Ideal |

Para definir os valores propostos, os usuários podem executar os comandos a seguir em uma janela com o administrador:

``` syntax
Powercfg -setacvalueindex scheme_balanced sub_processor PERFINCTHRESHOLD 60
Powercfg -setacvalueindex scheme_balanced sub_processor PERFDECTHRESHOLD 40
Powercfg -setacvalueindex scheme_balanced sub_processor PERFINCTIME 1
Powercfg -setacvalueindex scheme_balanced sub_processor PERFINCPOL 0
Powercfg -setactive scheme_balanced
```

Essa alteração se baseia o desempenho e a análise de compensação de energia usando as seguintes cargas de trabalho. Para os usuários que desejam mais bem ajustar a eficiência de energia com a certos requisitos de SLA, consulte o [considerações sobre desempenho de Hardware de servidor](../power.md).

>[!Note]
> Para obter recomendações adicionais e informações sobre como aproveitar os planos de energia para ajustar cargas de trabalho virtualizadas, leia [configuração do Hyper-v](../../role/hyper-v-server/configuration.md)

## <a name="specpower--java-workload"></a>SPECpower – carga de trabalho do JAVA

[SPECpower\_ssj2008](http://spec.org/power_ssj2008/), mais popular benchmark de especificações de padrão da indústria para características de capacidade e desempenho do servidor, é usado para verificar o impacto de energia. Uma vez que ela usa apenas **taxa de transferência** como a métrica de desempenho, o padrão **equilibrado** plano de energia fornece a melhor eficiência de energia.

A alteração proposta parâmetro consome energia ligeiramente mais alta na luz (ou seja, < = % 20) níveis de carga. Mas com o mais alto nível, carrega a diferença aumenta, e ele começa a consumir energia mesma como o **de alto desempenho** plano de energia depois do nível de carga de 60%. Para usar os parâmetros da alteração proposta, os usuários devem estar atento o custo de energia em média a alta carga níveis durante seu planejamento de capacidade do rack.

## <a name="geekbench-3"></a>GeekBench 3

[3 GeekBench](http://www.geekbench.com/geekbench3/) é um parâmetro de comparação de processador de plataforma cruzada que separa as pontuações de desempenho de núcleo único e vários núcleo. Ele simula um conjunto de cargas de trabalho incluindo cargas de trabalho inteiro (criptografias, compressões, processamento de imagens, etc.), cargas de ponto flutuante (modelagem, fractal, imagem nitidez, Desfoque da imagem, etc.) e cargas de trabalho de memória (streaming).

**Tempo de resposta** é uma medida principal em seu cálculo de pontuação. Em nosso sistema testado, o padrão **equilibrado** plano de energia tem cerca de 18% regressão na regressão de testes e cerca de 40% de núcleo único em testes de vários núcleos em comparação com o **alto desempenho** plano de energia. A alteração proposta remove essas regressões.

## <a name="diskspd"></a>DiskSpd

[Diskspd](https://en.wikipedia.org/wiki/Diskspd) é uma ferramenta de linha de comando para armazenamento de benchmark desenvolvida pela Microsoft. Ele é amplamente usado para gerar uma variedade de solicitações em relação a sistemas de armazenamento para a análise de desempenho de armazenamento.

Podemos configurar um [Cluster de Failover] e usado Diskspd para gerar aleatório e sequencial e leitura e/SS de gravação para os sistemas de armazenamento local e remoto com diferentes tamanhos de e/s. Nossos testes mostram que o tempo de resposta de e/s é muito sensível a frequência do processador em planos de energia diferente. O **equilibrado** plano de energia pode dobrar o tempo de resposta do que da **alto desempenho** plano de energia em determinadas cargas de trabalho. A alteração proposta remove a maioria das regressões.

>[!Important]
>A partir da processadores Intel [Broadwell] executando o Windows Server 2016, com muita frequência as decisões de gerenciamento de energia do processador é feitas no processador, em vez de nível de sistema operacional para atingir mais rápida adaptação às alterações de carga de trabalho. Os parâmetros PPM herdados usados pelo sistema operacional tenham impacto mínimo sobre as decisões de frequência real, exceto informando que o processador se deve favorecer força ou o desempenho ou limitar as frequências de mínimo e máxima. Portanto, a alteração do parâmetro PPM proposta tem como destino apenas para os sistemas de pré-Broadwell.

## <a name="see-also"></a>Consulte também
- [Considerações de desempenho de Hardware do servidor](../index.md)
- [Considerações de energia de Hardware do servidor](../power.md)
- [Energia e ajuste de desempenho](power-performance-tuning.md)
- [Ajuste de gerenciamento de energia do processador](processor-power-management-tuning.md)
- [Cluster de failover](https://technet.microsoft.com/library/cc725923.aspx)
