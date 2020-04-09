---
title: Considerações de hardware no ajuste de desempenho do AD
description: Considerações de hardware no ajuste de desempenho do AD
ms.prod: windows-server
ms.technology: performance-tuning-guide
ms.topic: article
ms.author: timwi; chrisrob; herbertm; kenbrumf;  mleary; shawnrab
author: phstee
ms.date: 10/16/2017
ms.openlocfilehash: c40faca06668adf6fd29a5e4e753e5790b8104b7
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80851909"
---
# <a name="hardware-considerations-in-adds-performance-tuning"></a>Considerações de hardware no adiciona ajuste de desempenho 

>[!Important]
> Veja a seguir um resumo das principais recomendações e considerações para otimizar o hardware do servidor para Active Directory cargas de trabalho abordadas em mais detalhes no artigo [planejamento de capacidade para Active Directory Domain Services](https://go.microsoft.com/fwlink/?LinkId=324566) . Os leitores são altamente incentivados a examinar o [planejamento de capacidade para Active Directory Domain Services](https://go.microsoft.com/fwlink/?LinkId=324566) para obter uma compreensão técnica mais alta e as implicações dessas recomendações.

## <a name="avoid-going-to-disk"></a>Evite ir para o disco

Active Directory caches a maior parte do banco de dados que a memória permite. Buscar páginas da memória são ordens de magnitude mais rápido do que ir para mídia física, independentemente de a mídia ser baseada em eixo ou SSD. Adicione mais memória para minimizar a e/s de disco.

-   Active Directory práticas recomendadas recomendam a colocação de RAM suficiente para carregar toda a DIT na memória, além de acomodar o sistema operacional e outros aplicativos instalados, como antivírus, software de backup, monitoramento e assim por diante.

    -   Para limitações das plataformas herdadas, consulte [uso de memória pelo processo Lsass. exe em controladores de domínio que executam o Windows server 2003 ou o windows 2000 Server](https://support.microsoft.com/kb/308356).

    -   Use a memória\\o contador de desempenho tempo médio de cache em espera de longo prazo &gt; 30 minutos.

-   Coloque o sistema operacional, os logs e o banco de dados em volumes separados. Se toda ou a maioria da DIT puder ser armazenada em cache, quando o cache estiver quente e sob um estado estável, isso se tornará menos relevante e oferecerá um pouco mais de flexibilidade no layout de armazenamento. Em cenários em que a DIT inteira não pode ser armazenada em cache, a importância de dividir o sistema operacional, os logs e o banco de dados em volumes separados se torna mais importante.

-   Normalmente, as proporções de e/s para a DIT são cerca de 90% de leitura e 10% de gravação. Os cenários em que os volumes de e/s de gravação excedem significativamente 10% a 20% são considerados de gravação pesada. Cenários de gravação intensa não se beneficiam muito do cache Active Directory. Para garantir a durabilidade transacional dos dados gravados no diretório, Active Directory não executa cache de gravação em disco. Em vez disso, ele confirma todas as operações de gravação no disco antes de retornar um status de conclusão bem-sucedido para uma operação, a menos que haja uma solicitação explícita para não fazer isso. Portanto, a e/s rápida de disco é importante para o desempenho de operações de gravação para Active Directory. Veja a seguir as recomendações de hardware que podem melhorar o desempenho para esses cenários:

    -   Controladores RAID de hardware

    -   Aumentar o número de discos de baixa latência/alto RPM que hospedam os arquivos DIT e log

    -   Gravação em cache no controlador

-   Examine o desempenho do subsistema de disco individualmente para cada volume. A maioria dos cenários de Active Directory são basicamente baseados em leitura, portanto, as estatísticas do volume que hospeda a DIT são as mais importantes para inspecionar. No entanto, não ignore o monitoramento do restante das unidades, incluindo o sistema operacional e as unidades de arquivos de log. Para determinar se o controlador de domínio está configurado corretamente para evitar que o armazenamento seja o afunilamento do desempenho, consulte a seção sobre subsistemas de armazenamento para obter recomendações de armazenamento de padrões. Em vários ambientes, a filosofia é garantir que haja espaço suficiente na carga para acomodar picos de aumentos ou cargas. Esses limites são limites de aviso em que a sala de cabeça para acomodar picos ou picos de carga se torna restrita e a capacidade de resposta do cliente degrada. Em suma, exceder esses limites não é um mau período (de 5 a 15 minutos algumas vezes por dia), no entanto, um sistema em execução sustentado com esses tipos de estatísticas não está armazenando em cache completamente o banco de dados e pode ser excessivamente tributado e deve ser investigado.

    -   Database = = instâncias de&gt; (Lsass/NTDSa)\\banco de dados de e/s lê latência média &lt; 15ms

    -   Database = = instâncias de&gt; (Lsass/NTDSa)\\leituras de banco de dados de e/s/s &lt; 10

    -   Database = = instâncias de&gt; (Lsass/NTDSa)\\a latência média de gravações de log de e/s &lt; 10 ms

    -   Database = = instâncias de&gt; (Lsass/NTDSa)\\gravações de log de e/s/s – somente informativo.

        Para manter a consistência dos dados, todas as alterações devem ser gravadas no log. Não há um número bom ou ruim aqui, é apenas uma medida de quanto o armazenamento tem suporte.

-   Planeje cargas de e/s de disco não principais, como verificações de backup e antivírus, para períodos de carga fora de pico. Além disso, use soluções de backup e antivírus que dão suporte ao recurso de e/s de baixa prioridade introduzido no Windows Server 2008 para reduzir a competição com as necessidades de e/s de Active Directory.

## <a name="dont-over-tax-the-processors"></a>Não há excesso de imposto sobre os processadores

Os processadores que não têm ciclos livres suficientes podem causar tempos de espera longos para obter threads para o processador para execução. Em vários ambientes, a filosofia é garantir que exista espaço suficiente para acomodar surtos ou picos de carga para minimizar o impacto na capacidade de resposta do cliente nesses cenários. Em suma, exceder os limites abaixo não é um problema em curto prazo (de 5 a 15 minutos algumas vezes por dia), no entanto, um sistema em execução sustentado com esses tipos de estatísticas não fornece nenhuma sala de cabeça para acomodar cargas anormais e pode ser facilmente colocado em um cenário em excesso de tributação. Os períodos de gastos de sistemas sustentados acima dos limites devem ser investigados para como reduzir as cargas do processador.

-   Para obter mais informações sobre como selecionar um processador, consulte [ajuste de desempenho para hardware de servidor](../../hardware/index.md).

-   Adicione hardware, otimize a carga, direcione clientes em outro lugar ou remova a carga do ambiente para reduzir a carga da CPU.

-   Use as informações do processador (\_total)\\% de utilização do processador &lt; contador de desempenho 60%.

## <a name="avoid-overloading-the-network-adapter"></a>Evite sobrecarregar o adaptador de rede

Assim como ocorre com processadores, a utilização excessiva do adaptador de rede causará longos tempos de espera para o tráfego de saída entrar na rede. A Active Directory tende a ter pequenas solicitações de entrada e a quantidades de dados significativamente maiores retornados aos sistemas cliente. Os dados enviados de longe excedem os dados recebidos. Em vários ambientes, a filosofia é garantir que haja espaço suficiente na carga para acomodar picos de aumentos ou cargas. Esse limite é um limite de aviso em que a sala de cabeça para acomodar sobretensões ou picos na carga se torna restrita e a capacidade de resposta do cliente degrada. Em suma, exceder esses limites não é um mau período (de 5 a 15 minutos algumas vezes por dia), no entanto, um sistema em execução sustentado com esses tipos de estatísticas está acima da tributada e deve ser investigado.

-   Para obter mais informações sobre como ajustar o subsistema de rede, consulte [ajuste de desempenho para subsistemas de rede](../../../../networking/technologies/network-subsystem/net-sub-performance-top.md).

-   Use o contador de desempenho comparar NetworkInterface (\*)\\bytes enviados/s com NetworkInterface (\*)\\a largura de banda atual. A proporção deve ser inferior a 60% utilizada.

## <a name="see-also"></a>Consulte também
- [Ajuste de desempenho Active Directory servidores](index.md)
- [Considerações sobre LDAP](ldap-considerations.md)
- [Posicionamento adequado dos controladores de domínio e considerações sobre o local](site-definition-considerations.md)
- [Solução de problemas de desempenho do ADDS](troubleshoot.md) 
- [Planejamento de capacidade para o Active Directory Domain Services](https://go.microsoft.com/fwlink/?LinkId=324566)