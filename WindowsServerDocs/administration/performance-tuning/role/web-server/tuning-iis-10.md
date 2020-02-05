---
title: Ajustando o IIS 10,0
description: Recommmendations de ajuste de desempenho para servidores Web do IIS 10,0 no Windows Server 16
ms.prod: windows-server
ms.technology: performance-tuning-guide
ms.topic: landing-page
ms.author: DavSo; Ericam; YaShi
author: phstee
ms.date: 10/16/2017
ms.openlocfilehash: 2f4d309de073e84aa0a1c568c7cfc5f31ee88d83
ms.sourcegitcommit: 3f9bcd188dda12dc5803defb47b2c3a907504255
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 02/04/2020
ms.locfileid: "77001871"
---
# <a name="tuning-iis-100"></a>Ajustando o IIS 10,0

O Serviços de Informações da Internet (IIS) 10,0 está incluído no Windows Server 2016. Ele usa um modelo de processo semelhante ao do IIS 8,5 e IIS 7,0. Um driver da Web de modo kernel (http. sys) recebe e roteia solicitações HTTP e satisfaz solicitações de seu cache de resposta. Os processos de trabalho se registram para subespaços de URL e o http. sys roteia a solicitação para o processo apropriado (ou o conjunto de processos para pools de aplicativos).

O HTTP. sys é responsável pelo gerenciamento de conexão e pela manipulação de solicitações. A solicitação pode ser servida do cache HTTP. sys ou transmitida para um processo de trabalho para manipulação adicional. Vários processos de trabalho podem ser configurados, o que fornece isolamento a um custo reduzido. Para obter mais informações sobre como funciona a manipulação de solicitações, consulte a seguinte figura:

![tratamento de solicitação no IIS 10,0](../../media/perftune-guide-iis-request-handling.png)

O HTTP. sys inclui um cache de resposta. Quando uma solicitação corresponde a uma entrada no cache de resposta, o HTTP. sys envia a resposta de cache diretamente do modo kernel. Algumas plataformas de aplicativos Web, como ASP.NET, fornecem mecanismos para permitir que qualquer conteúdo dinâmico seja armazenado em cache no cache do modo kernel. O manipulador de arquivo estático no IIS 10,0 armazena em cache automaticamente os arquivos solicitados com frequência no http. sys.

Como um servidor Web tem componentes de modo kernel e de modo de usuário, ambos os componentes devem ser ajustados para um desempenho ideal. Portanto, o ajuste do IIS 10,0 para uma carga de trabalho específica inclui a configuração a seguir:

-   HTTP. sys e o cache do modo kernel associado

-   Processos de trabalho e o modo de usuário do IIS, incluindo a configuração do pool de aplicativos

-   Determinados parâmetros de ajuste que afetam o desempenho

As seções a seguir discutem como configurar os aspectos do modo kernel e do modo de usuário do IIS 10,0.

## <a name="kernel-mode-settings"></a>Configurações do modo kernel

As configurações de HTTP. sys relacionadas ao desempenho se enquadram em duas categorias amplas: gerenciamento de cache e conexão e gerenciamento de solicitações. Todas as configurações do registro são armazenadas sob a seguinte entrada do registro:

``` syntax
HKEY_LOCAL_MACHINE\System\CurrentControlSet\Services\Http\Parameters
```

**Observação**  se o serviço http já estiver em execução, você deverá reiniciá-lo para que as alterações entrem em vigor.

Â 

## <a name="cache-management-settings"></a>Configurações de gerenciamento de cache

Um benefício que o HTTP. sys fornece é um cache de modo kernel. Se a resposta estiver no cache do modo kernel, você poderá atender totalmente uma solicitação HTTP do modo kernel, o que reduz significativamente o custo da CPU de tratamento da solicitação. No entanto, o cache do modo kernel do IIS 10,0 é baseado na memória física e o custo de uma entrada é a memória que ela ocupa.

Uma entrada no cache é útil somente quando é usada. No entanto, a entrada sempre consome memória física, se a entrada está sendo usada ou não. Você deve avaliar a utilidade de um item no cache (a economia de ser capaz de atendê-lo a partir do cache) e seu custo (a memória física ocupada) durante o tempo de vida da entrada considerando os recursos disponíveis (CPU e memória física) e a carga de trabalho requirement. O HTTP. sys tenta manter apenas os itens úteis, acessados ativamente no cache, mas você pode aumentar o desempenho do servidor Web ajustando o cache HTTP. sys para cargas de trabalho específicas.

A seguir estão algumas configurações úteis para o cache do modo kernel HTTP. sys:

-   **UriEnableCache** Valor padrão: 1

    Um valor diferente de zero permite a resposta do modo kernel e o cache de fragmento. Para a maioria das cargas de trabalho, o cache deve permanecer habilitado. Considere desabilitar o cache se você espera uma resposta muito baixa e o cache de fragmento.

-   **UriMaxCacheMegabyteCount** Valor padrão: 0

    Um valor diferente de zero que especifica a memória máxima disponível para o cache do modo kernel. O valor padrão, 0, permite que o sistema ajuste automaticamente a quantidade de memória disponível para o cache.

    **Observação** A especificação do tamanho define apenas o máximo, e o sistema pode não permitir que o cache cresça para o tamanho máximo definido.

    Â 

-   **UriMaxUriBytes** Valor padrão: 262144 bytes (256 KB)

    O tamanho máximo de uma entrada no cache do modo kernel. Respostas ou fragmentos maiores que este não são armazenados em cache. Se você tiver memória suficiente, considere aumentar o limite. Se a memória for limitada e as entradas grandes estiverem se tornando menores, talvez seja útil diminuir o limite.

-   **UriScavengerPeriod** Valor padrão: 120 segundos

    O cache HTTP. sys é periodicamente examinado por uma recuperação e as entradas que não são acessadas entre as verificações de recuperação são removidas. Definir o período de recuperação para um valor alto reduz o número de verificações de limpeza. No entanto, o uso de memória de cache pode aumentar porque as entradas acessadas com menos frequência podem permanecer no cache. Definir o período muito baixo causa verificações de recuperação mais frequentes e pode resultar em muitas liberações e rotatividade de cache.

## <a name="request-and-connection-management-settings"></a>Configurações de gerenciamento de solicitação e conexão

No Windows Server 2016, o HTTP. sys gerencia as conexões automaticamente. As seguintes configurações do registro não são mais usadas:

-   **MaxConnections**

    ``` syntax
    HKEY_LOCAL_MACHINE\System\CurrentControlSet\Services\Http\Parameters\MaxConnections
    ```

-   **IdleConnectionsHighMark**

    ``` syntax
    HKEY_LOCAL_MACHINE\System\CurrentControlSet\Services\Http\Parameters\IdleConnectionsHighMark
    ```

-   **IdleConnectionsLowMark**

    ``` syntax
    HKEY_LOCAL_MACHINE\System\CurrentControlSet\Services\Http\Parameters\IdleConnectionsLowMark
    ```

-   **IdleListTrimmerPeriod**

    ``` syntax
    HKEY_LOCAL_MACHINE\System\CurrentControlSet\Services\Http\Parameters\IdleListTrimmerPeriod
    ```

-   **RequestBufferLookasideDepth**

    ``` syntax
    HKEY_LOCAL_MACHINE\System\CurrentControlSet\Services\Http\Parameters\RequestBufferLookasideDepth
    ```

-   **InternalRequestLookasideDepth**

    ``` syntax
    HKEY_LOCAL_MACHINE\System\CurrentControlSet\Services\Http\Parameters\InternalRequestLookasideDepth
    ```


## <a name="user-mode-settings"></a>Configurações de modo de usuário

As configurações nesta seção afetam o comportamento do processo de trabalho do IISÂ 10,0. A maioria dessas configurações pode ser encontrada no seguinte arquivo de configuração XML:

% SystemRoot%\\system32\\inetsrv\\config\\applicationHost. config

Use Appcmd. exe, o console de gerenciamento do IIS 10,0, os cmdlets do PowerShell WebAdministration ou IISAdministration para alterá-los. A maioria das configurações é detectada automaticamente e não requer uma reinicialização dos processos de trabalho do IIS 10,0 ou do servidor de aplicativos Web. Para obter mais informações sobre o arquivo applicationHost. config, consulte [Introduction to ApplicationHost. config](https://www.iis.net/learn/get-started/planning-your-iis-architecture/introduction-to-applicationhostconfig).


## <a name="ideal-cpu-setting-for-numa-hardware"></a>Configuração ideal de CPU para hardware NUMA

A partir do Windows 2016, o IIS 10,0 dá suporte à atribuição de CPU ideal automática para seus threads de pool de threads para melhorar o desempenho e a escalabilidade em hardware NUMA. Esse recurso é habilitado por padrão e pode ser configurado por meio da seguinte chave do registro:

``` syntax
HKEY_LOCAL_MACHINE\System\CurrentControlSet\Services\InetInfo\Parameters\ThreadPoolUseIdealCpu
```

Com esse recurso habilitado, o Gerenciador de threads do IIS faz seu melhor esforço para distribuir uniformemente os threads do pool de threads do IIS em todas as CPUs em todos os nós NUMA com base em suas cargas atuais. Em geral, é recomendável manter essa configuração padrão inalterada para o hardware NUMA.

**Observação**  a configuração de CPU ideal é diferente das configurações de atribuição de nó numa do processo de trabalho (NumaNodeAssignment e numaNodeAffinityMode) introduzidas nas [configurações de CPU para um pool de aplicativos](https://www.iis.net/configreference/system.applicationhost/applicationpools/add/cpu). A configuração de CPU ideal afeta como o IIS distribui seus threads de pool de threads, enquanto as configurações de atribuição de nó NUMA do processo de trabalho determinam em qual nó NUMA um processo de trabalho inicia.

## <a name="user-mode-cache-behavior-settings"></a>Configurações de comportamento de cache de modo de usuário

Esta seção descreve as configurações que afetam o comportamento de cache no IISÂ 10,0. O cache de modo de usuário é implementado como um módulo que escuta os eventos de cache global que são gerados pelo pipeline integrado. Para desabilitar completamente o cache do modo de usuário, remova o módulo FileCacheModule (cachfile. dll) da lista de módulos instalados na seção de configuração System. WebServer/globalModules em applicationHost. config.

**System. DataServer/Caching**

|Atributo|Descrição|Padrão|
|--- |--- |--- |
|Habilitada|Desabilita o cache do IIS no modo de usuário quando definido como **false**. Quando a taxa de acesso ao cache é muito pequena, você pode desabilitar o cache completamente para evitar a sobrecarga associada ao caminho do código de cache. Desabilitar o cache de modo de usuário não desabilita o cache do modo kernel.|verdadeiro|
|enableKernelCache|Desabilita o cache do modo kernel quando definido como **false**.|verdadeiro|
|maxCacheSize|Limita o tamanho do cache do modo de usuário do IIS ao tamanho especificado em megabytes. O IIS ajusta o padrão dependendo da memória disponível. Escolha o valor com cuidado com base no tamanho do conjunto de arquivos acessados com frequência em comparação com a quantidade de RAM ou o espaço de endereço de processo do IIS.|0|
|maxResponseSize|Armazena em cache os arquivos até o tamanho especificado. O valor real depende do número e do tamanho dos maiores arquivos no conjunto de dados em comparação à RAM disponível. O cache de arquivos grandes e solicitados com frequência pode reduzir o uso da CPU, o acesso ao disco e as latências associadas.|262144|

## <a name="compression-behavior-settings"></a>Configurações de comportamento de compactação

O IIS a partir de 7,0 compacta o conteúdo estático por padrão. Além disso, a compactação de conteúdo dinâmico é habilitada por padrão quando o DynamicCompressionModule é instalado. Compactação reduz o uso da largura de banda, mas aumenta o uso da CPU O conteúdo compactado é armazenado em cache no cache do modo kernel, se possível. A partir do 8,5, o IIS permite que a compactação seja controlada independentemente para conteúdo estático e dinâmico. O conteúdo estático normalmente se refere ao conteúdo que não é alterado, como arquivos GIF ou HTM. O conteúdo dinâmico normalmente é gerado por scripts ou código no servidor, ou seja, páginas ASP.NET. Você pode personalizar a classificação de qualquer extensão específica como estática ou dinâmica.

Para desabilitar completamente a compactação, remova StaticCompressionModule e DynamicCompressionModule da lista de módulos na seção System. WebServer/globalModules em applicationHost. config.

**System. WebServer/httpCompression**

|Atributo|Descrição|Padrão|
|--- |--- |--- |
|staticCompression-EnableCpuUsage<br><br>staticCompression-DisableCpuUsage<br><br>dynamicCompression-EnableCpuUsage<br><br>dynamicCompression-DisableCpuUsage|Habilita ou desabilita a compactação se o uso percentual atual da CPU estiver acima ou abaixo dos limites especificados.<br><br>A partir do IIS 7,0, a compactação será desabilitada automaticamente se a CPU de estado estacionário aumentar acima do limite de desabilitação. A compactação será habilitada se a CPU cair abaixo do limite de habilitação.|50, 100, 50 e 90, respectivamente|
|Active|Especifica o diretório no qual as versões compactadas de arquivos estáticos são armazenadas temporariamente e armazenadas em cache. Considere mover esse diretório da unidade do sistema se ele for acessado com frequência.|Arquivos compactados temporários do%SystemDrive%\inetpub\temp\IIS|
|doDiskSpaceLimiting|Especifica se existe um limite para a quantidade de espaço em disco que todos os arquivos compactados podem ocupar. Os arquivos compactados são armazenados no diretório de compactação especificado pelo atributo **Directory** .|verdadeiro|
|maxDiskSpaceUsage|Especifica o número de bytes de espaço em disco que os arquivos compactados podem ocupar no diretório de compactação.<br><br>Essa configuração pode precisar ser aumentada se o tamanho total de todo o conteúdo compactado for muito grande.|100 MB|

**System. WebServer/urlCompression**

|Atributo|Descrição|Padrão|
|--- |--- |--- |
|doStaticCompression|Especifica se o conteúdo estático é compactado.|verdadeiro|
|doDynamicCompression|Especifica se o conteúdo dinâmico é compactado.|verdadeiro|

**Observação** Para servidores que executam o IIS 10,0 com baixo uso médio de CPU, considere habilitar a compactação para conteúdo dinâmico, especialmente se as respostas forem grandes. Isso deve ser feito primeiro em um ambiente de teste para avaliar o efeito sobre o uso da CPU da linha de base.


### <a name="tuning-the-default-document-list"></a>Ajustando a lista de documentos padrão

O módulo documento padrão manipula solicitações HTTP para a raiz de um diretório e os converte em solicitações para um arquivo específico, como default. htm ou index. htm. Em média, aroundÂ 25% de todas as solicitações na Internet passam pelo caminho de documento padrão. Isso varia significativamente para sites individuais. Quando uma solicitação HTTP não especifica um nome de arquivo, o módulo de documento padrão pesquisa a lista de documentos padrão permitidos para cada nome no sistema de arquivos. Isso pode afetar negativamente o desempenho, especialmente se o alcance do conteúdo exigir uma viagem de ida e volta da rede ou tocar em um disco.

Você pode evitar a sobrecarga, desabilitando seletivamente os documentos padrão e reduzindo ou ordenando a lista de documentos. Para sites que usam um documento padrão, você deve reduzir a lista somente para os tipos de documento padrão que são usados. Além disso, ordene a lista para que ela comece com o nome de arquivo de documento padrão acessado com mais frequência.

Você pode definir seletivamente o comportamento de documento padrão em URLs específicas Personalizando a configuração dentro de uma marca de local em applicationHost. config ou inserindo um arquivo Web. config diretamente no diretório de conteúdo. Isso permite uma abordagem híbrida, que permite documentos padrão somente onde eles são necessários e define a lista como o nome de arquivo correto para cada URL.

Para desabilitar completamente os documentos padrão, remova DefaultDocumentModule da lista de módulos na seção System. WebServer/globalModules em applicationHost. config.

**System. Fileserver/defaultDocument**

|Atributo|Descrição|Padrão|
|--- |--- |--- |
|habilitado|Especifica que os documentos padrão estão habilitados.|verdadeiro|
|elemento&gt; de &lt;de arquivos|Especifica os nomes de arquivo que são configurados como documentos padrão.|A lista padrão é default. htm, Default. asp, index. htm, index. html, iisstart. htm e default. aspx.|

## <a name="central-binary-logging"></a>Log binário central

Quando a sessão do servidor tem inúmeros grupos de URLs, o processo de criação de centenas de arquivos de log formatados para grupos de URL individuais e a gravação dos dados de log em um disco pode consumir rapidamente recursos valiosos de CPU e memória, criando assim o desempenho e problemas de escalabilidade. O log binário centralizado minimiza a quantidade de recursos do sistema usados para registro em log, enquanto, ao mesmo tempo, fornece dados de log detalhados para organizações que o exigem. A análise de logs de formato binário requer uma ferramenta de pós-processamento.

Você pode habilitar o log binário central definindo o atributo centralLogFileMode como CentralBinary e definindo o atributo **Enabled** como **true**. Considere mover o local do arquivo de log central para fora da partição do sistema e para uma unidade de log dedicada para evitar a contenção entre atividades do sistema e atividades de registro em log.

**System. applicationHost/log**

|Atributo|Descrição|Padrão|
|--- |--- |--- |
|centralLogFileMode|Especifica o modo de log para um servidor. Altere esse valor para CentralBinary para habilitar o registro em log binário central.|Site|

**System. applicationHost/log/centralBinaryLogFile**

|Atributo|Descrição|Padrão|
|--- |--- |--- |
|habilitado|Especifica se o log binário central está habilitado.|False|
|Active|Especifica o diretório onde as entradas de log são gravadas.|%SystemDrive%\inetpub\logs\LogFiles|


## <a name="application-and-site-tunings"></a>Ajustes do aplicativo e do site

As configurações a seguir estão relacionadas ao pool de aplicativos e aos ajustes do site.

**System. applicationHost/applicationPools/applicationPoolDefaults**

|Atributo|Descrição|Padrão|
|--- |--- |--- |
|queueLength|Indica ao HTTP. sys quantas solicitações são enfileiradas para um pool de aplicativos antes de solicitações futuras serem rejeitadas. Quando o valor dessa propriedade for excedido, o IIS rejeitará as solicitações subsequentes com um erro 503.<br><br>Considere aumentar isso para aplicativos que se comunicam com armazenamentos de dados de back-end de alta latência se 503 erros forem observados.|1000|
|enable32BitAppOnWin64|Quando true, permite que um aplicativo de 32 bits seja executado em um computador que tenha um processador de 64 bits.<br><br>Considere habilitar o modo de 32 bits se o consumo de memória for uma preocupação. Como os tamanhos de ponteiro e os tamanhos de instrução são menores, os aplicativos de 32 bits usam menos memória do que os aplicativos de 64 bits. A desvantagem de executar aplicativos de 32 bits em um computador de 64 bits é que o espaço de endereço de modo de usuário é limitado a 4 GB.|False|

**System. applicationHost/sites/VirtualDirectoryDefault**

|Atributo|Descrição|Padrão|
|--- |--- |--- |
|allowSubDirConfig|Especifica se o IIS procura arquivos Web. config em diretórios de conteúdo inferiores ao nível atual (true) ou não procura arquivos Web. config em diretórios de conteúdo inferiores ao nível atual (false). Ao impor uma limitação simples, que permite a configuração somente em diretórios virtuais, o IISÂ 10,0 pode saber que, a menos que **/nome de &lt;&gt;. htm** seja um diretório virtual, ele não deve procurar um arquivo de configuração. Ignorar as operações de arquivo adicionais pode melhorar significativamente o desempenho de sites que têm um conjunto muito grande de conteúdo estático acessado aleatoriamente.|verdadeiro|

## <a name="managing-iis-100-modules"></a>Gerenciando módulos do IIS 10,0

O IIS 10,0 foi fatorado em vários módulos extensíveis pelo usuário para dar suporte a uma estrutura modular. Essa fatoração tem um pequeno custo. Para cada módulo, o pipeline integrado deve chamar o módulo para cada evento que é relevante para o módulo. Isso acontece independentemente de o módulo precisar fazer qualquer trabalho. Você pode conservar os ciclos de CPU e a memória removendo todos os módulos que não são relevantes para um site específico.

Um servidor Web ajustado para arquivos estáticos simples pode incluir apenas os cinco módulos a seguir: UriCacheModule, HttpCacheModule, StaticFileModule, AnonymousAuthenticationModule e HttpLoggingModule.

Para remover módulos de applicationHost. config, remova todas as referências ao módulo das seções System. WebServer/Handlers e System. WebServer/modules, além de remover a declaração de módulo em System. WebServer/globalModules.

## <a name="classic-asp-settings"></a>Configurações ASP clássicas

O custo principal do processamento de uma solicitação ASP clássica inclui a inicialização de um mecanismo de script, a compilação do script ASP solicitado em um modelo ASP e a execução do modelo no mecanismo de script. Embora o custo de execução do modelo dependa da complexidade do script ASP solicitado, o módulo ASP clássico do IIS pode armazenar em cache os mecanismos de script na memória e os modelos de cache em memória e disco (somente se houver estouros de cache de modelo na memória) para aumentar o desempenho no Cenários associados à CPU.

As configurações a seguir são usadas para configurar o cache de modelo ASP clássico e o cache do mecanismo de script, e eles não afetam as configurações de ASP.NET.

**System. DataServer/ASP/cache**

|Atributo|Descrição|Padrão|
|--- |--- |--- |
|diskTemplateCacheDirectory|O nome do diretório que o ASP usa para armazenar modelos compilados quando o cache na memória estoura.<br><br>Recomendação: Defina como um diretório que não é usado intensamente, por exemplo, uma unidade que não é compartilhada com o sistema operacional, log do IIS ou outro conteúdo acessado com frequência.|Modelos compilados do%SystemDrive%\inetpub\temp\ASP|
|maxDiskTemplateCacheFiles|Especifica o número máximo de modelos ASP compilados que podem ser armazenados em cache no disco.<br><br>Recomendação: Defina como o valor máximo de 0x7FFFFFFF.|2000|
|scriptFileCacheSize|Esse atributo especifica o número máximo de modelos ASP compilados que podem ser armazenados em cache na memória.<br><br>Recomendação: Defina como pelo menos o número de scripts ASP frequentemente solicitados atendidos por um pool de aplicativos. Se possível, defina como quantos modelos ASP são permitidos para limites de memória.|500|
|scriptEngineCacheMax|Especifica o número máximo de mecanismos de script que serão mantidos em cache na memória.<br><br>Recomendação: Defina como pelo menos o número de scripts ASP frequentemente solicitados atendidos por um pool de aplicativos. Se possível, defina como quantos mecanismos de script forem permitido pelo limite de memória.|250|

**System. DataServer/ASP/limites**

|Atributo|Descrição|Padrão|
|--- |--- |--- |
|processorThreadMax|Especifica o número máximo de threads de trabalho por processador que o ASP pode criar. Aumente se a configuração atual for insuficiente para lidar com a carga, o que pode causar erros ao atender a solicitações ou causar a utilização de recursos de CPU.|25|

**System. DataServer/ASP/ComPlus**

|Atributo|Descrição|Padrão|
|--- |--- |--- |
|executeInMta|Defina como **true** se erros ou falhas forem detectados enquanto o IIS estiver atendendo ao conteúdo ASP. Isso pode ocorrer, por exemplo, ao hospedar vários sites isolados nos quais cada site é executado em seu próprio processo de trabalho. Normalmente, os erros são relatados do COM+ no Visualizador de Eventos. Essa configuração habilita o modelo de apartamento multi-threaded em ASP.|False|


## <a name="aspnet-concurrency-setting"></a>Configuração de simultaneidade ASP.NET

### <a name="aspnet-35"></a>ASP.NET 3,5
Por padrão, ASP.NET limita a simultaneidade de solicitação para reduzir o consumo de memória de estado estacionário no servidor. Aplicativos de simultaneidade alta podem precisar ajustar algumas configurações para melhorar o desempenho geral. Você pode alterar essa configuração no arquivo Aspnet. config:

``` syntax
<system.web>
  <applicationPool maxConcurrentRequestsPerCPU="5000"/>
</system.web>
```

A configuração a seguir é útil para usar totalmente os recursos em um sistema:

-   **maxConcurrentRequestPerCpu** Valor padrão: 5000

    Essa configuração limita o número máximo de solicitações de ASP.NET em execução simultaneamente em um sistema. O valor padrão é conservador para reduzir o consumo de memória de aplicativos ASP.NET. Considere aumentar esse limite em sistemas que executam aplicativos que executam operações de e/s síncronas e demoradas. Caso contrário, os usuários podem experimentar alta latência devido a falhas de fila ou solicitação devido à excedeção de limites de filas em uma carga alta quando a configuração padrão é usada.

### <a name="aspnet-46"></a>ASP.NET 4.6
Além da configuração de maxConcurrentRequestPerCpu, o ASP.NET 4,7 também fornece configurações para melhorar o desempenho nos aplicativos que dependem muito da operação assíncrona. A configuração pode ser alterada no arquivo Aspnet. config.

``` syntax
<system.web>
  <applicationPool percentCpuLimit="90" percentCpuLimitMinActiveRequestPerCpu="100"/>
</system.web>
```

-   **percentCpuLimit** Valor padrão: 90 a solicitação assíncrona tem alguns problemas de escalabilidade quando uma carga enorme (além dos recursos de hardware) é colocada nesse cenário. O problema é devido à natureza da alocação em cenários assíncronos. Nessas condições, a alocação ocorrerá quando a operação assíncrona for iniciada e ela será consumida quando for concluída. Nesse momento, itâs muito possível que os objetos tenham sido movidos para a geração 1 ou 2 por GC. Quando isso acontecer, aumentar a carga mostrará o aumento na solicitação por segundo (RPS) até um ponto. Depois de passarmos esse ponto, o tempo gasto em GC começará a se tornar um problema e o RPS começará a se ajustar, tendo um efeito de dimensionamento negativo. Para corrigir o problema, quando o uso da CPU exceder a configuração percentCpuLimit, as solicitações serão enviadas para a fila nativa ASP.NET.
-   **percentCpuLimitMinActiveRequestPerCpu** Valor padrão: 100 a limitação da CPU (configuração percentCpuLimit) não é baseada no número de solicitações, mas no quão caro elas são. Como resultado, pode haver apenas algumas solicitações com uso intensivo de CPU, causando um backup na fila nativa, sem nenhuma forma de esvaziá-la de solicitações de entrada. Para resolver esse problme, percentCpuLimitMinActiveRequestPerCpu pode ser usado para garantir que um número mínimo de solicitações esteja sendo atendido antes da limitação ser acionada.

## <a name="worker-process-and-recycling-options"></a>Opções de processo de trabalho e reciclagem

Você pode configurar opções para reciclar processos de trabalho do IIS e fornecer soluções práticas para a agudo de situações ou eventos sem a necessidade de intervenção ou redefinição de um serviço ou computador. Tais situações e eventos incluem vazamentos de memória, aumento de carga de memória ou processos de trabalho não responsivos ou ociosos. Em condições comuns, as opções de reciclagem podem não ser necessárias e a reciclagem pode ser desativada ou o sistema pode ser configurado para reciclar com muita frequência.

Você pode habilitar a reciclagem de processos para um aplicativo específico adicionando atributos ao elemento **Recycling/periodicRestart** . O evento de reciclagem pode ser disparado por vários eventos, incluindo o uso de memória, um número fixo de solicitações e um período de tempo fixo. Quando um processo de trabalho é reciclado, as solicitações em fila e em execução são descarregadas e um novo processo é iniciado simultaneamente para atender a novas solicitações. O elemento **Recycling/periodicRestart** é por aplicativo, o que significa que cada atributo na tabela a seguir é particionado por aplicativo.

**System. applicationHost/applicationPools/ApplicationPoolDefaults/reciclagem/periodicRestart**

|Atributo|Descrição|Padrão|
|--- |--- |--- |
|memória|Habilite a reciclagem de processo se o consumo de memória virtual exceder o limite especificado em kilobytes. Essa é uma configuração útil para computadores de 32 bits que têm um espaço de endereço pequeno e 2 GB. Ele pode ajudar a evitar solicitações com falha devido a erros de falta de memória.|0|
|privateMemory|Habilite a reciclagem de processos se as alocações de memória privada excederem um limite especificado em kilobytes.|0|
|requests|Habilite a reciclagem de processo após um determinado número de solicitações.|0|
|tempo|Habilitar a reciclagem de processo após um período de tempo especificado.|29:00:00|


## <a name="dynamic-worker-process-page-out-tuning"></a>Trabalho dinâmico – ajuste de página de processo

A partir do Windows Server 2012 R2, o IIS oferece a opção de configurar o processo de trabalho para suspender depois que eles ficam ociosos por um tempo (além da opção de terminar, que existia desde o IIS 7).

A principal finalidade dos recursos de encerramento de processo de trabalho ocioso e de término do processo de trabalho ocioso é conservar a utilização de memória no servidor, já que um site pode consumir muita memória mesmo que esteja apenas ali, ouvindo. Dependendo da tecnologia usada no site (conteúdo estático vs ASP.NET versus outras estruturas), a memória usada pode ser de, em qualquer lugar, de cerca de 10 MB a centenas de MBs, e isso significa que, se o servidor estiver configurado com muitos sites, descobrindo as configurações mais eficazes para seus sites podem melhorar drasticamente o desempenho de sites ativos e suspensos.

Antes de entrarmos em detalhes, devemos nos lembrar de que, se não houver nenhuma restrição de memória, provavelmente é melhor simplesmente definir os sites para nunca suspender ou encerrar. Afinal, thereâs pouco valor ao encerrar um processo de trabalho se ele for o único no computador.

**Observe**  no caso de o site executar código instável, como código com um vazamento de memória ou instável, definir o site para terminar em ociosidade pode ser uma alternativa rápida e anormal para corrigir o bug do código. Isso não é algo que incentivamos, mas em uma fragmentação, pode ser melhor usar esse recurso como um mecanismo de limpeza, enquanto uma solução mais permanente está em funcionamento.\]

Â 

Outro fator a ser considerado é que, se o site usa muita memória, o processo de suspensão leva a uma tarifa, pois o computador precisa gravar os dados usados pelo processo de trabalho em disco. Se o processo de trabalho estiver usando uma grande parte da memória, a suspensão poderá ser mais cara do que o custo de esperar que ele reinicie o backup.

Para fazer o melhor do recurso de suspensão do processo de trabalho, você precisa examinar seus sites em cada pool de aplicativos e decidir qual deve ser suspenso, o que deve ser encerrado e que deve estar ativo indefinidamente. Para cada ação e cada site, você precisa descobrir o período de tempo limite ideal.

O ideal é que os sites que você configurará para suspensão ou encerramento sejam aqueles que têm visitantes todos os dias, mas não o suficiente para garantir que eles fiquem ativos o tempo todo. Normalmente, esses são sites com cerca de 20 visitantes exclusivos por dia ou menos. Você pode analisar os padrões de tráfego usando os arquivos de log do site e calcular o tráfego diário médio.

Tenha em mente que, uma vez que um usuário específico se conecte ao site, ele normalmente permanecerá nele por pelo menos um tempo, fazendo solicitações adicionais e, portanto, apenas contar solicitações diárias pode não refletir com precisão os padrões reais de tráfego. Para obter uma leitura mais precisa, você também pode usar uma ferramenta, como o Microsoft Excel, para calcular o tempo médio entre as solicitações. Por exemplo:

||URL da solicitação|Tempo de solicitação|Delta|
|--- |--- |--- |--- |
|1|/SourceSilverLight/Geosource.web/grosource.html|10:01||
|2|/SourceSilverLight/Geosource.web/sliverlight.js|10:10|0:09|
|3|/SourceSilverLight/Geosource.web/clientbin/geo/1.aspx|10:11|0:01|
|추가를 클릭합니다.|/lClientAccessPolicy.xml|10:12|0:01|
|5|/SourceSilverLight/GeosourcewebService/Service. asmx|10:23|0:11|
|6|/SourceSilverLight/geosource. Web/GeoSearchServer... ¦.|11:50|1:27|
|7|/rest/Services/CachedServices/Silverlight_load_la... ¦|12:50|1:00|
|8|/rest/Services/CachedServices/Silverlight_basemap... ¦.|12:51|0:01|
|9|/rest/Services/DynamicService/Silverlight_basemap... ¦.|12:59|0:08|
|10|/rest/Services/CachedServices/Ortho_2004_cache. as...|13:40|0:41|
|11|/rest/Services/CachedServices/Ortho_2005_cache. js|13:40|0:00|
|12|/rest/Services/CachedServices/OrthoBaseEngine.aspx|13:41|0:01|

No entanto, a parte difícil é descobrir qual configuração aplicar para fazer sentido. Em nosso caso, o site obtém várias solicitações de usuários e a tabela acima mostra que um total de quatro sessões exclusivas ocorreram em um período de 4 horas. Com as configurações padrão para a suspensão do processo de trabalho do pool de aplicativos, o site seria encerrado após o tempo limite padrão de 20 minutos, o que significa que cada um desses usuários experimentaria o ciclo de rotação do site. Isso o torna um candidato ideal para a suspensão do processo de trabalho, porque, na maior parte do tempo, o site está ocioso e, portanto, a suspensão dele conservaria recursos e permite que os usuários cheguem ao site quase instantaneamente.

Uma observação final e muito importante sobre isso é que o desempenho do disco é crucial para esse recurso. Como o processo de suspensão e ativação envolve gravar e ler grandes quantidades de dados no disco rígido, é altamente recomendável usar um disco rápido para isso. Os SSDs (unidades de estado sólido) são ideais e altamente recomendados para isso, e você deve certificar-se de que o arquivo de paginação do Windows esteja armazenado nele (se o próprio sistema operacional não estiver instalado no SSD, configure o sistema operacional para mover o arquivo de paginação para ele).

Independentemente de você usar um SSD ou não, também recomendamos corrigir o tamanho do arquivo de paginação para acomodar a gravação dos dados de Page out sem redimensionamento de arquivo. O redimensionamento de arquivo de página pode acontecer quando o sistema operacional precisa armazenar dados no arquivo de paginação, porque, por padrão, o Windows está configurado para ajustar seu tamanho automaticamente com base na necessidade. Ao definir o tamanho como um fixo, você pode evitar o redimensionamento e melhorar muito o desempenho.

Para configurar um tamanho de arquivo de paginação pré-fixa, você precisa calcular seu tamanho ideal, o que depende de quantos sites você estará suspendendo e da quantidade de memória que eles consomem. Se a média for de 200 MB para um processo de trabalho ativo e você tiver 500 sites nos servidores que serão suspensos, o arquivo de paginação deverá ser pelo menos (200 \* 500) MB sobre o tamanho base do arquivo de paginação (portanto, base + 100 GB em nosso exemplo).

**Observação** Quando os sites são suspensos, eles consumirão aproximadamente 6 MB cada, portanto, em nosso caso, o uso de memória se todos os sites forem suspensos seria de cerca de 3 GB. Na realidade, no entanto, a youâre provavelmente nunca fará com que todas sejam suspensas ao mesmo tempo.

 
## <a name="transport-layer-security-tuning-parameters"></a>Parâmetros de ajuste de segurança da camada de transporte

O uso da segurança de camada de transporte (TLS) impõe custos adicionais de CPU. O componente mais caro do TLS é o custo de estabelecer um estabelecimento de sessão porque ele envolve um handshake completo. A reconexão, a criptografia e a descriptografia também somam-se ao custo. Para melhorar o desempenho do TLS, faça o seguinte:

-   Habilitar Keep-Alive de HTTP para sessões TLS. Isso elimina os custos de estabelecimento de sessão.

-   Reutilize as sessões quando apropriado, especialmente com tráfego não Keep-Alive.

-   Aplique seletivamente a criptografia somente a páginas ou partes do site que precisam dela, em vez de todo o site.

**Observação**
-   Chaves maiores fornecem mais segurança, mas também usam mais tempo de CPU.

-   Todos os componentes podem não precisar ser criptografados. No entanto, misturar HTTP e HTTPS simples pode resultar em um aviso pop-up que nem todo o conteúdo na página é seguro.

 
## <a name="internet-server-application-programming-interface-isapi"></a>Interface de programação de aplicativo de servidor da Internet (ISAPI)

Nenhum parâmetro de ajuste especial é necessário para aplicativos ISAPI. Se você escrever uma extensão ISAPI privada, certifique-se de que ela seja escrita para desempenho e uso de recursos.

## <a name="managed-code-tuning-guidelines"></a>Diretrizes de ajuste de código gerenciado

O modelo de pipeline integrado no IIS 10,0 permite um alto grau de flexibilidade e extensibilidade. Módulos personalizados que são implementados em código nativo ou gerenciado podem ser inseridos no pipeline ou podem substituir os módulos existentes. Embora esse modelo de extensibilidade ofereça conveniência e simplicidade, você deve ter cuidado antes de inserir novos módulos gerenciados que se conectam a eventos globais. Adicionar um módulo gerenciado global significa que todas as solicitações, incluindo solicitações de arquivo estático, devem tocar em código gerenciado. Os módulos personalizados são suscetíveis a eventos como a coleta de lixo. Além disso, os módulos personalizados adicionam um custo de CPU significativo devido ao marshaling de dados entre código nativo e gerenciado. Se possível, você deve definir a pré-condição como managedHandler para o módulo gerenciado.

Para obter um melhor desempenho de inicialização a frio, certifique-se de pré-compilar o site do ASP.NET ou aproveite o recurso de inicialização do aplicativo IIS para ativar o aplicativo.

Se o estado da sessão não for necessário, certifique-se de desativá-lo para cada página.

Se houver muitas operações associadas de e/s, tente usar a versão assíncrona de APIs relevantes que oferecerá uma escalabilidade muito melhor.

Também usar o cache de saída de forma adequada também aumentará o desempenho do seu site da Web.

Quando você executa vários hosts que contêm scripts ASP.NET no modo isolado (um pool de aplicativos por site), monitore o uso de memória. Verifique se o servidor tem RAM suficiente para o número esperado de pools de aplicativos em execução simultaneamente. Considere o uso de vários domínios de aplicativo em vez de vários processos isolados.


## <a name="other-issues-that-affect-iis-performance"></a>Outros problemas que afetam o desempenho do IIS

Os seguintes problemas podem afetar o desempenho do IIS:

-   Instalação de filtros que não têm reconhecimento de cache

    A instalação de um filtro que não reconhece o cache de HTTP faz com que o IIS desabilite completamente o cache, o que resulta em um baixo desempenho. Os filtros ISAPI que foram gravados antes do IISÂ 6,0 podem causar esse comportamento.

-   Solicitações de CGI (interface de gateway comum)

    Por motivos de desempenho, o uso de aplicativos CGI para atender a solicitações não é recomendado com o IIS. Frequentemente, criar e excluir processos CGI envolve uma sobrecarga significativa. As alternativas melhores incluem o uso de FastCGI, scripts de aplicativo ISAPI e scripts ASP ou ASP.NET. O isolamento está disponível para cada uma dessas opções.

## <a name="see-also"></a>Veja também
- [Ajuste de desempenho do servidor Web](index.md) 
- [Ajuste do HTTP 1.1/2](http-performance.md)
