---
title: Aprimoramentos do Windows Server 2016
description: O Windows Server 2016 aprimorou os algoritmos usados para corrigir a hora e a condição do relógio local para sincronização com o UTC.
author: dcuomo
ms.author: dacuo
manager: dougkim
ms.date: 10/17/2018
ms.topic: article
ms.prod: windows-server
ms.technology: networking
ms.openlocfilehash: 2723868251f90429fb0ad5e966c9222a6a22ab0c
ms.sourcegitcommit: 3a3d62f938322849f81ee9ec01186b3e7ab90fe0
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/23/2020
ms.locfileid: "77520638"
---
# <a name="windows-server-2016-improvements"></a>Aprimoramentos do Windows Server 2016

Este artigo inclui informações sobre os aprimoramentos feitos no Windows Server 2016.

## <a name="windows-time-service-and-ntp"></a>Serviço de Horário do Windows e NTP

O Windows Server 2016 aprimorou os algoritmos usados para corrigir a hora e a condição do relógio local para sincronização com o UTC. O NTP usa quatro valores para calcular a diferença de horário, com base nos carimbos de data/hora da solicitação/da resposta do cliente e da solicitação/da resposta do servidor. No entanto, as redes apresentam ruído e pode haver picos nos dados do NTP devido ao congestionamento da rede e a outros fatores que afetam a latência da rede. Os algoritmos do Windows 2016 equilibram esse ruído usando várias técnicas diferentes que resultam em um relógio estável e preciso. Além disso, a fonte que usamos para a hora precisa referencia uma API aprimorada que nos oferece uma melhor resolução. Com esses aprimoramentos, podemos alcançar uma precisão de 1 ms com relação ao UTC em um domínio.

## <a name="hyper-v"></a>Hyper-V

O Windows 2016 aprimorou o serviço TimeSync do Hyper-V. Os aprimoramentos incluem uma hora inicial mais precisa na inicialização ou na recuperação da VM e a correção da latência de interrupção para as amostras fornecidas ao w32time. Esse aprimoramento permite nos manter dentro de 10 μs do host com uma RMS (Raiz Quadrada Média, que indica variância) igual a 50 μs, mesmo em um computador com 75% de carga. Para obter mais informações, confira [Arquitetura do Hyper-V](https://msdn.microsoft.com/library/cc768520.aspx).

> [!NOTE]
> A carga foi criada com o parâmetro de comparação prime95 usando o perfil equilibrado.

Além disso, o nível de camada que o host relata para o convidado é mais transparente. Anteriormente, o host apresentava uma camada fixa igual a 2, independentemente da precisão. Com as alterações do Windows Server 2016, o host relata a primeira camada maior que a camada do host, o que resulta em um tempo melhor para os convidados virtuais. A camada do host é determinada pelo w32time em meio normal com base no tempo de origem. Os convidados do Windows 2016 ingressados no domínio encontrarão o relógio mais preciso, em vez de usar o padrão do host. Foi por esse motivo que aconselhamos desabilitar manualmente a configuração do Provedor de Tempo do Hyper-V nos computadores que participam de um domínio no Windows 2012 R2 e inferior.

## <a name="monitoring"></a>Monitoramento
Contadores do monitor de desempenho foram adicionados. Eles permitem que você defina a linha de base, monitore e solucione problemas de precisão de tempo. Esses contadores incluem:

|Contador|Descrição|
|----- | ----- |
|Diferença de Horário Calculado| A diferença de horário absoluto entre o relógio do sistema e a fonte de horário escolhida, conforme calculado pelo Serviço W32Time em microssegundos. Quando uma nova amostra válida fica disponível, o horário calculado é atualizado com a diferença de horário indicada pela amostra. Essa é a diferença de horário real do relógio local. O W32Time inicia a correção do relógio usando essa diferença e atualiza o horário calculado entre as amostras com a diferença de horário restante que precisa ser aplicada ao relógio local. A precisão do relógio pode ser acompanhada por meio desse contador de desempenho com um intervalo de sondagem baixo (por exemplo, 256 segundos ou menos) e buscando o valor do contador como menor do que o limite desejado de precisão do relógio.|
|Ajuste de Frequência do Relógio| O ajuste de frequência do relógio absoluto feito no relógio do sistema local pelo W32Time em partes por bilhão. Esse contador ajuda a visualizar as ações que estão sendo executadas pelo W32Time.|
|Atraso de Ida e Volta NTP| O atraso de ida e volta mais recente enfrentado pelo cliente NTP no recebimento de uma resposta do servidor em microssegundos. Esse é o tempo decorrido no cliente NTP entre a transmissão de uma solicitação para o servidor NTP e o recebimento de uma resposta válida do servidor. Esse contador ajuda a caracterizar os atrasos experimentados pelo cliente NTP. Viagens de ida e volta maiores ou variáveis podem adicionar ruído aos cálculos de tempo do NTP, o que, por sua vez, pode afetar a precisão da sincronização de horário por meio do NTP.|
|Contagem de Fontes do Cliente NTP| Número ativo de fontes de horário do NTP usadas pelo cliente NTP. Essa é uma contagem dos endereços IP ativos e distintos de servidores de horário que respondem às solicitações desse cliente. Esse número pode ser maior ou menor do que os pares configurados, dependendo da resolução DNS de nomes de pares e da capacidade de alcance atual.|
|Solicitações de Entrada do Servidor NTP| Número de solicitações recebidas pelo servidor NTP (solicitações/s).|
|Respostas de Saída do Servidor NTP| Número de solicitações respondidas pelo servidor NTP (respostas/s).|

Os três primeiros contadores se destinam a cenários de solução de problemas de precisão. A seção Solução de problemas de precisão de tempo e NTP abaixo, em Melhores práticas, traz mais detalhes.
Os últimos três contadores abrangem cenários do servidor NTP e são úteis quando determinam a carga e a linha de base do desempenho atual.

## <a name="configuration-updates-per-environment"></a>Atualizações de configuração por ambiente
Veja a seguir uma descrição das alterações na configuração padrão entre o Windows 2016 e as versões anteriores para cada função. As configurações do Windows Server 2016 e da Atualização de Aniversário do Windows 10 (build 14393) agora são exclusivas e é por isso que elas são mostradas como colunas separadas.

|Função|Configuração|Windows Server 2016|Windows 10|Windows Server 2012 R2<br>Windows Server 2008 R2<br>Windows 10|
|---|---|---|---|---|
|**Autônomo/Nano Server**||||
| |*Servidor de Horário*|time.windows.com|NA|time.windows.com|
| |*Frequência de Sondagem*|64 a 1.024 segundos|NA|Uma vez por semana|
| |*Frequência de Atualização do Relógio*|Uma vez por segundo|NA|Uma vez por hora|
|**Cliente Autônomo**||||
| |*Servidor de Horário*|NA|time.windows.com|time.windows.com|
| |*Frequência de Sondagem*|NA|Uma vez por dia|Uma vez por semana|
| |*Frequência de Atualização do Relógio*|NA|Uma vez por dia|Uma vez por semana|
|**Controlador de Domínio**||||
| |*Servidor de Horário*|Controlador de domínio primário/GTIMESERV|NA|Controlador de domínio primário/GTIMESERV|
| |*Frequência de Sondagem*|64 a 1.024 segundos|NA|1\.024 a 32.768 segundos|
| |*Frequência de Atualização do Relógio*|Uma vez por dia|NA|Uma vez por semana|
|**Servidor Membro do Domínio**||||
| |*Servidor de Horário*|DC|NA|DC|
| |*Frequência de Sondagem*|64 a 1.024 segundos|NA|1\.024 a 32.768 segundos|
| |*Frequência de Atualização do Relógio*|Uma vez por segundo|NA|Uma vez a cada 5 minutos|
|**Cliente Membro do Domínio**||||
| |*Servidor de Horário*|NA|DC|DC|
| |*Frequência de Sondagem*|NA|1\.204 a 32.768 segundos|1\.024 a 32.768 segundos|
| |*Frequência de Atualização do Relógio*|NA|Uma vez a cada 5 minutos|Uma vez a cada 5 minutos|
|**Convidado do Hyper-V**||||
| |*Servidor de Horário*|Escolhe a melhor opção com base na camada do host e no servidor de horário|Escolhe a melhor opção com base na camada do host e no servidor de horário|Usa o host como padrão|
| |*Frequência de Sondagem*|Com base na função acima|Com base na função acima|Com base na função acima|
| |*Frequência de Atualização do Relógio*|Com base na função acima|Com base na função acima|Com base na função acima|

>[!NOTE]
>Para o Linux no Hyper-V, confira a seção Como permitir que o Linux use a hora do host do Hyper-V abaixo.

## <a name="impact-of-increased-polling-and-clock-update-frequency"></a>Impacto da maior frequência de sondagem e de atualização do relógio

Para fornecer uma hora mais precisa, os padrões de frequências de sondagem e atualizações do relógio são aumentados, o que nos permite fazer pequenos ajustes com mais frequência. Isso causará mais tráfego UDP/NTP; no entanto, esses pacotes são pequenos, devendo haver muito pouco ou nenhum impacto sobre os links de banda larga. No entanto, o benefício é que o tempo deverá ser melhor em uma variedade maior de hardware e ambientes.

Em dispositivos com suporte de bateria, o aumento da frequência de sondagem pode causar problemas. Os dispositivos de bateria não armazenam a hora quando estão desligados. Quando eles são retomados, pode ser necessário correções frequentes no relógio. O aumento da frequência de sondagem fará com que o relógio fique instável e também poderá usar mais energia. A Microsoft recomenda que você não altere as configurações padrão do cliente.

Os controladores de domínio devem ser minimamente afetados mesmo com o efeito multiplicado do aumento das atualizações de clientes NTP em um domínio do AD. O NTP tem um consumo de recursos muito menor em comparação com outros protocolos e um impacto marginal. É mais provável que você alcance os limites de outras funcionalidades do domínio antes de ser afetado pelo aumento das configurações do Windows Server 2016. O Active Directory usa o NTP seguro, que tende a sincronizar a hora de maneira menos precisa do que o NTP simples, mas verificamos que ele atingirá os clientes que estão a duas camadas de distância do controlador de domínio primário.

Como um plano conservador, você deverá reservar 100 solicitações de NTP por segundo e por núcleo. Por exemplo, um domínio composto por quatro controladores de domínio com quatro núcleos cada, você deverá conseguir atender a 1.600 solicitações de NTP por segundo. Se você tiver 10 mil clientes configurados para sincronizar a hora uma vez a cada 64 segundos e as solicitações forem recebidas de maneira uniforme ao longo do tempo, verá 10.000/64 ou cerca de 160 solicitações/segundo, distribuídas por todos os controladores de domínio. Isso se enquadra facilmente dentro de nossas 1.600 solicitações de NTP/s com base neste exemplo. Essas são recomendações de planejamento conservadoras e, obviamente, têm uma grande dependência da rede, das velocidades e das cargas do processador, assim como sempre da linha de base e do teste nos ambientes.

Também é importante observar que, se os controladores de domínio estiverem em execução com uma carga considerável de CPU, maior que 40%, isso certamente adicionará ruído às respostas do NTP e afetará a precisão de tempo no domínio. Novamente, você precisa fazer o teste em seu ambiente para entender os resultados reais.

## <a name="time-accuracy-measurements"></a>Medidas de precisão de tempo
### <a name="methodology"></a>Metodologia
Para medir a precisão de tempo no Windows Server 2016, usamos uma variedade de ferramentas, métodos e ambientes. Você pode usar essas técnicas para medir e ajustar seu ambiente e determinar se os resultados da precisão atendem às suas necessidades. 

Nosso relógio de origem de domínio consistiu em dois servidores NTP de alta precisão com hardware de GPS. Também usamos um computador de teste de referência separado para medições, que também tinha um hardware de GPS de alta precisão instalado de outro fabricante. Para alguns dos testes, você precisará de uma fonte de relógio precisa e confiável para usar como referência, além da fonte de relógio do domínio.

Usamos quatro métodos diferentes para medir a precisão com máquinas virtuais e computadores físicos. Vários métodos forneceram meios independentes para validar os resultados.

1. Medir o relógio local, que é condicionado por w32tm, em nosso computador de teste de referência que tem o hardware de GPS separado. 
2. Medir os pings do NTP do servidor NTP para os clientes usando “stripchart” do W32tm
3. Medir os pings do NTP do cliente para o servidor NTP usando “stripchart” do W32tm
4. Medir os resultados do Hyper-V do host para o convidado usando o TSC (Contador de Carimbos de Data/Hora). Esse contador é compartilhado entre ambas as partições e a hora do sistema em ambas as partições. Calculamos a diferença entre a hora do host e a hora do cliente na máquina virtual. Em seguida, usamos o relógio de TSC para interpolar a hora do host por meio do convidado, pois as medidas não acontecem ao mesmo tempo. Além disso, usamos o relógio de TSV para considerar os atrasos e a latência na API.

O W32tm é interno, mas as outras ferramentas que usamos durante nossos testes estão disponíveis para o repositório da Microsoft no GitHub como software livre para teste e uso. O wiki no repositório traz mais informações que descrevem como usar as ferramentas para medidas.

> [https://github.com/Microsoft/Windows-Time-Calibration-Tools](https://github.com/Microsoft/Windows-Time-Calibration-Tools)

Os resultados do teste mostrados abaixo são um subconjunto de medidas que fizemos em um dos ambientes de teste. Eles ilustram a precisão mantida no início da hierarquia de tempo e o cliente de domínio filho no final da hierarquia de tempo. Isso é comparado com os mesmos computadores em uma topologia baseada em 2012 para comparação.

### <a name="topology"></a>Topologia
Para comparação, testamos uma topologia baseada no Windows Server 2012 R2 e no Windows Server 2016. Ambas as topologias consistem em dois computadores host físicos do Hyper-V que referenciam um computador Windows Server 2016 com o hardware de relógio de GPS instalado. Cada host executa três convidados do Windows ingressados no domínio, que são organizados de acordo com a topologia a seguir. As linhas representam a hierarquia de tempo e o protocolo/o transporte usado.

![Tempo do Windows](../media/Windows-Time-Service/Windows-2016-Accurate-Time/topology1.png)

![Tempo do Windows](../media/Windows-Time-Service/Windows-2016-Accurate-Time/topology2.png)

### <a name="graphical-results-overview"></a>Visão geral dos resultados gráficos
Os dois grafos a seguir representam a precisão de tempo de dois membros específicos em um domínio baseado na topologia acima. Cada grafo exibe os resultados do Windows Server 2012 R2 e 2016 sobrepostos, o que demonstra os aprimoramentos visualmente. A precisão foi medida no computador convidado em comparação com o host. Os dados gráficos representam um subconjunto de todo o conjunto de testes que fizemos e mostra os cenários de melhor e pior caso. 

![Tempo do Windows](../media/Windows-Time-Service/Windows-2016-Accurate-Time/topology3.png)

### <a name="performance-of-the-root-domain-pdc"></a>Desempenho do controlador de domínio primário do domínio raiz
O controlador de domínio primário raiz é sincronizado com o host do Hyper-V (usando o VMIC), que é um Windows Server 2016 com um hardware de GPS comprovado como sendo preciso e estável. Esse é um requisito crítico para uma precisão de 1 ms, que é mostrada como a área sombreada verde.

![Tempo do Windows](../media/Windows-Time-Service/Windows-2016-Accurate-Time/chart1.png)

### <a name="performance-of-the-child-domain-client"></a>Desempenho do cliente de domínio filho
O cliente de domínio filho é anexado a um controlador de domínio primário de domínio filho que se comunica com o controlador de domínio primário raiz. O tempo também está dentro do requisito de 1 ms.

![Tempo do Windows](../media/Windows-Time-Service/Windows-2016-Accurate-Time/chart2.png)

### <a name="long-distance-test"></a>Teste de longa distância
O gráfico a seguir compara 1 salto de rede virtual para 6 saltos de rede física com o Windows Server 2016. Dois gráficos são sobrepostos entre si com transparência para mostrar os dados sobrepostos. O aumento de saltos de rede significa uma latência mais alta e maiores desvios de tempo. O gráfico é ampliado e, portanto, os limites de 1 ms, representados pela área verde, são maiores. Como você pode ver, o tempo ainda está dentro de 1 ms com vários saltos. Ele é deslocado negativamente, o que demonstra uma assimetria da rede. É claro que cada rede é diferente e as medidas dependem de uma infinidade de fatores ambientais.

![Tempo do Windows](../media/Windows-Time-Service/Windows-2016-Accurate-Time/chart3.png)

## <a name="best-practices-for-accurate-timekeeping"></a>Melhores práticas para a manutenção de hora precisa
### <a name="solid-source-clock"></a>Relógio de origem sólido
A hora de um computador só é tão boa quanto o relógio de origem com o qual ela é sincronizada. Para atingir 1 ms de precisão, você precisará do hardware de GPS ou de um dispositivo de tempo em sua rede referenciado como o relógio de origem mestre. O uso do padrão de time.windows.com poderá não fornecer uma fonte de horário local e estável. Além disso, à medida que você ficar mais distante do relógio de origem, a rede afetará a precisão. Ter um relógio de origem mestre em cada data center é necessário para obter a melhor precisão.

### <a name="hardware-gps-options"></a>Opções de hardware de GPS
Há várias soluções de hardware que podem oferecer uma hora precisa. Em geral, as soluções atuais são baseadas em antenas de GPS. Também há soluções de modem de rádio e de conexão discada usando linhas dedicadas. Elas se conectam à rede como um dispositivo ou são conectadas a um computador, por exemplo, Windows por meio de um dispositivo PCIe ou USB. Diferentes opções fornecerão diferentes níveis de precisão e, como sempre, os resultados dependem do ambiente. As variáveis que afetam a precisão incluem disponibilidade de GPS, estabilidade e carga da rede e hardware de computador. Todos esses são fatores importantes ao escolher um relógio de origem, que, como dissemos, é um requisito para uma hora estável e precisa.

### <a name="domain-and-synchronizing-time"></a>Domínio e tempo de sincronização
Os membros do domínio usam a hierarquia de domínio para determinar qual computador eles usam como uma fonte para sincronizar a hora. Cada membro do domínio encontrará outro computador para sincronização e o salvará como a fonte de relógio. Cada tipo de membro do domínio segue um conjunto diferente de regras para encontrar uma fonte de relógio para sincronização de hora. O controlador de domínio primário na raiz da floresta é a fonte de relógio padrão para todos os domínios. Abaixo estão listadas diferentes funções e uma descrição de alto nível de como elas encontram uma fonte:


- **Controlador de domínio com função de controlador de domínio primário**: esse computador é a fonte de horário autoritativa para um domínio. Ele terá a hora mais precisa disponível no domínio e precisará ser sincronizado com um controlador de domínio no domínio pai, exceto nos casos em que a função GTIMESERV está habilitada.
- **Qualquer outro controlador de domínio**: esse computador funcionará como uma fonte de horário para clientes e servidores membro no domínio. Um controlador de domínio pode ser sincronizado com o controlador de domínio primário do próprio domínio ou com qualquer controlador de domínio no domínio pai.
- **Clientes/servidores membro**: esse computador pode ser sincronizado com qualquer controlador de domínio ou controlador de domínio primário do próprio domínio ou com um controlador de domínio ou um controlador de domínio primário no domínio pai.

De acordo com os candidatos disponíveis, um sistema de pontuação é usado para encontrar a melhor fonte de horário. Esse sistema leva em conta a confiabilidade da fonte de horário e a localização relativa. Isso ocorre uma vez quando a hora é o serviço iniciado. Caso precise ter um controle mais refinado de como o tempo é sincronizado, adicione bons servidores de horário em localizações específicas ou adicione redundância. Confira a seção [Especificar um serviço de hora confiável local usando o GTIMESERV] para obter mais informações.

#### <a name="mixed-os-environments-win2012r2-and-win2008r2"></a>Ambientes de sistema operacional misto (Win2012R2 e Win2008R2)
Embora um ambiente de domínio puro do Windows Server 2016 seja necessário para obter a melhor precisão, ainda há benefícios no uso de um ambiente misto. A implantação do Hyper-V do Windows Server 2016 em um domínio do Windows 2012 beneficiará os convidados devido ao aprimoramentos mencionados acima, mas somente se os convidados também forem o Windows Server 2016. Um controlador de domínio primário do Windows Server 2016 poderá fornecer um tempo mais preciso devido aos algoritmos aprimorados e ele será uma fonte mais estável. Como a substituição do controlador de domínio primário pode não ser uma opção, adicione um controlador de domínio do Windows Server 2016 com o conjunto de roll GTIMESERV que será uma atualização na precisão para o domínio. Um controlador de domínio do Windows Server 2016 pode fornecer um tempo melhor para os clientes de horário downstream; no entanto, ele só é tão bom quanto o tempo do NTP de origem.

Além disso, conforme mencionado acima, as frequências de sondagem e atualização do relógio foram modificadas com o Windows Server 2016. Elas podem ser alteradas manualmente para os controladores de domínio de nível inferior ou aplicadas por meio da Política de Grupo. Embora não tenhamos testado essas configurações, elas deverão se comportar bem no Win2008R2 e no Win2012R2 e fornecer alguns benefícios.

As versões anteriores ao Windows Server 2016 apresentaram vários problemas na manutenção de hora precisa, o que resultou no descompasso da hora do sistema imediatamente após um ajuste. Por isso, a obtenção de amostras de tempo de uma origem NTP precisa com frequência e o condicionamento do relógio local com os dados levam a um descompasso menor nos relógios do sistema no período de intra-amostragem, resultando em uma melhor manutenção de hora precisa nas versões de sistema operacional de nível inferior. A melhor precisão observada foi de aproximadamente 5 ms quando um cliente NTP do Windows Server 2012 R2, definido com as configurações de alta precisão, sincronizou a hora em um servidor NTP preciso do Windows 2016.

Em alguns cenários que envolvem controladores de domínio convidados, as amostras de TimeSync do Hyper-V podem atrapalhar a sincronização de hora do domínio. Isso não deverá ser mais um problema para os convidados do Server 2016 em execução em hosts do Hyper-V do Server 2016.

Para desabilitar o serviço TimeSync do Hyper-V em fornecer amostras para o w32time, defina a seguinte chave do Registro do convidado:

 HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Services\W32Time\TimeProviders\VMICTimeProvider "Enabled"=dword:00000000

#### <a name="allowing-linux-to-use-hyper-v-host-time"></a>Como permitir que o Linux use a hora do host do Hyper-V
Nos convidados do Linux em execução no Hyper-V, normalmente, os clientes são configurados para usar o daemon NTP para a sincronização de hora em servidores NTP. Se a distribuição do Linux der suporte ao protocolo TimeSync versão 4 e o convidado do Linux tiver o serviço de integração TimeSync habilitado, ele será sincronizado em relação à hora do host. Isso pode levar a uma manutenção de hora inconsistente se ambos os métodos estão habilitados.

Para sincronização exclusiva em relação à hora do host, é recomendável desabilitar a sincronização de hora do NTP seguindo uma destas opções:

- Desabilitar os servidores NTP no arquivo ntp.conf
- ou desabilitar o daemon NTP

Nessa configuração, o parâmetro do servidor de horário é esse host. A Frequência de Sondagem é de 5 segundos e a Frequência de Atualização do Relógio também é de 5 segundos.

Para sincronização exclusiva no NTP, é recomendável desabilitar o serviço de integração TimeSync no convidado.

> [!NOTE]
> Observação: O suporte para a hora precisa com os convidados do Linux exige um recurso que só é compatível com os kernels do Linux upstream mais recentes e não é algo que já está amplamente disponível em todas as distribuições do Linux ainda. Veja [Máquinas virtuais do Linux e do FreeBSD compatíveis com o Hyper-V no Windows](https://technet.microsoft.com/windows-server-docs/virtualization/hyper-v/supported-linux-and-freebsd-virtual-machines-for-hyper-v-on-windows) para obter mais detalhes sobre as distribuições de suporte.

#### <a name="specify-a-local-reliable-time-service-using-gtimeserv"></a>Especificar um serviço de hora confiável local usando o GTIMESERV
Você pode especificar um ou mais controladores de domínio como relógios de origem precisos usando sinalizadores do GTIMESERV, o Servidor de Horário Certo. Por exemplo, os controladores de domínio específicos equipados com hardware de GPS podem ser sinalizados como um GTIMESERV. Isso garantirá que o domínio referencie um relógio com base no hardware de GPS.

> [!NOTE]
> Encontre mais informações sobre os sinalizadores de domínio na documentação do protocolo [MS-ADTS](https://msdn.microsoft.com/library/mt226583.aspx).

TIMESERV é outro Sinalizador de Serviços de Domínio relacionado que indica se um computador está autoritativo no momento, o que poderá ser alterado se um controlador de domínio perder a conexão. Um controlador de domínio nesse estado retornará “Camada Desconhecida” quando consultado via NTP. Depois de tentar várias vezes, o controlador de domínio registrará o Evento 36 do Serviço de Hora de Evento do Sistema.

Caso você deseje configurar um controlador de domínio como um GTIMESERV, isso poderá ser configurado manualmente usando o comando a seguir. Nesse caso, o controlador de domínio está usando outros computadores como o relógio mestre. Isso pode ser um dispositivo ou um computador dedicado.

 w32tm /config /manualpeerlist:"master_clock1,0x8 master_clock2,0x8" /syncfromflags:manual /reliable:yes /update

> [!NOTE]
> Para obter mais informações, confira [Configurar o Serviço de Horário do Windows](https://technet.microsoft.com/library/cc731191.aspx)

Se o controlador de domínio tiver o hardware de GPS instalado, você precisará usar estas etapas para desabilitar o cliente NTP e habilitar o servidor NTP.

Comece desabilitando o cliente NTP e habilite o servidor NTP usando essas alterações de chave do Registro.

 reg add HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Services\w32time\TimeProviders\NtpClient /v Enabled /t REG_DWORD /d 0 /f

 reg add HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Services\w32time\TimeProviders\NtpServer /v Enabled /t REG_DWORD /d 1 /f

Em seguida, reinicie o Serviço de Horário do Windows

 net stop w32time && net start w32time

Por fim, indique que esse computador tem uma fonte de horário confiável usando.
  
 w32tm /config /reliable:yes /update

Para verificar se as alterações foram feitas corretamente, execute os comandos a seguir que afetam os resultados mostrados abaixo. 

 w32tm /query /configuration

Valor|Configuração esperada|
----- | ----- |
AnnounceFlags| 5 (Local)|
NtpServer |(Local)|
DllName |C:\WINDOWS\SYSTEM32\w32time.DLL (Local)|
Habilitada |1 (Local)|
NtpClient| (Local)|

 w32tm /query /status /verbose

Valor| Configuração esperada|
----- | ----- |
Camada| 1 (referência primária – sincronizada por relógio de rádio)|
ReferenceId| 0x4C4F434C (nome de origem: "LOCAL")|
Origem| Relógio CMOS local|
Deslocamento de fase| 0,0000000s|
Função de servidor| 576 (Servidor de Tempo Confiável)|

#### <a name="windows-server-2016-on-3rd-party-virtual-platforms"></a>Windows Server 2016 em plataformas virtuais de terceiros
Quando o Windows é virtualizado, por padrão, o hipervisor é responsável por fornecer a hora. Porém, os membros ingressados no domínio precisam ser sincronizados com o controlador de domínio para que o Active Directory funcione corretamente. É melhor desabilitar qualquer virtualização de tempo entre o convidado e o host de qualquer plataforma virtual de terceiros.

#### <a name="discovering-the-hierarchy"></a>Como descobrir a hierarquia
Como a cadeia de hierarquia de tempo para a fonte do relógio mestre é dinâmica em um domínio e negociada, você precisará consultar o status de um computador específico para entender a fonte de horário e a cadeia para o relógio de origem mestre. Isso pode ajudar a diagnosticar problemas de sincronização de hora.

Considerando que você deseja solucionar problemas de um cliente específico, a primeira etapa é entender a fonte de horário usando este comando do w32tm.

 w32tm /query /status

Os resultados exibem a origem, entre outras coisas. A origem indica com quem você sincroniza a hora no domínio. Essa é a primeira etapa desta hierarquia de tempo dos computadores.
Em seguida, use a entrada da fonte acima e o parâmetro /StripChart para encontrar a próxima fonte de horário na cadeia.

 w32tm /stripchart /computer:MySourceEntry /packetinfo /samples:1

Também útil, o comando a seguir lista cada controlador de domínio que ele pode encontrar no domínio especificado e imprime um resultado que permite que você determine cada parceiro. Esse comando incluirá os computadores que foram configurados manualmente.
 
 w32tm /monitor /domain:my_domain

Usando a lista, você pode rastrear os resultados por meio do domínio e entender a hierarquia, bem como a diferença de horário em cada etapa. Ao localizar o ponto em que a diferença de horário fica significativamente pior, você pode identificar a raiz do horário incorreto. Daí em diante, você poderá tentar entender o motivo pelo qual esse horário está incorreto ativando o log do w32tm.

#### <a name="using-group-policy"></a>Como usar a Política de Grupo
Você pode usar a Política de Grupo para atingir uma precisão mais estrita, por exemplo, atribuir clientes para usar servidores NTP específicos ou controlar como os sistemas operacionais de nível inferior são configurados quando virtualizados. Abaixo está uma lista de possíveis cenários e configurações relevantes da Política de Grupo:

**Domínios Virtualizados**: para controlar os controladores de domínio virtualizados no Windows 2012 R2 para que eles sincronizem a hora com o domínio, não com o host do Hyper-V, você pode desabilitar essa entrada do Registro.  Para o controlador de domínio primário, o ideal é não desabilitar a entrada, pois o host do Hyper-V fornecerá a fonte de horário mais estável. A entrada do Registro exigirá que você reinicie o serviço w32time depois que ele for alterado.

 [HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Services\W32Time\TimeProviders\VMICTimeProvider] "Enabled"=dword:00000000

**Cargas Sensíveis à Precisão**: para cargas de trabalho sensíveis à precisão de tempo, você pode configurar grupos de computadores para definir os servidores NTP e as configurações de hora relacionadas, como a frequência de sondagem e atualização do relógio. Isso normalmente é tratado pelo domínio, mas para obter mais controle, você poderá definir computadores específicos como destino para apontá-los diretamente para o relógio mestre.

Configuração da Diretiva de Grupo| Novo valor|
----- | ----- |
NtpServer| ClockMasterName,0x8|
MinPollInterval| 6 a 64 segundos|
MaxPollInterval| 6|
UpdateInterval| 100 – uma vez por segundo|
EventLogFlags| 3 – Todo o log de tempo especial|

> [!NOTE]
> As configurações de NtpServer e EventLogFlags estão localizadas em System\Windows Time Service\Time Providers usando as configurações Configurar o cliente NTP do Windows. As outras três estão localizadas em System\Windows Time Service usando as definições de Configuração Global.

**Remoto de Cargas Sensíveis à Precisão Remota**: para os sistemas em domínios de filial, por exemplo, para Varejo e PCI (Setor de Crédito de Pagamento), o Windows usa as informações do site atual e o localizador de controlador de domínio para encontrar um controlador de domínio local, a menos que haja uma fonte de horário NTP manual configurada. Esse ambiente exige 1 segundo de precisão, que usa a convergência mais rápida para a hora correta. Essa opção permite que o serviço w32time atrase o relógio. Se isso for aceitável e atender aos seus requisitos, você poderá criar a política a seguir.  Assim como ocorre com qualquer ambiente, lembre-se de testar a sua rede e definir a linha de base dela. 

Configuração da Diretiva de Grupo| Novo valor|
----- | ----- |
MaxAllowedPhaseOffset| 1, se houver mais de um segundo, defina o relógio com a hora correta.|

A configuração MaxAllowedPhaseOffset está localizada em System\Windows Time Service usando as definições de Configuração Global.

> [!NOTE]
> Para obter mais informações sobre a Política de Grupo e as entradas relacionadas, confira [Ferramentas do Serviço de Horário do Windows](windows-time-service-tools-and-settings.md) e o artigo Configurações no TechNet.

## <a name="azure-and-windows-iaas-considerations"></a>Considerações sobre IaaS do Windows e do Azure

### <a name="azure-virtual-machine-active-directory-domain-services"></a>Máquina Virtual do Azure: Active Directory Domain Services
Se a VM do Azure que executa o Active Directory Domain Services fizer parte de uma floresta local existente do Active Directory, o TimeSync (VMIC) deverá ser desabilitado. Isso serve para permitir que todos os controladores de domínio na floresta, físicos e virtuais, usem uma só hierarquia de sincronização de hora. Veja o white paper de melhor prática [“Como executar controladores de domínio no Hyper-V”](https://technet.microsoft.com/library/virtual_active_directory_domain_controller_virtualization_hyperv.aspx)

### <a name="azure-virtual-machine-domain-joined-machine"></a>Máquina Virtual do Azure: Computador ingressado no domínio
Se você estiver hospedando um computador que está ingressado no domínio em uma floresta existente do Active Directory, virtual ou física, a melhor prática será desabilitar a sincronização de hora para o convidado e garantir que o W32Time esteja configurado para sincronizar com o controlador de domínio por meio da configuração da hora como Type=NTP5

### <a name="azure-virtual-machine-standalone-workgroup-machine"></a>Máquina Virtual do Azure: Computador de grupo de trabalho autônomo
Se a VM do Azure não estiver ingressada em um domínio nem for um controlador de domínio, a recomendação será manter a configuração de hora padrão e fazer com que a VM seja sincronizada com o host.

## <a name="windows-application-requiring-accurate-time"></a>Aplicativo do Windows exigindo a hora precisa
### <a name="time-stamp-api"></a>API de carimbo de data/hora
Os programas que exigem a maior precisão em relação ao UTC, e não à passagem de tempo, devem usar a API [GetSystemTimePreciseAsFileTime](https://msdn.microsoft.com/library/windows/desktop/Hh706895.aspx). Isso garante que o aplicativo obtenha a Hora do Sistema, que é condicionada pelo Serviço de Horário do Windows.

### <a name="udp-performance"></a>Desempenho do UDP
Se você tiver um aplicativo que use a comunicação UDP para transações e for importante minimizar a latência, haverá algumas entradas do Registro relacionadas que você poderá usar para configurar um intervalo de portas a serem excluídas da porta do mecanismo de filtragem base. Isso aprimorará a latência e aumentará a taxa de transferência. No entanto, as alterações no Registro devem ser limitadas aos administradores experientes. Além disso, essa solução alternativa exclui as portas de serem protegidas pelo firewall. Confira a referência do artigo abaixo para obter mais informações.

Para o Windows Server 2012 e o Windows Server 2008, você precisará instalar um hotfix primeiro. Veja este artigo da base de dados: [Perda de datagrama ao executar um aplicativo receptor multicast no Windows 8 e no Windows Server 2012](https://support.microsoft.com/kb/2808584)

### <a name="update-network-drivers"></a>Atualizar drivers de rede
Alguns fornecedores de rede têm atualizações de driver que aprimoram o desempenho com relação à latência do driver e ao buffer de pacotes UDP. Contate o fornecedor da rede para ver se há atualizações para ajudar com a taxa de transferência UDP.

## <a name="logging-for-auditing-purposes"></a>Log para fins de auditoria
Para cumprir as regulamentações de rastreamento de tempo, você poderá arquivar manualmente os logs do w32tm, os logs de eventos e as informações do monitor de desempenho. Posteriormente, as informações arquivadas poderão ser usadas para atestar a conformidade em um momento específico no passado. Os fatores a seguir são usados para indicar a precisão.

1. Precisão do relógio usando o contador de monitor de desempenho Diferença de Horário Calculado. Isso mostra o relógio com a precisão desejada.
2. Fonte do relógio que procura “resposta de par de” nos logs do w32tm.  Após o texto da mensagem está o endereço IP ou o VMIC, que descreve a fonte de horário e o próximo na cadeia de relógios de referência a serem validados.
3. Status da condição do relógio usando os logs do w32tm para validar que a “Disciplina de ClockDispl: \*SKEW\*TIME\*” está ocorrendo. Isso indica que o w32tm está ativo no momento.

### <a name="event-logging"></a>Log de eventos
Para obter a história completa, você também precisará das informações do log de eventos. Se você coletar o log de eventos do sistema e filtrar em Time-Server, Microsoft-Windows-Kernel-Boot e Microsoft-Windows-Kernel-General, poderá descobrir se há outras influências que alteraram a hora, por exemplo, terceiros. Esses logs podem ser necessários para eliminar a interferência externa. A Política de Grupo pode afetar os logs de eventos que são gravados no log. Confira a seção acima sobre como usar a Política de Grupo para obter mais detalhes.

### <a name="w32time-debug-logging"></a><a name="W32Logging"></a>Log de depuração do W32Time
Para habilitar o w32tm para fins de auditoria, o comando a seguir habilita o log que mostra as atualizações periódicas do relógio e indica o relógio de origem. Reinicie o serviço para habilitar o novo log. 

Para obter mais informações, confira [Como ativar o log de depuração no Serviço de Horário do Windows](https://support.microsoft.com/kb/816043).

 w32tm /debug /enable /file:C:\Windows\Temp\w32time-test.log /size:10000000 /entries:0-73,103,107,110

### <a name="performance-monitor"></a>Desempenho do sistema
O Serviço de Horário do Windows Server 2016 expõe os contadores de desempenho que podem ser usados para coletar logs de auditoria. Eles podem ser registrados em log local ou remotamente. Você pode registrar os contadores Diferença de Horário do Computador e Atraso de Ida e Volta. Além disso, como qualquer contador de desempenho, você pode monitorá-los remotamente e criar alertas usando o System Center Operations Manager. Você pode, por exemplo, usar um alerta para informar quando a Diferença de Horário se desloca da precisão desejada. O [Pacote de Gerenciamento do System Center](https://social.technet.microsoft.com/wiki/contents/articles/15251.system-center-management-pack-authoring-guide.aspx) traz mais informações.

### <a name="windows-traceability-example"></a>Exemplo de rastreabilidade do Windows
Em arquivos de log do w32tm, o ideal é validar dois tipos de informação. O primeiro é uma indicação de que o arquivo de log tem o relógio da condição no momento. Isso prova que o relógio estava sendo condicionado pelo Serviço de Horário do Windows no momento da controvérsia.

 151802 20:18:32,9821765s – ClockDispln Discipline: *SKEW*TIME* - PhCRR:223 CR:156250 UI:100 phcT:65 KPhO:14307 151802 20:18:33,9898460s - ClockDispln Discipline: *SKEW*TIME* - PhCRR:1 CR:156250 UI:100 phcT:64 KPhO:41 151802 20:18:44,1090410s - ClockDispln Discipline: *SKEW*TIME* - PhCRR:1 CR:156250 UI:100 phcT:65 KPhO:38

O ponto principal é que você vê as mensagens prefixadas com ClockDispln Discipline, que é a prova de que o w32time está interagindo com o relógio do sistema.
 
Em seguida, você precisará localizar o último relatório no log antes do momento da controvérsia que relata o computador de origem que está sendo usado no momento como o relógio de referência. Isso pode ser um endereço IP, um nome do computador ou o provedor VMIC, que indica que ele está sendo sincronizado com o host do Hyper-V. O exemplo a seguir fornece um endereço IPv4 10.197.216.105.

 151802 20:18:54,6531515s – Resposta do par 10.197.216.105,0x8 (ntp.m|0x8|0.0.0.0:123->10.197.216.105:123), ofs: +00.0012218s

Agora que você validou o primeiro sistema na cadeia de tempo de referência, você precisará investigar o arquivo de log na fonte de horário de referência e repetir as mesmas etapas. Isso continuará até que você acesse um relógio físico, como um GPS ou uma fonte de horário conhecida como o NIST. Se o relógio de referência for um hardware de GPS, os logs do fabricante também poderão ser necessários.

## <a name="network-considerations"></a>Considerações sobre a rede
Os algoritmos do protocolo NTP têm uma dependência da simetria da rede. À medida que o número de saltos de rede aumenta, a probabilidade de assimetria também aumenta. Portanto, é difícil prever quais tipos de imprecisões você verá nos ambientes específicos. 

O Monitor de Desempenho e os novos contadores de Tempo do Windows no Windows Server 2016 podem ser usados para avaliar a precisão dos ambientes e criar linhas de base. Além disso, você pode executar a solução de problemas para determinar a diferença atual de qualquer computador em sua rede.

Há dois padrões gerais de hora precisa na rede. O PTP ([Protocolo de Tempo de Precisão – IEEE 1588](https://www.nist.gov/el/intelligent-systems-division-73500/introduction-ieee-1588)) tem requisitos mais rígidos referentes à infraestrutura de rede, mas, muitas vezes, pode fornecer a precisão abaixo de microssegundo. O NTP ([protocolo NTP – RFC 1305](https://tools.ietf.org/html/rfc1305)) funciona em uma grande variedade de redes e ambientes, o que facilita o gerenciamento. 

O Windows dá suporte ao NTP Simples (RFC2030) por padrão em computadores não ingressados no domínio. Para os computadores ingressados no domínio, usamos um NTP seguro chamado [MS-SNTP](https://msdn.microsoft.com/library/cc246877.aspx), que aproveita os segredos negociados no domínio, os quais fornecem uma vantagem de gerenciamento em relação ao NTP Autenticado descrito no RFC1305 e no RFC5905.  

Os protocolos ingressados no domínio e não ingressados no domínio exigem a porta UDP 123. Para obter mais informações sobre as melhores práticas do NTP, confira [Versão preliminar da IETF de melhores práticas atuais do protocolo NTP](https://tools.ietf.org/html/draft-ietf-ntp-bcp-00).

### <a name="reliable-hardware-clock-rtc"></a>RTC (Relógio de Hardware Confiável)
O Windows não percorre o tempo, a menos que determinados limites sejam excedidos, mas em vez disso, disciplina o relógio. Isso significa que o w32tm ajusta a frequência do relógio em um intervalo regular, usando a configuração de Frequência de Atualização do Relógio, que usa como padrão uma vez por segundo com o Windows Server 2016. Se o relógio estiver atrasado, ele acelerará a frequência e, se ele estiver adiantado, reduzirá a frequência. No entanto, durante esse tempo entre os ajustes de frequência do relógio, o relógio do hardware fica no controle. Se houver um problema com o firmware ou com o relógio do hardware, a hora do computador poderá se tornar menos precisa.

Essa é outra razão pela qual você precisará fazer testes e definir a linha de base em seu ambiente. Se o contador de desempenho “Diferença de Horário Calculado” não se estabilizar com a precisão que você tem como meta, o ideal será verificar se o firmware está atualizado. Como outro teste, você pode ver se o hardware duplicado reproduz o mesmo problema.

### <a name="troubleshooting-time-accuracy-and-ntp"></a>Solução de problemas de precisão de tempo e NTP
Use a seção Como descobrir a hierarquia acima para entender a origem da hora imprecisa. Observando a diferença de horário, localize o ponto na hierarquia em que o tempo mais diverge da origem NTP. Depois que você entender a hierarquia, o ideal será tentar entender por que essa fonte de horário específica não recebe a hora precisa. 

Concentrando-se no sistema com o tempo divergente, use estas ferramentas abaixo para coletar mais informações a fim de ajudar a determinar o problema e encontrar uma resolução. A referência de UpstreamClockSource abaixo é o relógio descoberto com “w32tm /config /status”.


- Logs do evento do sistema
- Habilite o log usando: w32tm logs - w32tm /debug /enable /file:C:\Windows\Temp\w32time-test.log /size:10000000 /entries:0-300
- Chave do Registro do w32Time HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Services\W32Time
- Rastreamentos de rede local
- Contadores de desempenho (no computador local ou em UpstreamClockSource)
- W32tm /stripchart /computer:UpstreamClockSource
- Execute ping em UpstreamClockSource para entender a latência e o número de saltos para a origem
- UpstreamClockSource do Tracert

Problema| Sintomas| Resolução|
----- | ----- | ----- |
| O relógio de TSC local não é estável.| Usando Perfmon – computador físico, sincronize o relógio estável, mas você ainda verá intervalos de 1 ou 2 minutos de vários 100 us. | A atualização do firmware ou a validação de outro hardware não exibe o mesmo problema.|
| Latência de rede| O stripchart do w32tm exibe um RoundTripDelay de mais de 10 ms. A variação no atraso causa um ruído tão grande quanto ½ do tempo de ida e volta, por exemplo, um atraso que só ocorre em uma direção.<br><br>UpstreamClockSource é de vários saltos, conforme indicado pelo ping. A TTL deve estar próxima a 128.<br><br>Use o Tracert para localizar a latência em cada salto.  | Encontre uma fonte de relógio mais próxima para a hora. Uma solução é instalar um relógio de origem no mesmo segmento ou apontá-lo manualmente para o relógio de origem que está geograficamente mais próximo. Para um cenário de domínio, adicione um computador com a função GTimeServ. | 
Não é possível acessar a origem NTP de maneira confiável| W32tm /stripchart retorna intermitentemente “Tempo limite de solicitação atingido” |A origem NTP não está respondendo|
A origem NTP não está respondendo| Verifique os contadores do Perfmon de Contagem de Fontes do Cliente NTP, Solicitações de Entrada do Servidor NTP, Respostas de Saída do Servidor NTP e determine seu uso em comparação com as linhas de base.| Usando contadores de desempenho do servidor, determine se a carga foi alterada em referência às suas linhas de base.<br><br>Há problemas de congestionamento de rede?|
O controlador de domínio não está usando o relógio mais preciso| Alterações na topologia ou no relógio de tempo mestre adicionado recentemente.| w32tm /resync /rediscover|
Os relógios do cliente estão em descompasso| O evento 36 do Serviço de Horário no log de eventos do sistema e/ou um texto no arquivo de log que descreva que: Contador "Contagem de Fontes de Horário do Cliente NTP" indo de 1 para 0|Solucione os problemas da fonte upstream e entenda se ela está tendo problemas de desempenho.|

### <a name="baselining-time"></a>Tempo de definição de linha de base
A definição de linha de base é importante para que você possa primeiro entender o desempenho e a precisão da sua rede e compará-los com a linha de base no futuro quando ocorrerem problemas. O ideal será criar uma linha de base do controlador de domínio primário raiz ou de qualquer computador marcado com o GTIMESRV. Também recomendamos que você defina uma linha de base do controlador de domínio primário em cada floresta. Finalmente, escolha qualquer controlador de domínio ou computador crítico que tenha características interessantes, como distância ou altas cargas e defina linhas de base para ele.

Também é útil definir a linha de base do Windows Server 2016 vs 2012 R2. No entanto, você só tem w32tm /stripchart como uma ferramenta que pode ser usada para comparação, pois o Windows Server 2012 R2 não tem contadores de desempenho. Você deve escolher dois computadores com as mesmas características ou atualizar um computador e comparar os resultados após a atualização. O adendo Medições de Tempo do Windows traz mais informações sobre medidas detalhadas entre 2016 e 2012.

Usando todos os contadores de desempenho do w32time, colete dados por, pelo menos, uma semana. Isso garantirá que você tenha uma referência suficiente para consideração para vários na rede ao longo do tempo e uma execução suficiente para ter certeza de que a precisão de tempo é estável.

### <a name="ntp-server-redundancy"></a>Redundância do servidor NTP
Para a configuração manual do servidor NTP usada com computadores não ingressados no domínio ou o controlador de domínio primário, ter mais de um servidor é uma boa medida de redundância em caso de disponibilidade. Isso também pode oferecer maior precisão, supondo que todas as fontes sejam precisas e estáveis. No entanto, se a topologia não for bem projetada ou se as fontes de horário não forem estáveis, a precisão resultante poderá ser pior; portanto, tenha cuidado. O limite de servidores de horário compatíveis que o w32time pode referenciar manualmente é 10. 

## <a name="leap-seconds"></a>Segundos bissextos
O período de rotação da Terra varia ao longo do tempo, causado por eventos climáticos e geológicos. Normalmente, a variação é de cerca de um segundo a cada dois anos. Sempre que a variação do tempo atômico aumenta muito, uma correção de um segundo (para cima ou para baixo) é inserida, chamada de segundo bissexto. Isso é feito de maneira que a diferença nunca exceda 0,9 segundo. Essa correção é anunciada com seis meses de antecedência da correção real. Antes do Windows Server 2016, o Serviço de Horário da Microsoft não reconhecia os segundos bissextos, mas dependia do serviço de horário externo para cuidar disso. Com o aumento da precisão de tempo do Windows Server 2016, a Microsoft está trabalhando em uma solução mais adequada para o problema de segundo bissexto.

## <a name="secure-time-seeding"></a>Propagação de Tempo Segura
O W32Time no Server 2016 inclui o recurso Propagação de Tempo Segura. Esse recurso determina o tempo atual aproximado das conexões SSL de saída. Esse valor temporal é usado para monitorar o relógio do sistema local e corrigir os erros grosseiros. Leia mais sobre o recurso [nesta postagem no blog](https://blogs.msdn.microsoft.com/w32time/2016/09/28/secure-time-seeding-improving-time-keeping-in-windows/). Em implantações com fontes de horário confiáveis e computadores bem monitorados que incluem o monitoramento de diferenças de horário, você pode optar por não usar o recurso Propagação de Tempo Segura e, em vez disso, confiar em sua infraestrutura existente. 

Desabilite o recurso com estas etapas:

1. Defina o valor de configuração do Registro de UtilizeSSLTimeData como 0 em um computador específico:

    reg add HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Services\w32time\Config /v UtilizeSslTimeData /t REG_DWORD /d 0 /f

2. Se não for possível reinicializar o computador imediatamente por algum motivo, notifique o serviço W32Time sobre a atualização de configuração. Isso interrompe o monitoramento e a imposição de tempo com base nos dados de tempo coletados das conexões SSL.

    W32tm.exe /config /update

3. A reinicialização do computador torna a configuração efetiva imediatamente e também faz com que ela pare de coletar os dados de tempo das conexões SSL. A última parte tem uma sobrecarga muito pequena e não deve ser uma preocupação de desempenho.

4. Para aplicar essa configuração em um domínio inteiro, defina o valor de UtilizeSSLTimeData na configuração da Política de Grupo do W32Time como 0 e publique a configuração. Quando a configuração for selecionada por um Cliente da Política de Grupo, o serviço W32Time será notificado e interromperá o monitoramento e a imposição de tempo usando dados de tempo do SSL. A coleta de dados de tempo do SSL será interrompida quando cada computador for reinicializado. Se o domínio tiver laptops/tablets compactos portáteis e outros dispositivos, o ideal será excluir esses computadores dessa alteração de política. Esses dispositivos acabarão esgotando a bateria e precisarão do recurso Propagação de Tempo Segura para inicializar o tempo.
