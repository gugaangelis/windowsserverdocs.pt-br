---
title: Hosts de sessão da área de trabalho remota de ajuste de desempenho
description: Ajuste de desempenho diretrizes para Hosts de sessão de área de trabalho remota
ms.prod: windows-server-threshold
ms.technology: performance-tuning-guide
ms.topic: article
ms.author: HammadBu; VladmiS
author: phstee
ms.date: 10/16/2017
ms.openlocfilehash: e45d1abb545ad46e654c811a0347c589bd12adf0
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59863237"
---
# <a name="performance-tuning-remote-desktop-session-hosts"></a>Hosts de sessão da área de trabalho remota de ajuste de desempenho


Este tópico discute como selecionar o hardware do Host da sessão da área de trabalho remota (Host de sessão de área de trabalho remota), ajustar o host e ajustar os aplicativos.

**Neste tópico:**

-   [Selecionando o hardware adequado para desempenho](#hw)

-   [Ajustando aplicativos para o Host de sessão de área de trabalho remota](#apps)

-   [Host da sessão da área de trabalho remota parâmetros de ajuste](#host)

## <a href="" id="hw"></a>Selecionando o hardware adequado para desempenho


Para uma implantação de servidor de Host de sessão de área de trabalho remota, a escolha de hardware será regida pelo conjunto de aplicativos e como os usuários usá-los. Os principais fatores que afetam o número de usuários e a experiência deles são CPU, memória, disco e gráficos. Esta seção contém diretrizes adicionais que são específicas para os servidores de Host de sessão de área de trabalho remota e está relacionada principalmente ao ambiente de vários usuário de servidores de Host de sessão de área de trabalho remota.

### <a name="cpu-configuration"></a>Configuração da CPU

Configuração da CPU é conceitualmente determinada multiplicando-se a CPU necessária para dar suporte a uma sessão pelo número de sessões que o sistema deve dar suporte, mantendo uma zona de buffer para lidar com picos temporários. Vários processadores lógicos podem ajudar a reduzir situações anormais de congestionamento da CPU, que geralmente são causadas por alguns segmentos overactive contidos por um número semelhante de processadores lógicos.

Portanto, os processadores lógicos mais em um sistema, quanto menor a margem Amortecedor deve ser criada para a estimativa de uso da CPU, o que resulta em uma porcentagem maior de carga ativa por CPU. Um fator importante a lembrar é que se duplicar o número de CPUs faz a capacidade de CPU não double.

### <a name="memory-configuration"></a>Configuração de memória

Configuração de memória é dependente de aplicativos que os usuários empreguem; No entanto, a quantidade necessária de memória pode ser estimada usando a fórmula a seguir: TotalMem = OSMem + SessionMem \* NS

OSMem é a quantidade de memória que do sistema operacional precisa para ser executado (como imagens binárias do sistema, estruturas de dados e assim por diante), SessionMem é quanto os processos em execução em uma sessão da memória exigem e NS é o número de sessões ativas de destino. A quantidade de memória necessária para uma sessão é determinada principalmente pela referência de memória privada definido para aplicativos e processos de sistema que estão em execução na sessão. Páginas de código ou dados compartilhadas tem pouco efeito, porque apenas uma cópia está presente no sistema.

Uma observação interessante (supondo que o sistema de disco que está fazendo backup de arquivo de paginação não altera) é que, quanto maior o número de sessões simultâneas ativas, o sistema de planos de suporte, quanto maior a alocação de memória por sessão deve ser. Se a quantidade de memória é alocada por sessão não for aumentada, o número de falhas de página que sessões ativas gera aumenta com o número de sessões. Essas falhas, eventualmente, saturar o subsistema de e/s. Aumentando a quantidade de memória é alocada por sessão, a probabilidade de incorrer em falhas de página diminui, que ajuda a reduzir a taxa geral de falhas de página.

### <a name="disk-configuration"></a>Configuração de disco

Armazenamento é um dos aspectos mais ignorados quando você configura servidores Host da sessão de área de trabalho remota, e pode ser a limitação mais comuns em sistemas que são implantados no campo.

A atividade de disco que é gerada em um servidor de Host de sessão de área de trabalho remota típica afeta as seguintes áreas:

-   Arquivos do sistema e os binários do aplicativo

-   Arquivos de página

-   Perfis de usuário e dados de usuário

O ideal é que essas áreas devem ser feitas por dispositivos de armazenamento distinto. Usar as configurações de RAID distribuídas ou outros tipos de armazenamento de alto desempenho ainda mais melhora o desempenho. É altamente recomendável que você use adaptadores de armazenamento com o cache de gravação de bateria. Controladores com o cache de gravação de disco oferecem suporte aprimorado para as operações de gravação síncrona. Como todos os usuários têm uma seção separada, operações de gravação síncrona são significativamente mais comuns em um servidor de Host de sessão de área de trabalho remota. Hives do registro periodicamente são salvos em disco por meio de operações de gravação síncrona. Para habilitar essas otimizações, no console do gerenciamento de disco, abra o **propriedades** caixa de diálogo para o disco de destino e, na **diretivas** guia, selecione o **Ativar gravação em cache em o disco** e **desativar a liberação de buffer de cache de gravação do Windows** nas caixas de seleção de dispositivo.

### <a name="network-configuration"></a>Configuração de rede

Uso de rede para um servidor de Host de sessão de área de trabalho remota inclui duas categorias principais:

-   Uso do tráfego de conexão de Host de sessão de área de trabalho remota é determinado quase que exclusivamente pelos padrões de desenho que são exibidos pelos aplicativos em execução dentro de sessões e o tráfego de e/s redirecionada de dispositivos.

    Por exemplo, aplicativos de tratamento de processamento de texto e dados de entrada consumam largura de banda de aproximadamente 10 a 100 kilobits por segundo, enquanto que gráficos sofisticados e reprodução de vídeo causar aumentos significativos no uso de largura de banda.

-   Conexões de back-end, como perfis móveis, aplicativo acesso aos compartilhamentos de arquivos, servidores de banco de dados, servidores de email e servidores HTTP.

    O volume e o perfil de tráfego de rede é específico para cada implantação.

## <a href="" id="apps"></a>Ajustando aplicativos para o Host de sessão de área de trabalho remota


A maioria do uso da CPU em um servidor Host de sessão de área de trabalho remota é orientada por aplicativos. Aplicativos da área de trabalho normalmente são otimizados em direção a capacidade de resposta com o objetivo de minimizar o tempo que levará a um aplicativo para responder a uma solicitação de usuário. No entanto, em um ambiente de servidor, é igualmente importante minimizar a quantidade total de uso da CPU que é necessária para concluir uma ação para evitar afetar negativamente outras sessões.

Quando você configura os aplicativos que devem ser usados em um servidor Host de sessão de área de trabalho remota, considere as seguintes sugestões:

-   Minimizar o processamento de loop ocioso em segundo plano

    Exemplos típicos são desabilitar a verificação de ortografia e gramática em segundo plano, dados de indexação para pesquisa, e gravações em segundo plano.

-   Minimize a frequência com que um aplicativo executa uma verificação de estado ou a atualização.

    Desabilitar esses comportamentos ou aumentar o intervalo entre iterações de sondagem e o timer de acionamento significativamente beneficia o uso da CPU porque o efeito de tais atividades é amplificado rapidamente para muitas sessões ativas. Exemplos típicos são ícones de status de conexão e atualizações de informações da barra de status.

-   Minimize a contenção de recursos entre os aplicativos, reduzindo sua frequência de sincronização.

    Exemplos de tais recursos incluem as chaves do registro e arquivos de configuração. Exemplos de componentes de aplicativos e recursos são um indicador de status (como notificações de shell), operações de indexação ou monitoramento de alterações e sincronização offline.

-   Desabilite os processos desnecessários que estão registrados para começar com entrada do usuário ou uma inicialização de sessão.

    Esses processos podem contribuir significativamente o custo de uso da CPU ao criar uma nova sessão de usuário, que geralmente é um processo intensivo de CPU, e pode ser muito caro em cenários de manhã. Use MsConfig.exe ou o MsInfo32.exe para obter uma lista de processos que são iniciados na entrada do usuário. Para obter mais informações, você pode usar [Autoruns para Windows](https://technet.microsoft.com/sysinternals/bb963902.aspx).

Para o consumo de memória, você deve considerar o seguinte:

-   Verifique se que as DLLs carregadas por um aplicativo não são realocados.

    -   DLLs realocados podem ser verificados selecionando o modo de exibição do processo de DLL, conforme mostrado na figura a seguir, usando [Process Explorer](https://technet.microsoft.com/sysinternals/bb896653.aspx).

    -   Aqui podemos ver que y.dll foi realocado porque x.dll já ocupado seu endereço de base padrão e não foi habilitado o ASLR

        ![dlls realocados](../../media/perftune-guide-relocated-dlls.png)

        Se as DLLs são realocadas, é impossível compartilhar código entre as sessões, que aumenta significativamente o volume de uma sessão. Isso é um dos problemas mais comuns de desempenho relacionados à memória em um servidor Host de sessão de área de trabalho remota.

-   Para aplicativos comuns de runtime (CLR) do idioma, use o gerador de imagem nativa (Ngen.exe) para aumentar o compartilhamento de página e reduzir a sobrecarga da CPU.

    Quando possível, aplica técnicas semelhantes a outros mecanismos de execução semelhantes.

## <a href="" id="host"></a>Host da sessão da área de trabalho remota parâmetros de ajuste


### <a name="page-file"></a>Arquivo de paginação

Tamanho do arquivo de página insuficiente pode causar memória falhas de alocação em aplicativos ou componentes do sistema. Você pode usar o contador de desempenho de bytes de memória confirmada de monitorar quanta memória virtual confirmada no sistema.

### <a name="antivirus"></a>Antivírus

Instalando o software antivírus em um servidor de Host de sessão de área de trabalho remota bastante afeta o desempenho geral do sistema, especialmente o uso da CPU. É altamente recomendável que você excluir da lista ativa monitoramento todas as pastas que contêm arquivos temporários, especialmente aquelas que serviços e outros componentes do sistema geram.

### <a name="task-scheduler"></a>Agendador de Tarefas

O Agendador de tarefas permite que você examine a lista de tarefas que estão agendadas para diferentes eventos. Para um servidor de Host de sessão de área de trabalho remota, é útil se concentrar especificamente nas tarefas que são configurados para executar em ocioso, na entrada do usuário ou sessão se conectar e desconectar. Por causa das especificidades da implantação, muitas dessas tarefas podem ser desnecessárias.

### <a name="desktop-notification-icons"></a>Ícones de notificação da área de trabalho

Ícones de notificação na área de trabalho podem ter mecanismos de atualização relativamente caros. Você deve desabilitar as notificações, removendo o componente que registra-os da lista de inicialização ou alterando a configuração em aplicativos e componentes do sistema para desabilitá-los. Você pode usar **personalizar ícones de notificações** para examinar a lista de notificações que estão disponíveis no servidor.

### <a name="remotefx-data-compression"></a>Compactação de dados do RemoteFX

Microsoft RemoteFX compactação pode ser configurada por meio da política de grupo sob **configuração do computador &gt; modelos administrativos &gt; componentes do Windows &gt; Remote Desktop Services &gt; remoto Host de sessão da área de trabalho &gt; ambiente de sessões remotas &gt; configurar a compactação de dados do RemoteFX**. Três valores são possíveis:

-   **Otimizado para usar menos memória** consome a menor quantidade de memória por sessão, mas tem a menor taxa de compactação e, portanto, o mais alto consumo de largura de banda.

-   **Equilibra a largura de banda de rede e memória** reduzido o consumo de largura de banda enquanto aumenta um pouco o consumo de memória (aproximadamente 200 KB por sessão).

-   **Otimizado para usar menos largura de banda de rede** reduz ainda mais o uso de largura de banda de rede a um custo de aproximadamente 2 MB por sessão. Se você quiser usar essa configuração, você deve avaliar o número máximo de sessões e testar a esse nível com essa configuração antes de colocar o servidor em produção.

Você também pode optar por não usar um algoritmo de compactação do RemoteFX. Escolher não usar um algoritmo de compactação do RemoteFX irá usar mais largura de banda de rede e só é recomendado se você estiver usando um dispositivo de hardware que foi projetado para otimizar o tráfego de rede. Mesmo se você optar por não usar um algoritmo de compactação do RemoteFX, alguns dados de gráficos serão compactados.

### <a name="device-redirection"></a>Redirecionamento de dispositivo

Redirecionamento de dispositivo pode ser configurado usando diretiva de grupo sob **configuração do computador &gt; modelos administrativos &gt; componentes do Windows &gt; Remote Desktop Services &gt; área de trabalho remota Host de sessão &gt; dispositivo e o redirecionamento de recursos** ou usando o **coleção de sessão** no Gerenciador do servidor de caixa de propriedades.

Em geral, o redirecionamento de dispositivo aumenta quanto as conexões de servidor de Host de sessão de área de trabalho remota de largura de banda de rede usam porque os dados são trocados entre os dispositivos nos computadores cliente e os processos em execução na sessão de servidor. A extensão do aumento é uma função da frequência de operações que são executadas pelos aplicativos que estão em execução no servidor em relação os dispositivos redirecionados.

Redirecionamento de impressora e Plug and Play redirecionamento de dispositivo também aumenta o uso de CPU ao entrar. Você pode redirecionar impressoras de duas maneiras:

-   Correspondência com base no driver redirecionamento de impressora quando um driver de impressora deve ser instalado no servidor. Versões anteriores do Windows Server usam neste método.

-   Introduzido no Windows Server 2008, o redirecionamento de driver de impressora Easy Print usa um driver de impressora comum para todas as impressoras.

É recomendável o método Easy Print porque ele faz com que menos uso de CPU para a instalação de impressora em tempo de conexão. O método de driver correspondente faz com que o aumento no uso da CPU porque requer que o serviço de spooler para carregar drivers diferentes. Para o uso de largura de banda, Easy Print faz com que o uso de largura de banda de rede um pouco maior, mas não é significativa o suficiente para compensar os benefícios de desempenho, gerenciabilidade e confiabilidade.

Redirecionamento de áudio faz com que um fluxo de tráfego de rede. Redirecionamento de áudio também permite que os usuários executem aplicativos de multimídia que normalmente têm alto consumo de CPU.

### <a name="client-experience-settings"></a>Configurações de experiência do cliente

Por padrão, a Conexão de área de trabalho remota (RDC) escolhe automaticamente o direito de experiência de configuração com base na adequação da conexão de rede entre os computadores cliente e servidor. É recomendável que a configuração de RDC permaneça com **detectar a qualidade da conexão automaticamente**.

Para usuários avançados, o RDC fornece controle sobre uma variedade de configurações que influenciam o desempenho de largura de banda de rede para a conexão dos serviços de área de trabalho remota. Você pode acessar as configurações a seguir usando o **experiência** guia Conexão de área de trabalho remota ou como as configurações no arquivo RDP.

As configurações a seguir se aplicam ao se conectar a qualquer computador:

-   **Desabilitar o papel de parede** (desabilitar papel de parede: i:0#.w|Contoso) não mostra o papel de parede da área de trabalho em conexões redirecionadas. Essa configuração pode reduzir significativamente o uso de largura de banda se o papel de parede da área de trabalho consiste em uma imagem ou outros tipos de conteúdo com custos significativos para o desenho.

-   **Cache de bitmap** (Bitmapcachepersistenable:i:1) quando essa configuração estiver habilitada, ele cria um cache do lado do cliente de bitmaps que são processados na sessão. Ele fornece uma melhoria significativa no uso de largura de banda, e ele sempre deve ser habilitado (a menos que haja outras considerações de segurança).

-   **Mostrar conteúdo do windows ao arrastar** (desabilitar a janela inteira arrastar: i:1) quando essa configuração estiver desabilitada, ela reduz a largura de banda, exibindo somente o quadro de janela, em vez de todo o conteúdo quando a janela é arrastada.

-   **Animação de menus e janelas** (Disable menu anims:i:1 e Disable cursor configuração: i:1): Quando essas configurações são desabilitadas, ele reduz a largura de banda, desabilitando a animação em menus (por exemplo, o esmaecimento) e cursores.

-   **Suavização de fonte** (Permitir suavização de fonte: i:0#.w|Contoso) suporte a renderização de fonte ClearType de controles. Ao se conectar a computadores que executam o Windows 8 ou Windows Server 2012 e superior, habilitar ou desabilitar essa configuração não tem um impacto significativo sobre o uso de largura de banda. No entanto, para computadores que executam versões anteriores ao Windows 7 e Windows 2008 R2, habilitar essa configuração afeta o consumo de largura de banda de rede significativamente.

As configurações a seguir se aplicam somente ao se conectar a computadores que executam o Windows 7 e versões anteriores do sistema operacional:

-   **Composição da área de trabalho** essa configuração tem suporte apenas para uma sessão remota para um computador que executa o Windows 7 ou Windows Server 2008 R2.

-   **Estilos visuais** (Desabilitar temas: i:1) quando essa configuração estiver desabilitada, ela reduz a largura de banda, simplificando desenhos de tema que usam o tema clássico.

Usando o **experiência** guia dentro de Conexão de área de trabalho remota, você pode escolher sua velocidade de conexão de influenciar o desempenho de largura de banda de rede. O exemplo a seguir lista as opções que estão disponíveis para configurar sua velocidade de conexão:

-   **Detectar a qualidade da conexão automaticamente** (tipo de Conexão: i:7) quando essa configuração estiver habilitada, a Conexão de área de trabalho remota escolhe automaticamente configurações resultarão na experiência de usuário ideal com base na qualidade da conexão. (Essa configuração é recomendada ao se conectar a computadores que executam o Windows 8 ou Windows Server 2012 e superior).

-   **Modem (56 Kbps)** (tipo de Conexão: i:1) essa configuração permite que o bitmap persistente em cache.

-   **Baixa velocidade banda larga (256 Kbps - 2 Mbps)** (tipo de Conexão: i:2) essa configuração permite que os estilos de cache e o visual de bitmap persistente.

-   **Celular/satélite (2 Mbps - 16 Mbps com alta latência)** (tipo de Conexão: i:3) essa configuração permite que a composição da área de trabalho, bitmap persistente em cache, os estilos visuais e plano de fundo da área de trabalho.

-   **Banda larga a alta velocidade (2 Mbps – 10 Mbps)** (tipo de Conexão: i:4) essa configuração permite que a composição da área de trabalho, Mostrar conteúdo do windows ao arrastar, animação de menus e janelas, bitmap persistente em cache, os estilos visuais e plano de fundo da área de trabalho.

-   **Rede de longa distância (10 Mbps ou superior com alta latência)** (tipo de Conexão: i:5) essa configuração permite que a composição da área de trabalho, Mostrar conteúdo do windows ao arrastar, animação de menus e janelas, bitmap persistente em cache, os estilos visuais e plano de fundo da área de trabalho.

-   **LAN (10 Mbps ou superior)** (tipo de Conexão: i:6) essa configuração permite que a composição da área de trabalho, Mostrar conteúdo do windows ao arrastar, animação de menus e janelas, bitmap persistente em cache, temas e plano de fundo da área de trabalho.

### <a name="desktop-size"></a>Tamanho da área de trabalho

Tamanho da área de trabalho para sessões remotas pode ser controlado usando a guia Exibir na Conexão de área de trabalho remota ou usando o arquivo de configuração de RDP (desktopwidth:i:1152 e desktopheight:i:864). Quanto maior o tamanho da área de trabalho, quanto maior o consumo de memória e largura de banda que está associado essa sessão. O tamanho máximo de área de trabalho atual é de 4096 x 2048.
