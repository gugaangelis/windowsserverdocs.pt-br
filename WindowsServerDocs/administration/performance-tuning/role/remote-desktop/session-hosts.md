---
title: Hosts de sessão Área de Trabalho Remota de ajuste de desempenho
description: Diretrizes de ajuste de desempenho para hosts de sessão Área de Trabalho Remota
ms.topic: article
ms.author: hammadbu
author: phstee
ms.date: 10/22/2019
ms.openlocfilehash: 82a89454cfab7cda9de4375c843cde81af5b0544
ms.sourcegitcommit: 7cacfc38982c6006bee4eb756bcda353c4d3dd75
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/14/2020
ms.locfileid: "90078173"
---
# <a name="performance-tuning-remote-desktop-session-hosts"></a>Hosts de sessão Área de Trabalho Remota de ajuste de desempenho


Este tópico discute como selecionar o hardware de Host da Sessão da Área de Trabalho Remota (Host da Sessão RD), ajustar o host e ajustar os aplicativos.

**Neste tópico:**

-   [Selecionar o hardware adequado para desempenho](#selecting-the-proper-hardware-for-performance)

-   [Ajustando aplicativos para Host da Sessão da Área de Trabalho Remota](#tuning-applications-for-remote-desktop-session-host)

-   [Host da Sessão da Área de Trabalho Remota parâmetros de ajuste](#remote-desktop-session-host-tuning-parameters)

## <a name="selecting-the-proper-hardware-for-performance"></a>Selecionar o hardware adequado para desempenho


Para uma implantação do Host da Sessão RD Server, a escolha do hardware é regida pelo conjunto de aplicativos e como os usuários os usam. Os principais fatores que afetam o número de usuários e sua experiência são CPU, memória, disco e gráficos. Esta seção contém diretrizes adicionais específicas para Host da Sessão RD servidores e está principalmente relacionada ao ambiente de vários usuários de servidores Host da Sessão RD.

### <a name="cpu-configuration"></a>Configuração da CPU

A configuração de CPU é determinada de acordo com a multiplicação da CPU necessária para dar suporte a uma sessão pelo número de sessões às quais o sistema deve dar suporte, mantendo, ao mesmo tempo, uma zona de buffer para lidar com picos temporários. Vários processadores lógicos podem ajudar a reduzir situações de congestionamento de CPU anormal, que geralmente são causadas por alguns threads superativos que estão contidos por um número semelhante de processadores lógicos.

Portanto, quanto mais processadores lógicos estiver em um sistema, menor será a margem de amortecedor que deve ser incorporada à estimativa de uso da CPU, o que resultará em um percentual maior de carga ativa por CPU. Um fator importante a ser lembrado é que dobrar o número de CPUs não dá uma capacidade de CPU dupla.

### <a name="memory-configuration"></a>Configuração de memória

A configuração de memória depende dos aplicativos que os usuários empregam; no entanto, a quantidade necessária de memória pode ser estimada usando a seguinte fórmula: TotalMem = OSMem + SessionMem \* ns

OSMem é a quantidade de memória que o sistema operacional exige para ser executado (como imagens binárias do sistema, estruturas de dados etc.), SessionMem é a quantidade de processos de memória em execução em uma sessão necessária, e NS é o número de destino de sessões ativas. A quantidade de memória necessária para uma sessão é basicamente determinada pela referência de memória privada definida para aplicativos e processos do sistema em execução dentro da sessão. O código compartilhado ou as páginas de dados têm pouco efeito porque apenas uma cópia está presente no sistema.

Uma observação interessante (supondo que o sistema de disco que está fazendo backup do arquivo de paginação não é alterado) é que quanto maior o número de sessões ativas simultâneas que o sistema planeja oferecer suporte, maior será a alocação de memória por sessão. Se a quantidade de memória alocada por sessão não for aumentada, o número de falhas de página que as sessões ativas geram aumenta com o número de sessões. Essas falhas eventualmente sobrecarregam o subsistema de e/s. Ao aumentar a quantidade de memória alocada por sessão, a probabilidade de incorrer em falhas de página diminui, o que ajuda a reduzir a taxa geral de falhas de página.

### <a name="disk-configuration"></a>Configuração de disco

O armazenamento é um dos aspectos mais despercebidos quando você configura servidores de Host da Sessão RD, e pode ser a limitação mais comum em sistemas implantados no campo.

A atividade de disco gerada em um servidor Host da Sessão RD típico afeta as seguintes áreas:

-   Arquivos do sistema e binários do aplicativo

-   Arquivos de paginação

-   Perfis de usuário e dados de usuário

O ideal é que essas áreas sejam submetidas a backup por dispositivos de armazenamento distintos. O uso de configurações de RAID distribuídas ou outros tipos de armazenamento de alto desempenho melhora ainda mais o desempenho. É altamente recomendável que você use adaptadores de armazenamento com cache de gravação com suporte de bateria. Os controladores com cache de gravação em disco oferecem suporte aprimorado para operações de gravação síncronas. Como todos os usuários têm um Hive separado, as operações de gravação síncrona são significativamente mais comuns em um servidor de Host da Sessão RD. Os hives do registro são salvos periodicamente no disco usando operações de gravação síncronas. Para habilitar essas otimizações, no console de gerenciamento de disco, abra a caixa de diálogo **Propriedades** do disco de destino e, na guia **políticas** , selecione **Habilitar cache de gravação no disco** e **desativar a liberação do buffer de cache de gravação do Windows** nas caixas de seleção do dispositivo.

### <a name="network-configuration"></a>Configuração de rede

O uso de rede para um servidor de Host da Sessão RD inclui duas categorias principais:

-   O uso do tráfego de conexão Host da Sessão RD é determinado quase exclusivamente pelos padrões de desenho que são reportados pelos aplicativos em execução dentro das sessões e o tráfego de e/s de dispositivos redirecionados.

    Por exemplo, os aplicativos que manipulam o processamento de texto e a entrada de dados consomem largura de banda de aproximadamente 10 a 100 kilobits por segundo, enquanto gráficos avançados e reprodução de vídeo causam aumentos significativos no uso de largura de banda

-   Conexões de back-end, como perfis móveis, acesso a aplicativos a compartilhamentos de arquivos, servidores de banco de dados, servidores de email e servidores HTTP.

    O volume e o perfil do tráfego de rede são específicos para cada implantação.

## <a name="tuning-applications-for-remote-desktop-session-host"></a>Ajustando aplicativos para Host da Sessão da Área de Trabalho Remota


A maior parte do uso da CPU em um servidor Host da Sessão RD é orientada por aplicativos. Os aplicativos de desktop geralmente são otimizados para a capacidade de resposta com o objetivo de minimizar quanto tempo um aplicativo leva para responder a uma solicitação de usuário. No entanto, em um ambiente de servidor, é igualmente importante minimizar a quantidade total de uso da CPU necessária para concluir uma ação para evitar afetar negativamente outras sessões.

Considere as seguintes sugestões ao configurar aplicativos que serão usados em um servidor de Host da Sessão RD:

-   Minimizar o processamento de loop ocioso em segundo plano

    Exemplos típicos estão desabilitando a gramática em segundo plano e verificação ortográfica, indexação de dados para pesquisa e gravações em segundo plano.

-   Minimize a frequência com que um aplicativo executa uma verificação de estado ou uma atualização.

    A desabilitação desses comportamentos ou o aumento do intervalo entre as iterações de sondagem e o acionamento do temporizador beneficia significativamente o uso da CPU, pois o efeito dessas atividades é rapidamente amplificado para muitas sessões ativas. Exemplos típicos são os ícones de status da conexão e as informações da barra de status.

-   Minimize a contenção de recursos entre aplicativos, reduzindo sua frequência de sincronização.

    Exemplos desses recursos incluem chaves do registro e arquivos de configuração. Exemplos de componentes e recursos de aplicativos são indicadores de status (como notificações de Shell), indexação em segundo plano ou monitoramento de alterações e sincronização offline.

-   Desabilite os processos desnecessários que estão registrados para começar com a entrada do usuário ou uma inicialização da sessão.

    Esses processos podem contribuir significativamente com o custo do uso da CPU ao criar uma nova sessão de usuário, que geralmente é um processo intensivo de CPU, e pode ser muito caro em cenários de manhã. Use MsConfig.exe ou MsInfo32.exe para obter uma lista de processos que são iniciados na entrada do usuário. Para obter informações mais detalhadas, você pode usar o [Autoruns para Windows](/sysinternals/downloads/autoruns).

Para o consumo de memória, você deve considerar o seguinte:

-   Verifique se as DLLs carregadas por um aplicativo não estão realocadas.

    -   DLLs realocadas podem ser verificadas selecionando processar exibição de DLL, conforme mostrado na figura a seguir, usando o [Gerenciador de processos](/sysinternals/downloads/process-explorer).

    -   Aqui, podemos ver que y.dll foi realocada porque x.dll já ocupava seu endereço base padrão e a ASLR não estava habilitada

        ![DLLs realocadas](../../media/perftune-guide-relocated-dlls.png)

        Se as DLLs forem realocadas, é impossível compartilhar seu código entre as sessões, o que aumenta significativamente a superfície de uma sessão. Esse é um dos problemas de desempenho mais comuns relacionados à memória em um servidor Host da Sessão RD.

-   Para aplicativos Common Language Runtime (CLR), use o gerador de imagem nativa (Ngen.exe) para aumentar o compartilhamento de página e reduzir a sobrecarga de CPU.

    Quando possível, aplique técnicas semelhantes a outros mecanismos de execução semelhantes.

## <a name="remote-desktop-session-host-tuning-parameters"></a>Host da Sessão da Área de Trabalho Remota parâmetros de ajuste


### <a name="page-file"></a>Arquivo de paginação

O tamanho do arquivo de paginação insuficiente pode causar falhas de alocação de memória em aplicativos ou componentes do sistema. Você pode usar o contador de desempenho bytes de memória para o confirmados para monitorar a quantidade de memória virtual confirmada no sistema.

### <a name="antivirus"></a>Antivírus

A instalação do software antivírus em um Host da Sessão RD Server afeta muito o desempenho geral do sistema, especialmente o uso da CPU. É altamente recomendável que você exclua da lista de monitoramento ativo todas as pastas que mantêm arquivos temporários, especialmente aqueles que os serviços e outros componentes do sistema geram.

### <a name="task-scheduler"></a>Agendador de Tarefas

Agendador de Tarefas permite que você examine a lista de tarefas que estão agendadas para eventos diferentes. Para um servidor de Host da Sessão RD, é útil se concentrar especificamente nas tarefas que são configuradas para serem executadas em tempo ocioso, na entrada do usuário ou na conexão e na desconexão da sessão. Devido às especificidades da implantação, muitas dessas tarefas podem ser desnecessárias.

### <a name="desktop-notification-icons"></a>Ícones de notificação da área de trabalho

Os ícones de notificação na área de trabalho podem ter mecanismos de atualização bastante caros. Você deve desabilitar todas as notificações removendo o componente que os registra na lista de inicialização ou alterando a configuração em aplicativos e componentes do sistema para desabilitá-los. Você pode usar os **ícones personalizar notificações** para examinar a lista de notificações disponíveis no servidor.

### <a name="remote-desktop-protocol-data-compression"></a>Compactação de dados protocolo RDP

Protocolo RDP compactação pode ser configurada usando política de grupo em **configuração do computador** &gt; **modelos administrativos** &gt; **componentes do Windows** &gt; **serviços de área de trabalho remota** &gt; **host da sessão da área de trabalho remota** &gt; **ambiente de sessão remota** &gt; **Configurar a compactação para dados do RemoteFX**. Três valores são possíveis:

-   **Otimizado para usar menos memória** Consome a menor quantidade de memória por sessão, mas tem a menor taxa de compactação e, portanto, o consumo de largura de banda mais alto.

-   **Equilibra a memória e a largura de banda da rede** Redução do consumo de largura de banda enquanto aumenta a margem de consumo de memória (aproximadamente 200 KB por sessão).

-   **Otimizado para usar menos largura de banda de rede** Reduz ainda mais o uso de largura de banda de rede a um custo de aproximadamente 2 MB por sessão. Se você quiser usar essa configuração, avalie o número máximo de sessões e teste para esse nível com essa configuração antes de posicionar o servidor em produção.

Você também pode optar por não usar um algoritmo de compactação protocolo RDP, portanto, é recomendável usá-lo apenas com um dispositivo de hardware criado para otimizar o tráfego de rede. Mesmo que você opte por não usar um algoritmo de compactação, alguns dados gráficos serão compactados.

### <a name="device-redirection"></a>Redirecionamento de dispositivo

O redirecionamento de dispositivo pode ser configurado usando política de grupo em **configuração do computador** &gt; **modelos administrativos** &gt; **componentes do Windows** &gt; **serviços de área de trabalho remota** &gt; **host da sessão da área de trabalho remota** &gt; **redirecionamento de dispositivo e recurso** ou usando a caixa Propriedades da **coleção de sessões** no Gerenciador do servidor.

Geralmente, o redirecionamento de dispositivo aumenta a quantidade de largura de banda de rede Host da Sessão RD conexões do servidor, pois os dados são trocados entre dispositivos nos computadores cliente e processos que estão em execução na sessão do servidor. A extensão do aumento é uma função da frequência das operações executadas pelos aplicativos que estão em execução no servidor em relação aos dispositivos redirecionados.

O redirecionamento de impressora e Plug and Play redirecionamento de dispositivo também aumenta o uso da CPU na entrada. Você pode redirecionar impressoras de duas maneiras:

-   O redirecionamento baseado em driver de impressora correspondente quando um driver para a impressora deve ser instalado no servidor. Versões anteriores do Windows Server usavam esse método.

-   Introduzido no Windows Server 2008, o redirecionamento de driver de impressora de impressão fácil usa um driver de impressora comum para todas as impressoras.

Recomendamos o método Print fácil porque ele causa menos uso da CPU para a instalação da impressora no momento da conexão. O método de driver correspondente causa um aumento no uso da CPU porque requer que o serviço de spooler carregue drivers diferentes. Para o uso da largura de banda, a impressão fácil causa um aumento no uso da largura de banda da rede, mas não é significativa o suficiente para deslocar os outros benefícios de desempenho, capacidade de gerenciamento e confiabilidade.

O redirecionamento de áudio causa um fluxo constante de tráfego de rede. O redirecionamento de áudio também permite que os usuários executem aplicativos multimídia que normalmente têm alto consumo de CPU.

### <a name="client-experience-settings"></a>Configurações de experiência do cliente

Por padrão, Conexão de Área de Trabalho Remota (RDC) escolhe automaticamente a configuração de experiência correta com base na adequação da conexão de rede entre o servidor e os computadores cliente. Recomendamos que a configuração de RDC permaneça em **detectar qualidade de conexão automaticamente**.

Para usuários avançados, a RDC fornece controle sobre uma variedade de configurações que influenciam o desempenho da largura de banda da rede para a conexão Serviços de Área de Trabalho Remota. Você pode acessar as configurações a seguir usando a guia **experiência** em conexão de área de trabalho remota ou como configurações no arquivo RDP.

As seguintes configurações se aplicam ao se conectar a qualquer computador:

-   **Desabilitar papel de parede** (desabilitar papel de parede: i: 0) não mostra o papel de parede da área de trabalho em conexões redirecionadas. Essa configuração pode reduzir significativamente o uso de largura de banda se o papel de parede da área de trabalho consiste em uma imagem ou outro conteúdo com custos significativos para o desenho.

-   **Cache de bitmap** (Bitmapcachepersistenable: i: 1) quando essa configuração é habilitada, ela cria um cache do lado do cliente de bitmaps que são processados na sessão. Ele fornece uma melhoria significativa no uso da largura de banda e deve ser sempre habilitado (a menos que haja outras considerações de segurança).

-   **Mostrar o conteúdo do Windows ao arrastar** (desabilitar a janela inteira arrastar: i: 1) quando essa configuração for desabilitada, ela reduzirá a largura de banda exibindo apenas o quadro da janela em vez de todo o conteúdo quando a janela for arrastada.

-   **Animação de menu e janela** (desabilite o menu anims: i: 1 e desabilite a configuração do cursor: i: 1): quando essas configurações são desabilitadas, ela reduz a largura de banda ao desabilitar a animação em menus (como esmaecimento) e cursores.

-   **Suavização de fontes** (permitir suavização de fontes: i: 0) controla o suporte à renderização de fonte ClearType. Ao se conectar a computadores que executam o Windows 8 ou o Windows Server 2012 e posterior, a habilitação ou desabilitação dessa configuração não tem um impacto significativo no uso da largura de banda. No entanto, para computadores que executam versões anteriores ao Windows 7 e Windows 2008 R2, a habilitação dessa configuração afeta significativamente o consumo de largura de banda da rede.

As configurações a seguir se aplicam somente ao se conectar a computadores que executam o Windows 7 e versões anteriores do sistema operacional:

-   **Composição de área de trabalho** Essa configuração tem suporte apenas para uma sessão remota em um computador que executa o Windows 7 ou o Windows Server 2008 R2.

-   **Estilos visuais** (desabilitar temas: i: 1) quando essa configuração é desabilitada, ela reduz a largura de banda simplificando desenhos de tema que usam o tema clássico.

Usando a guia **experiência** em conexão de área de trabalho remota, você pode escolher a velocidade de conexão para influenciar o desempenho da largura de banda da rede. O seguinte lista as opções disponíveis para configurar a velocidade de conexão:

-   **Detectar a qualidade da conexão automaticamente** (tipo de conexão: i: 7) quando essa configuração é habilitada, conexão de área de trabalho remota escolhe automaticamente as configurações que resultarão na melhor experiência do usuário com base na qualidade da conexão. (Essa configuração é recomendada ao se conectar a computadores que executam o Windows 8 ou o Windows Server 2012 e posterior).

-   **Modem (56 Kbps)** (tipo de conexão: i: 1) essa configuração habilita o Caching de bitmap persistente.

-   **Banda larga de baixa velocidade (256 kbps-2 Mbps)** (tipo de conexão: i: 2) essa configuração habilita o cache de bitmap persistente e estilos visuais.

-   **Celular/satélite (2 Mbps-16 Mbps com alta latência)** (tipo de conexão: i: 3) essa configuração habilita a composição da área de trabalho, o cache de bitmap persistente, os estilos visuais e o plano de fundo da área de trabalho.

-   **Banda larga de alta velocidade (2 Mbps – 10 Mbps)** (tipo de conexão: i: 4) essa configuração habilita a composição de área de trabalho, mostra o conteúdo do Windows ao arrastar, animação de menu e janela, cache de bitmap persistente, estilos visuais e plano de fundo da área de trabalho.

-   **WAN (10 Mbps ou superior com alta latência)** (tipo de conexão: i: 5) essa configuração habilita a composição de área de trabalho, mostra o conteúdo do Windows ao arrastar, animação de menu e janela, cache de bitmap persistente, estilos visuais e plano de fundo da área de trabalho.

-   **LAN (10 Mbps ou superior)** (tipo de conexão: i: 6) essa configuração habilita a composição de área de trabalho, mostra o conteúdo do Windows ao arrastar, animação de menu e janela, cache de bitmap persistente, temas e plano de fundo da área de trabalho.

### <a name="desktop-size"></a>Tamanho da área de trabalho

O tamanho da área de trabalho para sessões remotas pode ser controlado usando a guia Exibir em Conexão de Área de Trabalho Remota ou usando o arquivo de configuração de RDP (desktopwidth: i: 1152 e desktopheight: i: 864). Quanto maior o tamanho da área de trabalho, maior será o consumo de memória e largura de banda associado a essa sessão. O tamanho máximo atual da área de trabalho é 4096 x 2048.