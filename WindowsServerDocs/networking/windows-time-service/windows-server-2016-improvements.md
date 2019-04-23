

## <a name="windows-server-2016-improvements"></a>Aprimoramentos do Windows Server 2016
### <a name="windows-time-service-and-ntp"></a>NTP e o serviço de tempo do Windows
Windows Server 2016 melhorou os algoritmos que usa a hora correta e o relógio local para sincronizar com o UTC de condição.  NTP usa 4 valores para calcular a diferença de tempo, com base em carimbos de hora do cliente de solicitação/resposta e solicitação/resposta do servidor.  No entanto, redes são "ruidosas" e pode haver picos nos dados de NTP devido a congestionamento da rede e outros fatores que afetam a latência de rede.  Algoritmos de Windows 2016 médio esse ruído usando uma série de técnicas diferentes que resulta em um relógio estável e preciso.  Além disso, o código-fonte usamos para referências de tempo preciso uma API aprimorada que nos dá a melhor resolução.  Com essas melhorias somos capazes de atingir uma precisão de 1 ms em relação ao UTC em um domínio.

### <a name="hyper-v"></a>Hyper-V
Windows 2016 melhorou o serviço TimeSync Hyper-V. Melhorias incluem o tempo inicial mais preciso no início da VM ou restauração da VM e correção de latência de interrupção para os exemplos fornecidos para w32time.  Essa melhoria permite permanecer 10µs do host com um RMS (raiz média ao quadrado, que indica a variância), de 50µs, até mesmo em um computador com a carga de 75%. Para obter mais informações, consulte [arquitetura do Hyper-V](https://msdn.microsoft.com/library/cc768520.aspx).

> [!NOTE]
> Carga foi criada usando o benchmark prime95 usando perfil equilibrada.

Além disso, o nível de camada que o Host informa ao convidado é mais transparente.  Anteriormente, o Host apresentaria um stratum fixo de 2, independentemente de sua precisão.  Com as alterações no Windows Server 2016, o host informa um stratum maior do que a camada de host, o que resulta em melhor momento para convidados virtuais.  A camada do host é determinada pelo w32time por meios normais com base em seu tempo de origem.  Convidados encontrará o relógio mais preciso, em vez de padronizar para o host de 2016 do Windows ingressados no domínio.  Foi por isso que é aconselhável para desabilitar manualmente a configuração de provedor de tempo do Hyper-V para máquinas de participantes em um domínio no Windows 2012R2 e abaixo.

### <a name="monitoring"></a>Monitoramento
Contadores de desempenho foram adicionados.  Eles permitem a linha de base, monitoram e solucionar problemas de precisão de tempo.  Esses contadores incluem:

Contador|Descrição|
----- | ----- |
Computado a diferença de horário|   O tempo absoluto deslocamento entre o relógio do sistema e a fonte de tempo selecionado, conforme calculado pelo serviço W32Time em microssegundos. Quando um novo exemplo válido estiver disponível, o tempo calculado é atualizado com o deslocamento de tempo indicado pelo exemplo. Isso é o deslocamento de tempo real do relógio local. W32Time inicia usando esse deslocamento de correção de relógio e atualiza o tempo calculado entre amostras com o deslocamento de tempo restantes que precisa ser aplicada para o relógio local. Precisão do relógio pode ser controlado usando este contador de desempenho com um intervalo de sondagem de baixa (por exemplo: 256 segundos ou menos) e procurando o valor do contador a ser menores que o limite de precisão de relógio desejado.|
Ajuste de frequência do relógio| O ajuste de frequência do relógio absoluto feito ao relógio do sistema local por W32Time em partes cada bilhão. Esse contador ajuda a visualizar as ações que estão sendo tomadas pelo W32time.|
Atraso de ida e volta NTP|    Atraso de ida e volta mais recente no cliente do NTP em receber uma resposta do servidor, em microssegundos. Esse é o tempo decorrido no cliente entre transmitir uma solicitação para o servidor NTP e receber uma resposta válida do servidor NTP. Esse contador ajuda a caracterizar os atrasos experientes pelo cliente NTP. Idas e voltas maiores ou variadas podem adicionar ruído para cálculos de tempo NTP, que por sua vez, podem afetar a precisão da sincronização de horário por meio de NTP.|
Contagem de origem do cliente NTP|    Número de ativo das fontes de horário NTP que está sendo usado pelo cliente NTP. Essa é uma contagem de Active Directory e distintos endereços IP dos servidores de tempo que estão respondendo às solicitações desse cliente. Esse número pode ser maior ou menor que os pares configurados, dependendo da resolução DNS de nomes de par e a capacidade de alcance atual.|
Servidor NTP solicitações de entrada|   Número de solicitações recebidas pelo servidor NTP (solicitações/s).|
Respostas de saída do servidor NTP|  Número de solicitações atendidas pelo servidor NTP (respostas/s).|

Os contadores de 3 primeiros cenários para solução de problemas de precisão de destino.  A precisão de tempo de solução de problemas e o NTP seção abaixo, sob [práticas recomendadas](#BestPractices), tem mais detalhes.
Os últimos 3 contadores abrangem cenários de servidor NTP e são úteis quando determinar a carga e a linha de base seu desempenho atual.

### <a name="configuration-updates-per-environment"></a>Atualizações de configuração por ambiente
O exemplo a seguir descreve as alterações na configuração de padrão entre Windows 2016 e versões anteriores para cada função.  As configurações do Windows Server 2016 e atualização de aniversário do Windows 10 (build 14393), agora são exclusivos, por isso, há são mostrados como colunas separadas. 

|Função|Configuração|Windows Server 2016|Windows 10|Windows Server 2012 R2</br>Windows Server 2008 R2</br>Windows 10|
|---|---|---|---|---|
|**Standalone/Nano Server**||||
| |*Servidor de horário*|time.windows.com|N/D|time.windows.com|
| |*Frequência de sondagem*|64 - 1024 segundos|N/D|Uma vez por semana|
| |*Frequência de atualização de relógio*|Uma vez por segundo|N/D|Uma vez por hora|
|**Cliente autônomo**||||
| |*Servidor de horário*|N/D|time.windows.com|time.windows.com|
| |*Frequência de sondagem*|N/D|Uma vez por dia|Uma vez por semana|
| |*Frequência de atualização de relógio*|N/D|Uma vez por dia|Uma vez por semana|
|**Controlador de domínio**||||
| |*Servidor de horário*|PDC/GTIMESERV|N/D|PDC/GTIMESERV|
| |*Frequência de sondagem*|64-1024 segundos|N/D|1024 - 32768 segundos|
| |*Frequência de atualização de relógio*|Uma vez por dia|N/D|Uma vez por semana|
|**Servidor membro do domínio**||||
| |*Servidor de horário*|DC|N/D|DC|
| |*Frequência de sondagem*|64-1024 segundos|N/D|1024 - 32768 segundos|
| |*Frequência de atualização de relógio*|Uma vez por segundo|N/D|Uma vez a cada 5 minutos|
|**Cliente de membro do domínio**||||
| |*Servidor de horário*|N/D|DC|DC|
| |*Frequência de sondagem*|N/D|1204 - 32768 segundos|1024 - 32768 segundos|
| |*Frequência de atualização de relógio*|N/D|Uma vez a cada 5 minutos|Uma vez a cada 5 minutos|
|**Convidado do Hyper-V**||||
| |*Servidor de horário*|Escolhe a melhor opção com base na camada do servidor de Host e a hora|Escolhe a melhor opção com base na camada do servidor de Host e a hora|Padrões para Host|
| |*Frequência de sondagem*|Com base na função acima|Com base na função acima|Com base na função acima|
| |*Frequência de atualização de relógio*|Com base na função acima|Com base na função acima|Com base na função acima|

>[!NOTE]
>Para o Linux no Hyper-V, consulte o [permitindo que o Linux para usar a hora do Host do Hyper-V](#AllowingLinux) seção abaixo.

### <a name="impact-of-increased-polling-and-clock-update-frequency"></a>Impacto do aumento de sondagem e a frequência de atualização de relógio
Para fornecer um tempo mais preciso, os valores padrão para atualizações de relógio e frequências de sondagem são aumentados que nos permitem fazer pequenos ajustes com mais frequência.  Isso fará com que mais tráfego UDP/NTP, no entanto, esses pacotes são pequenos, por isso deve haver muito pouco ou nenhum impacto em links de banda larga. No entanto, o benefício é que a hora deve ser melhor em uma variedade de ambientes e de hardware.

Para dispositivos de bateria reserva, aumentar a frequência de sondagem pode causar problemas.  Dispositivos de bateria não armazenam o momento enquanto estiverem desativada.  Ao serem retomadas, ele pode exigir frequentes correções para o relógio.  Aumentar a frequência de sondagem fará com que o relógio fique instável e também pode usar mais energia.  A Microsoft recomenda que você não altere as configurações padrão do cliente.

Controladores de domínio devem ser minimamente afetados, mesmo com o efeito multiplicado das atualizações de aumento de clientes do NTP em um domínio do AD.  NTP tem um consumo de recursos muito menor em comparação com outros protocolos e um impacto marginal.  Há mais probabilidade de alcançar os limites para outra funcionalidade de domínio antes que está sendo afetado pelas configurações do maior para o Windows Server 2016.  Active Directory usar NTP seguro, o que tende a hora de sincronização com uma precisão menor do que NTP simple, mas verificamos se que ele podem chegar a camada de clientes dois para longe do controlador de domínio primário.

Como um plano conservadora, você deve reservar 100 solicitações NTP por segundo por núcleo.  Por exemplo, um domínio composto por 4 controladores de domínio com 4 núcleos, você deve ser capaz de atender às solicitações NTP 1600 por segundo.  Se você tiver 10 clientes k configurados para sincronizar uma vez a cada 64 segundos, a hora e as solicitações são recebidas uniformemente ao longo do tempo, você verá 10.000/64 ou cerca de 160 solicitações por segundo, espalhadas por todos os controladores de domínio.  Isso se encaixa facilmente no nosso 1600 NTP solicitações/s com base neste exemplo.  Esses são conservadoras recomendações de planejamento e certamente têm uma grande dependência em sua rede, velocidades de processador e de carga, assim, como sempre linha de base e teste em seus ambientes.

Também é importante observar que se seus controladores de domínio estiver executando com uma carga de CPU considerável, mais de 40%, isso quase certamente Adicionar ruído para respostas NTP e afetar sua precisão de tempo em seu domínio.  Novamente, você precisa testar em seu ambiente para entender os resultados reais.

## <a name="time-accuracy-measurements"></a>Medidas de precisão de tempo
### <a name="methodology"></a>Metodologia
Para medir a precisão de tempo para o Windows Server 2016, nós usamos uma variedade de ambientes, métodos e ferramentas.  Você pode usar essas técnicas para medir e ajustar seu ambiente e determinar se os resultados de exatidão atender às suas necessidades. 

Nosso relógio de origem do domínio é formada por dois servidores NTP de alta precisão com hardware GPS.  Também usamos uma máquina de teste de referência separada para as medidas, que também tinham o hardware GPS de alta precisão instalado de outro fabricante.  Para alguns dos testes, você precisará de uma fonte de relógio precisas e confiáveis para usar como referência, além de sua fonte de relógio do domínio.

Usamos os quatro métodos diferentes para medir a precisão com máquinas virtuais e físicas. Vários métodos fornecidos significa independentes para validar os resultados.


1. Medir o relógio local, que é condicionado w32tm, em relação à nossa máquina de teste de referência que tem o hardware separado de GPS.  
2.  Medida pings NTP do servidor NTP para os clientes que usam W32tm "stripchart"
3.  Medida pings NTP do cliente para o servidor NTP usando W32tm "stripchart"
4.  Medida Hyper-V resulta do host para o convidado usando o contador de carimbo de data / hora (TSC).  Esse contador é compartilhado entre partições e a hora do sistema em ambas as partições.  Calculamos a diferença do host e o horário do cliente na máquina virtual.  Em seguida, usamos o relógio TSC para interpolar o tempo de host do convidado, já que as medidas não acontecem ao mesmo tempo.  Além disso, usamos o fator de relógio TSV os atrasos e a latência na API.

W32tm é interno, mas outras ferramentas utilizadas durante o nosso teste estão disponíveis para o repositório da Microsoft no GitHub como código-fonte aberto para seus testes e o uso.  O WIKI do repositório tem mais informações que descrevem como usar as ferramentas para fazer medições.

> [https://github.com/Microsoft/Windows-Time-Calibration-Tools](https://github.com/Microsoft/Windows-Time-Calibration-Tools)

Os resultados de teste mostrados a seguir são um subconjunto de medidas que fizemos em um dos ambientes de teste.  Eles ilustram a precisão mantida no início da hierarquia de tempo e cliente do domínio filho no final da hierarquia de tempo.  Isso é comparado com as mesmas máquinas em uma topologia de 2012 com base para comparação.

### <a name="topology"></a>Topologia
Para comparação, testamos um Windows Server 2012 R2 e Windows Server 2016 com base em topologia.  Ambas as topologias consistem em duas máquinas host Hyper-V físicas que fazem referência a um computador Windows Server 2016 com hardware de relógio GPS instalado.  Cada host executa 3 convidados do windows ingressados no domínio, que são organizados de acordo com a topologia a seguir.  As linhas representam a hierarquia de tempo e o protocolo/transporte é usado.

![Tempo do Windows](../media/Windows-Time-Service/Windows-2016-Accurate-Time/topology1.png)

![Tempo do Windows](../media/Windows-Time-Service/Windows-2016-Accurate-Time/topology2.png)

### <a name="graphical-results-overview"></a>Visão geral gráfica de resultados
Os grafos a seguir representam a precisão de tempo para dois membros específicos em um domínio com base na topologia acima.  Cada gráfico exibirá o Windows Server 2012 R2 e 2016 resultados sobrepostos, que demonstra as melhorias visualmente.  A precisão foi medida na máquina convidada em comparação com o host.  Os dados de gráficos representam um subconjunto de todo o conjunto de testes que fizemos e mostra o melhor das hipóteses e os piores cenários.  

![Tempo do Windows](../media/Windows-Time-Service/Windows-2016-Accurate-Time/topology3.png)

### <a name="performance-of-the-root-domain-pdc"></a>Desempenho do domínio raiz da PDC
O PDC raiz é sincronizado com o host do Hyper-V (usando VMIC) que é um Windows Server 2016 com hardware GPS que provou para ser precisas e estável.  Isso é um requisito essencial para precisão ms 1, o que é mostrado como verde área sombreada.

![Tempo do Windows](../media/Windows-Time-Service/Windows-2016-Accurate-Time/chart1.png)

### <a name="performance-of-the-child-domain-client"></a>Desempenho do cliente do domínio filho
O cliente do domínio filho é anexado a um PDC do domínio filho que se comunica com o PDC raiz.  Tempo também está dentro de 1 ms requisito.

![Tempo do Windows](../media/Windows-Time-Service/Windows-2016-Accurate-Time/chart2.png)


### <a name="long-distance-test"></a>Teste de longa distância
O gráfico a seguir compara o salto de rede virtual de 1 a 6 saltos de rede física com o Windows Server 2016.  Dois gráficos estão sobrepostos entre si com transparência para mostrar dados sobrepostos.  Cada vez maior de saltos de rede significa que a latência mais alta e desvios de tempo maiores.  O gráfico é ampliada e, portanto, o 1 ms limites, representados pela área de verde, é maior.  Como você pode ver, o tempo ainda estiver dentro de 1 ms com vários saltos.  Ele é negativamente deslocado, que demonstra uma assimetria de rede.  Obviamente, cada rede é diferente e medidas dependem de uma variedade de fatores ambientais.

![Tempo do Windows](../media/Windows-Time-Service/Windows-2016-Accurate-Time/chart3.png)

## <a name="BestPractices"></a>Práticas recomendadas para manutenção de horas precisa
### <a name="solid-source-clock"></a>Relógio de origem sólida
Um tempo de máquinas só é tão bom quanto o relógio de origem, que ele sincroniza com o.  Para atingir a 1 ms de precisão, você precisará de hardware GPS ou um dispositivo de tempo em sua rede que você referenciar como o relógio de origem mestre.  Usando o padrão de time.windows.com, pode não fornecer uma origem de hora local e estável.  Além disso, como você obtém mais distante o relógio do código-fonte, a rede afeta a precisão.  Tendo um relógio de origem mestre em cada data center é necessária para a maior precisão.

### <a name="hardware-gps-options"></a>Opções de GPS de hardware
Há várias soluções de hardware que podem oferecer tempo preciso.  Em geral, soluções hoje em dia são baseadas em antenas GPS.  Também há rádio e soluções de modem dial-up usando linhas dedicadas.  Eles conectam à sua rede como um dispositivo ou conecte um PC, por exemplo, Windows por meio de um dispositivo USB ou de PCIe.  Diferentes opções fornecerão diferentes níveis de precisão e, como sempre, os resultados dependem de seu ambiente.  Variáveis que afetam a precisão incluem disponibilidade GPS, estabilidade da rede e de carga e de Hardware para PCs.  Esses são fatores importantes ao escolher um relógio de origem, conforme declarei, é um requisito para tempo estável e preciso.

### <a name="domain-and-synchronizing-time"></a>Sincronizar a hora e domínio
Membros do domínio usam a hierarquia de domínio para determinar qual máquina usarem como uma fonte para sincronizar a hora.  Cada membro do domínio será encontrar outro computador para sincronizar com o e salve-o como fonte do relógio.  Cada tipo de membro do domínio segue um conjunto diferente de regras para encontrar uma origem de relógio para sincronização de hora.  O PDC na raiz da floresta é a fonte de tempo padrão para todos os domínios.  Abaixo estão as diferentes funções e a descrição de alto nível sobre como eles encontram uma fonte de:


- **Controlador de domínio com a função de PDC** – esta máquina é a fonte de horário autoritativo para um domínio. Ele terá a hora mais precisa disponível no domínio e deve sincronizar com um controlador de domínio no domínio pai, exceto em casos em que [GTIMESERV](#GTIMESERV) função está habilitada. 
- **Qualquer outro controlador de domínio** – esse computador atuará como uma fonte de tempo para clientes e servidores membro no domínio. Um controlador de domínio pode ser sincronizados com o PDC do seu próprio domínio ou qualquer controlador de domínio em seu domínio pai.
- **Servidores de clientes/membro** – esta máquina pode sincronizar com qualquer controlador de domínio ou o PDC do seu próprio domínio, ou um controlador de domínio ou PDC no domínio pai.

Um sistema de pontuação com base nos candidatos disponíveis, é usado para encontrar a melhor fonte de tempo.  Esse sistema leva em conta a confiabilidade de fonte de tempo e seu local relativo.  Isso acontece depois que quando o tempo é o serviço foi iniciado.  Se você precisar ter controle mais refinado dos como sincroniza a hora, você pode adicionar servidores de horário BOM em locais específicos ou adicionar redundância.  Consulte a [especificar um Local confiável tempo de serviço usando GTIMESERV](#GTIMESERV) seção para obter mais informações.

#### <a name="mixed-os-environments-win2012r2-and-win2008r2"></a>Ambientes de sistema operacional misto (Win2012R2 e Win2008R2)
Embora um ambiente puro do domínio do Windows Server 2016 é necessário para a maior precisão, ainda existem benefícios em um ambiente misto.  Implantar o Windows Server 2016 Hyper-V em um domínio do Windows 2012, os convidados serão beneficiados devido aos aprimoramentos mencionado acima, mas somente se os convidados também são Windows Server 2016.  Um PDC do Windows Server 2016, será capaz de fornecer o tempo mais preciso por causa de algoritmos aprimorados, ela será uma fonte mais estável.  Como substituir seu PDC pode não ser uma opção, em vez disso, você pode adicionar um controlador de domínio do Windows Server 2016 com o [GTIMESERV](#GTIMESERV) reverter o conjunto que seria uma atualização em precisão para seu domínio.  Um controlador de domínio do Windows Server 2016 podem oferecer melhor momento para clientes de tempo de downstream, no entanto, ele só é tão bom quanto o seu tempo NTP do código-fonte.

Também, conforme mencionado acima, a frequência de sondagem e atualização do relógio ter sido modificada com o Windows Server 2016.  Esses podem ser alteradas manualmente para seus controladores de domínio de nível inferior ou podem ser aplicadas por meio da diretiva de grupo.  Embora não testamos essas configurações, deve se comportam bem em Win2008R2 e Win2012R2 e fornecer alguns benefícios.

Versões anteriores do Windows Server 2016 tinha um vários problemas, mantendo o tempo preciso manter que resultou no tempo de sistema há imediatamente após um ajuste foi feito.  Por causa disso, como obter amostras de tempo de uma fonte NTP precisa com frequência e condicionamento o relógio local com os dados leva à menor descompasso em seus relógios do sistema no período de amostragem redes internas, resultando em um melhor tempo manter versões de sistema operacional de nível inferior. A melhor precisão observada era aproximadamente 5 ms quando um cliente do Windows Server 2012 R2 NTP, configurado com as configurações de alta precisão, sincronizado seu tempo de um servidor Windows 2016 NTP preciso.

Em alguns cenários que envolvem a controladores de domínio do convidado, exemplos TimeSync Hyper-V podem interromper sincronização de horário do domínio.  Isso não deve ser um problema para convidados do Server 2016 em execução em hosts do Server 2016 Hyper-V.

Para desabilitar o serviço do Hyper-V TimeSync do fornecimento de exemplos para w32time, defina a seguinte chave do registro convidado:

    HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Services\W32Time\TimeProviders\VMICTimeProvider 
    "Enabled"=dword:00000000

#### <a name="AllowingLinux"></a>Permitindo que o Linux para usar a hora do Host do Hyper-V
Para convidados do Linux em execução no Hyper-V, os clientes normalmente são configurados para usar o daemon do NTP para sincronização de hora em relação a servidores NTP.  Se a distribuição do Linux dá suporte ao protocolo TimeSync versão 4 e o convidado do Linux tem o serviço de integração TimeSync habilitado, ele sincronizará com o tempo de host. Isso pode levar a inconsistente tempo mantendo se ambos os métodos estão habilitados.

Para sincronizar exclusivamente com o tempo de host, é recomendável desabilitar a sincronização de horário NTP por qualquer um:

- Desabilitando todos os servidores NTP no arquivo NTP.
- ou desabilitando o daemon do NTP

Nessa configuração, o parâmetro de servidor de horário é este host.  Sua frequência de sondagem é de 5 segundos e a frequência de atualização do relógio também é de 5 segundos.

Para sincronizar exclusivamente pela NTP, é recomendável desabilitar o serviço de integração TimeSync no convidado.

> [!NOTE]
> Observação:  Suporte para o tempo preciso com convidados do Linux requer um recurso que é compatível apenas com os kernels de Linux mais recente upstream e não é algo que é amplamente disponíveis em todas as distribuições do Linux ainda. Referencie [máquinas virtuais de suporte para Linux e FreeBSD para Hyper-V no Windows](https://technet.microsoft.com/windows-server-docs/virtualization/hyper-v/supported-linux-and-freebsd-virtual-machines-for-hyper-v-on-windows) para obter mais detalhes sobre as distribuições de suporte.

#### <a name="GTIMESERV"></a>Especifique um serviço de tempo confiável Local usando GTIMESERV
Você pode especificar um ou mais controladores de domínio como relógios precisas de origem usando o GTIMESERV, um bom tempo de servidor, sinalizadores.  Por exemplo, controladores de domínio específicos equipados com hardware GPS podem ser sinalizadas como um GTIMESERV.  Isso garantirá que seu domínio faz referência a um relógio com base no hardware GPS.

> [!NOTE]
> Para obter mais informações sobre os sinalizadores de domínio podem ser encontradas na [documentação do protocolo MS-ADTS](https://msdn.microsoft.com/library/mt226583.aspx).

TIMESERV é outro relacionados domínio serviços de sinalizador que indica se uma máquina é autoritativa no momento, que pode alterar se um controlador de domínio perde a conexão.  Um controlador de domínio nesse estado retornará "Stratum desconhecido" quando consultado por meio de NTP.  Depois de tentar várias vezes, o controlador de domínio registrará em log eventos de serviço de tempo System evento 36.

Se você quiser configurar um controlador de domínio como um GTIMESERV, isso pode ser configurado manualmente usando o comando a seguir.  Nesse caso, o controlador de domínio está usando outro computador (es) como o relógio do mestre.  Isso pode ser um dispositivo ou computador dedicado.

    w32tm /config /manualpeerlist:”master_clock1,0x8 master_clock2,0x8” /syncfromflags:manual /reliable:yes /update

> [!NOTE]
> Para obter mais informações, consulte [configurar o serviço de tempo do Windows](https://technet.microsoft.com/library/cc731191.aspx)

Se o controlador de domínio tem o hardware GPS instalado, você precisará usar estas etapas para desabilitar o cliente NTP e habilitar o servidor NTP.

Comece desabilitando o cliente NTP e habilitar o servidor NTP usando essas alterações de chave do registro.

    reg add HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Services\w32time\TimeProviders\NtpClient /v Enabled /t REG_DWORD /d 0 /f

    reg add HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Services\w32time\TimeProviders\NtpServer /v Enabled /t REG_DWORD /d 1 /f

Em seguida, reinicie o serviço de tempo do Windows

    net stop w32time && net start w32time

Por fim, você indica que esta máquina tem uma fonte de horário confiável usando.
   
    w32tm /config /reliable:yes /update

Para verificar as alterações foram feitas corretamente, você pode executar os seguintes comandos que afetam os resultados mostrados abaixo. 

    w32tm /query /configuration

Valor|Configuração esperada|
----- | ----- |
AnnounceFlags|  5 (local)|
NtpServer   |(Local)|
DllName |C:\WINDOWS\SYSTEM32\w32time.DLL (Local)|
Enabled |1 (local)|
NtpClient|  (Local)|

    w32tm /query /status /verbose

Valor|  Configuração esperada|
----- | ----- |
Stratum|    1 (referência principal - sincronizado por um relógio de rádio)|
ReferenceId|    0x4C4F434C (nome de origem:  "LOCAL")|
Source| Relógio CMOS local|
Deslocamento de fase|   0.0000000s|
Função de servidor|    (Serviço de tempo confiável) de 576|

#### <a name="windows-server-2016-on-3rd-party-virtual-platforms"></a>Windows Server 2016 em plataformas de Virtual 3ª partes
Por padrão, quando o Windows é virtualizado, o hipervisor é responsável por fornecer o tempo.  Mas os membros precisam ser sincronizados com o controlador de domínio na ordem para o Active Directory funcione corretamente ingressado no domínio.  É melhor desabilitar a virtualização qualquer hora entre o convidado e o host de qualquer 3ª plataformas virtual de terceiros.

#### <a name="discovering-the-hierarchy"></a>Descoberta de hierarquia
Como a cadeia de hierarquia de tempo para a origem de relógio mestre é negociado e dinâmico em um domínio, você precisará consultar o status de uma determinada máquina para entender é a fonte de tempo e a cadeia para o relógio de origem mestre.  Isso pode ajudar a diagnosticar problemas de sincronização de tempo.

Você deseja solucionar problemas de um cliente específico; a primeira etapa é entender sua fonte de tempo usando este comando w32tm.

    w32tm /query /status

Os resultados exibem o código-fonte, entre outras coisas.  A origem indica com quem você sincronizar a hora no domínio.  Essa é a primeira etapa dessa hierarquia de tempo de máquinas.
Em seguida, use a entrada de origem acima e use o parâmetro de /StripChart para localizar a fonte de tempo Avançar na cadeia.

    w32tm /stripchart /computer:MySourceEntry /packetinfo /samples:1

Também é útil, o comando a seguir lista cada controlador de domínio, que ele pode encontrar no domínio especificado e imprime um resultado que permite determinar cada parceiro.  Esse comando incluirá as máquinas que foram configuradas manualmente.
    
    w32tm /monitor /domain:my_domain

Usando a lista, você pode rastrear os resultados por meio do domínio e entender a hierarquia, bem como o deslocamento de hora em cada etapa.  Ao localizar o ponto em que o deslocamento de horário obtém consideravelmente pior, você pode identificar a raiz do tempo incorreto.  A partir daí, você pode tentar entender por que esse horário está incorreto Ativando [w32tm log](#W32Logging). 

#### <a name="using-group-policy"></a>Usando a diretiva de grupo
Você pode usar diretiva de grupo para realizar a precisão mais rígida, por exemplo, atribuindo clientes para servidores NTP específicos de uso ou para o controle como de baixo nível do sistema operacional é configurado quando virtualizado.  
Abaixo está uma lista dos possíveis cenários e configurações de diretiva de grupo relevantes:

**Virtualizados domínios** – a fim de controlar os controladores de domínio virtualizados no 2012R2 do Windows para que eles sincronizarem a hora com seu domínio, em vez de com o host do Hyper-V, você pode desabilitar essa entrada de registro.   Para que o PDC, você não deseja desabilitar a entrada, como o host Hyper-V entregará a fonte de tempo mais estável.  A entrada do registro exige que você reinicie o serviço w32time depois que ele é alterado.

    [HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Services\W32Time\TimeProviders\VMICTimeProvider]
    "Enabled"=dword:00000000

**Precisão de cargas confidenciais** - tempo precisão confidenciais cargas de trabalho, você pode configurar grupos de computadores para definir servidores NTP e qualquer tempo as configurações relacionadas, como frequência de atualização de sondagem e o relógio.  Isso normalmente é tratado pelo domínio, mas para obter mais controle, você poderá visá máquinas específicas para apontar diretamente para o relógio do mestre.

Configuração da Diretiva de Grupo|   Novo valor|
----- | ----- |
NtpServer|  ClockMasterName, 0x8|
MinPollInterval|    6 – 64 segundos|
MaxPollInterval|    6|
UpdateInterval| 100 – uma vez por segundo|
EventLogFlags|  3 – todos os logs do tempo especial|

> [!NOTE]
> As configurações de NtpServer e EventLogFlags estão localizadas em System\Windows tempo Service\Time provedores usando as configurações de configurar o cliente do Windows NTP.  As outras 3 estão localizados sob o serviço de tempo System\Windows usando as definições de configuração Global.

**Remoto precisão confidenciais cargas remotas** – para sistemas em domínios de ramificação para a instância de varejo e o setor de crédito de pagamento (PCI), Windows usa as informações do site atual e fonte de tempo de localizador do controlador de domínio para localizar um controlador de domínio local, a menos que haja um NTP manual configurado.  Este ambiente requer um segundo de precisão, que usa uma convergência mais rápida com a hora correta.  Essa opção permite que o serviço w32time retroceder o relógio.  Se isso é aceitável e atenda às suas necessidades, você pode criar a política a seguir.   Assim como acontece com qualquer ambiente, garante que a linha de base e de teste de sua rede. 

Configuração da Diretiva de Grupo|   Novo valor|
----- | ----- |
MaxAllowedPhaseOffset|  1, mais do que na segunda, defina o relógio para horário correto.|

A configuração MaxAllowedPhaseOffset está localizada no serviço de tempo System\Windows usando as definições de configuração Global.

> [!NOTE]
> Para obter mais informações sobre a diretiva de grupo e as entradas relacionadas, consulte [ferramentas de serviço de tempo do Windows](windows-time-service-tools-and-settings.md) e configurações de artigo no TechNet.

## <a name="azure-and-windows-iaas-considerations"></a>Considerações de IaaS do Windows Azure

### <a name="azure-virtual-machine-active-directory-domain-services"></a>Máquina Virtual do Azure: Active Directory Domain Services
Se a VM do Azure executando o Active Directory Domain Services fizer parte de um local existente a floresta do Active Directory, em seguida, TimeSync(VMIC), deve ser desabilitada. Isso é para permitir que todos os controladores de domínio na floresta, física e virtual, use uma hierarquia de sincronização única vez. Consulte o white paper de prática recomendado ["executando o domínio controladores no Hyper-V"](https://technet.microsoft.com/library/virtual_active_directory_domain_controller_virtualization_hyperv.aspx)

### <a name="azure-virtual-machine-domain-joined-machine"></a>Máquina Virtual do Azure: computador ingressado no domínio
Se você estiver hospedando um computador que é associado a uma floresta existente do Active Directory do domínio, virtual ou física, a prática recomendada é desabilitar TimeSync para o convidado e verifique se que W32Time está configurado para sincronizar com seu controlador de domínio por meio de configuração de tempo para Tipo = NTP5

### <a name="azure-virtual-machine-standalone-workgroup-machine"></a>Máquina Virtual do Azure: Computador de grupo de trabalho autônomo
Se a VM do Azure não está associada a um domínio e não é um controlador de domínio, a recomendação é manter a configuração de tempo padrão e fazer com que a máquina virtual sincronizar com o host.

## <a name="windows-application-requiring-accurate-time"></a>Hora precisa de necessidade do aplicativo de Windows
### <a name="time-stamp-api"></a>Carimbo de data / hora API
Programas que exigem a maior precisão em relação ao UTC e não com o passar do tempo, devem usar o [GetSystemTimePreciseAsFileTime API](https://msdn.microsoft.com/library/windows/desktop/Hh706895.aspx).  Isso garante que seu aplicativo obtém a hora do sistema, que é condicionada pelo serviço de tempo do Windows.

### <a name="udp-performance"></a>Desempenho de UDP
Se você tiver um aplicativo que usa comunicação de UDP para transações e é importante para minimizar a latência, há algumas relacionadas a entradas do registro que você pode usar para configurar um intervalo de portas a serem excluídos da base de mecanismo de filtragem de porta.  Isso melhorará a latência e aumentar a taxa de transferência.  No entanto, as alterações no registro devem ser limitadas a administradores experientes.  Além disso, essa solução alternativa exclui as portas que estão sendo protegidos pelo firewall.  Consulte a referência do artigo abaixo para obter mais informações.

Para Windows Server 2012 e Windows Server 2008, você precisará instalar um Hotfix pela primeira vez.  Você pode fazer referência a este artigo da KB: [Perda de datagrama quando você executa um aplicativo de receptor de difusão seletiva no Windows 8 e no Windows Server 2012](https://support.microsoft.com/kb/2808584)

### <a name="update-network-drivers"></a>Atualizar os Drivers de rede
Alguns fornecedores de rede têm atualizações de driver que melhorar o desempenho em relação a latência de driver e pacotes UDP de buffer.  Entre em contato com seu fornecedor de rede para ver se há atualizações para ajudar com a taxa de transferência UDP.

## <a name="logging-for-auditing-purposes"></a>Registro em log para fins de auditoria
Para cumprir as normas de rastreamento de tempo, você pode arquivar manualmente w32tm logs, logs de eventos e informações de monitor de desempenho.  Posteriormente, as informações de arquivados podem ser usadas para atestar a conformidade em um momento específico no passado.  Os fatores a seguir são usados para indicar a precisão.


1. Precisão usando o contador de monitor de desempenho calculado de deslocamento de hora do relógio.  Isso mostra o relógio com na precisão desejada.
2.  Origem de relógio procurando "Resposta de mesmo nível do" nos logs de w32tm.   O texto da mensagem a seguir é o endereço IP ou VMIC, que descreve a fonte de tempo e o próximo na cadeia de relógios de referência para validar.
3.  Status condição usando os logs de w32tm para validar que do relógio "disciplina ClockDispl: \*DISTORCER\*tempo\*"estão ocorrendo.  Isso indica que w32tm está ativo no momento.

### <a name="event-logging"></a>Log de eventos
Para obter a história completa, você precisará também informações de log de eventos.  Coletando o log de eventos do sistema, e a filtragem no servidor de horário, Microsoft-Windows-Kernel-inicialização, Microsoft-Windows-Kernel-geral, você poderá descobrir se há outras que foram alterados a hora, por exemplo, os terceiros influências.  Esses logs podem ser necessários para eliminar a interferência externa.  Diretiva de grupo pode afetar quais logs de eventos são gravados no log.  Consulte a seção acima sobre usando a diretiva de grupo para obter mais detalhes.

### <a name="W32Logging"></a>W32Time do log de depuração
Para habilitar w32tm para fins de auditoria, o comando a seguir habilita o log que mostra as atualizações periódicas do relógio e indica o relógio do código-fonte.  Reinicie o serviço para habilitar o log de novo.  

Para obter mais informações, consulte [como ativar o log de depuração no serviço de tempo do Windows](https://support.microsoft.com/kb/816043).

    w32tm /debug /enable /file:C:\Windows\Temp\w32time-test.log /size:10000000 /entries:0-73,103,107,110

### <a name="performance-monitor"></a>Monitor de Desempenho
O serviço de tempo do Windows Server 2016 Windows expõe contadores de desempenho que podem ser usados para coletar o registro em log de auditoria.  Eles podem ser registrados localmente ou remotamente.  Você pode registrar os contadores de atraso de ida e volta e deslocamento de hora do computador.  
E, como qualquer contador de desempenho, você pode monitorá-los remotamente e criar alertas usando o System Center Operations Manager.  Você pode, por exemplo, usar um alerta para quando o deslocamento de tempo tiver um descompasso do que a precisão desejada do alarme você.  O [o pacote de gerenciamento do System Center](https://social.technet.microsoft.com/wiki/contents/articles/15251.system-center-management-pack-authoring-guide.aspx) tem mais informações.

### <a name="windows-traceability-example"></a>Exemplo de rastreamento do Windows
W32tm dos arquivos de log você deseja validar dois tipos de informação.  A primeira é uma indicação de que o arquivo de log no momento é o relógio de condição.  Isso prova que o seu relógio foi condicionado pelo serviço de tempo do Windows ao território disputado tempo.

    151802 20:18:32.9821765s - ClockDispln Discipline: *SKEW*TIME* - PhCRR:223 CR:156250 UI:100 phcT:65 KPhO:14307
    151802 20:18:33.9898460s - ClockDispln Discipline: *SKEW*TIME* - PhCRR:1 CR:156250 UI:100 phcT:64 KPhO:41
    151802 20:18:44.1090410s - ClockDispln Discipline: *SKEW*TIME* - PhCRR:1 CR:156250 UI:100 phcT:65 KPhO:38

O ponto principal é que você vê mensagens prefixadas com disciplina ClockDispln que é a prova w32time está interagindo com o relógio do sistema.
 
Em seguida, você precisa localizar o último relatório no log antes da hora de território disputada que relata o computador de origem que está sendo usado como o relógio de referência.  Isso pode ser um endereço IP, nome do computador ou o provedor VMIC, que indica que ele está em sincronização com o Host do Hyper-V.  O exemplo a seguir fornece um endereço IPv4 da 10.197.216.105.

    151802 20:18:54.6531515s - Response from peer 10.197.216.105,0x8 (ntp.m|0x8|0.0.0.0:123->10.197.216.105:123), ofs: +00.0012218s

Agora que você tiver validado o primeiro sistema na cadeia de tempo de referência, você precisa investigar o arquivo de log na fonte de tempo de referência e repita as mesmas etapas.  Isso continua até chegar a um relógio físico, como GPS ou uma fonte de tempo conhecido como o NIST.  Se o relógio de referência for hardware GPS, em seguida, os logs do fabricados também podem ser necessários.

## <a name="network-considerations"></a>Considerações de rede
Os algoritmos de protocolo NTP têm uma dependência de simetria da sua rede.  Como o aumento o número de saltos de rede, aumenta a probabilidade de assimetria.  Lá, é difícil prever quais tipos de precisões você verá no seu ambiente específico. 

Monitor de desempenho e os novos contadores de tempo do Windows no Windows Server 2016 podem ser usados para avaliar a precisão de seus ambientes e criar linhas de base. Além disso, você pode executar a solução de problemas para determinar o deslocamento atual de qualquer computador na sua rede.

Há dois padrões gerais para o tempo preciso pela rede.  PTP ([precisão Time Protocol - 1588 IEEE](https://www.nist.gov/el/intelligent-systems-division-73500/introduction-ieee-1588)) tem requisitos mais rigorosas na infraestrutura de rede, mas geralmente pode fornecer a precisão de subpropriedades microssegundos.  NTP ([Network Time Protocol – RFC 1305](https://tools.ietf.org/html/rfc1305)) funciona em uma variedade maior de redes e ambientes, o que torna mais fácil de gerenciar. 

Windows dá suporte a simples NTP (RFC2030) por padrão para computadores de fora do domínio unidas.  Para computadores ingressados no domínio, podemos usar um chamada de NTP seguro [MS-SNTP](https://msdn.microsoft.com/library/cc246877.aspx), que aproveita os segredos de domínio negociado que fornecem uma vantagem de gerenciamento sobre NTP autenticado descrito em RFC1305 e RFC5905.   

O domínio e os protocolos de fora do domínio Unidos requer a porta UDP 123.  Para obter mais informações sobre as práticas recomendadas NTP, consulte [rede tempo protocolo melhores práticas IETF rascunho atual](https://tools.ietf.org/html/draft-ietf-ntp-bcp-00).

### <a name="reliable-hardware-clock-rtc"></a>Relógio de Hardware confiável (RTC)
Windows faz o tempo de etapa, a menos que determinados limites forem excedidos, mas disciplinas em vez disso, o relógio.  Isso significa que w32tm ajusta a frequência do relógio em intervalos regulares, usando a frequência de atualização de relógio de configuração, que assume como padrão uma vez por segundo com o Windows Server 2016.  Se o relógio está por trás, ele acelera a frequência e se ele estiver em frente, ele desacelera a frequência.  No entanto, durante esse tempo entre os ajustes de frequência do relógio, o relógio de hardware está no controle.  Se houver um problema com o firmware ou o relógio de hardware, o tempo no computador pode se tornar menos preciso.

Este é outro motivo, que é necessário testar e linha de base em seu ambiente.  Se o contador de desempenho "Computado deslocamento de tempo" não se estabilizar na precisão de que destino, você talvez queira verificar se que o firmware está atualizado.  Como outro teste, você pode ver se o hardware duplicados reproduzir o mesmo problema.

### <a name="troubleshooting-time-accuracy-and-ntp"></a>NTP e a precisão de tempo de solução de problemas
Você pode usar o descobrindo a seção de hierarquia acima para compreender a origem de uma hora imprecisa.  Examinar a diferença de tempo, encontre o ponto na hierarquia em que o tempo diverge o máximo proveito de sua fonte de NTP.  Depois de compreender a hierarquia, você vai querer tente e entender por que essa fonte de tempo específico não recebe o horário com precisão.  

Concentrando-se no sistema com tempo divergente, você pode usar essas ferramentas abaixo para obter mais informações para ajudá-lo a determinar o problema e para encontrar uma resolução.  A referência de UpstreamClockSource abaixo, é o relógio descoberto usando "w32tm /config /status".


- Logs de eventos do sistema
- Habilitar usando o registro em log: logs de w32tm - w32tm /debug /enable /file:C:\Windows\Temp\w32time-test.log /size:10000000 /entries:0-300
- w32Time Registry key HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Services\W32Time
- Rastreamentos de rede local
- Contadores de desempenho (do computador local ou o UpstreamClockSource)
- W32tm /stripchart /computer:UpstreamClockSource
- PING UpstreamClockSource para entender a latência e o número de saltos para código-fonte
- Tracert UpstreamClockSource

Problema|    Sintomas|   Resolução|
----- | ----- | ----- |
Relógio TSC local não é estável.| Usando o relógio estável do Perfmon - computador físico – sincronização relógio, mas você ainda ver que cada 1 a 2 minutos de vários 100us. |   Atualizar o Firmware ou validar um hardware diferente não exibe o mesmo problema.|
Latência de rede|    stripchart w32tm exibe um RoundTripDelay de mais de 10 ms.  Variação no atraso causar ruído tão grande quanto ½ do tempo de ida e volta, por exemplo, um atraso que está apenas em uma única direção.</br></br>UpstreamClockSource é vários saltos, conforme indicado pelo PING.  TTL deve ser próximo 128.</br></br>Use o Tracert para localizar a latência em cada salto.    | Localize uma fonte de relógio mais próxima de tempo.  Uma solução é instalar um relógio de origem no mesmo segmento ou apontar manualmente ao relógio do código-fonte que seja geograficamente mais próximo.  Para um cenário de domínio, adicione uma máquina com a função GTimeServ. |  
Não é possível acessar confiável a origem NTP|    /Stripchart w32tm intermitentemente retorna "Atingiu o tempo limite de solicitação"    |Origem de NTP não responsiva|
Origem de NTP não responsiva|    Verifique os contadores de desempenho contagem de origem do cliente NTP, solicitações de entrada de servidor NTP, NTP Server para respostas de saída e determinar seu uso em comparação com suas linhas de base.|    Usando contadores de desempenho do servidor, determine se a carga foi alterado em referência a suas linhas de base.</br></br>Há rede congestionamento problemas?|
Controlador de domínio não usando o relógio mais preciso|    Alterações na topologia ou relógio mestre adicionados recentemente.|   w32tm /resync /rediscover|
Relógios do cliente estão se desvia| Serviço de tempo do evento 36 no log de eventos do sistema e/ou o texto no arquivo de log que descreve que: Contador de "contagem de fonte de tempo de cliente NTP" vai de 1 para 0|Solucionar problemas de fonte de upstream e entender se ele está em execução em problemas de desempenho.|

### <a name="baselining-time"></a>Tempo de linha de base
Linha de base é importante para que o primeiro, você pode entender o desempenho e a precisão da sua rede e compare com a linha de base no futuro, quando ocorrem problemas.  Você vai querer da linha de base o PDC raiz ou todas as máquinas marcado com o GTIMESRV.  Também recomendamos você linha de base de controlador de domínio primário em cada floresta.  Por fim, escolha qualquer críticos controladores de domínio ou computadores que têm características interessantes, como distância ou linha de base e altas cargas esses.

Ele também é útil para a linha de base Windows Server 2016 vs 2012 R2, no entanto, você só precisa w32tm /stripchart como uma ferramenta que você pode usar para comparar, como o Windows Server 2012 R2 não tem os contadores de desempenho.  Você deve escolher duas máquinas com as mesmas características, ou atualizar uma máquina e comparar os resultados após a atualização.  O adendo de medições de tempo do Windows tem mais informações sobre como fazer medidas detalhadas entre 2016 e 2012.

Usando os todos os w32time contadores de desempenho, colete dados para pelo menos uma semana.  Isso garantirá que você tem suficiente de uma referência para levar em conta para diversos na rede ao longo do tempo e suficiente de uma execução para fornecer a confiança de que sua precisão de tempo é estável.

### <a name="ntp-server-redundancy"></a>Redundância do servidor NTP
Para configuração manual do servidor NTP usada com máquinas unidas de fora do domínio ou controlador de domínio primário, ter mais de um servidor é uma medida de boa redundância no caso de disponibilidade.  Ele também pode oferecer maior precisão, supondo que todas as fontes sejam precisas e estável.  No entanto, se a topologia não está bem projetada, ou as fontes de tempo não estão estáveis, a precisão resultante pode ser pior portanto, tenha cuidado.  O limite de tempo com suporte manualmente pode fazer referência a servidores w32time é 10. 

## <a name="leap-seconds"></a>Segundos Intercalares
Período de rotação da Terra varia ao longo do tempo, causado por eventos climatic e geological. Normalmente, a variação é cerca de um segundo cada dois anos. Sempre que a variação de tempo atômico ficar muito grande, uma correção de um segundo (para cima ou para baixo) é inserido, chamado de uma fração de segundo. Isso é feito de forma que a diferença nunca exceda 0.9 segundos. Essa correção é anunciada seis meses à frente a correção real. Antes do Windows Server 2016, o serviço de tempo da Microsoft não estava ciente segundos intercalares, mas contavam com o serviço de tempo externa para cuidar disso. Com a precisão de cada vez maior do Windows Server 2016, a Microsoft está trabalhando em uma solução mais adequada para o problema de fração de segundo.

## <a name="secure-time-seeding"></a>Proteger a propagação de tempo
W32Time no Server 2016 inclui o recurso de proteger a propagação do tempo. Este recurso determina o tempo aproximado atual de conexões de SSL de saída.  Esse valor de tempo é usado para monitorar o relógio do sistema local e corrija os erros brutos. Você pode ler mais sobre o recurso no [esta postagem de blog](https://blogs.msdn.microsoft.com/w32time/2016/09/28/secure-time-seeding-improving-time-keeping-in-windows/).  Em implantações com uma fonte de horário confiável (s) e de computadores bem monitorados que incluem o monitoramento para os deslocamentos de tempo, você pode optar por não usar o recurso de proteger a propagação de tempo e se baseiam em sua infraestrutura existente em vez disso. 

Você pode desabilitar o recurso com estas etapas:

1.  Defina o valor da configuração do registro UtilizeSSLTimeData como 0 em uma máquina específica:

    reg add HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Services\w32time\Config /v UtilizeSslTimeData /t REG_DWORD /d 0 /f


2.  Se não for possível reinicializar o computador imediatamente, por alguma razão, você pode notificar o serviço W32time sobre a atualização de configuração. Isso interrompe o monitoramento em tempo e imposição com base nos dados de tempo coletados de conexões SSL. 

    W32tm.exe /config /update

3.  Reiniciar a máquina torna a configuração em vigor imediatamente e também faz com que ele parar de coletar os dados de tempo de conexões SSL.  A última parte tem uma sobrecarga muito pequena e não deve ser uma preocupação de desempenho.

4.  Para aplicar essa configuração em todo o domínio, defina o valor de UtilizeSSLTimeData na configuração de diretiva de grupo W32time como 0 e a configuração de publicação.  Quando a configuração é captada por um cliente de diretiva de grupo, o serviço W32time será notificado e ele deixará de usar dados de tempo SSL de imposição e monitoramento em tempo. A coleta de dados de tempo do SSL será interrompido quando cada computador é reinicializado. Se o domínio tiver laptops/tablets slim portáteis e outros dispositivos, você talvez queira excluir tais máquinas dessa alteração na política. Esses dispositivos, eventualmente, enfrentará de bateria e precisa a propagação de hora segura de recursos para inicializar seu tempo.


