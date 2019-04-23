---
title: Ajuste do IIS 10.0
description: Recommmendations para servidores de web IIS 10.0 no Windows Server de 16 de ajuste de desempenho
ms.prod: windows-server-threshold
ms.technology: performance-tuning-guide
ms.topic: landing-page
ms.author: DavSo; Ericam; YaShi
author: phstee
ms.date: 10/16/2017
ms.openlocfilehash: 16ea5c857d99e8a69f528e81178911236341dbb8
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59869847"
---
# <a name="tuning-iis-100"></a>Ajuste do IIS 10.0

Serviços de informações da Internet (IIS) 10.0 está incluído com o Windows Server 2016. Ele usa um modelo de processo semelhante do IIS 8.5 e o IIS 7.0. Um driver de web de modo kernel (http. sys) recebe e encaminha solicitações HTTP e atende às solicitações do seu cache de resposta. Processos de trabalho se registrar para subespaços de URL e HTTP. sys encaminha a solicitação para o processo apropriado (ou conjunto de processos para pools de aplicativos).

O HTTP. sys é responsável pelo gerenciamento de conexão e manipulação de solicitação. A solicitação pode ser servida do cache de HTTP. sys ou passada para um processo de trabalho para manipulação adicional. Vários processos de trabalho podem ser configurados, que fornece isolamento a um custo reduzido. Para obter mais informações sobre como funciona o tratamento de solicitação, consulte a figura a seguir:

![solicitar tratamento no iis 10.0](../../media/perftune-guide-iis-request-handling.png)

O HTTP. sys inclui um cache de resposta. Quando uma solicitação corresponde a uma entrada no cache de resposta, o HTTP. sys envia a resposta do cache diretamente do modo kernel. Algumas plataformas de aplicativo web, como ASP.NET, fornecem mecanismos para permitir que qualquer conteúdo dinâmico seja armazenado em cache no cache do modo kernel. O manipulador de arquivo estático no IIS 10.0 automaticamente armazena em cache arquivos solicitados com frequência em http. sys.

Como um servidor web tem componentes de modo kernel e o modo de usuário, ambos os componentes devem ser ajustados para desempenho ideal. Portanto, ajuste o IIS 10.0 para uma carga de trabalho específica inclui a configuração a seguir:

-   O HTTP. sys e o cache de modo de kernel associados

-   Processos de trabalho e o IIS de modo de usuário, incluindo a configuração do pool de aplicativos

-   Determinados parâmetros de ajuste que afetam o desempenho

As seções a seguir discutem como configurar os aspectos de modo kernel e o modo de usuário do IIS 10.0.

## <a name="kernel-mode-settings"></a>Configurações do modo kernel

Configurações de HTTP. sys relacionados ao desempenho se enquadram em duas amplas categorias: gerenciamento e o gerenciamento de conexão e solicitação de cache. Todas as configurações do registro são armazenadas na seguinte entrada do registro:

``` syntax
HKEY_LOCAL_MACHINE\System\CurrentControlSet\Services\Http\Parameters
```

**Observação**   se o serviço HTTP já está em execução, você deve reiniciá-lo para que as alterações entrem em vigor.

Â 

## <a name="cache-management-settings"></a>Configurações de gerenciamento de cache

Um dos benefícios fornecidos pelo HTTP. sys é um cache de modo kernel. Se a resposta estiver no cache do modo kernel, você pode atender a uma solicitação HTTP completamente do modo kernel, o que reduz significativamente o custo de CPU de lidar com a solicitação. No entanto, o cache de kernel do IIS 10.0 é baseado na memória física, e o custo de uma entrada é a memória que ele ocupa.

Uma entrada no cache é útil somente quando ele é usado. No entanto, a entrada sempre consome memória física, se a entrada está sendo usada. Você deve avaliar a utilidade de um item no cache (a economia seja capaz de atender a partir do cache) e seu custo (a memória física ocupada) durante a vida útil da entrada levando em consideração os recursos disponíveis (CPU e memória física) e a carga de trabalho requisitos. O HTTP. sys tenta manter somente itens úteis, ativamente acessados no cache, mas você podem aumentar o desempenho do servidor web ajustando o cache de HTTP. sys para cargas de trabalho específicas.

A seguir estão algumas configurações úteis para o cache de modo kernel HTTP. sys:

-   **UriEnableCache** valor padrão: 1

    Permite que um valor diferente de zero, a resposta de modo kernel e o cache de fragmento. Para a maioria das cargas de trabalho, o cache deve permanecer habilitado. Considere desabilitar o cache, se você espera uma resposta muito baixos e cache de fragmento.

-   **UriMaxCacheMegabyteCount** valor padrão: 0

    Um valor diferente de zero que especifica a memória máxima que está disponível para o cache do modo kernel. O valor padrão, 0, permite que o sistema ajuste automaticamente a quantidade de memória está disponível para o cache.

    **Observação** especificando o tamanho define apenas o máximo e o sistema talvez não permitam o cache de atingir o tamanho máximo do conjunto.

    Â 

-   **UriMaxUriBytes** valor padrão: 262144 bytes (256 KB)

    O tamanho máximo de uma entrada no cache do modo kernel. Não são armazenados em cache respostas ou fragmentos de maiores do que isso. Se você tiver memória suficiente, considere aumentar o limite. Se a memória é limitada e entradas grandes têm lotado out menores, ele pode ser útil diminuir o limite.

-   **UriScavengerPeriod** valor padrão: 120 segundos

    O cache de HTTP. sys é verificado periodicamente por um eliminador e entradas que não são acessadas entre verificações de recuperação são removidas. Definir o período de recuperação como um valor alto reduz o número de verificações de recuperação. No entanto, o uso de memória de cache pode aumentar porque as entradas mais antigas e acessadas com menos frequência podem permanecer no cache. Definir o período de muito baixo faz com que verificações de recuperação mais frequentes e pode resultar em muitas liberações e rotatividade do cache.

## <a name="request-and-connection-management-settings"></a>Configurações de gerenciamento de solicitação e de conexão

No Windows Server 2016, o HTTP. sys gerencia conexões automaticamente. As seguintes configurações do registro não são mais usadas:

-   **maxConnections**

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


## <a name="user-mode-settings"></a>Configurações do modo de usuário

As configurações nesta seção afetam o comportamento do processo de trabalho IISÂ 10.0. A maioria dessas configurações pode ser encontrada no arquivo de configuração XML a seguir:

%SystemRoot%\\system32\\inetsrv\\config\\applicationHost.config

Use Appcmd.exe, o Console de gerenciamento do IIS 10.0, o WebAdministration ou Cmdlets do PowerShell de administração de IIS para alterá-los. A maioria das configurações são detectados automaticamente e eles não exigem uma reinicialização do servidor de aplicativos web ou processos de trabalho do IIS 10.0. Para obter mais informações sobre o arquivo applicationHost. config, consulte [Introdução ao applicationHost. config](http://www.iis.net/learn/get-started/planning-your-iis-architecture/introduction-to-applicationhostconfig).


## <a name="ideal-cpu-setting-for-numa-hardware"></a>Configuração ideal de CPU para hardware de NUMA

A partir da Windows 2016, o IIS 10.0 dá suporte a atribuição automática de CPU ideal para seus threads do pool aprimorar o desempenho e escalabilidade no hardware NUMA. Esse recurso é habilitado por padrão e pode ser configurado por meio da seguinte chave do registro:

``` syntax
HKEY_LOCAL_MACHINE\System\CurrentControlSet\Services\InetInfo\Parameters\ThreadPoolUseIdealCpu
```

Com esse recurso habilitado, o Gerenciador do IIS thread torna seu melhor esforço para distribuir uniformemente as threads de pool IIS em todas as CPUs em todos os nós com base em suas cargas atuais. Em geral, é recomendável manter essa configuração inalterado para hardware de NUMA padrão.

**Observação**   a configuração ideal de CPU é diferente da trabalho processo NUMA nó as configurações de atribuição (numaNodeAssignment e numaNodeAffinityMode) introduzidas no [configurações de CPU para um Pool de aplicativos](https://www.iis.net/configreference/system.applicationhost/applicationpools/add/cpu). A configuração ideal de CPU afeta como o IIS distribui seus threads de pool de thread, enquanto as configurações de atribuição de nó do de processo do trabalhador determinam em qual nó NUMA inicia um processo de trabalho.

## <a name="user-mode-cache-behavior-settings"></a>Configurações de comportamento de cache do modo de usuário

Esta seção descreve as configurações que afetam o comportamento de cache do IISÂ 10.0. O cache do modo de usuário é implementado como um módulo que escuta os eventos de cache globais que são gerados pelo pipeline integrado. Para desativar completamente o cache do modo de usuário, remova o módulo de FileCacheModule (cachfile.dll) na lista de módulos instalados na seção de configuração system.webServer/globalModules no applicationHost. config.

**system.webServer/caching**

|Atributo|Descrição|Padrão|
|--- |--- |--- |
|Enabled|Desabilita o cache do IIS de modo de usuário quando definido como **falsos**. Quando atingir o cache de taxa é muito pequena, que você pode desabilitar o cache completamente para evitar a sobrecarga que está associada com o caminho do código de cache. Desabilitar o cache de modo de usuário não desabilita o cache do modo kernel.|True|
|enableKernelCache|Desabilita o cache do modo kernel quando definido como **falsos**.|True|
|maxCacheSize|Limita o tamanho de cache de modo de usuário do IIS para o tamanho especificado em Megabytes. IIS ajusta o padrão dependendo da memória disponível. Escolha o valor cuidadosamente, com base no tamanho do conjunto de frequentemente acessado arquivos em relação à quantidade de RAM ou espaço de endereço de processo do IIS.|0|
|maxResponseSize|Armazena em cache os arquivos até o tamanho especificado. O valor real depende do número e tamanho dos arquivos maiores no conjunto de dados versus a RAM disponível. Armazenamento em cache grandes, arquivos solicitados com frequência podem reduzir latências associados, acesso ao disco e uso da CPU.|262144|

## <a name="compression-behavior-settings"></a>Configurações de comportamento de compactação

IIS 7.0 a partir compacta o conteúdo estático por padrão. Além disso, a compactação de conteúdo dinâmico é habilitada por padrão, quando o DynamicCompressionModule está instalado. A compactação reduz o uso de largura de banda, mas aumenta o uso de CPU. Conteúdo compactado é armazenado em cache no cache do modo kernel se possível. A partir da 8.5, IIS permite que a compactação ser controladas de forma independente para conteúdo estático e dinâmico. Conteúdo estático geralmente se refere ao conteúdo que não são alterados, como arquivos GIF ou HTM. Normalmente, o conteúdo dinâmico é gerado por scripts ou código no servidor, ou seja, as páginas do ASP.NET. Você pode personalizar a classificação de qualquer extensão específica como estático ou dinâmico.

Para desabilitar completamente a compactação, remova StaticCompressionModule e DynamicCompressionModule na lista de módulos na seção system.webServer/globalModules no applicationHost. config.

**system.webServer/httpCompression**

|Atributo|Descrição|Padrão|
|--- |--- |--- |
|staticCompression-EnableCpuUsage<br><br>staticCompression-DisableCpuUsage<br><br>dynamicCompression-EnableCpuUsage<br><br>dynamicCompression-DisableCpuUsage|Habilita ou desabilita a compactação se a porcentagem atual de uso da CPU ficar acima ou abaixo dos limites especificados.<br><br>Começando com o IIS 7.0, compactação será desabilitada automaticamente se a CPU de estado estacionário aumenta acima do limite de desabilitação. Compactação está habilitada se a CPU cair abaixo do limite de habilitar.|50, 100, 50 e 90, respectivamente|
|Diretório|Especifica o diretório em que as versões compactadas dos arquivos estáticos são temporariamente armazenadas e armazenadas em cache. Considere mover esse diretório de unidade do sistema se ele for acessado com frequência.|%SystemDrive%\inetpub\temp\IIS Temporary Compressed Files|
|doDiskSpaceLimiting|Especifica se existe um limite para quanto espaço em disco podem ocupar todos os arquivos compactados. Arquivos compactados são armazenados no diretório de compactação especificado pelo **directory** atributo.|True|
|maxDiskSpaceUsage|Especifica o número de bytes de espaço em disco que podem ser ocupado por arquivos compactados no diretório de compactação.<br><br>Essa configuração talvez precise ser aumentado se o tamanho total de todo o conteúdo compactado é grande demais.|100 MB|

**system.webServer/urlCompression**

|Atributo|Descrição|Padrão|
|--- |--- |--- |
|doStaticCompression|Especifica se o conteúdo estático é compactado.|True|
|doDynamicCompression|Especifica se o conteúdo dinâmico é compactado.|True|

**Observação** para servidores que executam o IIS 10.0 que têm pouca utilização da CPU média, considere a habilitação da compactação de conteúdo dinâmico, especialmente se as respostas são grandes. Isso deve ser feito pela primeira vez em um ambiente de teste para avaliar o efeito sobre o uso da CPU da linha de base.


### <a name="tuning-the-default-document-list"></a>Ajuste a lista de documentos padrão

O módulo de documento padrão trata as solicitações HTTP para a raiz de um diretório e as traduz em solicitações para um arquivo específico, como Default. htm ou index. htm. Em média, aroundÂ 25 por cento de todas as solicitações na Internet percorrer o caminho do documento padrão. Isso varia significativamente para sites individuais. Quando uma solicitação HTTP não especifica um nome de arquivo, o módulo de documento padrão procura a lista de documentos padrão permitido para cada nome no sistema de arquivos. Isso pode afetar adversamente o desempenho, especialmente se atingir o conteúdo requer fazendo uma rede de viagem de ida e volta ou tocando um disco.

Você pode evitar a sobrecarga ao desabilitar seletivamente os documentos padrão e ao reduzir ou ordenação a lista de documentos. Para sites que usam um documento padrão, você deve reduzir a lista para apenas os tipos de documento padrão que são usados. Além disso, ordene a lista para que ele começa com mais frequência nome do arquivo de documento padrão acessados.

Seletivamente, você pode definir o comportamento de documento padrão nas URLs específicas ao personalizar a configuração dentro de uma marca de local no applicationHost. config ou inserindo um arquivo Web. config diretamente no diretório de conteúdo. Isso permite que uma abordagem híbrida, que permite que os documentos padrão apenas onde são necessários e define nome da lista para o arquivo correto para cada URL.

Para desabilitar completamente os documentos padrão, remova DefaultDocumentModule na lista de módulos na seção system.webServer/globalModules no applicationHost. config.

**system.webServer/defaultDocument**

|Atributo|Descrição|Padrão|
|--- |--- |--- |
|habilitado|Especifica que os documentos padrão estão habilitados.|True|
|&lt;arquivos&gt; elemento|Especifica os nomes de arquivo são configurados como documentos padrão.|A lista padrão é default. htm, default. asp, index. htm, index. HTML, Iisstart. htm e default. aspx.|

## <a name="central-binary-logging"></a>Registro em log binário central

Quando a sessão de servidor tem vários grupos de URL sob ele, o processo de criação de centenas de arquivos de log formatada para grupos individuais de URL e gravar os dados de log em um disco pode consumir rapidamente valiosos recursos de CPU e memória, criando, assim, desempenho e problemas de escalabilidade. O log binário centralizado minimiza a quantidade de recursos do sistema que são usados para registrar em log, e, ao mesmo tempo, fornecendo dados detalhados de log para as organizações que precisam dele. Análise de logs em formato binário requer uma ferramenta de pós-processamento.

Você pode habilitar o registro em log binário central, definindo o atributo de centralLogFileMode a CentralBinary e definindo o **habilitada** atributo **True**. Considere mover o local do arquivo de log central off a partição do sistema e em uma unidade de log dedicado para evitar a contenção entre as atividades do sistema e atividades de registro em log.

**system.applicationHost/log**

|Atributo|Descrição|Padrão|
|--- |--- |--- |
|centralLogFileMode|Especifica o modo de registro em log para um servidor. Altere esse valor para CentralBinary para habilitar o registro em log binário central.|Site|

**system.applicationHost/log/centralBinaryLogFile**

|Atributo|Descrição|Padrão|
|--- |--- |--- |
|habilitado|Especifica se o registro em log binário central está habilitado.|False|
|Diretório|Especifica o diretório em que as entradas de log são gravadas.|%SystemDrive%\inetpub\logs\LogFiles|


## <a name="application-and-site-tunings"></a>Aplicativo e site tunings

As configurações a seguir estão relacionadas a tunings de site e o pool de aplicativo.

**system.applicationHost/applicationPools/applicationPoolDefaults**

|Atributo|Descrição|Padrão|
|--- |--- |--- |
|queueLength|Indica ao HTTP. sys quantas solicitações são enfileiradas para um pool de aplicativos antes das solicitações futuras serão rejeitadas. Quando o valor dessa propriedade for excedido, o IIS rejeita solicitações subsequentes com o erro 503.<br><br>Considere aumentar isso para aplicativos que se comunicam com armazenamentos de dados de back-end de alta latência se 503 erros forem observados.|1000|
|enable32BitAppOnWin64|Quando for verdadeiro, permite que um aplicativo de 32 bits seja executado em um computador que possui um processador de 64 bits.<br><br>Considere habilitar o modo de 32 bits, se o consumo de memória for uma preocupação. Como os tamanhos de ponteiro e tamanhos de instrução são menores, aplicativos de 32 bits usam menos memória do que os aplicativos de 64 bits. A desvantagem para executar aplicativos de 32 bits em um computador de 64 bits é que o espaço de endereço do modo de usuário é limitado a 4 GB.|False|

**system.applicationHost/sites/VirtualDirectoryDefault**

|Atributo|Descrição|Padrão|
|--- |--- |--- |
|allowSubDirConfig|Especifica se o IIS procura por arquivos Web. config em diretórios de conteúdo é menores do que o atual nível (True) ou não procura por arquivos Web. config em diretórios de conteúdo é menores do que o nível atual (False). Impondo uma limitação simple, que permite a configuração somente em diretórios virtuais, 10.0 IISÂ possa saber que, a menos que  **/ &lt;nome&gt;. htm** é um diretório virtual, ele não deve procurar um arquivo de configuração. Ignorar as operações de arquivo adicionais pode melhorar significativamente o desempenho de sites que têm um conjunto muito grande de conteúdo estático aleatoriamente acessado.|True|

## <a name="managing-iis-100-modules"></a>Gerenciamento de módulos do IIS 10.0

IIS 10.0 foi decomposto em vários módulos extensíveis de usuário para dar suporte a uma estrutura modular. Essa fatoração tem um pequeno custo. Para cada módulo do pipeline integrado deve chamar o módulo para todos os eventos que são relevantes para o módulo. Isso ocorre independentemente se o módulo deve fazer qualquer trabalho. Você pode economizar ciclos de CPU e memória, removendo todos os módulos que não são relevantes para um site específico.

Um servidor web que está ajustado para arquivos estáticos simples pode incluir apenas os cinco seguintes módulos: UriCacheModule, HttpCacheModule, StaticFileModule, AnonymousAuthenticationModule e HttpLoggingModule.

Para remover os módulos de applicationHost. config, remova todas as referências para o módulo de seções, além de remover a declaração de módulo em system.webServer/globalModules system.webServer/handlers e webServer/modules.

## <a name="classic-asp-settings"></a>Configurações do ASP clássicas

Principais custo de processamento de uma solicitação ASP clássica inclui a inicialização de um mecanismo de script, Compilando o script ASP solicitado em um modelo ASP e execução do modelo no mecanismo de script. Enquanto o custo de execução do modelo depende da complexidade do script ASP solicitado, o módulo ASP clássico do IIS pode armazenar em cache os mecanismos de script em modelos de memória e cache de memória e disco (somente se o estouro do cache de modelos na memória) para melhorar o desempenho em Cenários de vinculado à CPU.

As configurações a seguir são usadas para configurar o cache de modelos ASP clássico e o cache de mecanismo de script, e elas não afetam as configurações do ASP.NET.

**system.webServer/asp/cache**

|Atributo|Descrição|Padrão|
|--- |--- |--- |
|diskTemplateCacheDirectory|O nome do diretório que o ASP usa para armazenar modelos compilados quando o cache na memória estoura.<br><br>Recomendação: Definido como um diretório que não é muito usado, por exemplo, uma unidade que não é compartilhada com o sistema operacional, o log do IIS, ou outro conteúdo são acessados com frequência.|%SystemDrive%\inetpub\temp\ASP Compiled Templates|
|maxDiskTemplateCacheFiles|Especifica o número máximo de modelos ASP compilados que podem ser armazenados em cache em disco.<br><br>Recomendação: Definido como o valor máximo de 0x7FFFFFFF.|2000|
|scriptFileCacheSize|Esse atributo especifica o número máximo de modelos ASP compilados que podem ser armazenados em cache na memória.<br><br>Recomendação: Definido como pelo menos tantos conforme o número de scripts ASP solicitados com frequência atendido por um pool de aplicativos. Se possível, defina a tantos modelos ASP como permitir que os limites de memória.|500|
|scriptEngineCacheMax|Especifica o número máximo de mecanismos de script que irá manter em cache na memória.<br><br>Recomendação: Definido como pelo menos tantos conforme o número de scripts ASP solicitados com frequência atendido por um pool de aplicativos. Se possível, defina a tantos mecanismos de script, pois permite que o limite de memória.|250|

**system.webServer/asp/limits**

|Atributo|Descrição|Padrão|
|--- |--- |--- |
|processorThreadMax|Especifica o número máximo de threads de trabalho por processador que o ASP pode criar. Aumente se a configuração atual não é suficiente para lidar com a carga, que pode causar erros quando ele estiver atendendo a solicitações ou fazer com que o uso insuficiente de recursos de CPU.|25|

**system.webServer/asp/comPlus**

|Atributo|Descrição|Padrão|
|--- |--- |--- |
|executeInMta|Definido como **verdadeira** se falhas ou erros forem detectadas enquanto o IIS está servindo ASP conteúdo. Isso pode ocorrer, por exemplo, ao hospedar vários sites isolados em que cada site é executado em seu próprio processo de trabalho. Normalmente, os erros são relatados na COM+ no Visualizador de eventos. Essa configuração permite que o modelo de apartment com multithread em ASP.|False|


## <a name="aspnet-concurrency-setting"></a>Configuração de simultaneidade do ASP.NET

### <a name="aspnet-35"></a>ASP.NET 3.5
Por padrão, a ASP.NET limita a simultaneidade de solicitação para reduzir o consumo de memória de estado estável no servidor. Aplicativos de alta simultaneidade talvez seja necessário ajustar algumas configurações para melhorar o desempenho geral. Você pode alterar essa configuração no arquivo ASPNET config:

``` syntax
<system.web>
  <applicationPool maxConcurrentRequestsPerCPU="5000"/>
</system.web>
```

A seguinte configuração é útil para usar totalmente os recursos em um sistema:

-   **maxConcurrentRequestPerCpu** valor padrão: 5000

    Essa configuração limita o número máximo de solicitações do ASP.NET em execução simultânea em um sistema. O valor padrão é conservador para reduzir o consumo de memória de aplicativos ASP.NET. Considere aumentar esse limite em sistemas que executam aplicativos que realizam operações de e/s síncronas, longo. Caso contrário, os usuários podem experimentar alta latência devido a falhas de enfileiramento de mensagens ou a solicitação devido a exceder os limites de fila em uma alta carga quando a configuração padrão é usada.

### <a name="aspnet-46"></a>ASP.NET 4.6
Além da configuração maxConcurrentRequestPerCpu, o ASP.NET 4.7 também fornece as configurações para melhorar o desempenho dos aplicativos que dependem fortemente de operação assíncrona. A configuração pode ser alterada no arquivo ASPNET config.

``` syntax
<system.web>
  <applicationPool percentCpuLimit="90" percentCpuLimitMinActiveRequestPerCpu="100"/>
</system.web>
```

-   **percentCpuLimit** Default value: Solicitação assíncrona 90 tem alguns problemas de escalabilidade quando uma enorme carga (além dos recursos de hardware) é colocada em um cenário como esse. O problema é devido à natureza de alocação em cenários assíncronos. Nessas condições, alocação ocorrerá quando a operação assíncrona é iniciado, e ele será consumido quando ele for concluído. Naquela época, s é muito possível os objetos foram movidos para a geração 1 ou 2 por GC. Quando isso acontece, aumentando a carga será mostram o aumento na solicitação por segundo (rps) até que um ponto. Depois que podemos passar esse ponto, o tempo gasto em GC será iniciado para se tornar um problema e o rps dará início à dip, ter um efeito negativo de colocação em escala. Para corrigir o problema, quando o uso da cpu excede percentCpuLimit, as solicitações serão enviadas para a fila nativa do ASP.NET.
-   **percentCpuLimitMinActiveRequestPerCpu** Default value: Limitação da CPU 100 (configuração percentCpuLimit) não é baseado no número de solicitações, mas no caro como eles são. Como resultado, pode haver algumas solicitações de uso intensivo de CPU, fazendo com que um backup na fila nativa sem uma maneira para esvaziar-além das solicitações de entrada. Para resolver esse problme, percentCpuLimitMinActiveRequestPerCpu pode ser usado para garantir que um número mínimo de solicitações está sendo atendido antes de limitação é ativada.

## <a name="worker-process-and-recycling-options"></a>Processo de trabalho e a reciclagem de opções

Você pode configurar as opções de reciclagem de processos de trabalho do IIS e fornecer soluções práticas para situações agudas ou eventos sem a necessidade de intervenção ou redefinir um computador ou serviço. Tais situações e eventos incluem processos de trabalho sem resposta ou ocioso, aumento da carga memória ou vazamentos de memória. Em condições normais, reciclagem opções não pode ser necessárias e reciclagem pode ser desativada ou o sistema pode ser configurado para reciclar muito raramente.

Você pode habilitar a reciclagem de processo para um aplicativo específico com a adição de atributos para o **reciclagem/periodicRestart** elemento. O evento de reciclagem pode ser disparado por vários eventos, incluindo o uso de memória, um número fixo de solicitações e um período de tempo fixo. Quando um processo de trabalho é reciclado, as solicitações enfileiradas e em execução são descarregadas e um novo processo é iniciado simultaneamente para atender novas solicitações. O **reciclagem/periodicRestart** elemento é por aplicativo, o que significa que cada atributo na tabela a seguir é particionado em uma base por aplicativo.

**system.applicationHost/applicationPools/ApplicationPoolDefaults/recycling/periodicRestart**

|Atributo|Descrição|Padrão|
|--- |--- |--- |
|memória|Habilite a reciclagem de processo, se o consumo de memória virtual excede o limite especificado em kilobytes. Essa é uma configuração útil para computadores de 32 bits que têm um pequeno, 2 GB de espaço de endereço. Ele pode ajudar a evitar solicitações com falha devido a erros de falta de memória.|0|
|privateMemory|Habilite a reciclagem de processo se as alocações de memória privada excederem um limite especificado em kilobytes.|0|
|requests|Habilite a reciclagem de processo após um determinado número de solicitações.|0|
|time|Habilite a reciclagem de processo após um período de tempo especificado.|29:00:00|


## <a name="dynamic-worker-process-page-out-tuning"></a>Ajuste dinâmico do processo de trabalho fora da página

A partir do Windows Server 2012 R2, o IIS oferece a opção de configuração do processo de trabalho para suspender após eles terem sido ociosos por algum tempo (além da opção de encerramento, o que existe desde o IIS 7).

É o principal objetivo do processo de trabalhador inativo página-out e recursos de encerramento do processo de trabalhador inativo conservar a utilização de memória no servidor, uma vez que um site pode consumir muita memória, mesmo se ele está apenas sentada lá, escutando. Dependendo da tecnologia usada no site (portas estáticas vs conteúdo ASP.NET vs outras estruturas), a memória usada pode ser de cerca de 10 MB a centenas de MB, e isso significa que, se seu servidor está configurado com vários sites, descobrir as configurações mais eficientes para seus sites podem melhorar consideravelmente o desempenho de sites do Active Directory e foram suspensos.

Antes de entrarmos em detalhes, podemos deve ter em mente que se não houver nenhuma restrição de memória, provavelmente será melhor simplesmente definir os sites nunca suspender ou encerrar. Afinal de contas, s há pouco valor na terminação um trabalhador processar se for o único na máquina.

**Observação**   caso o site executa código instável, como código com um vazamento de memória, ou caso contrário instável, a configuração de site para terminar em ocioso pode ser uma alternativa rápida e simples para corrigir o bug de código. Isso não é algo que nós o encorajamos, mas em uma situação, talvez seja melhor usar esse recurso como um mecanismo de limpeza, enquanto uma solução mais permanente está em andamento.\]

Â 

Outro fator a considerar é que, se o site usar muita memória, em seguida, o processo de suspensão leva um pedágio, porque o computador deve gravar os dados usados pelo processo de trabalho em disco. Se o processo de trabalho estiver usando um grande bloco de memória, em seguida, a suspensão pode ser mais cara do que o custo de ter que esperar iniciar um backup.

Para fazer o melhor do que o recurso de suspensão do processo de trabalho, você precisará examinar seus sites em cada pool de aplicativos e decidir qual deverá ser suspenso, que devem ser encerrado e que deve estar ativo por tempo indeterminado. Para cada ação e cada site, você precisa descobrir o período de tempo limite ideal.

O ideal é que os sites que você configurará para suspensão ou encerramento são aqueles que têm os visitantes todos os dias, mas não o suficiente para justificar a manter ativo o tempo todo. Geralmente, esses são os sites com cerca de 20 visitantes exclusivos por dia ou menos. Você pode analisar os padrões de tráfego usando arquivos de log do site e calcular o tráfego diário médio.

Tenha em mente que quando um usuário específico se conecta ao site, eles normalmente permanecerá nele pelo menos algum tempo, fazendo solicitações adicionais e então basta contagem de solicitações diárias podem não refletir precisamente os padrões de tráfego real. Para obter uma leitura mais precisa, você também pode usar uma ferramenta, como o Microsoft Excel, para calcular o tempo médio entre as solicitações. Por exemplo: 

||URL da solicitação|Tempo de solicitação|Delta|
|--- |--- |--- |--- |
|1|/SourceSilverLight/Geosource.web/grosource.html|10:01||
|2|/SourceSilverLight/Geosource.web/sliverlight.js|10:10|0:09|
|3|/SourceSilverLight/Geosource.web/clientbin/geo/1.aspx|10:11|0:01|
|4|/lClientAccessPolicy.xml|10:12|0:01|
|5|/ SourceSilverLight/GeosourcewebService/Service.asmx|10:23|0:11|
|6|/ SourceSilverLight/Geosource.web/GeoSearchServer...¦.|11:50|1:27|
|7|/rest/Services/CachedServices/Silverlight_load_la...¦|12:50|1:00|
|8|/rest/Services/CachedServices/Silverlight_basemap...¦.|12:51|0:01|
|9|/rest/Services/DynamicService/ Silverlight_basemap...¦.|12:59|0:08|
|10|/rest/Services/CachedServices/Ortho_2004_cache.as...|13:40|0:41|
|11|/rest/Services/CachedServices/Ortho_2005_cache.js|13:40|0:00|
|12|/rest/Services/CachedServices/OrthoBaseEngine.aspx|13:41|0:01|

A parte difícil, no entanto, é descobrir qual configuração Aplicar para fazer sentido. Em nosso caso, o site obtém um monte de solicitações de usuários e a tabela acima mostra que um total de sessões exclusivas 4 ocorreu em um período de 4 horas. Com as configurações padrão para a suspensão do processo de trabalho do pool de aplicativos, o site será encerrado depois que o tempo de limite padrão de 20 minutos, que significa que cada um desses usuários experimentará o ciclo de aceleração de site. Isso torna um candidato ideal para a suspensão do processo de trabalho, porque para a maioria das vezes, o site está ocioso, e, portanto, suspendê-la seria conservar os recursos e permitir que os usuários acessar os sites quase instantaneamente.

Uma observação final e é muito importante sobre isso é que o desempenho do disco é crucial para esse recurso. Como o processo de ativação e suspensão envolvem a gravação e leitura de grande quantidade de dados para o disco rígido, é altamente recomendável usar um disco rápido para isso. Unidades de estado sólido (SSDs) são ideais e altamente recomendada para isso, e certifique-se de que o arquivo de paginação do Windows é armazenado no mesmo (se o próprio sistema operacional não estiver instalado na SSD, configurar o sistema operacional para mover o arquivo de paginação para ele).

Se você usar um SSD ou não, é recomendável também corrigir o tamanho do arquivo de paginação para acomodar a gravar os dados de saída de página sem redimensionamento de arquivo. Redimensionamento do arquivo de paginação pode acontecer quando o sistema operacional precisa armazenar dados no arquivo de paginação, porque por padrão, o Windows está configurado ajustar automaticamente seu tamanho com base em precisa. Definindo o tamanho para uma correção, você pode evitar o redimensionamento e melhorar muito o desempenho.

Para configurar um tamanho de arquivo de página previamente fixo, você precisa calcular seu tamanho ideal, que depende de quantos sites, você irá suspender e a quantidade de memória que eles consomem. Se a média é de 200 MB para um processo de trabalho ativos e você tiver 500 sites nos servidores que serão ser suspensão, o arquivo de paginação deve ser pelo menos (200 \* 500) MB em relação ao tamanho base do arquivo de página (portanto, base + 100 GB em nosso exemplo).

**Observação** quando sites são suspensos, elas consumirão aproximadamente 6 MB cada, portanto, em nosso caso, o uso de memória se todos os sites são suspensos seria cerca de 3 GB. Na verdade, no entanto, você re provavelmente nunca vai fazer com que eles suspenso ao mesmo tempo.

 
## <a name="transport-layer-security-tuning-parameters"></a>Ajustar parâmetros de segurança de camada de transporte

O uso de segurança de camada de transporte (TLS) impõe um custo adicional da CPU. O componente mais caro de TLS é o custo de estabelecer um estabelecimento da sessão porque ela envolve um handshake completo. Reconexão, criptografia e descriptografia de também adicionam o custo. Para melhorar o desempenho de TLS, faça o seguinte:

-   Habilite keep-alive de HTTP para sessões TLS. Isso elimina os custos do estabelecimento de sessão.

-   Reutilizar sessões quando apropriado, especialmente com o tráfego não-keep-alive.

-   Aplica seletivamente a criptografia apenas para páginas ou partes do site que precisam dele, em vez disso, todo o site.

**Observação**
-   Chaves maiores fornecem mais segurança, mas eles também usam mais tempo de CPU.

-   Todos os componentes talvez não precise ser criptografado. No entanto, misturar simples HTTP e HTTPS pode resultar em um pop-up avisando que não todo o conteúdo na página é seguro.

 
## <a name="internet-server-application-programming-interface-isapi"></a>Interface de programação de aplicativo de servidor da Internet (ISAPI)

Não há parâmetros de ajuste especiais são necessárias para aplicativos ISAPI. Se você escrever uma extensão ISAPI particular, certifique-se de que ele é gravado para desempenho e uso de recursos.

## <a name="managed-code-tuning-guidelines"></a>Diretrizes de ajuste de código gerenciado

O modelo de pipeline integrado no IIS 10.0 permite um alto grau de flexibilidade e extensibilidade. Módulos personalizados que são implementados em código nativo ou gerenciado podem ser inseridos no pipeline, ou podem substituir os módulos existentes. Embora esse modelo de extensibilidade oferece conveniência e a simplicidade, você deve ter cuidado antes de inserir novos módulos gerenciados que se conectam em eventos globais. Adicionar que um módulo gerenciado global significa que todas as solicitações, incluindo solicitações de arquivo estático devem toque de código gerenciado. Módulos personalizados são suscetíveis a eventos, como coleta de lixo. Além disso, os módulos personalizados adicionar significativo custo de CPU devido a dados de marshaling entre código gerenciado e nativo. Se possível, você deve definir a pré-condição como managedHandler para módulo gerenciado.

Para obter um melhor desempenho de inicialização a frio, certifique-se de que você pré-compilar o site da web ou usar inicialização do aplicativo IIS recurso do ASP.NET para aquecer o aplicativo.

Se o estado de sessão não for necessária, certifique-se de que você desativá-lo para cada página.

Se houver várias e/s associados a operações, tente usar a versão assíncrona de APIs relevantes que dará a você escalabilidade muito melhor.

Também usando o Cache de saída corretamente também melhorará o desempenho do seu site da web.

Quando você executar vários hosts que contêm scripts do ASP.NET no modo isolado (um pool de aplicativos por site), monitore o uso de memória. Certifique-se de que o servidor tem RAM suficiente para o número esperado de em pools de aplicativos em execução simultaneamente. Considere o uso de vários domínios de aplicativo em vez de vários processos isolados.


## <a name="other-issues-that-affect-iis-performance"></a>Outros problemas que afetam o desempenho do IIS

Os seguintes problemas podem afetar o desempenho do IIS:

-   Instalação de filtros que não estão cientes de cache

    A instalação de um filtro que não está ciente de cache HTTP faz com que o IIS desabilitar completamente o serviço de cache, o que resulta em baixo desempenho. Filtros ISAPI que foram gravados antes IISÂ 6.0 podem causar esse comportamento.

-   Solicitações comuns de Interface CGI (Gateway)

    Por motivos de desempenho, não é recomendável o uso de aplicativos CGI para atender às solicitações com o IIS. Com frequência, criação e exclusão de processos CGI envolvem uma sobrecarga significativa. Alternativas melhores incluem o uso de FastCGI, scripts do aplicativo ISAPI e scripts ASP ou ASP.NET. Isolamento está disponível para cada uma dessas opções.

# <a name="see-also"></a>Consulte também
- [Ajuste de desempenho do servidor de Web](index.md) 
- [O HTTP 1.1/2 de ajuste](http-performance.md)
