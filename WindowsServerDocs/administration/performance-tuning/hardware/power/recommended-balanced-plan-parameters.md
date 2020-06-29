---
title: Parâmetros de plano de energia balanceados recomendados para tempos de resposta rápidos
description: Parâmetros de plano de energia balanceados recomendados para tempos de resposta rápidos
ms.prod: windows-server
ms.technology: performance-tuning-guide
ms.topic: conceptual
ms.author: qizha;tristanb
author: phstee
ms.date: 10/16/2017
ms.openlocfilehash: 62dc6168e76bf3951443df0f06c47a8684d2df26
ms.sourcegitcommit: 771db070a3a924c8265944e21bf9bd85350dd93c
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/27/2020
ms.locfileid: "85471581"
---
# <a name="recommended-balanced-power-plan-parameters-for-workloads-requiring-quick-response-times"></a>Parâmetros de plano de energia balanceados recomendados para cargas de trabalho que exigem tempos de resposta rápidos

O plano de energia **equilibrado** padrão usa a **taxa de transferência** como a métrica de desempenho para ajuste. Durante o estado estacionário, a **taxa de transferência** não é alterada com utilizações variadas até que o sistema esteja totalmente sobrecarregado (aproximadamente 100% de utilização).  Como resultado, o plano de energia **equilibrado** favorece muito a potência com a minimização da frequência do processador e a maximização da utilização.

No entanto, o **tempo de resposta** pode aumentar exponencialmente com aumentos de utilização. Hoje em dia, o requisito de tempo de resposta rápido aumentou drasticamente. Embora a Microsoft tenha sugerido os usuários para alternar para o plano de energia de **alto desempenho** quando eles precisam de um tempo de resposta rápido, alguns usuários não querem perder o benefício de energia durante os níveis de carga leve a médio. Portanto, a Microsoft fornece o seguinte conjunto de alterações de parâmetro sugeridas para as cargas de trabalho que exigem tempo de resposta rápido.


| Parâmetro | Descrição | Valor Padrão | Valor proposto |
|------------------------|--------------------------------------------------------------------------------------------------------------------------------------------------------|----------------------------------------------------------------------------------|-----------------------------------------------------------------------------------------------------------------------------------------------------------|
| Limite de aumento de desempenho do processador | Limite de utilização acima do qual a frequência deve aumentar | 90 | 60 |
| Limite de redução de desempenho do processador | Limite de utilização abaixo do qual a frequência é reduzida | 80 | 40 |
| Tempo de aumento de desempenho do processador | Número de janelas de verificação de PPM antes que a frequência seja aumentada | 3 | 1 |
| Política de aumento de desempenho do processador | Quão rápido a frequência deve aumentar | Single | Ideal |

Para definir os valores propostos, os usuários podem executar os seguintes comandos em uma janela com o administrador:

``` syntax
Powercfg -setacvalueindex scheme_balanced sub_processor PERFINCTHRESHOLD 60
Powercfg -setacvalueindex scheme_balanced sub_processor PERFDECTHRESHOLD 40
Powercfg -setacvalueindex scheme_balanced sub_processor PERFINCTIME 1
Powercfg -setacvalueindex scheme_balanced sub_processor PERFINCPOL 0
Powercfg -setactive scheme_balanced
```

Essa alteração se baseia na análise do desempenho e da compensação de energia usando as cargas de trabalho a seguir. Para os usuários que desejam ajustar ainda mais a eficiência de energia com determinados requisitos de SLA, consulte [considerações de desempenho de hardware do servidor](../power.md).

>[!Note]
> Para obter recomendações adicionais e informações sobre como aproveitar os planos de energia para ajustar cargas de trabalho virtualizadas, leia [configuração do Hyper-v](../../role/hyper-v-server/configuration.md)

## <a name="specpower--java-workload"></a>SPECpower – carga de trabalho JAVA

[SPECpower \_ ssj2008](http://spec.org/power_ssj2008/), o benchmark de especificação padrão da indústria mais popular para características de energia e desempenho do servidor, é usado para verificar o impacto de energia. Como ele usa apenas a **taxa de transferência** como métrica de desempenho, o plano de energia **equilibrado** padrão fornece a melhor eficiência de energia.

A alteração de parâmetro proposta consome uma potência ligeiramente mais alta na luz (ou seja, <= 20%) níveis de carga. Mas com o nível de carga mais alto, a diferença aumenta e começa a consumir a mesma potência que o plano de energia de **alto desempenho** após o nível de carga de 60%. Para usar os parâmetros de alteração propostos, os usuários devem estar cientes do custo de energia em níveis de carga médio a alto durante o planejamento de capacidade de seu rack.

## <a name="geekbench-3"></a>GeekBench 3

[GeekBench 3](http://www.geekbench.com/geekbench3/) é um benchmark de processador de plataforma cruzada que separa as pontuações para o desempenho de núcleo único e vários núcleos. Ele simula um conjunto de cargas de trabalho, incluindo cargas de trabalho de inteiros (criptografias, compactações, processamento de imagens, etc.), cargas de trabalho de ponto flutuante (modelagem, fractal, nitidez de imagem, Desfoque de imagem, etc.) e cargas de trabalho de memória (streaming).

O **tempo de resposta** é uma medida principal em seu cálculo de pontuação. Em nosso sistema testado, o plano de energia **equilibrado** padrão tem aproximadamente 18% de regressão em testes de núcleo único e ~ 40% de regressão em testes de vários núcleos em comparação com o plano de energia de **alto desempenho** . A alteração proposta remove essas regressões.

## <a name="diskspd"></a>DiskSpd

[Diskspd](https://en.wikipedia.org/wiki/Diskspd) é uma ferramenta de linha de comando para o benchmark de armazenamento desenvolvido pela Microsoft. Ele é amplamente usado para gerar uma variedade de solicitações em sistemas de armazenamento para a análise de desempenho de armazenamento.

Configuramos um [cluster de failover] e usamos Diskspd para gerar Random e Sequential, e ler e gravar o IOs nos sistemas de armazenamento local e remoto com tamanhos de e/s diferentes. Nossos testes mostram que o tempo de resposta de e/s é muito sensível à frequência do processador em diferentes planos de energia. O plano de energia **equilibrado** poderia dobrar o tempo de resposta do plano de energia de **alto desempenho** em determinadas cargas de trabalho. A alteração proposta remove a maioria das regressões.

>[!Important]
>A partir de processadores Intel [Broadwell] que executam o Windows Server 2016, a maioria das decisões de gerenciamento de energia do processador é feita no processador em vez do nível do sistema operacional para obter uma adaptação mais rápida às alterações de carga de trabalho. Os parâmetros herdados PPM usados pelo sistema operacional têm um impacto mínimo sobre as decisões de frequência reais, exceto a informando ao processador se ele deve favorecer a energia ou o desempenho, ou limitar as frequências mínimas e máximas. Portanto, a alteração de parâmetro de PPM proposta é direcionada apenas para os sistemas Broadwell.

## <a name="see-also"></a>Consulte Também
- [Considerações sobre o desempenho de hardware](../index.md)
- [Server Hardware Power Considerations](../power.md) (Considerações de energia de hardware do servidor)
- [Power and Performance Tuning](power-performance-tuning.md) (Energia e ajuste de desempenho)
- [Processor Power Management Tuning](processor-power-management-tuning.md) (Ajuste de gerenciamento de energia do processador)
- [Cluster de failover](https://technet.microsoft.com/library/cc725923.aspx)
