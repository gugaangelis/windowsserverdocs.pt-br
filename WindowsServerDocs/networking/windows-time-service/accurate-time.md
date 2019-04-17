---
ms.assetid: 72a90d00-56ee-48a9-9fae-64cbad29556c
title: Tempo precisas 2016 do Windows
description: ''
author: shortpatti
ms.author: pashort
manager: brianlic
ms.date: 3/12/2018
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: networking
ms.openlocfilehash: f4b61dbf07fbc21820dd7b9326bbc990e4db3602
ms.sourcegitcommit: fb4e2ace2e0a29e0f6b028f1cb945cab6aa6ee2c
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/05/2018
---
# <a name="windows-server-2016-accurate-time"></a>Tempo precisas do Windows Server 2016

>Aplica-se a: Windows Server 2016

Precisão de sincronização de hora no Windows Server 2016 foi aprimorado substancialmente, mantendo completo para trás compatibilidade NTP com versões mais antigas do Windows. Em condições operacionais razoáveis, você pode manter um 1 ms precisão em relação ao UTC ou superior para Windows Server 2016 e atualização de aniversário do Windows 10 membros do domínio.

O serviço de tempo do Windows é um componente que usa um modelo de plug-in para provedores de sincronização de tempo de cliente e servidor.  Há dois provedores de cliente interno no Windows e plug-ins de terceiros estão disponíveis. Um provedor de usa [NTP (RFC 1305)](https://tools.ietf.org/html/rfc1305) ou [MS-NTP](https://msdn.microsoft.com/en-us/library/cc246877.aspx) para sincronizar a hora do sistema local para um servidor compatível com referência NTP e/ou MS-NTP. O outro provedor é do Hyper-V e sincroniza máquinas virtuais (VM) para o host do Hyper-V.  Quando houver vários provedores, o Windows escolher o melhor provedor usando o nível de camada em primeiro lugar, seguido pelo atraso de raiz, dispersão de raiz e finalmente deslocamento de tempo.

>[!NOTE]
>Para uma rápida visão geral do serviço de tempo do Windows, dê uma olhada neste [visão geral vídeo ](https://aka.ms/WS2016TimeVideo).

<!-- Not sure what to do with the following -->
Neste tópico, vamos discutir … estes tópicos relacionados a habilitação de tempo preciso: 

- Melhorias
- Medições
- Práticas recomendadas

>[!IMPORTANT]
> Um adendo referenciado pelo artigo do tempo de precisão de 2016 do Windows pode ser baixado [aqui](http://windocs.blob.core.windows.net/windocs/WindowsTimeSyncAccuracy_Addendum.pdf).  Este documento fornece mais detalhes sobre nossas metodologias de teste e medição.



> [!NOTE] 
> Modelo de plug-in do provedor de tempo do windows é [documentado no TechNet](https://msdn.microsoft.com/en-us/library/windows/desktop/ms725475%28v=vs.85%29.aspx).
<!-- -->





## <a name="domain-hierarchy"></a>Hierarquia de domínio
Configurações autônomas e de domínio funcionam de forma diferente.

- Membros do domínio usam um protocolo NTP seguro, que usa autenticação para garantir a segurança e a autenticidade de referência do tempo.  Membros do domínio sincronizam com um relógio mestre determinado pela hierarquia de domínio e um sistema de pontuação.  Em um domínio, há uma camada hierárquica de stratums de tempo, por meio do qual cada DC aponta para um controlador de domínio pai com uma camada de tempo mais preciso.  A hierarquia é resolvido para o PDC ou um controlador de domínio na floresta raiz ou um controlador de domínio com o sinalizador de domínio GTIMESERV, que indica um bom servidor de horário para o domínio.  Consulte o [especificar um Local confiável tempo serviço usando GTIMESERV](#GTIMESERV) seção abaixo.

- Computadores autônomos são configurados para usar time.windows.com por padrão.  Esse nome é resolvido pelo provedor, que deve apontar para um recurso de propriedade da Microsoft.  Como todas as referências de hora localizado remotamente, interrupções de rede, pode impedir a sincronização.  Carrega o tráfego de rede e caminhos de rede assimétricas podem reduzir a precisão da sincronização de tempo.  Para uma precisão de 1 ms, você não pode depender de um fontes de tempo remoto.

Desde que os convidados Hyper-V terão pelo menos dois provedores de tempo do Windows para você escolher, a hora de host e NTP, você pode ver comportamentos diferentes ao domínio ou autônomo durante a execução como um convidado.

> [!NOTE] 
> Para obter mais informações sobre a hierarquia de domínio e o sistema de pontuação, consulte o ["Qual é o serviço de tempo do Windows"?](https://blogs.msdn.microsoft.com/w32time/2007/07/07/what-is-windows-time-service/) Postagem de blog.

> [!NOTE]
> Camada é um conceito usado em provedores de NTP e o Hyper-V, e seu valor indica o local de relógios na hierarquia.  Camada 1 é reservada para o relógio do nível mais alto e a camada 0 é reservada para o hardware considerado precisos e tem pouco ou sem atraso associado a ele.  Camada 2 conversar com servidores de camada 1, camada 3 de camada 2 e assim por diante.  Enquanto uma camada inferior geralmente indica um relógio mais preciso, é possível encontrar discrepâncias.  Além disso, W32time aceita somente em tempo de camada 15 ou abaixo.  Para ver a camada de um cliente, use *w32tm /query /status*.

## <a name="critical-factors-for-accurate-time"></a>Fatores críticos para o tempo preciso
Em todos os casos de tempo preciso, há três fatores importantes:

1. **Relógio de origem sólida** -o relógio de fonte em seu domínio precisa ser estável e precisas. Isso normalmente significa instalar um dispositivo GPS ou ao apontar para uma origem de camada 1, levando #3 em conta. A analogia chegar, se você tiver dois barcos em água e você está tentando medir a altitude deles em comparação com os outros, sua precisão é melhor se o barco de origem é bastante estável e não se movendo. O mesmo vale para o tempo e se o relógio de fonte não é estável, toda a cadeia de relógios sincronizados é afetada e ampliada em cada estágio. Ele também deve ser acessível porque interrupções na conexão causará interferência com a sincronização de tempo. E, finalmente, ele deve ser seguro. Se o tempo de referência não for corretamente mantido ou operado por usuários potencialmente mal-intencionados, você pode expor seu domínio a ataques de tempo com base.
2. **Relógio do cliente estável** -relógios um cliente estável garante que o desvio natural do oscillator é containable.  NTP usa vários exemplos de potencialmente vários servidores NTP condição e disciplina o relógio de computadores locais.  Ele não etapa o horário de verão, mas em vez disso diminui ou acelera o relógio local que você aborda a precisão tempo rapidamente e fique preciso entre solicitações NTP.  No entanto, se oscillator do relógio do computador cliente não é estável, mais flutuações entre ajustes podem ocorrer, e os algoritmos de que Windows usa para o relógio da condição não funcionam com precisão.  Em alguns casos, as atualizações de firmware podem ser necessários para o tempo preciso.
3. **A comunicação NTP simétrica** -é fundamental que a conexão para a comunicação NTP é simétrico.  NTP usa cálculos para ajustar o tempo que supõem que o patch de rede é simétrico.  Se o caminho do pacote NTP leva indo para o servidor recebe um valor diferente de tempo para retornar, a precisão é afetada.  Por exemplo, o caminho pode mudar devido a alterações em uma topologia de rede ou pacotes sendo roteadas para dispositivos que possuem interface diferentes velocidades.


Para dispositivos alimentado por bateria, móveis e portátil, você deve considerar estratégias diferentes.  Em conformidade com a nossa recomendação, mantendo o tempo preciso requer o relógio para ser disciplinada uma vez por segundo, que se correlaciona com a frequência de atualização do relógio. Essas configurações consumirá mais energia da bateria que o esperado e pode interferir com economia de energia modos disponíveis no Windows para esses dispositivos. Dispositivos com bateria também tem determinados modos de energia que pararem todos os aplicativos sejam executados, que interfere com a capacidade do W32time disciplina o relógio e manter tempo preciso. Além disso, relógios em dispositivos móveis podem não estar muito precisos em primeiro lugar.  Ambiente condições ambientais afetam a precisão do relógio e um dispositivo móvel pode mover de uma condição de ambiente para a próxima que pode interferir com a sua capacidade de manter o tempo com precisão.  Portanto, a Microsoft não recomenda que você configure dispositivos portáteis alimentado por bateria com configurações de alta precisão. 

## <a name="why-is-time-important"></a>Por que o tempo é importante?  


## <a name="windows-server-2016-improvements"></a>Melhorias do Windows Server 2016
### <a name="windows-time-service-and-ntp"></a>NTP e serviço de tempo do Windows
Windows Server 2016 melhorou os algoritmos que ele usa para corrigir o tempo e o relógio do local para sincronizar com UTC da condição.  NTP usa 4 valores para calcular o deslocamento de tempo, com base na hora da solicitação/resposta de cliente e servidor solicitação/resposta.  No entanto, as redes são perturbador e pode haver picos nos dados de NTP devido a congestionamento da rede e de outros fatores que afetam a latência da rede.  Algoritmos de Windows 2016 média out esse ruído usando várias técnicas diferentes que resulta em um relógio estável e preciso.  Além disso, a origem usamos para referências de tempo precisas uma API aprimorada que oferece melhor resolução.  Com essas melhorias podem alcançamos 1 precisão ms em relação ao UTC em um domínio.

### <a name="hyper-v"></a>Hyper-V
Windows 2016 melhorou o serviço TimeSync Hyper-V. Melhorias incluem o tempo inicial mais preciso na tela inicial VM ou restauração de VM e correção de latência de interrupção para obter exemplos fornecidos para w32time.  Essa melhoria nos permite manter 10µs com-in do host com um RMS (raiz média ao quadrado, que indica a variação), de 50µs, até mesmo em uma máquina com carga de 75%.

> [!NOTE]
> Consulte este artigo sobre [arquitetura do Hyper-V](https://msdn.microsoft.com/library/cc768520.aspx) para obter mais informações.

> [!NOTE]
> Carga foi criada usando benchmark prime95 usando perfil equilibrada.

Além disso, o nível de camada que relata o Host para o convidado é mais transparente.  Anteriormente, o Host apresentaria um nível fixo das 2, independentemente de sua precisão.  Com as mudanças no Windows Server 2016, o host relata uma camada maior do que a camada de host, o que resulta em melhor momento para convidados virtuais.  A camada de host é determinada pelo w32time por meios normais, com base no seu tempo de origem.  Windows 2016 convidados encontrará o relógio mais preciso, em vez de no padrão para o host integrado ao domínio.  Foi por isso que nós aconselhável para desativar manualmente a configuração de provedor de tempo do Hyper-V para máquinas participando de um domínio no Windows 2012R2 e abaixo.

### <a name="monitoring"></a>Monitoramento
Contadores de desempenho foram adicionados.  Elas permitem a linha de base, monitoram e solucionar problemas de precisão de tempo.  Esses contadores incluem:

Contador|Descrição|
----- | ----- |
Calculado deslocamento de tempo|   O tempo absoluto offset entre o relógio do sistema e a origem da hora escolhida, como calculado pelo serviço W32Time em microssegundos. Quando uma nova amostra válida estiver disponível, o tempo calculado é atualizado com o deslocamento de tempo indicado pela amostra. Este é o deslocamento de tempo real do relógio do local. W32Time inicia a correção de relógio usando esse deslocamento e atualiza o tempo entre as amostras calculado com o deslocamento de tempo restante que precisa ser aplicada ao relógio local. Precisão de relógio pode ser controlado com esse contador de desempenho com um intervalo de sondagem baixo (ex: 256 segundos ou menos) e procurando o valor de contador seja menor do que o limite de precisão de relógio desejado.|
Ajuste de frequência do relógio| O ajuste de frequência do relógio absoluto feito para o relógio do sistema local por W32Time em partes cada bilhão. Esse contador ajuda a visualizar as ações sendo realizadas pelo W32time.|
Atraso de ida e volta NTP|    Atraso de ida e volta mais recente enfrentado pelo cliente NTP no recebimento de uma resposta do servidor em microssegundos. Este é o tempo decorrido no cliente NTP entre transmitir uma solicitação para o servidor NTP e receber uma resposta válida do servidor. Esse contador ajuda caracterizam os atrasos experientes pelo cliente NTP. Maiores ou variáveis idas e voltas podem adicionar ruído para cálculos de tempo NTP, que por sua vez podem afetar a precisão de sincronização de tempo por meio do NTP.|
Contagem de origem do cliente NTP|    Active diversas fontes de tempo de NTP está sendo usado pelo cliente NTP. Isso é uma contagem de distintas, active endereços IP dos servidores de tempo que respondem às solicitações do cliente. Esse número pode ser maior ou menor do que os pares configurados, dependendo da resolução DNS de nomes de mesmo nível e capacidade de abrangência atual.|
Servidor NTP solicitações de entrada|   Número de solicitações recebidas pelo servidor NTP (solicitações/s).|
Saída de respostas do servidor NTP|  Número de solicitações respondida por servidor NTP (respostas/s).|

Os 3 primeiros contadores direcionar cenários para solução de problemas de precisão.  A solução de problemas de precisão de tempo e NTP seção abaixo, em [práticas recomendadas](#BestPractices), tem mais detalhes.
Os últimos 3 contadores abrangem cenários de servidor NTP e são úteis quando determinar a carga e a linha de base seu desempenho atual.

### <a name="configuration-updates-per-environment"></a>Atualizações de configuração por ambiente
A seguir descreve as alterações na configuração padrão entre o Windows 2016 e versões anteriores para cada função.  As configurações para o Windows Server 2016 e Windows 10 Anniversary Update(build 14393), agora são exclusivos por isso, há são mostrados como colunas separadas. 

|Função|Configuração|Windows Server 2016|Windows 10 versão 1607|Windows Server 2012 R2</br>Windows Server 2008 R2</br>Windows 10|
|---|---|---|---|---|
|**Servidor autônomo/Nano**||||
| |*Servidor de horário*|time.windows.com|NA|time.windows.com|
| |*Frequência de sondagem*|64 - 1024 segundos|NA|Uma vez por semana|
| |*Frequência de atualização do relógio*|Uma vez por segundo|NA|Uma vez por hora|
|**Cliente autônomo**||||
| |*Servidor de horário*|NA|time.windows.com|time.windows.com|
| |*Frequência de sondagem*|NA|Uma vez por dia|Uma vez por semana|
| |*Frequência de atualização do relógio*|NA|Uma vez por dia|Uma vez por semana|
|**Controlador de domínio**||||
| |*Servidor de horário*|PDC/GTIMESERV|NA|PDC/GTIMESERV|
| |*Frequência de sondagem*|64-1024 segundos|NA|1024 - 32768 segundos|
| |*Frequência de atualização do relógio*|Uma vez por dia|NA|Uma vez por semana|
|**Servidor membro do domínio**||||
| |*Servidor de horário*|DC|NA|DC|
| |*Frequência de sondagem*|64-1024 segundos|NA|1024 - 32768 segundos|
| |*Frequência de atualização do relógio*|Uma vez por segundo|NA|Uma vez a cada 5 minutos|
|**Cliente de membro do domínio**||||
| |*Servidor de horário*|NA|DC|DC|
| |*Frequência de sondagem*|NA|1204 - 32768 segundos|1024 - 32768 segundos|
| |*Frequência de atualização do relógio*|NA|Uma vez a cada 5 minutos|Uma vez a cada 5 minutos|
|**Convidado Hyper-V**||||
| |*Servidor de horário*|Escolher a melhor opção com base no nível das servidor Host e a hora|Escolher a melhor opção com base no nível das servidor Host e a hora|Padrões de Host|
| |*Frequência de sondagem*|Com base na função acima|Com base na função acima|Com base na função acima|
| |*Frequência de atualização do relógio*|Com base na função acima|Com base na função acima|Com base na função acima|

>[!NOTE]
>Para o Linux no Hyper-V, consulte o [Linux que permite usar o tempo de Host do Hyper-V](#AllowingLinux) seção abaixo.

### <a name="impact-of-increased-polling-and-clock-update-frequency"></a>Impacto do aumento de sondagem e a frequência de atualização do relógio
Para fornecer um tempo mais preciso, os padrões de sondagem frequências e atualizações de relógio são aumentados que nos permitem fazer pequenos ajustes com mais frequência.  Isso fará com que mais tráfego UDP/NTP, no entanto, esses pacotes são pequenos para que deve haver pouco ou nenhum impacto sobre links de banda larga. No entanto, a vantagem é que tempo deve ser melhor em uma ampla variedade de hardware e ambientes.

Para dispositivos com bateria, aumentando a frequência de sondagem pode causar problemas.  Dispositivos de bateria não armazenam o tempo enquanto estiver desativado.  Quando eles retornarem, ele pode exigir correções frequentes para o relógio.  Aumentar a frequência de sondagem fará com que o relógio instável e também pode usar mais energia.  A Microsoft recomenda que você não alterar as configurações padrão do cliente.

Controladores de domínio devem ser afetados minimamente mesmo com o efeito multiplicado das atualizações maiores de clientes NTP em um domínio do AD.  NTP tem um consumo de recursos muito menor em comparação com um impacto baixa e de outros protocolos.  Você têm mais probabilidade de atingir limites de outra funcionalidade do domínio antes de ser afetados pelas configurações de maior para o Windows Server 2016.  No Active Directory usar NTP seguro, que tende a sincronizar tempo menos preciso do que NTP simple, mas constatamos que ele podem chegar a camada de clientes dois longe o PDC.

Como um plano conservador, você deve reservar 100 solicitações NTP por segundo por núcleo.  Por exemplo, um domínio composto de 4 controladores de domínio com 4 núcleos, você deve ser capaz de atender às solicitações NTP 1600 por segundo.  Se você tiver 10 k os clientes configurados para sincronizar tempo uma vez a cada 64 segundos e as solicitações são recebidas de maneira uniforme ao longo do tempo, você veria 10.000/64 ou em torno de 160 solicitações/segundo, espalhados em todos os controladores de domínio.  Isso se encaixa facilmente dentro nosso 1600 NTP solicitações/s com base neste exemplo.  Esses são conservadoras recomendações de planejamento e é claro têm uma grande dependência em sua rede, velocidades de processador e é carregado, assim como sempre linha de base e teste em seus ambientes.

Também é importante observar que se seus controladores de domínio estiver executando com uma carga de CPU considerável, mais de 40%, isso certamente adicionará ruído para respostas NTP e afetar sua precisão de tempo em seu domínio.  Novamente, você precisa testar em seu ambiente para entender os resultados reais.

## <a name="time-accuracy-measurements"></a>Medições de precisão de tempo
### <a name="methodology"></a>Metodologia
Para medir a precisão de tempo para o Windows Server 2016, usamos uma variedade de ferramentas, métodos e ambientes.  Você pode usar essas técnicas para medir e ajustar seu ambiente e determinar se os resultados de precisão atender às suas necessidades. 

Nosso relógio de origem do domínio consistia dois servidores NTP de alta precisão com o hardware GPS.  Também usamos uma máquina de teste de referência separados para medições, que também tinham hardware GPS de alta precisão instalado de um fabricante diferente.  Para alguns dos testes, você precisará de uma fonte de relógio exata e confiável para usar como uma referência além de sua fonte de relógio de domínio.

Nós usamos quatro métodos diferentes para medir a precisão com físicos e máquinas virtuais. Vários métodos fornecidos significa independente para validar os resultados.


1. Comparar o relógio do local, que está condicionada à w32tm, com nossa máquina de teste de referência que tem o hardware GPS separado.  
2.  Measure NTP ping do servidor NTP clientes usando W32tm "stripchart"
3.  Measure NTP ping do cliente para o servidor NTP usando W32tm "stripchart"
4.  Hyper-V Measure resulta do host para o convidado usando o contador de carimbo de data / hora (TSC).  Esse contador é compartilhado entre partições e a hora do sistema em ambas as partições.  Calculamos a diferença de tempo o host e o tempo de cliente na máquina virtual.  Em seguida, usamos o relógio TSC para interpolar a hora de host de convidado, já que as medidas não acontecem ao mesmo tempo.  Além disso, usamos o fator de relógio TSV os atrasos e latência na API.

W32tm é interna, mas as outras ferramentas que usamos durante o nosso teste estão disponíveis para o repositório da Microsoft no GitHub como código-fonte aberto para seu uso e testes.  WIKI no repositório tem mais informações sobre como usar as ferramentas para fazer medições.

> [https://github.com/Microsoft/Windows-Time-Calibration-Tools](https://github.com/Microsoft/Windows-Time-Calibration-Tools)

Os resultados do teste mostrados a seguir são um subconjunto de medidas que fizemos em um dos ambientes de teste.  Eles ilustram a precisão mantida no início da hierarquia de tempo e cliente do domínio filho no final da hierarquia de tempo.  Isso é comparado com as mesmas máquinas em uma topologia de 2012 com base para comparação.

### <a name="topology"></a>Topologia
Para efeito de comparação, testamos um 2012R2 do Windows Server e o Windows Server 2016 com base em topologia.  Ambas as topologias consistem em dois computadores host Hyper-V físicos que faça referência a um computador Windows Server 2016 com GPS relógio hardware instalado.  Cada host executa 3 ingressado no domínio windows convidados, que são organizados de acordo com a topologia a seguir.  As linhas representam a hierarquia de tempo e o protocolo/transporte que é usado.

![Tempo do Windows](media/Windows-2016-Accurate-Time/topology1.png)

![Tempo do Windows](media/Windows-2016-Accurate-Time/topology2.png)

### <a name="graphical-results-overview"></a>Visão geral gráfica resultados
Os dois gráficos a seguir representam a precisão de tempo para os dois membros específicos em um domínio com base na topologia acima.  Cada gráfico exibe tanto 2012R2 do Windows Server 2016 resultados sobrepostos, que demonstra as melhorias visualmente.  A precisão foi meça a distância entre com a máquina de convidado em comparação com o host.  Os dados gráficos representa um subconjunto de todo o conjunto de testes que fizemos e mostra o melhor caso e piores cenários.  

![Tempo do Windows](media/Windows-2016-Accurate-Time/topology3.png)

### <a name="performance-of-the-root-domain-pdc"></a>Desempenho do domínio raiz PDC
O PDC raiz é sincronizado com o host Hyper-V (usando VMIC) que é um Windows Server 2016 com hardware de GPS provou para ser precisos e estáveis.  Isso é um requisito essencial para 1 ms precisão, que é mostrado como a área sombreada verde.

![Tempo do Windows](media/Windows-2016-Accurate-Time/chart1.png)

### <a name="performance-of-the-child-domain-client"></a>Desempenho do cliente de domínio do filho
O cliente de domínio filho está anexado a um PDC de domínio filho que se comunica com o PDC raiz.  Tempo também é dentro do 1 ms requisito.

![Tempo do Windows](media/Windows-2016-Accurate-Time/chart2.png)


### <a name="long-distance-test"></a>Teste de longa distância
O gráfico a seguir compara 1 salto de rede virtual para 6 saltos de rede física com o Windows Server 2016.  Dois gráficos estão sobrepostos em si com transparência para mostrar dados sobrepostos.  Crescente saltos de rede significam mais alta latência e desvios de tempo maiores.  O gráfico é ampliada e, portanto o 1 ms limites, representados pela área de verde, é maior.  Como você pode ver, o tempo é ainda dentro de 1 ms com vários saltos.  Ele é negativamente deslocado, que demonstra uma assimetria de rede.  Obviamente, cada rede é diferente e medidas dependem de uma variedade de fatores ambientais.

![Tempo do Windows](media/Windows-2016-Accurate-Time/chart3.png)

## <a name="BestPractices"></a>Práticas recomendadas para Cronometragem precisa
### <a name="solid-source-clock"></a>Relógio de origem sólida
Um tempo de máquinas só é tão boa quanto o relógio de origem que ele sincroniza com.  Para obter 1 ms de precisão, você precisará de hardware GPS ou em um aparelho de tempo em sua rede que você faz referência como o relógio de origem mestre.  Usando o padrão de time.windows.com, não pode fornecer um estável e uma fonte de hora local.  Além disso, à medida que se mais longe o relógio de origem, a rede afeta a precisão.  Ter um relógio de origem mestre em cada dados central é necessária para a melhor precisão.

### <a name="hardware-gps-options"></a>Opções de GPS de hardware
Há várias soluções de hardware que podem oferecer tempo preciso.  Em geral, soluções de hoje são baseadas em antenas do GPS.  Também há rádio e soluções de modem dial-up usando linhas dedicadas.  Ele anexar à sua rede como um dispositivo ou conecte a um computador, por exemplo, Windows por meio de um dispositivo USB ou PCIe.  Opções diferentes fornecerão diferentes níveis de precisão e como sempre, resultados dependem do seu ambiente.  Variáveis que afetam a precisão incluem disponibilidade GPS, estabilidade da rede e load e Hardware do computador.  Esses são fatores importantes ao escolher um relógio de origem, conforme afirmado, é um requisito para o tempo estável e preciso.

### <a name="domain-and-synchronizing-time"></a>Domínio e sincronização de hora
Membros do domínio usam a hierarquia de domínio para determinar qual máquina usarem como uma fonte para sincronizar o tempo.  Cada membro do domínio vai encontrar outro computador para sincronizar com e salvá-lo como fonte do relógio.  Cada tipo de membro do domínio segue um conjunto de regras diferente para encontrar uma fonte de relógio para sincronização de tempo.  O PDC na raiz da floresta é a origem de relógio padrão para todos os domínios.  Listados abaixo são funções diferentes e descrição resumida de como eles encontram uma fonte:


- **Controlador de domínio com a função PDC** – esta máquina é a fonte de tempo de autorização para um domínio. Ele terá a hora mais precisa disponível no domínio e deve sincronizar com um controlador de domínio no domínio pai, exceto em casos onde [GTIMESERV](#GTIMESERV) função está habilitada. 
- **Qualquer outro controlador de domínio** – essa máquina vai atuar como uma fonte de tempo para clientes e servidores membro no domínio. Um controlador de domínio pode sincronizar com o PDC do seu próprio domínio ou qualquer controlador de domínio no domínio pai.
- **Servidores de clientes/membro** – essa máquina pode sincronizar com qualquer controlador de domínio ou PDC de seu próprio domínio, ou um controlador de domínio ou PDC no domínio pai.

De acordo com os candidatos disponíveis, um sistema de pontuação é usado para encontrar a melhor fonte de tempo.  Esse sistema leva em conta a confiabilidade da fonte de tempo e o local relativo.  Isso ocorre depois que quando o tempo é serviço iniciado.  Se você precisar de controle mais refinado de como sincroniza o tempo, você pode adicionar servidores de boa hora em locais específicos ou adicionar redundância.  Consulte o [especificar um Local confiável tempo serviço usando GTIMESERV](#GTIMESERV) seção para obter mais informações.

#### <a name="mixed-os-environments-win2012r2-and-win2008r2"></a>Ambientes de sistema operacional misto (Win2012R2 e Win2008R2)
Enquanto um ambiente de domínio do Windows Server 2016 puro é necessário para a melhor precisão, ainda há benefícios em um ambiente misto.  Implantação do Windows Server 2016 Hyper-V em um domínio do Windows 2012 se beneficiarão os convidados devido as melhorias mencionado acima, mas somente se os convidados também são Windows Server 2016.  Windows Server 2016 PDC, será capaz de fornecer mais preciso tempo devido os algoritmos aprimorados será uma fonte mais estável.  Como substituir seu PDC talvez não seja uma opção, em vez disso, você pode adicionar um controlador de domínio do Windows Server 2016 com o [GTIMESERV](#GTIMESERV) jogue conjunto que seria uma atualização na precisão do seu domínio.  Um controlador de domínio do Windows Server 2016 pode fornecer o melhor momento para clientes de tempo downstream, entretanto, só é tão boa quanto o tempo de NTP de origem.

Além disso, como mencionado anteriormente, frequências de sondagem e atualizar o relógio foram modificadas com o Windows Server 2016.  Elas podem ser alteradas manualmente para seus controladores de domínio de nível inferior ou aplicadas por meio da política de grupo.  Enquanto não testamos essas configurações, eles devem se comportar bem no Win2008R2 e Win2012R2 e alguns benefícios.

Versões antes que o Windows Server 2016 tinha um vários problemas mantendo tempo preciso mantendo que fazia o tempo sistema pelo vento imediatamente depois que um ajuste foi feito.  Por isso, obter exemplos de tempo de uma fonte NTP precisa com frequência e condicionamento o relógio local com os dados leva a menor desvio em seus relógios de sistema no período entre sampling, resultando em melhor tempo mantendo versões de sistema operacional de nível inferior. A melhor precisão observada foi aproximadamente 5 ms quando um cliente do Windows Server 2012R2 NTP, com as configurações de alta precisão, sincronizados seu tempo de um servidor Windows 2016 NTP preciso.

Em alguns cenários que envolvem controladores de domínio de convidado, amostras TimeSync Hyper-V podem interromper a sincronização de tempo de domínio.  Isso não deve ser um problema para convidados Server 2016 executando em hosts 2016 Hyper-V Server.

Para desativar o serviço do Hyper-V TimeSync do fornecimento de amostras para w32time, defina a seguinte chave do registro convidado:

    HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Services\W32Time\TimeProviders\VMICTimeProvider 
    "Enabled"=dword:00000000

#### <a name="AllowingLinux"></a>Permitindo que o Linux usar o tempo de Host do Hyper-V
Para Linux convidados em execução no Hyper-V, os clientes geralmente são configurados para usar o daemon NTP para sincronização de tempo contra servidores NTP.  Se a distribuição Linux dá suporte ao protocolo de versão 4 de TimeSync e o convidado do Linux tem o serviço de integração TimeSync habilitado, ele sincronizará contra o tempo de host. Isso pode levar a inconsistente tempo mantendo se ambos os métodos são habilitados.

Para sincronizar exclusivamente contra o tempo de host, é recomendável desabilitar a sincronização de tempo NTP pelo:

- Desabilitar todos os servidores NTP no arquivo ntp.conf
- Ou desabilitando o daemon NTP

Nesta configuração, o parâmetro de servidor de horário é esse host.  Sua frequência de sondagem é 5 segundos e a frequência de atualização do relógio também é 5 segundos.

Para sincronizar exclusivamente sobre NTP, é recomendável desativar o serviço de integração TimeSync no convidado.

> [!NOTE]
> Observação: O suporte para o tempo preciso com Linux convidados requer um recurso que é compatível apenas com os mais recentes kernels Linux upstream e não é algo que ainda amplamente disponíveis em todos os distros Linux. Consulte [suporte Linux e FreeBSD máquinas virtuais do Hyper-V no Windows](https://technet.microsoft.com/en-us/windows-server-docs/virtualization/hyper-v/supported-linux-and-freebsd-virtual-machines-for-hyper-v-on-windows) para obter mais detalhes sobre distribuições de suporte.

#### <a name="GTIMESERV"></a>Especificar um serviço de tempo confiável Local usando GTIMESERV
Você pode especificar um ou mais controladores de domínio como relógios de fonte precisa usando o GTIMESERV, o servidor de horário na BOM, sinaliza.  Por exemplo, controladores de domínio específico equipados com hardware GPS podem ser marcados como um GTIMESERV.  Isso garantirá que o domínio faz referência a um relógio com base no hardware de GPS.

> [!NOTE]
> Mais informações sobre sinalizadores de domínio podem ser encontradas no [documentação de protocolo MS-ADTS](https://msdn.microsoft.com/library/mt226583.aspx).

TIMESERV é outro relacionados domínio serviços sinalizador que indica se uma máquina é autoritativa atualmente, que pode mudar se um controlador de domínio perde a conexão.  Um controlador de domínio nesse estado retornará "Camada desconhecido" quando consultada por NTP.  Depois de experimentar várias vezes, o controlador de domínio registrará 36 de evento do sistema evento serviço de tempo.

Se você quiser configurar um controlador de domínio como um GTIMESERV, isso pode ser configurado manualmente usando o comando a seguir.  Nesse caso o controlador de domínio estiver usando outro máquinas como o relógio mestre.  Isso pode ser um dispositivo ou computador dedicado.

    w32tm /config /manualpeerlist:”master_clock1,0x8 master_clock2,0x8” /syncfromflags:manual /reliable:yes /update

> [!NOTE]
> Para obter mais informações, consulte [configurar o serviço de tempo do Windows](https://technet.microsoft.com/library/cc731191.aspx)

Se o controlador de domínio tem o hardware GPS instalado, você precisa usar estas etapas para desativar o cliente NTP e permitir que o servidor NTP.

Comece desativando o cliente NTP e permitir que o servidor NTP usando essas alterações de chave do registro.

    reg add HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Services\w32time\TimeProviders\NtpClient /v Enabled /t REG_DWORD /d 0 /f

    reg add HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Services\w32time\TimeProviders\NtpServer /v Enabled /t REG_DWORD /d 1 /f

Em seguida, reiniciar o serviço de tempo do Windows

    net stop w32time && net start w32time

Por fim, você indicar que este computador tem uma fonte de tempo confiável usando.
   
    w32tm /config /reliable:yes /update

Para verificar as alterações de tem sido feitas corretamente, você pode executar os seguintes comandos que afetam os resultados mostrados abaixo. 

    w32tm /query /configuration

Valor|Configuração esperada|
----- | ----- |
AnnounceFlags|  5 (local)|
NtpServer   |(Local)|
DllName |C:\WINDOWS\SYSTEM32\w32time.DLL (local)|
Habilitado |1 (local)|
NtpClient|  (Local)|

    w32tm /query /status /verbose

Valor|  Configuração esperada|
----- | ----- |
Camada|    1 (referência principal - syncd ao lado do relógio rádio)|
ID de referência|    0x4C4F434C (nome da fonte: "LOCAL")|
Fonte| Relógio CMOS local|
Fase deslocamento|   0.0000000s|
Função de servidor|    576 (serviço de tempo confiável)|

#### <a name="windows-server-2016-on-3rd-party-virtual-platforms"></a>Windows Server 2016 em 3ª plataformas virtuais festa
Por padrão, quando o Windows for virtualizado, o hipervisor é responsável pelo fornecimento de tempo.  Mas integrado ao domínio membros precisam ser sincronizado com o controlador de domínio na ordem do Active Directory funcionar corretamente.  É aconselhável desabilitar a virtualização qualquer tempo entre o convidado e o host de qualquer 3ª plataformas virtuais de terceiros.

#### <a name="discovering-the-hierarchy"></a>Descobrindo a hierarquia
Desde que a cadeia de hierarquia de tempo para a origem de relógio mestre é dinâmica em um domínio e negociado, você precisará consultar o status de um determinado computador para entender é origem em tempo e cadeia para o relógio de origem mestre.  Isso pode ajudar a diagnosticar problemas de sincronização de tempo.

Dada a você deseja solucionar um cliente específico; a primeira etapa é entender sua fonte de tempo usando este comando w32tm.

    w32tm /query /status

Os resultados exibem a origem entre outras coisas.  A origem indica com quem você sincroniza tempo no domínio.  Esta é a primeira etapa dessa hierarquia de tempo de máquinas.
Em seguida, use a entrada de origem de cima e use o /StripChart parâmetro para encontrar a próxima origem de tempo na cadeia.

    w32tm /stripchart /computer:MySourceEntry /packetinfo /samples:1

Também é útil, o comando a seguir lista cada controlador de domínio que ele encontra no domínio especificado e imprime um resultado que permite que você determine cada parceiro.  Esse comando incluirá computadores que tenham sido configurados manualmente.
    
    w32tm /monitor /domain:my_domain

Usando a lista, você pode rastrear os resultados por meio do domínio e entender a hierarquia, bem como o deslocamento de tempo em cada etapa.  Localizando o ponto em que fica pior significativamente o deslocamento de tempo, você pode identificar a raiz do tempo incorreto.  A partir daí, você pode tentar para entender por que esse tempo é incorreto Ativando [w32tm log](#W32Logging). 

#### <a name="using-group-policy"></a>Usando política de grupo
Você pode usar política de grupo para realizar a classificação mais rígida precisão, por exemplo, atribuindo clientes para uso específico NTP servidores ou SO de nível inferior como controle é configurado quando virtualizados.  
A seguir está uma lista de configurações de política de grupo relevantes e cenários possíveis:

**Virtualizados domínios** - fim de controlar virtualizados controladores de domínio no Windows 2012R2 para que eles sincronizam tempo com seu domínio, em vez de com o host do Hyper-V, você pode desabilitar essa entrada do registro.   Para o PDC, você não deseja desativar a entrada, como o host do Hyper-V fornecerá a fonte de tempo mais estável.  A entrada do registro requer que você reinicie o serviço w32time depois que ele é alterado.

    [HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Services\W32Time\TimeProviders\VMICTimeProvider]
    "Enabled"=dword:00000000

**Precisão confidenciais cargas** – para tempo precisão confidenciais cargas de trabalho, você poderia definir grupos de máquinas para definir os servidores NTP e qualquer relacionado as configurações de tempo, como sondagem e o relógio frequência de atualização.  Isso normalmente é manipulado pelo domínio, mas para ter mais controle você pode ter como alvo máquinas específicas para apontar diretamente para o relógio mestre.

Configuração de política de grupo|   Novo valor|
----- | ----- |
NtpServer|  ClockMasterName, 0x8|
MinPollInterval|    6 – 64 segundos|
MaxPollInterval|    6|
UpdateInterval| 100 – uma vez por segundo|
EventLogFlags|  3 – todos os logs de tempo especial|

> [!NOTE]
> As configurações NtpServer e EventLogFlags estão localizadas em System\Widows tempo Service\Time provedores usando as configurações de configurar o Windows NTP Client.  As outras 3 estão localizadas em serviço de tempo de System\Windows usando as configurações globais.

**Remoto precisão confidenciais cargas remoto** – para sistemas em domínios de ramificação para a instância de varejo e o setor de crédito de pagamento (PCI), o Windows usa as informações do site atual e localizador de DC para encontrar um controlador de domínio local, a menos que haja um manual NTP tempo origem configurada.  Esse ambiente requer 1 segundo de precisão, que usa uma convergência mais rápida a hora corretas.  Essa opção permite que o serviço w32time retroceder o relógio.  Se isso é aceitável e atende às suas necessidades, você pode criar a seguinte política.   Assim como com qualquer ambiente, garante que a linha de base e de teste sua rede. 

Configuração de política de grupo|   Novo valor|
----- | ----- |
MaxAllowedPhaseOffset|  1, mais do que na segunda, definir o relógio para horário correto.|

A configuração MaxAllowedPhaseOffset está localizada em serviço de tempo de System\Windows usando as configurações globais.

> [!NOTE]
> Para obter mais informações sobre a política de grupo e entradas relacionadas, consulte [ferramentas de serviço de tempo do Windows](windows-time-service-tools-and-settings.md) e artigo de configurações no TechNet.

#### <a name="azure-and-windows-iaas-considerations"></a>Considerações do Azure e Windows IaaS

##### <a name="azure-virtual-machine-active-directory-domain-services"></a>Máquina Virtual Azure: Serviços de domínio do Active Directory
Se a VM do Azure que executam serviços de domínio do Active Directory é parte de um existente no local floresta do Active Directory, em seguida, TimeSync(VMIC), deverá ser desabilitada. Isso é para permitir que todos os controladores de domínio na floresta, física e virtual usar uma hierarquia de sincronização de hora único. Consulte o whitepaper prática recomendado ["executando o domínio controladores no Hyper-V"](https://technet.microsoft.com/en-us/library/virtual_active_directory_domain_controller_virtualization_hyperv.aspx)

##### <a name="azure-virtual-machine-domain-joined-machine"></a>Máquina Virtual Azure: A máquina de domínio
Se você estiver hospedando um computador que é o domínio que tenha ingressado em uma floresta do Active Directory existentes, virtual ou física, a prática recomendada é desabilitar TimeSync do convidado e certifique-se de W32Time está configurado para sincronizar com o controlador de domínio por meio de configuração do tempo para o tipo = NTP5

##### <a name="azure-virtual-machine-standalone-workgroup-machine"></a>Máquina Virtual Azure: Computador de grupo de trabalho autônomo
Se a VM do Azure não tenha ingressada em um domínio, nem é um controlador de domínio, a recomendação é manter a configuração padrão do tempo e fazer com que a VM sincronize com o host.

### <a name="windows-application-requiring-accurate-time"></a>Tempo precisas de necessidade do aplicativo de Windows
#### <a name="time-stamp-api"></a>Carimbo de data / hora API
Programas que exigem a maior precisão em relação ao UTC e não a passagem de tempo, devem usar o [GetSystemTimePreciseAsFileTime API](https://msdn.microsoft.com/library/windows/desktop/Hh706895.aspx).  Isso garante que seu aplicativo obtém a hora do sistema, que está condicionada pelo serviço de tempo do Windows.

#### <a name="udp-performance"></a>Desempenho de UDP
Se você tiver um aplicativo que usa UDP comunicação para transações e ele importante minimizar a latência, existem alguns relacionados entradas do registro que você pode usar para configurar um intervalo de portas serem excluídos do portar a mecanismo de filtragem básica.  Isso melhora os dois a latência e aumentar a taxa de transferência.  No entanto, as alterações no registro devem ser limitadas a administradores experientes.  Além disso, essa solução alternativa exclui portas sejam protegidas pelo firewall.  Consulte a referência de artigo abaixo para obter mais informações.

Para Windows Server 2012 e Windows Server 2008, você precisará instalar um Hotfix pela primeira vez.  Você pode referenciar neste artigo KB: [perda de datagrama quando você executa um aplicativo receptor multicast no Windows 8 e no Windows Server 2012](https://support.microsoft.com/en-us/kb/2808584)

#### <a name="update-network-drivers"></a>Atualizar Drivers de rede
Alguns fornecedores de rede tem atualizações de driver que melhorar o desempenho em relação a latência de drivers e pacotes UDP de buffer.  Contate o fornecedor de rede para ver se há atualizações para ajudar com taxa de transferência UDP.

### <a name="logging-for-auditing-purposes"></a>Registro em log para fins de auditoria
Para cumprir as normas de rastreamento de tempo, você pode arquivar manualmente w32tm logs, logs de eventos e informações de monitoramento de desempenho.  Mais tarde, as informações arquivadas podem ser usadas para atestar conformidade em um momento específico no passado.  Os seguintes fatores são usados para indicar a precisão.


1. Precisão de relógio usando o contador do monitor de desempenho calculado deslocamento de tempo.  Isso mostra o relógio dentro da precisão desejada.
2.  Fonte de relógio procurando por "Resposta de par de" nos logs do w32tm.   Após o texto de mensagem é o endereço IP ou VMIC, que descreve a fonte de tempo e a próxima na cadeia de relógios de referência para validar.
3.  Status de condição usando os logs w32tm para validar que de clock "disciplina ClockDispl: \*SKEW\*TIME\*" estão ocorrendo.  Isso indica que w32tm está ativo no momento.

#### <a name="event-logging"></a>Log de eventos
Para obter a história completa, você também precisará informações de log de eventos.  Coletando o log de eventos do sistema, e filtragem no servidor de horário, Microsoft-Windows-inicialização do Kernel, Microsoft-Windows-Kernel-geral, você poderá descobrir se há outras influências que mudaram o tempo, por exemplo, terceiros.  Esses logs talvez seja necessários descartar interferência externa.  Política de grupo pode afetar quais logs de eventos são gravados no log.  Consulte a seção acima usando política de grupo para obter mais detalhes.

#### <a name="W32Logging"></a>Log de depuração W32Time
Para habilitar w32tm para fins de auditoria, o seguinte comando habilita o log que mostra as atualizações periódicas do relógio e indica o relógio de origem.  Reinicie o serviço para habilitar o log de novo.  

Para obter mais informações, consulte [como ativar o log de depuração no Windows Time Service](https://support.microsoft.com/en-us/kb/816043).

    w32tm /debug /enable /file:C:\Windows\Temp\w32time-test.log /size:10000000 /entries:0-73,103,107,110

#### <a name="performance-monitor"></a>Monitor de desempenho
O serviço de tempo do Windows Server 2016 Windows expõe contadores de desempenho que podem ser usados para coletar o registro em log de auditoria.  Eles podem ser registrados local ou remotamente.  Você pode gravar o deslocamento de tempo do computador e contadores de atraso de ida e volta.  
E como qualquer contador de desempenho, você pode monitorá-las remotamente e criar alertas usando o System Center Operations Manager.  Você pode, por exemplo, usar um alerta para você de alarme quando o deslocamento de tempo se move da precisão desejada.  O [System Center Management Pack](https://social.technet.microsoft.com/wiki/contents/articles/15251.system-center-management-pack-authoring-guide.aspx) tem mais informações.

#### <a name="windows-traceability-example"></a>Exemplo de rastreamento do Windows
W32tm dos arquivos de log, você desejará validar duas partes de informações.  A primeira é uma indicação de que o arquivo de log é atualmente relógio condição.  Isso provar que o relógio estava sendo condicionado pelo serviço de tempo do Windows no momento em disputa.

    151802 20:18:32.9821765s - ClockDispln Discipline: *SKEW*TIME* - PhCRR:223 CR:156250 UI:100 phcT:65 KPhO:14307
    151802 20:18:33.9898460s - ClockDispln Discipline: *SKEW*TIME* - PhCRR:1 CR:156250 UI:100 phcT:64 KPhO:41
    151802 20:18:44.1090410s - ClockDispln Discipline: *SKEW*TIME* - PhCRR:1 CR:156250 UI:100 phcT:65 KPhO:38

O principal ponto é que você está vendo o prefixo disciplina ClockDispln que é w32time prova de mensagens está interagindo com o relógio do sistema.
 
Em seguida, você precisa encontrar o último relatório no log de antes da hora em disputa que relata o computador de origem que está sendo usado atualmente como o relógio de referência.  Isso poderia ser um endereço IP, nome do computador ou o provedor VMIC, que indica que ele está sincronizando com o Host do Hyper-V.  O exemplo a seguir fornece um endereço IPv4 10.197.216.105.

    151802 20:18:54.6531515s - Response from peer 10.197.216.105,0x8 (ntp.m|0x8|0.0.0.0:123->10.197.216.105:123), ofs: +00.0012218s

Agora que você tenha validado o primeiro sistema na cadeia de tempo de referência, você precisa para investigar o arquivo de log na fonte de tempo de referência e repita as mesmas etapas.  Esse processo continua até chegar a um relógio de físico, como uma fonte de horário conhecida como NIST ou GPS.  Se o relógio de referência é GPS de hardware, em seguida, logs do fabricado também podem ser necessários.

### <a name="network-considerations"></a>Considerações sobre a rede
Os algoritmos de protocolo NTP tem uma dependência em simetria da sua rede.  Como seu aumentar o número de saltos de rede, aumenta a probabilidade de assimetria.  Lá, é difícil prever quais tipos de precisões você verá no seu ambiente específico. 

Monitor de desempenho e novos contadores de tempo do Windows no Windows Server 2016 podem ser usados para avaliar sua precisão de ambientes e criar linhas de base. Além disso, você pode executar a solução de problemas para determinar o deslocamento atual de qualquer computador em sua rede.

Há dois padrões gerais de tempo preciso pela rede.  PTP ([protocolo de tempo de precisão - 1588 IEEE](https://www.nist.gov/el/intelligent-systems-division-73500/introduction-ieee-1588)) tem mais requisitos na infraestrutura de rede, mas geralmente podem fornecer precisão subunidades microssegundos.  NTP ([protocolo de tempo de rede – RFC 1305](https://tools.ietf.org/html/rfc1305)) funciona em uma variedade maior de redes e ambientes, que torna mais fácil de gerenciar. 

Windows dá suporte a NTP simples (RFC2030) por padrão para computadores conectados sem domínio.  Para computadores integrado ao domínio, nós usamos um NTP seguro chamado [MS-SNTP](https://msdn.microsoft.com/en-us/library/cc246877.aspx), que utiliza o domínio negociado segredos que fornecem uma vantagem de gerenciamento sobre NTP autenticados descrito em RFC1305 e RFC5905.   

O domínio e os protocolos conectados sem domínio requer porta UDP 123.  Para obter mais informações sobre as práticas recomendadas NTP, consulte [rede tempo protocolo melhores práticas IETF rascunho atual](https://tools.ietf.org/html/draft-ietf-ntp-bcp-00).

### <a name="reliable-hardware-clock-rtc"></a>Relógio de Hardware confiável (RTC)
O Windows faz não tempo etapa, a menos que determinados limites forem excedidos, mas em vez disso, disciplinas o relógio.  Isso significa que w32tm ajusta a frequência do relógio em um intervalo normal, usando a frequência de atualização de relógio configuração, que tem o padrão de uma vez por segundo com o Windows Server 2016.  Se o relógio estiver atrás, ele acelera a frequência e se ele estiver em frente, ele fica mais lenta a frequência.  No entanto, durante esse tempo entre os ajustes de frequência do relógio, o relógio de hardware está no controle.  Se houver um problema com o firmware ou o relógio de hardware, o tempo no computador pode se tornar menos preciso.

Esse é outro motivo, que você precisa testar e linha de base em seu ambiente.  Se o contador de desempenho "Calculado deslocamento de tempo" não estabilizar com a precisão que você está direcionando, convém verificar que o firmware é atualizado.  Como outro teste, você pode ver se hardware duplicado reproduzir o mesmo problema.

### <a name="troubleshooting-time-accuracy-and-ntp"></a>Solução de problemas de precisão de tempo e NTP
Você pode usar a detecção a seção de hierarquia acima para compreender a origem do tempo impreciso.  Olhando para o deslocamento de tempo, encontre o ponto na hierarquia em que o tempo é divergente o máximo da fonte de NTP.  Depois de compreender a hierarquia, você vai querer experimentar e entender por que essa fonte de tempo específico não recebe tempo preciso.  

Focalizar o sistema com o tempo divergentes, você pode usar essas ferramentas abaixo para obter mais informações para ajudá-lo a determinar o problema e encontrar uma resolução.  A referência de UpstreamClockSource abaixo, é o relógio descoberto usando "w32tm /config /status".


- Logs de eventos do sistema
- Ativar usando o registro em log: logs w32tm - w32tm /debug /enable /file: C:\Windows\Temp\w32time-test.log//size: 10000000 /entries: 0-300
- chave do registro w32Time HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Services\W32Time
- Rastreamentos de rede local
- Contadores de desempenho (a partir da máquina local ou o UpstreamClockSource)
- W32tm /stripchart /computer: UpstreamClockSource
- PING UpstreamClockSource para entender a latência e o número de saltos para fonte
- Tracert UpstreamClockSource

Problema|    Sintomas|   Resolução|
----- | ----- | ----- |
Relógio TSC local não é estável.| Usando relógio estável do Perfmon - computador físico – Sincronizar relógio, mas você ainda verá que cada minutos de 1 a 2 de vários 100us. |   Atualizar o Firmware ou validar um hardware diferente não exibe o mesmo problema.|
Latência da rede|    stripchart w32tm exibe um RoundTripDelay de mais de 10 ms.  Variação em atraso causar ruído tão grande quanto ½ do tempo de ida e volta, por exemplo, um atraso que é somente em uma direção.</br></br>UpstreamClockSource é vários saltos, conforme indicado pelo PING.  TTL deve estar perto de 128.</br></br>Use Tracert para encontrar a latência a cada salto.    | Encontre uma fonte mais próxima do relógio por tempo.  Uma solução é instalar um relógio de origem no mesmo segmento ou apontar manualmente para relógio de origem que é geograficamente mais próximo.  Para um cenário de domínio, adicione uma máquina com a função GTimeServ. |  
Não é possível acessar confiavelmente a origem NTP|    W32tm /stripchart intermitentemente retorna "Solicitação atingiu o tempo limite"    |Fonte NTP não esteja respondendo|
Fonte NTP não esteja respondendo|    Verifique os contadores de desempenho contagem de origem do cliente NTP, solicitações de entrada de servidor NTP, NTP Server para respostas de saída e determinar o uso de comparação com suas linhas de base.|    Usando contadores de desempenho do servidor, determine se a carga foi alterada na referência de suas linhas de base.</br></br>Existe rede congestionamento problemas?|
Controlador de domínio não usando o relógio mais preciso|    Alterações na topologia ou relógio mestre adicionado recentemente.|   w32tm /resync /rediscover|
Relógios do cliente são pelo vento| Serviço de tempo do evento 36 no log de eventos do sistema e/ou texto no arquivo de log que descreve: contador "Contagem de origem de tempo de cliente NTP" vai de 1 para 0|Solucionar problemas a origem upstream e entender se ele está em execução em problemas de desempenho.|

### <a name="baselining-time"></a>Tempo de linha de base
Linha de base é importante para que o primeiro, você pode entender o desempenho e a precisão de sua rede e compare com a linha de base no futuro quando ocorrem problemas.  Você vai querer baseline raiz PDC ou qualquer máquinas marcado com a GTIMESRV.  Também recomendamos você PDC em cada floresta de linha de base.  Por fim, escolha qualquer crítica controladores de domínio ou computadores que têm características interessantes, como distância ou alto é carregado e linha de base desses.

Também é útil para a linha de base Windows Server 2016 vs 2012 R2, no entanto, você tem apenas w32tm /stripchart como uma ferramenta que você pode usar para comparar, desde que o Windows Server 2012R2 não tem contadores de desempenho.  Você deve selecionar dois computadores com as mesmas características, ou atualizar um computador e comparar os resultados após a atualização.  Adendo de medições de tempo do Windows tem mais informações sobre como fazer medições detalhadas entre 2016 e 2012.

Usando os todos os w32time contadores de desempenho, colete dados pelo menos uma semana.  Isso garantirá que você tem suficiente de uma referência para levar em conta vários na rede ao longo do tempo e suficiente de uma execução para fornecer a confiança de que sua precisão de tempo é estável.

### <a name="ntp-server-redundancy"></a>NTP Server redundância
Para configuração manual do servidor NTP usada com computadores conectados sem domínio ou o PDC, ter mais de um servidor é uma medida de boa redundância em caso de disponibilidade.  Ele também pode dar melhor precisão, pressupondo que todas as fontes são precisos e estáveis.  No entanto, se a topologia não está bem projetada ou as fontes de tempo não são estáveis, a precisão resultante pode ser pior para cuidado.  O limite de tempo com suporte w32time de servidores pode referenciar manualmente é 10. 

## <a name="leap-seconds"></a>Segundos de salto
Período de rotação da Terra varia de acordo com o tempo, causado por eventos climatic e geológico. Normalmente, a variação é um segundo cada dois anos. Sempre que a variação de tempo atomic ficar muito grande, uma correção de um segundo (para cima ou para baixo) é inserida, chamado uma fração de segundo. Isso é feito de forma que a diferença nunca excede 0,9 segundos. Essa correção é anunciada seis meses à frente a correção real. Antes do Windows Server 2016, o serviço de tempo da Microsoft não reconhecia de salto segundos, mas dependia o serviço de horário externo cuidar disso. Com a precisão de cada vez maior de Windows Server 2016, a Microsoft está trabalhando em uma solução mais adequada para o problema de fração de segundo.

## <a name="secure-time-seeding"></a>Proteger a propagação de tempo
W32Time no servidor 2016 inclui o recurso segura propagação de tempo. Esse recurso determina o tempo aproximado atual de saída conexões SSL.  Esse valor de hora é usado para monitorar o relógio do sistema local e corrija os erros brutos. Você pode ler mais sobre o recurso no [esta postagem no blog](https://blogs.msdn.microsoft.com/w32time/2016/09/28/secure-time-seeding-improving-time-keeping-in-windows/).  Em implantações com uma fonte de tempo confiável e bem monitoradas máquinas que incluem o monitoramento de deslocamentos de tempo, você pode optar por não usar o recurso segura propagação de tempo e dependem de sua infraestrutura existente em vez disso. 

Você pode desabilitar o recurso com estas etapas:

1.  Defina o valor de configuração do registro UtilizeSSLTimeData como 0 em uma máquina específica:

    reg adicionar HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Services\w32time\Config /v UtilizeSslTimeData /t REG_DWORD /d 0 /f


2.  Se você conseguir reinicializar o computador imediatamente devido a algum motivo, você pode notificar serviço W32time sobre a atualização de configuração. Isso impede que o monitoramento de tempo e coletados com base nos dados de tempo de imposição de conexões SSL. 

    W32tm.exe /config /update

3.  Reiniciar a máquina torna a configuração efetiva imediatamente e também faz com que ele interromper a coleta de quaisquer dados de tempo de conexões SSL.  A última parte tem uma sobrecarga muito pequena e não deve ser uma preocupação perf.

4.  Para aplicar essa configuração em um domínio inteiro, defina o valor de UtilizeSSLTimeData na configuração de política de grupo W32time como 0 e a configuração de publicação.  Quando a configuração é escolhida por um cliente de política de grupo, serviço W32time é notificado e ele irá parar monitoramento de tempo e a imposição de uso de dados de tempo SSL. A coleta de dados de tempo SSL irá parar quando cada computador for reiniciado. Se o seu domínio tiver portátil fino notebooks/tablets e outros dispositivos, convém excluir tais máquinas dessa alteração de política. Esses dispositivos eventualmente fica voltada para consumo de bateria e precisa a propagação de tempo segura de recursos para inicializar seu tempo.














 













 




