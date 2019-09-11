---
title: Aprimoramentos do Windows Server 2016
description: O Windows Server 2016 aprimorou os algoritmos que ele usa para corrigir a hora e a condição do relógio local para sincronizar com o UTC.
author: dcuomo
ms.author: dacuo
manager: dougkim
ms.date: 10/17/2018
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: networking
ms.openlocfilehash: 2b8c6148af21e94e4a56661402f36dcb2e636461
ms.sourcegitcommit: f6490192d686f0a1e0c2ebe471f98e30105c0844
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/10/2019
ms.locfileid: "70871837"
---
## <a name="windows-server-2016-improvements"></a>Aprimoramentos do Windows Server 2016

### <a name="windows-time-service-and-ntp"></a>Serviço de tempo do Windows e NTP
O Windows Server 2016 aprimorou os algoritmos que ele usa para corrigir a hora e a condição do relógio local para sincronizar com o UTC.  O NTP usa quatro valores para calcular o deslocamento de tempo, com base nos carimbos de data/hora da solicitação/resposta do cliente e solicitação/resposta do servidor.  No entanto, as redes estão ruidosas, e pode haver picos nos dados do NTP devido ao congestionamento da rede e outros fatores que afetam a latência de rede.  Os algoritmos do Windows 2016 oferecem a média desse ruído usando várias técnicas diferentes que resultam em um relógio estável e preciso.  Além disso, a fonte que usamos para o tempo preciso faz referência a uma API aprimorada que nos oferece uma melhor resolução.  Com esses aprimoramentos, é possível alcançar uma precisão de 1 ms com relação ao UTC em um domínio.

### <a name="hyper-v"></a>Hyper-V
O Windows 2016 melhorou o serviço TimeSync do Hyper-V. Os aprimoramentos incluem um tempo inicial mais preciso na inicialização da VM ou na correção de recuperação de VM e de latência para exemplos fornecidos ao W32Time.  Essa melhoria nos permite manter-se 10μS do host com um RMS, (a média raiz ao quadrado, que indica variância), de 50 μs, mesmo em um computador com 75% de carga. Para obter mais informações, consulte [arquitetura do Hyper-V](https://msdn.microsoft.com/library/cc768520.aspx).

> [!NOTE]
> O carregamento foi criado usando o parâmetro de comparação Prime95 usando o perfil equilibrado.

Além disso, o nível de estrato que o host relata para o convidado é mais transparente.  Anteriormente, o host apresentaria um estrato fixo de 2, independentemente de sua exatidão.  Com as alterações no Windows Server 2016, o host relata um estrato maior do que a camada de host, o que resulta em um tempo melhor para convidados virtuais.  A camada de host é determinada pelo W32Time a meio normal com base em seu tempo de origem.  Os convidados do Windows 2016 ingressados no domínio encontrarão o relógio mais preciso, em vez de usar o padrão para o host.  Foi por esse motivo que aconselhamos desabilitar manualmente a configuração do provedor de tempo do Hyper-V para computadores que participam de um domínio no Windows 2012R2 e abaixo.

### <a name="monitoring"></a>Monitoramento
Os contadores do monitor de desempenho foram adicionados.  Eles permitem que você linha de base, monitore e solucione problemas de precisão do tempo.  Esses contadores incluem:

Contador|Descrição|
----- | ----- |
Deslocamento de tempo calculado|   O deslocamento de tempo absoluto entre o relógio do sistema e a fonte de tempo escolhida, conforme calculado pelo serviço W32Time em microssegundos. Quando um novo exemplo válido estiver disponível, o tempo computado será atualizado com o deslocamento de tempo indicado pelo exemplo. Esse é o deslocamento de tempo real do relógio local. O W32time inicia a correção de relógio usando esse deslocamento e atualiza o tempo computado entre os exemplos com o deslocamento de tempo restante que precisa ser aplicado ao relógio local. A precisão do relógio pode ser rastreada usando esse contador de desempenho com um intervalo de sondagem baixo (por exemplo, 256 segundos ou menos) e procurando o valor do contador como menor do que o limite de precisão do relógio desejado.|
Ajuste de frequência do relógio| O ajuste de frequência de relógio absoluto para o relógio do sistema local por W32Time em partes por bilhão. Esse contador ajuda a visualizar as ações que estão sendo executadas pelo W32time.|
Atraso de ida e volta de NTP|    Atraso de viagem de ida e volta mais recente enfrentado pelo cliente NTP no recebimento de uma resposta do servidor em microssegundos. Esse é o tempo decorrido no cliente NTP entre a transmissão de uma solicitação para o servidor NTP e o recebimento de uma resposta válida do servidor. Esse contador ajuda a caracterizar os atrasos experimentados pelo cliente NTP. Viagens de ida e volta maiores ou variáveis podem adicionar ruído a cálculos de tempo de NTP, o que, por sua vez, pode afetar a precisão da sincronização de tempo por meio do NTP.|
Contagem de origem do cliente NTP|    Número ativo de fontes de tempo NTP que estão sendo usadas pelo cliente NTP. Essa é uma contagem de endereços IP ativos e distintos de servidores de tempo que estão respondendo às solicitações deste cliente. Esse número pode ser maior ou menor do que os pares configurados, dependendo da resolução DNS de nomes de pares e da capacidade de alcance atual.|
Solicitações de entrada do servidor NTP|   Número de solicitações recebidas pelo servidor NTP (solicitações/s).|
Respostas de saída do servidor NTP|  Número de solicitações respondidas pelo servidor NTP (respostas/s).|

Os três primeiros contadores se destinam a cenários de solução de problemas de precisão.  A seção precisão de tempo de solução de problemas e NTP abaixo, em [práticas recomendadas](#BestPractices), tem mais detalhes.
Os últimos 3 contadores abrangem cenários de servidor NTP e são úteis quando determinam a carga e a linha de base do desempenho atual.

### <a name="configuration-updates-per-environment"></a>Atualizações de configuração por ambiente
O seguinte descreve as alterações na configuração padrão entre o Windows 2016 e versões anteriores para cada função.  As configurações da atualização de aniversário do Windows Server 2016 e do Windows 10 (Build 14393), agora são exclusivas, e é por isso que há uma exibição de colunas separadas. 

|Role|Configuração|Windows Server 2016|Windows 10|Windows Server 2012 R2</br>Windows Server 2008 R2</br>Windows 10|
|---|---|---|---|---|
|**Servidor autônomo/nano**||||
| |*Servidor de horário*|time.windows.com|N/D|time.windows.com|
| |*Frequência de sondagem*|64-1024 segundos|N/D|Uma vez por semana|
| |*Frequência de atualização do relógio*|Uma vez por segundo|N/D|Uma vez por hora|
|**Cliente autônomo**||||
| |*Servidor de horário*|N/D|time.windows.com|time.windows.com|
| |*Frequência de sondagem*|N/D|Uma vez por dia|Uma vez por semana|
| |*Frequência de atualização do relógio*|N/D|Uma vez por dia|Uma vez por semana|
|**Controlador de domínio**||||
| |*Servidor de horário*|PDC/GTIMESERV|N/D|PDC/GTIMESERV|
| |*Frequência de sondagem*|64-1024 segundos|N/D|1024-32768 segundos|
| |*Frequência de atualização do relógio*|Uma vez por dia|N/D|Uma vez por semana|
|**Servidor membro do domínio**||||
| |*Servidor de horário*|DC|N/D|DC|
| |*Frequência de sondagem*|64-1024 segundos|N/D|1024-32768 segundos|
| |*Frequência de atualização do relógio*|Uma vez por segundo|N/D|Uma vez a cada 5 minutos|
|**Cliente membro do domínio**||||
| |*Servidor de horário*|N/D|DC|DC|
| |*Frequência de sondagem*|N/D|1204-32768 segundos|1024-32768 segundos|
| |*Frequência de atualização do relógio*|N/D|Uma vez a cada 5 minutos|Uma vez a cada 5 minutos|
|**Convidado do Hyper-V**||||
| |*Servidor de horário*|Escolhe a melhor opção com base no estrato do host e do servidor de horário|Escolhe a melhor opção com base no estrato do host e do servidor de horário|O padrão é o host|
| |*Frequência de sondagem*|Com base na função acima|Com base na função acima|Com base na função acima|
| |*Frequência de atualização do relógio*|Com base na função acima|Com base na função acima|Com base na função acima|

>[!NOTE]
>Para o Linux no Hyper-V, consulte a seção [permitindo que o Linux use o tempo de host do Hyper-v](#AllowingLinux) abaixo.

### <a name="impact-of-increased-polling-and-clock-update-frequency"></a>Impacto da maior frequência de sondagem e de atualização do relógio
Para fornecer um tempo mais preciso, os padrões para frequências de sondagem e atualizações de relógio são aumentados, o que nos permite fazer pequenos ajustes com mais frequência.  Isso causará mais tráfego UDP/NTP. no entanto, esses pacotes são pequenos, portanto, deve haver muito pouco ou nenhum impacto sobre links de banda larga. No entanto, o benefício é que o tempo deve ser melhor em uma variedade maior de hardware e ambientes.

Para dispositivos com suporte à bateria, aumentar a frequência de sondagem pode causar problemas.  Os dispositivos de bateria não armazenam o tempo enquanto estiverem desativados.  Quando eles são retomados, pode exigir correções frequentes no relógio.  Aumentar a frequência de sondagem fará com que o relógio fique instável e também poderá usar mais energia.  A Microsoft recomenda que você não altere as configurações padrão do cliente.

Os controladores de domínio devem ser minimamente impactados mesmo com o efeito multiplicado das atualizações maiores de clientes NTP em um domínio do AD.  O NTP tem um consumo de recursos muito menor em comparação com outros protocolos e um impacto marginal.  É mais provável que você atinja os limites para outras funcionalidades de domínio antes de ser afetado pelas maiores configurações do Windows Server 2016.  Active Directory usa o NTP seguro, que tende a sincronizar o tempo menos preciso do que o NTP simples, mas verificamos se ele escala verticalmente para os clientes dois estrato fora do PDC.

Como um plano conservador, você deve reservar 100 solicitações de NTP por segundo por núcleo.  Por exemplo, um domínio composto de 4 DCs com 4 núcleos cada, você deve ser capaz de atender a 1600 solicitações de NTP por segundo.  Se você tiver clientes de 10k configurados para sincronizar o tempo uma vez a cada 64 segundos e as solicitações forem recebidas uniformemente ao longo do tempo, você verá 10000/64 ou cerca de 160 solicitações/segundo, espalhadas por todos os DCs.  Isso fica facilmente dentro de nossas 1600 solicitações de NTP/s com base neste exemplo.  Essas são recomendações de planejamento conservadoras e, obviamente, têm uma grande dependência em sua rede, velocidades e cargas de processador, assim como sempre a linha de base e o teste em seus ambientes.

Também é importante observar que, se os controladores de domínio estiverem em execução com uma carga considerável de CPU, maior que 40%, isso certamente adicionará ruído às respostas de NTP e afetará sua precisão de tempo em seu domínio.  Novamente, você precisa testar em seu ambiente para entender os resultados reais.

## <a name="time-accuracy-measurements"></a>Medidas de precisão de tempo
### <a name="methodology"></a>Metodologia
Para medir a precisão do tempo do Windows Server 2016, usamos uma variedade de ferramentas, métodos e ambientes.  Você pode usar essas técnicas para medir e ajustar seu ambiente e determinar se os resultados de exatidão atendem às suas necessidades. 

Nosso relógio de origem de domínio consistiu em dois servidores NTP de alta precisão com hardware de GPS.  Também usamos um computador de teste de referência separado para medições, que também tinha hardware de GPS de alta precisão instalado de um fabricante diferente.  Para alguns dos testes, você precisará de uma fonte de relógio precisa e confiável para usar como referência, além da fonte do relógio do domínio.

Usamos quatro métodos diferentes para medir a precisão com máquinas virtuais e físicas. Vários métodos forneciam meios independentes para validar os resultados.


1. Meça o relógio local, que é condicionado por w32tm, em nosso computador de teste de referência que tem hardware GPS separado.  
2.  Medir pings de NTP do servidor NTP para clientes usando w32tm "stripchart"
3.  Meça pings de NTP do cliente para o servidor NTP usando w32tm "stripchart"
4.  Meça os resultados do Hyper-V do host para o convidado usando o contador de carimbo de data/hora (TSC).  Esse contador é compartilhado entre ambas as partições e a hora do sistema em ambas as partições.  Calculamos a diferença entre a hora do host e a hora do cliente na máquina virtual.  Em seguida, usamos o relógio de TSC para interpolar o tempo de host do convidado, já que as medidas não acontecem ao mesmo tempo.  Além disso, usamos o fator de relógio TSV atraso e latência na API.

O W32tm é interno, mas as outras ferramentas que usamos durante nossos testes estão disponíveis para o repositório da Microsoft no GitHub como código-fonte aberto para teste e uso.  O WIKI no repositório tem mais informações que descrevem como usar as ferramentas para fazer medidas.

> [https://github.com/Microsoft/Windows-Time-Calibration-Tools](https://github.com/Microsoft/Windows-Time-Calibration-Tools)

Os resultados de teste mostrados abaixo são um subconjunto de medidas que fizemos em um dos ambientes de teste.  Eles ilustram a precisão mantida no início da hierarquia de tempo e o cliente de domínio filho no final da hierarquia de tempo.  Isso é comparado com os mesmos computadores em uma topologia baseada em 2012 para comparação.

### <a name="topology"></a>Topologia
Para comparação, testamos uma topologia baseada no Windows Server 2012R2 e no Windows Server 2016.  Ambas as topologias consistem em dois computadores físicos de host Hyper-V que fazem referência a um computador com Windows Server 2016 com hardware de tempo de GPS instalado.  Cada host executa 3 convidados do Windows ingressados no domínio, que são organizados de acordo com a topologia a seguir.  As linhas representam a hierarquia de tempo e o protocolo/transporte usado.

![Tempo do Windows](../media/Windows-Time-Service/Windows-2016-Accurate-Time/topology1.png)

![Tempo do Windows](../media/Windows-Time-Service/Windows-2016-Accurate-Time/topology2.png)

### <a name="graphical-results-overview"></a>Visão geral dos resultados gráficos
Os dois grafos a seguir representam a precisão de tempo para dois membros específicos em um domínio com base na topologia acima.  Cada grafo exibe os resultados de 2012R2 e 2016 do Windows Server sobrepostos, o que demonstra as melhorias visualmente.  A precisão foi medida de com-no computador convidado em comparação com o host.  Os dados gráficos representam um subconjunto de todo o conjunto de testes que fizemos e mostra os cenários de melhor caso e pior caso.  

![Tempo do Windows](../media/Windows-Time-Service/Windows-2016-Accurate-Time/topology3.png)

### <a name="performance-of-the-root-domain-pdc"></a>Desempenho do PDC do domínio raiz
O PDC raiz é sincronizado com o host Hyper-V (usando VMIC), que é um Windows Server 2016 com hardware de GPS que é comprovado para ser preciso e estável.  Esse é um requisito crítico para uma precisão de 1 ms, que é mostrado como a área sombreada verde.

![Tempo do Windows](../media/Windows-Time-Service/Windows-2016-Accurate-Time/chart1.png)

### <a name="performance-of-the-child-domain-client"></a>Desempenho do cliente de domínio filho
O cliente de domínio filho é anexado a um PDC de domínio filho que se comunica com o PDC raiz.  O tempo de ti também está dentro do requisito de 1 ms.

![Tempo do Windows](../media/Windows-Time-Service/Windows-2016-Accurate-Time/chart2.png)


### <a name="long-distance-test"></a>Teste de longa distância
O gráfico a seguir compara 1 salto de rede virtual a 6 saltos de rede física com o Windows Server 2016.  Dois gráficos são sobrepostos entre si com transparência para mostrar dados sobrepostos.  O aumento de saltos de rede significa latência mais alta e desvios de tempo maiores.  O gráfico é ampliado e, portanto, os limites de 1 ms, representados pela área verde, são maiores.  Como você pode ver, o tempo ainda está dentro de 1 ms com vários saltos.  Ele é deslocado negativamente, o que demonstra uma assimetria de rede.  É claro que cada rede é diferente e as medidas dependem de uma infinidade de fatores ambientais.

![Tempo do Windows](../media/Windows-Time-Service/Windows-2016-Accurate-Time/chart3.png)

## <a name="BestPractices"></a>Práticas recomendadas para o modo de manutenção preciso
### <a name="solid-source-clock"></a>Relógio de origem sólido
A hora de computadores é tão boa quanto o relógio de origem com o qual sincroniza.  Para atingir 1 ms de precisão, você precisará de hardware de GPS ou de um dispositivo de tempo em sua rede referenciado como o relógio de origem mestre.  Usar o padrão de time.windows.com, não pode fornecer uma fonte de horário estável e local.  Além disso, à medida que você ficar mais distante do relógio de origem, a rede afetará a precisão.  Ter um relógio de origem mestre em cada data center é necessário para obter a melhor precisão.

### <a name="hardware-gps-options"></a>Opções de hardware GPS
Há várias soluções de hardware que podem oferecer tempo preciso.  Em geral, as soluções atuais são baseadas em antenas GPS.  Também há soluções de modem de rádio e dial-up usando linhas dedicadas.  Eles se conectam à sua rede como um dispositivo ou são conectados a um PC, por exemplo, Windows por meio de um dispositivo PCIe ou USB.  Opções diferentes fornecerão níveis diferentes de precisão e, como sempre, os resultados dependem do seu ambiente.  Variáveis que afetam a precisão incluem disponibilidade de GPS, estabilidade de rede e carga e hardware de PC.  Esses são fatores importantes ao escolher um relógio de origem, que como dissemos, é um requisito de tempo estável e preciso.

### <a name="domain-and-synchronizing-time"></a>Domínio e tempo de sincronização
Os membros do domínio usam a hierarquia de domínio para determinar qual computador ele usa como uma fonte para sincronizar o tempo.  Cada membro do domínio encontrará outro computador com o qual será sincronizado e o salvará como fonte do relógio.  Cada tipo de membro do domínio segue um conjunto diferente de regras para encontrar uma fonte de relógio para sincronização de horário.  O PDC na raiz da floresta é a fonte do relógio padrão para todos os domínios.  Abaixo estão listadas funções diferentes e descrição de alto nível sobre como elas encontram uma fonte:


- **Controlador de domínio com função de PDC** – essa máquina é a fonte de tempo autoritativa para um domínio. Ele terá o tempo mais preciso disponível no domínio e deverá sincronizar com um DC no domínio pai, exceto nos casos em que a função [GTIMESERV](#GTIMESERV) está habilitada. 
- **Qualquer outro controlador de domínio** – essa máquina atuará como uma fonte de tempo para clientes e servidores membro no domínio. Um DC pode sincronizar com o PDC de seu próprio domínio ou qualquer DC em seu domínio pai.
- **Clientes/servidores membros** – este computador pode sincronizar com qualquer DC ou PDC de seu próprio domínio ou um DC ou PDC no domínio pai.

Com base nos candidatos disponíveis, um sistema de pontuação é usado para encontrar a melhor fonte de tempo.  Esse sistema leva em conta a confiabilidade da fonte de tempo e seu local relativo.  Isso ocorre uma vez quando o horário é o serviço iniciado.  Se você precisar ter um controle mais preciso de como o tempo é sincronizado, poderá adicionar bons servidores de tempo em locais específicos ou adicionar redundância.  Consulte a seção [especificar um serviço de horário confiável local usando o GTIMESERV](#GTIMESERV) para obter mais informações.

#### <a name="mixed-os-environments-win2012r2-and-win2008r2"></a>Ambientes de sistema operacional mistos (Win2012R2 e Win2008R2)
Embora um ambiente de domínio puro do Windows Server 2016 seja necessário para obter a melhor precisão, ainda há benefícios em um ambiente misto.  A implantação do Windows Server 2016 Hyper-V em um domínio do Windows 2012 beneficiará os convidados devido às melhorias mencionadas acima, mas somente se os convidados também forem Windows Server 2016.  Um PDC do Windows Server 2016 poderá fornecer um tempo mais preciso devido aos algoritmos aprimorados que será uma fonte mais estável.  Como substituir seu PDC pode não ser uma opção, você pode adicionar um DC do Windows Server 2016 com o conjunto de roll [GTIMESERV](#GTIMESERV) que seria uma atualização em precisão para seu domínio.  Um controlador de domínio do Windows Server 2016 pode fornecer um tempo melhor para clientes de tempo downstream. no entanto, ele é tão bom quanto seu tempo de NTP de origem.

Além disso, conforme mencionado acima, a sondagem do relógio e frequências de atualização foram modificadas com o Windows Server 2016.  Eles podem ser alterados manualmente para os DCs de nível inferior ou aplicados por meio da diretiva de grupo.  Embora não tenhamos testado essas configurações, elas devem se comportar bem em Win2008R2 e Win2012R2 e fornecer alguns benefícios.

As versões anteriores ao Windows Server 2016 tiveram vários problemas, mantendo o tempo preciso mantendo o resultado da desativação do tempo do sistema imediatamente após a tomada de um ajuste.  Por isso, a obtenção de amostras de tempo de uma fonte NTP precisa com frequência e a condicionamento do relógio local com os dados leva a uma descompasso menor nos relógios do sistema no período de intra-amostragem, resultando em um melhor tempo mantendo as versões de so de nível inferior. A melhor precisão observada foi de aproximadamente 5 ms quando um cliente do Windows Server 2012R2 NTP, configurado com as configurações de alta precisão, sincronizou seu tempo de um servidor NTP do Windows 2016 exato.

Em alguns cenários que envolvem controladores de domínio de convidado, os exemplos TimeSync do Hyper-V podem interromper a sincronização de horário do domínio.  Isso não deve ser um problema para os convidados do servidor 2016 em execução no servidor 2016 hosts Hyper-V.

Para desabilitar o serviço TimeSync do Hyper-V de fornecer amostras para W32Time, defina a seguinte chave do registro de convidado:

    HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Services\W32Time\TimeProviders\VMICTimeProvider 
    "Enabled"=dword:00000000

#### <a name="AllowingLinux"></a>Permitindo que o Linux use o tempo de host do Hyper-V
Para convidados do Linux em execução no Hyper-V, os clientes normalmente são configurados para usar o daemon NTP para sincronização de tempo em servidores NTP.  Se a distribuição do Linux der suporte ao protocolo TimeSync versão 4 e o convidado do Linux tiver o serviço de integração TimeSync habilitado, ele será sincronizado em relação ao tempo do host. Isso pode levar a tempo inconsistente mantendo se ambos os métodos estiverem habilitados.

Para sincronizar exclusivamente em relação à hora do host, é recomendável desabilitar a sincronização de tempo do NTP de uma das opções:

- Desabilitando servidores NTP no arquivo NTP. conf
- ou desabilitando o daemon NTP

Nessa configuração, o parâmetro de servidor de tempo é esse host.  Sua frequência de sondagem é de 5 segundos e a frequência de atualização do relógio também é de 5 segundos.

Para sincronizar exclusivamente por meio do NTP, é recomendável desabilitar o serviço de integração TimeSync no convidado.

> [!NOTE]
> Observação:  O suporte para o tempo preciso com convidados do Linux requer um recurso que só tem suporte nos kernels do Linux de upstream mais recentes e não é algo que está amplamente disponível em todos os distribuições do Linux ainda. Consulte as [máquinas virtuais Linux e FreeBSD com suporte para o Hyper-V no Windows](https://technet.microsoft.com/windows-server-docs/virtualization/hyper-v/supported-linux-and-freebsd-virtual-machines-for-hyper-v-on-windows) para obter mais detalhes sobre as distribuições de suporte.

#### <a name="GTIMESERV"></a>Especificar um serviço de horário confiável local usando GTIMESERV
Você pode especificar um ou mais controladores de domínio como relógios de origem precisos usando o GTIMESERV, o bom servidor de horário, os sinalizadores.  Por exemplo, controladores de domínio específicos equipados com hardware GPS podem ser sinalizados como um GTIMESERV.  Isso garantirá que seu domínio faça referência a um relógio com base no hardware de GPS.

> [!NOTE]
> Mais informações sobre sinalizadores de domínio podem ser encontradas na [documentação do protocolo MS-ADTS](https://msdn.microsoft.com/library/mt226583.aspx).

Timeserv é outro sinalizador de serviços de domínio relacionado que indica se um computador está autoritativo no momento, que pode ser alterado se um DC perder a conexão.  Um controlador de domínio nesse estado retornará "estrato desconhecido" quando consultado via NTP.  Depois de tentar várias vezes, o DC registrará o evento 36 do serviço de tempo de evento de sistema.

Se você quiser configurar um controlador de domínio como um GTIMESERV, isso poderá ser configurado manualmente usando o comando a seguir.  Nesse caso, o DC está usando outra máquina (s) como o relógio mestre.  Isso pode ser um dispositivo ou uma máquina dedicada.

    w32tm /config /manualpeerlist:”master_clock1,0x8 master_clock2,0x8” /syncfromflags:manual /reliable:yes /update

> [!NOTE]
> Para obter mais informações, consulte [Configurar o serviço de tempo do Windows](https://technet.microsoft.com/library/cc731191.aspx)

Se o controlador de domínio tiver o hardware GPS instalado, você precisará usar estas etapas para desabilitar o cliente NTP e habilitar o servidor NTP.

Comece desabilitando o cliente NTP e habilite o servidor NTP usando essas alterações de chave do registro.

    reg add HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Services\w32time\TimeProviders\NtpClient /v Enabled /t REG_DWORD /d 0 /f

    reg add HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Services\w32time\TimeProviders\NtpServer /v Enabled /t REG_DWORD /d 1 /f

Em seguida, reinicie o serviço de tempo do Windows

    net stop w32time && net start w32time

Por fim, você indica que esse computador tem uma fonte de tempo confiável usando.
   
    w32tm /config /reliable:yes /update

Para verificar se as alterações foram feitas corretamente, você pode executar os seguintes comandos que afetam os resultados mostrados abaixo. 

    w32tm /query /configuration

Valor|Configuração esperada|
----- | ----- |
AnnounceFlags|  5 (local)|
NtpServer   |Local|
DllName |C:\WINDOWS\SYSTEM32\w32time. DLL (local)|
Enabled |1 (local)|
Feita|  Local|

    w32tm /query /status /verbose

Valor|  Configuração esperada|
----- | ----- |
NTP|    1 (referência primária-sincronizada por relógio de rádio)|
ReferenceId|    0x4C4F434C (nome de origem:  "LOCAL")|
Origem| Relógio CMOS local|
Deslocamento de fase|   0.0000000 s|
Função de servidor|    576 (serviço de tempo confiável)|

#### <a name="windows-server-2016-on-3rd-party-virtual-platforms"></a>Windows Server 2016 em plataformas virtuais de terceiros
Quando o Windows é virtualizado, por padrão, o hipervisor é responsável por fornecer tempo.  Mas os membros ingressados no domínio precisam ser sincronizamos com o controlador de domínio para que Active Directory funcionem corretamente.  É melhor desabilitar qualquer virtualização de tempo entre o convidado e o host de qualquer plataforma virtual de terceiros.

#### <a name="discovering-the-hierarchy"></a>Descobrindo a hierarquia
Como a cadeia de hierarquia de tempo para a origem do relógio mestre é dinâmica em um domínio e negociada, você precisará consultar o status de uma máquina específica para entender a fonte de tempo e a cadeia para o relógio de origem mestre.  Isso pode ajudar a diagnosticar problemas de sincronização de horário.

Dado que você deseja solucionar problemas de um cliente específico; a primeira etapa é entender sua fonte de tempo usando esse comando W32tm.

    w32tm /query /status

Os resultados exibem a origem entre outras coisas.  A origem indica com quem você sincroniza a hora no domínio.  Esta é a primeira etapa desta hierarquia de tempo de computadores.
Em seguida, use a entrada de origem acima e use o parâmetro/StripChart para localizar a fonte da próxima hora na cadeia.

    w32tm /stripchart /computer:MySourceEntry /packetinfo /samples:1

Também é útil, o comando a seguir lista cada controlador de domínio que ele pode encontrar no domínio especificado e imprime um resultado que permite que você determine cada parceiro.  Esse comando incluirá os computadores que foram configurados manualmente.
    
    w32tm /monitor /domain:my_domain

Usando a lista, você pode rastrear os resultados por meio do domínio e entender a hierarquia, bem como o deslocamento de tempo em cada etapa.  Ao localizar o ponto em que o deslocamento de tempo fica significativamente pior, você pode identificar a raiz da hora incorreta.  A partir daí, você pode tentar entender por que esse tempo está incorreto ativando o [log de w32tm](#W32Logging). 

#### <a name="using-group-policy"></a>Usando Política de Grupo
Você pode usar Política de Grupo para atingir uma precisão mais estrita, por exemplo, atribuir clientes para usar servidores NTP específicos ou para controlar como OS sistemas operacionais de nível inferior são configurados quando virtualizados.  
Abaixo está uma lista de possíveis cenários e configurações de Política de Grupo relevantes:

**Domínios virtualizados** – para controlar os controladores de domínio virtualizados no Windows 2012R2 para que eles sincronizem a hora com seu domínio, e não com o host Hyper-V, você pode desabilitar essa entrada de registro.   Para o PDC, você não deseja desabilitar a entrada, pois o host Hyper-V fornecerá a fonte de tempo mais estável.  A entrada do registro requer que você reinicie o serviço W32Time depois que ele for alterado.

    [HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Services\W32Time\TimeProviders\VMICTimeProvider]
    "Enabled"=dword:00000000

**Cargas sensíveis à precisão** -para cargas de trabalho confidenciais de precisão de tempo, você pode configurar grupos de computadores para definir os servidores NTP e quaisquer configurações de hora relacionadas, como a frequência de atualização e de sondagem do relógio.  Isso normalmente é tratado pelo domínio, mas para obter mais controle, você pode direcionar computadores específicos para apontar diretamente para o relógio mestre.

Configuração da Diretiva de Grupo|   Novo valor|
----- | ----- |
NtpServer|  ClockMasterName, 0x8|
MinPollInterval|    6 a 64 segundos|
MaxPollInterval|    6|
UpdateInterval| 100 – uma vez por segundo|
EventLogFlags|  3 – todo o log de tempo especial|

> [!NOTE]
> As configurações do NtpServer e do EventLogFlags estão localizadas em System\Windows time Service\Time providers using the Configure Windows NTP Client Settings.  Os outros 3 estão localizados em System\Windows time Service usando as definições de configuração global.

**Carregamento remoto sensível à precisão** remota – para sistemas em domínios de ramificação para varejo de instância e PCI (setor de crédito de pagamento), o Windows usa as informações do site atual e o localizador de DC para localizar um controlador de domínio local, a menos que haja uma fonte de tempo NTP manual configurada .  Esse ambiente requer 1 segundo de precisão, que usa convergência mais rápida para a hora correta.  Essa opção permite que o serviço W32Time mova o relógio para trás.  Se isso for aceitável e atender aos seus requisitos, você poderá criar a política a seguir.   Assim como ocorre com qualquer ambiente, certifique-se de testar e fazer a linha de base da sua rede. 

Configuração da Diretiva de Grupo|   Novo valor|
----- | ----- |
MaxAllowedPhaseOffset|  1, se houver mais de em segundo, defina o relógio para a hora correta.|

A configuração MaxAllowedPhaseOffset está localizada em serviço de tempo System\Windows usando as definições de configuração global.

> [!NOTE]
> Para obter mais informações sobre a política de grupo e as entradas relacionadas, consulte o artigo ferramentas e configurações de [serviço de tempo do Windows](windows-time-service-tools-and-settings.md) no TechNet.

## <a name="azure-and-windows-iaas-considerations"></a>Considerações sobre IaaS do Azure e do Windows

### <a name="azure-virtual-machine-active-directory-domain-services"></a>Máquina virtual do Azure: Active Directory Domain Services
Se a VM do Azure em execução Active Directory Domain Services fizer parte de uma floresta de Active Directory local existente, o TimeSync (VMIC) deverá ser desabilitado. Isso é para permitir que todos os DCs na floresta, físico e virtual, usem uma única hierarquia de sincronização de tempo. Consulte o White Paper de prática recomendada ["executando controladores de domínio no Hyper-V"](https://technet.microsoft.com/library/virtual_active_directory_domain_controller_virtualization_hyperv.aspx)

### <a name="azure-virtual-machine-domain-joined-machine"></a>Máquina virtual do Azure: Computador ingressado no domínio
Se você estiver hospedando um computador que está ingressado em um domínio em uma floresta Active Directory existente, virtual ou física, a prática recomendada é desabilitar a sincronização de tempo para o convidado e garantir que o W32Time esteja configurado para sincronizar com seu controlador de domínio por meio da configuração da hora para Type = NTP5

### <a name="azure-virtual-machine-standalone-workgroup-machine"></a>Máquina virtual do Azure: Computador de grupo de trabalho autônomo
Se a VM do Azure não estiver unida a um domínio, nem for um controlador de domínio, a recomendação será manter a configuração de hora padrão e fazer com que a VM seja sincronizada com o host.

## <a name="windows-application-requiring-accurate-time"></a>Aplicativo do Windows exigindo tempo preciso
### <a name="time-stamp-api"></a>API de carimbo de data/hora
Os programas que exigem a maior precisão em relação ao UTC, e não à passagem de tempo, devem usar a [API GetSystemTimePreciseAsFileTime](https://msdn.microsoft.com/library/windows/desktop/Hh706895.aspx).  Isso garante que seu aplicativo obtenha o horário do sistema, que é condicionado pelo serviço de tempo do Windows.

### <a name="udp-performance"></a>Desempenho de UDP
Se você tiver um aplicativo que usa a comunicação UDP para transações e for importante minimizar a latência, há algumas entradas de registro relacionadas que você pode usar para configurar um intervalo de portas a serem excluídas da porta do mecanismo de filtragem base.  Isso melhorará a latência e aumentará a taxa de transferência.  No entanto, as alterações no registro devem ser limitadas a administradores experientes.  Além disso, essa solução alternativa exclui as portas de serem protegidas pelo firewall.  Consulte a referência do artigo abaixo para obter mais informações.

Para o Windows Server 2012 e o Windows Server 2008, você precisará instalar um hotfix primeiro.  Você pode fazer referência a este artigo da base de conhecimento: [Perda de datagrama ao executar um aplicativo receptor multicast no Windows 8 e no Windows Server 2012](https://support.microsoft.com/kb/2808584)

### <a name="update-network-drivers"></a>Atualizar drivers de rede
Alguns fornecedores de rede têm atualizações de driver que melhoram o desempenho com relação à latência de driver e ao buffer de pacotes UDP.  Entre em contato com seu fornecedor de rede para ver se há atualizações para ajudar com a taxa de transferência UDP.

## <a name="logging-for-auditing-purposes"></a>Registro em log para fins de auditoria
Para atender às normas de rastreamento de tempo, você pode arquivar manualmente logs de w32tm, logs de eventos e informações do monitor de desempenho.  Posteriormente, as informações arquivadas podem ser usadas para atestar a conformidade em um momento específico no passado.  Os fatores a seguir são usados para indicar a precisão.


1. Precisão do relógio usando o contador de monitor de desempenho de deslocamento de tempo computado.  Isso mostra o relógio com a precisão desejada.
2.  Fonte do relógio que procura "resposta de par de" nos logs de W32tm.   Após o texto da mensagem está o endereço IP ou VMIC, que descreve a fonte de tempo e o próximo na cadeia de relógios de referência a serem validados.
3.  Status da condição de relógio usando os logs de w32tm para validar que "disciplina de ClockDispl: \*O\*tempo\*de distorção "está ocorrendo.  Isso indica que o W32tm está ativo no momento.

### <a name="event-logging"></a>Log de eventos
Para obter a história completa, você também precisará de informações do log de eventos.  Coletando o log de eventos do sistema e filtrando no time-Server, Microsoft-Windows-kernel-boot, Microsoft-Windows-kernel-General, você poderá descobrir se há outras influências que alteraram a hora, por exemplo, terceiros.  Esses logs podem ser necessários para eliminar a interferência externa.  A diretiva de grupo pode afetar quais logs de eventos são gravados no log.  Consulte a seção acima sobre como usar Política de Grupo para obter mais detalhes.

### <a name="W32Logging"></a>Log de depuração do W32time
Para habilitar o W32tm para fins de auditoria, o comando a seguir habilita o registro em log que mostra as atualizações periódicas do relógio e indica o relógio de origem.  Reinicie o serviço para habilitar o novo log.  

Para obter mais informações, consulte [como ativar o log de depuração no serviço de tempo do Windows](https://support.microsoft.com/kb/816043).

    w32tm /debug /enable /file:C:\Windows\Temp\w32time-test.log /size:10000000 /entries:0-73,103,107,110

### <a name="performance-monitor"></a>Monitor de Desempenho
O serviço Windows Server 2016 Windows Time expõe contadores de desempenho que podem ser usados para coletar logs de auditoria.  Eles podem ser registrados localmente ou remotamente.  Você pode registrar os contadores de deslocamento de horário do computador e atraso de ida e volta.  
E, como qualquer contador de desempenho, você pode monitorá-los remotamente e criar alertas usando System Center Operations Manager.  Você pode, por exemplo, usar um alerta para soar quando o deslocamento de tempo se desloca da precisão desejada.  O [pacote de gerenciamento do System Center](https://social.technet.microsoft.com/wiki/contents/articles/15251.system-center-management-pack-authoring-guide.aspx) tem mais informações.

### <a name="windows-traceability-example"></a>Exemplo de rastreamento do Windows
Em arquivos de log do w32tm, você desejará validar duas partes de informação.  A primeira é uma indicação de que o arquivo de log tem o relógio de condição no momento.  Isso prova que o relógio estava sendo condicionado pelo serviço de tempo do Windows no momento da disputa.

    151802 20:18:32.9821765s - ClockDispln Discipline: *SKEW*TIME* - PhCRR:223 CR:156250 UI:100 phcT:65 KPhO:14307
    151802 20:18:33.9898460s - ClockDispln Discipline: *SKEW*TIME* - PhCRR:1 CR:156250 UI:100 phcT:64 KPhO:41
    151802 20:18:44.1090410s - ClockDispln Discipline: *SKEW*TIME* - PhCRR:1 CR:156250 UI:100 phcT:65 KPhO:38

O ponto principal é que você vê mensagens prefixadas com a disciplina ClockDispln, que é a prova de que o W32Time está interagindo com o relógio do sistema.
 
Em seguida, você precisa localizar o último relatório no log antes do tempo de contestado que relata o computador de origem que está sendo usado no momento como o relógio de referência.  Pode ser um endereço IP, um nome de computador ou o provedor VMIC, que indica que está sincronizando com o host do Hyper-V.  O exemplo a seguir fornece um endereço IPv4 de 10.197.216.105.

    151802 20:18:54.6531515s - Response from peer 10.197.216.105,0x8 (ntp.m|0x8|0.0.0.0:123->10.197.216.105:123), ofs: +00.0012218s

Agora que você validou o primeiro sistema na cadeia de tempo de referência, você precisa investigar o arquivo de log na fonte de tempo de referência e repetir as mesmas etapas.  Isso continuará até que você chegue a um relógio físico, como GPS ou uma fonte de tempo conhecida como NIST.  Se o relógio de referência for um hardware de GPS, os logs dos fabricados também poderão ser necessários.

## <a name="network-considerations"></a>Considerações de rede
Os algoritmos de protocolo NTP têm uma dependência da simetria da sua rede.  À medida que aumenta o número de saltos de rede, a probabilidade de assimetria aumenta.  Nesse caso, é difícil prever quais tipos de imprecisões você verá em seus ambientes específicos. 

O monitor de desempenho e os novos contadores de tempo do Windows no Windows Server 2016 podem ser usados para avaliar a precisão dos seus ambientes e criar linhas de base. Além disso, você pode executar a solução de problemas para determinar o deslocamento atual de qualquer computador em sua rede.

Há dois padrões gerais de tempo preciso na rede.  O PTP ([protocolo de tempo de precisão-IEEE 1588](https://www.nist.gov/el/intelligent-systems-division-73500/introduction-ieee-1588)) tem requisitos mais rígidos na infraestrutura de rede, mas, muitas vezes, pode fornecer a precisão de menos microssegundos.  O NTP ([protocolo NTP – RFC 1305](https://tools.ietf.org/html/rfc1305)) funciona em uma grande variedade de redes e ambientes, o que torna mais fácil gerenciar. 

O Windows dá suporte ao NTP simples (RFC2030) por padrão para computadores não ingressados no domínio.  Para computadores ingressados no domínio, usamos um NTP seguro chamado [MS-SNTP](https://msdn.microsoft.com/library/cc246877.aspx), que aproveita os segredos negociados no domínio, que fornecem uma vantagem de gerenciamento sobre o NTP autenticado, descrito em RFC1305 e RFC5905.   

O domínio e os protocolos não ingressados no domínio exigem a porta UDP 123.  Para saber mais sobre as práticas recomendadas de NTP, confira [protocolo NTP rascunho da IETF de práticas recomendadas](https://tools.ietf.org/html/draft-ietf-ntp-bcp-00).

### <a name="reliable-hardware-clock-rtc"></a>Relógio de hardware confiável (RTC)
O Windows não Percorra o tempo, a menos que determinados limites sejam excedidos, mas, em vez disso, disciplinam o relógio.  Isso significa que o W32tm ajusta a frequência do relógio em um intervalo regular, usando a configuração frequência de atualização do relógio, que usa como padrão uma segunda com o Windows Server 2016.  Se o relógio estiver atrás, ele acelerará a frequência e, se ela estiver à frente, reduzirá a frequência.  No entanto, durante esse tempo entre os ajustes de frequência do relógio, o relógio do hardware está no controle.  Se houver um problema com o firmware ou com o relógio do hardware, a hora no computador poderá se tornar menos precisa.

Essa é outra razão pela qual você precisa testar e fazer a linha de base em seu ambiente.  Se o contador de desempenho "deslocamento de tempo computado" não se estabilizar com a precisão que você está direcionando, talvez você queira verificar se o firmware está atualizado.  Como outro teste, você pode ver se o hardware duplicado reproduza o mesmo problema.

### <a name="troubleshooting-time-accuracy-and-ntp"></a>Solucionando problemas de precisão de tempo e NTP
Você pode usar a seção descobrindo a hierarquia acima para entender a origem do tempo impreciso.  Olhando o deslocamento de tempo, localize o ponto na hierarquia em que o tempo deriva o máximo de sua origem de NTP.  Depois de entender a hierarquia, você desejará tentar entender por que essa fonte de tempo específica não recebe tempo preciso.  

Concentrando-se no sistema com o tempo divergente, você pode usar essas ferramentas abaixo para coletar mais informações para ajudá-lo a determinar o problema e encontrar uma resolução.  A referência de UpstreamClockSource abaixo é o relógio descoberto usando "w32tm/config/status".


- Logs de eventos do sistema
- Habilitar o registro em log usando: w32tm logs-w32tm/Debug/Enable/file: C:\Windows\Temp\w32time-test.log/Size: 10000000/Entries: 0-300
- chave do registro w32Time HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Services\W32Time
- Rastreamentos de rede local
- Contadores de desempenho (do computador local ou do UpstreamClockSource)
- W32tm/stripchart/Computer: UpstreamClockSource
- PING UpstreamClockSource para entender a latência e o número de saltos para a origem
- UpstreamClockSource de tracert

Problema|    Sintomas|   Resolução|
----- | ----- | ----- |
O relógio de TSC local não é estável.| Usando Perfmon-computador físico – relógio estável do relógio de sincronização, mas você ainda vê que a cada 1-2 minutos de vários 100US. |   Atualizar o firmware ou validar hardware diferente não exibe o mesmo problema.|
Latência de rede|    w32tm stripchart exibe um RoundTripDelay de mais de 10 ms.  A variação no atraso causa ruído tão grande quanto 1/2 do tempo de ida e volta, por exemplo, um atraso que só está em uma direção.</br></br>UpstreamClockSource é vários saltos, conforme indicado pelo PING.  A TTL deve ser próxima de 128.</br></br>Use tracert para localizar a latência em cada salto.    | Encontre uma fonte de relógio mais próxima para o tempo.  Uma solução é instalar um relógio de origem no mesmo segmento ou apontar manualmente para o relógio de origem que está geograficamente mais próximo.  Para um cenário de domínio, adicione um computador com a função GTimeServ. |  
Não é possível acessar a origem do NTP de maneira confiável|    W32tm/stripchart retorna intermitentemente "tempo limite de solicitação esgotado"    |A origem do NTP não está respondendo|
A origem do NTP não está respondendo|    Verifique os contadores Perfmon para contagem de origem do cliente NTP, solicitações de entrada do servidor NTP, respostas de saída do servidor NTP e determine seu uso em comparação com as linhas de base.|    Usando contadores de desempenho do servidor, determine se a carga foi alterada em referência às suas linhas de base.</br></br>Há problemas de congestionamento de rede?|
O controlador de domínio não está usando o relógio mais preciso|    Alterações na topologia ou no relógio de tempo mestre adicionado recentemente.|   w32tm/resync/rediscover|
Os relógios do cliente estão descompassos| Evento de serviço de tempo 36 no log de eventos do sistema e/ou texto no arquivo de log que descreve que: Contador "contagem de origens de tempo do cliente NTP" indo de 1 para 0|Solucionar problemas de origem de upstream e entender se ele está sendo executado em problemas de desempenho.|

### <a name="baselining-time"></a>Tempo de linha de base
A linha de base é importante para que você possa primeiro entender o desempenho e a precisão da sua rede e comparar com o que ocorre no futuro quando ocorrem problemas.  Você desejará criar uma linha de base do PDC raiz ou de quaisquer computadores marcados com o GTIMESRV.  Também recomendamos que você linha de base do PDC em cada floresta.  Finalmente, escolha todos os DCs ou máquinas críticos que tenham características interessantes, como distância ou altas cargas e linhas de base.

Também é útil para a linha de base do Windows Server 2016 vs 2012 R2. no entanto, você só tem w32tm/stripchart como uma ferramenta que pode ser usada para comparar, já que o Windows Server 2012R2 não tem contadores de desempenho.  Você deve escolher dois computadores com as mesmas características ou atualizar um computador e comparar os resultados após a atualização.  O adendo às medições de tempo do Windows tem mais informações sobre como fazer medidas detalhadas entre 2016 e 2012.

Usando todos os contadores de desempenho do W32Time, colete dados por pelo menos uma semana.  Isso garantirá que você tenha o suficiente de uma referência para considerar vários na rede ao longo do tempo e o suficiente de uma execução para fornecer confiança de que sua precisão de tempo é estável.

### <a name="ntp-server-redundancy"></a>Redundância de servidor NTP
Para a configuração manual do servidor NTP usada com computadores não ingressados no domínio ou o PDC, ter mais de um servidor é uma boa medida de redundância em caso de disponibilidade.  Ele também pode oferecer maior precisão, supondo que todas as fontes sejam precisas e estáveis.  No entanto, se a topologia não estiver bem projetada ou se as fontes de tempo não forem estáveis, a precisão resultante poderá ser pior, portanto, é recomendável.  O limite de servidores de tempo com suporte W32Time pode ser referenciado manualmente é 10. 

## <a name="leap-seconds"></a>Segundos bissextos
O período de rotação da terra varia ao longo do tempo, causado por eventos Climatic e geological. Normalmente, a variação é cerca de um segundo a cada dois anos. Sempre que a variação do tempo atômico cresce muito grande, uma correção de um segundo (para cima ou para baixo) é inserida, chamada um segundo bissexto. Isso é feito de forma que a diferença nunca exceda 0,9 segundos. Essa correção é anunciada seis meses à frente da correção real. Antes do Windows Server 2016, o serviço de tempo da Microsoft não estava ciente de segundos bissextos, mas dependia do serviço de tempo externo para cuidar disso. Com a maior precisão de tempo do Windows Server 2016, a Microsoft está trabalhando em uma solução mais adequada para o segundo problema bissexto.

## <a name="secure-time-seeding"></a>Propagação de tempo seguro
O W32time no servidor 2016 inclui o recurso de propagação de tempo seguro. Esse recurso determina o tempo atual aproximado das conexões SSL de saída.  Esse valor de tempo é usado para monitorar o relógio do sistema local e corrigir quaisquer erros brutos. Você pode ler mais sobre o recurso nesta [postagem no blog](https://blogs.msdn.microsoft.com/w32time/2016/09/28/secure-time-seeding-improving-time-keeping-in-windows/).  Em implantações com fontes de tempo confiáveis e computadores bem monitorados que incluem o monitoramento de deslocamentos de tempo, você pode optar por não usar o recurso de propagação de tempo seguro e, em vez disso, confiar em sua infraestrutura existente. 

Você pode desabilitar o recurso com estas etapas:

1.  Defina o valor de configuração do registro UtilizeSSLTimeData como 0 em um computador específico:

    REG add HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Services\w32time\Config/v UtilizeSslTimeData/t REG_DWORD/d 0/f


2.  Se não for possível reinicializar o computador imediatamente por algum motivo, você poderá notificar o serviço W32time sobre a atualização de configuração. Isso interrompe o monitoramento e a imposição de tempo com base nos dados de tempo coletados de conexões SSL. 

    W32tm. exe/config/Update

3.  A reinicialização da máquina torna a configuração efetiva imediatamente e também faz com que ela pare de coletar todos os dados de tempo das conexões SSL.  A última parte tem uma sobrecarga muito pequena e não deve ser uma preocupação com o desempenho.

4.  Para aplicar essa configuração em um domínio inteiro, defina o valor de UtilizeSSLTimeData na configuração da política de grupo W32time como 0 e publique a configuração.  Quando a configuração é selecionada por um cliente Política de Grupo, o serviço W32time é notificado e interromperá o monitoramento e a imposição de tempo usando dados de tempo SSL. A coleta de dados de tempo SSL será interrompida quando cada computador for reinicializado. Se o seu domínio tiver laptops/tablets Slim portáteis e outros dispositivos, talvez você queira excluir esses computadores dessa alteração de política. Esses dispositivos acabarão se esgotando na bateria e precisarão do recurso de propagação de tempo seguro para inicializar seu tempo.


