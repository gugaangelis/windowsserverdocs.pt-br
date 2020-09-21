---
title: Planejar sua implantação do WSUS
description: O tópico WUS (Windows Server Update Service) – uma visão geral do processo de planejamento da implantação com links para os tópicos relacionados
ms.topic: article
ms.assetid: 35865398-b011-447a-b781-1c52bc0c9e3a
ms.author: lizross
author: eross-msft
manager: mtillman
ms.date: 05/24/2018
ms.openlocfilehash: 1d3638b7a05c406293035c7f0a0e8854ed2ecee9
ms.sourcegitcommit: 5344adcf9c0462561a4f9d47d80afc1d095a5b13
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/18/2020
ms.locfileid: "90766799"
---
# <a name="plan-your-wsus-deployment"></a>Planejar sua implantação do WSUS

>Aplica-se a: Windows Server 2019, Windows Server (Canal Semestral), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

A primeira etapa na implantação do WSUS (Windows Server Update Services) é tomar decisões importantes, por exemplo decidir o cenário de implantação do WSUS, escolher uma topologia de rede e entender os requisitos do sistema. A lista de verificação a seguir resume as etapas envolvidas nos preparativos para sua implantação.

|Tarefa|Descrição|
|----|--------|
|[1.1. Examinar as considerações e os requisitos do sistema](#11-review-considerations-and-system-requirements)|Examine a lista de considerações e requisitos do sistema para garantir que você tenha todos os componentes de hardware e software necessários para implantar o WSUS.|
|[1.2. Escolher o cenário de implantação do WSUS](#12-choose-a-wsus-deployment-scenario)|Decida qual cenário de implantação do WSUS será usado.|
|[1.3. Escolher uma estratégia de armazenamento do WSUS](#13-choose-a-wsus-storage-strategy)|Decida qual estratégia de armazenamento do WSUS é mais adequada à sua implantação.|
|[1.4. Escolher os idiomas de atualização do WSUS](#14-choose-wsus-update-languages)|Decida quais idiomas de atualização do WSUS serão instalados.|
|[1.5. Planejar grupos de computadores do WSUS](#15-plan-wsus-computer-groups)|Planeje a abordagem de grupo de computadores do WSUS que será usada para sua implantação.|
|[1.6. Planejar considerações de desempenho do WSUS: Serviço de Transferência Inteligente em Segundo Plano](#16-plan-wsus-performance-considerations)|Planeje um design do WSUS para garantir desempenho otimizado.|
|[1.7. Planejar definições das atualizações automáticas](#17-plan-automatic-updates-settings)|Planeje como você configurará as definições de atualizações automáticas para o seu cenário.|

## <a name="11-review-considerations-and-system-requirements"></a>1,1. Examinar as considerações iniciais e os requisitos do sistema

### <a name="system-requirements"></a>Requisitos do Sistema

Os requisitos de hardware e de software de banco de dados são orientados pelo número de computadores clientes que estão sendo atualizados em sua organização.  Para habilitar a função de servidor do WSUS, confirme se o servidor atende aos requisitos do sistema e se você tem as permissões necessárias para concluir a instalação seguindo estas diretrizes:

-   Os requisitos de hardware do servidor para habilitar a função do WSUS são associados aos requisitos de hardware. Os requisitos mínimos de hardware do WSUS são:

    -   **Processador:** processador x64 de 1,4 gigahertz (GHz) (o recomendado é 2 GHz ou mais)

    -   **Memória:** o WSUS requer um adicional de 2 GB de RAM do que é exigido pelo servidor e todos os outros serviços ou software.

    -   **Espaço em disco disponível:** 40 GB ou mais é recomendável

    -   **Adaptador de rede:** 100 Mbps (megabits por segundo) ou mais (1 GB é recomendável)

> [!NOTE]
> Essas diretrizes pressupõem que os clientes do WSUS estejam sincronizando com o servidor a cada oito horas para um rollup de 30.000 clientes. Se eles sincronizarem com mais frequência, haverá um incremento correspondente na carga do servidor.

-   Requisitos de software:

    -   Para exibir relatórios, o WSUS requer o [Microsoft Report Viewer Redistributable 2008](https://www.microsoft.com/download/details.aspx?id=6576). No Windows Server 2016, o WSUS requer [Microsoft Report Viewer Runtime 2012](https://www.microsoft.com/download/details.aspx?id=35747)

-   Se você instalar funções ou atualizações de software que exigem a reinicialização do servidor quando a instalação for concluída, reinicie o servidor antes de habilitar a função de servidor do WSUS.

-   O Microsoft .NET Framework 4.0 precisa estar instalado no servidor em que a função de servidor do WSUS será instalada.

-   A conta Autoridade NT\Serviço de Rede precisa ter permissões de Controle Total para as seguintes pastas para que o snap-in Administração do WSUS seja exibido corretamente:

    -   %windir%\Microsoft.NET\Framework\v4.0.30319\Temporary ASP.NET Files

        > [!NOTE]
        > Esse caminho pode não existir antes da instalação da função Servidor Web que contém o IIS (Serviços de Informações da Internet).

    -   %windir%\Temp

-   Confirme se a conta que você pretende usar para instalar o WSUS é membro do grupo Administradores Locais.

### <a name="installation-considerations"></a>Considerações de instalação

Durante o processo de instalação, o WSUS instalará estes componentes:

-   Cmdlets de .NET API e do Windows PowerShell

-   WID (Banco de Dados Interno do Windows), que é usado pelo WSUS

-   Serviços usados pelo WSUS, a saber:

    -   Serviços de Atualização

    -   Serviço de Relatório na Web

    -   Serviço Web do Cliente

    -   Serviço Web de Autenticação da Web Simples

    -   Serviço de Sincronização do Servidor

    -   Serviço Web de Autenticação de DSS

### <a name="features-on-demand-considerations"></a>Considerações sobre os Recursos sob Demanda

Esteja ciente de que configurar computadores cliente (incluindo servidores) para atualizar usando o WSUS resultará nas seguintes limitações:

1. Funções de servidor que tiveram suas cargas removidas usando os Recursos sob Demanda não podem ser instaladas sob demanda pelo Microsoft Update. Você deve fornecer uma origem de instalação no momento em que tentar instalar tais funções de servidor ou configurar uma origem para os Recursos sob Demanda na Política de Grupo.

2. As edições do cliente Windows não poderão instalar o .NET 3.5 sob demanda por meio da Web. Aplicam-se ao .NET 3.5 as mesmas considerações que para as funções de servidor.

   > [!NOTE]
   > Configurar uma origem de instalação dos Recursos sob Demanda não envolve WSUS. Para obter informações sobre como configurar os Recursos, consulte [Configurar Recursos sob Demanda no Windows Server](/previous-versions/windows/it-pro/windows-server-2012-R2-and-2012/jj127275(v=ws.11)).

3. Os dispositivos corporativos que executam o Windows 10, versão 1709 ou 1803, não podem instalar nenhum Recurso sob Demanda diretamente do WSUS. Para instalar Recursos sob Demanda, [crie um arquivo de recurso (repositório lado a lado)](/previous-versions/windows/it-pro/windows-server-2012-R2-and-2012/jj127275%28v=ws.11%29#create-a-feature-file-or-side-by-side-store) ou obtenha o pacote do Recurso sob Demanda de uma das seguintes fontes:
   - [VLSC](https://www.microsoft.com/licensing/servicecenter) (Centro de Atendimento de Licenciamento por Volume) – o acesso de VL é necessário
   - Portal OEM – o acesso OEM é necessário
   - Download do MSDN – assinatura do MSDN é obrigatória

     Os pacotes de Recurso sob Demanda obtidos individualmente podem ser instalados usando [opções de linha de comando do DISM](/windows-hardware/manufacture/desktop/dism-operating-system-package-servicing-command-line-options).

### <a name="wsus-database-requirements"></a>Requisitos de banco de dados do WSUS
O WSUS requer um dos seguintes bancos de dados:

-   WID (Banco de Dados Interno do Windows)

-   Qualquer versão do Microsoft SQL Server com suporte. Para mais informações, confira [Política de Clico de Vida da Microsoft](/lifecycle/products/?products=sql-server).

As edições a seguir do SQL Server têm suporte pelo WSUS:

-   Standard

-   Enterprise

-   Express

> [!NOTE]
> O SQL Server Express 2008 R2 tem uma limitação de tamanho máximo do banco de dados de 10 GB. Esse tamanho de banco de dados provavelmente é suficiente para o WSUS, apesar de não haver nenhum benefício apreciável em usá-lo no lugar do WID. O banco de dados WID tem um requisito de memória RAM mínimo de 2 GB, além dos requisitos de sistema padrão do Windows Server.

Você pode instalar a função do WSUS em um computador separado do computador servidor de banco de dados. Nesse caso, os seguintes critérios adicionais se aplicam:

1.  O servidor de banco de dados não pode ser configurado como controlador de domínio.

2.  O servidor do WSUS não pode executar os Serviços de Área de Trabalho Remota.

3.  O servidor de banco de dados precisa estar no mesmo domínio do diretório ativo do servidor do WSUS ou então ter uma relação de confiança com o domínio do diretório ativo do servidor do WSUS.

4.  O servidor do WSUS e o servidor de banco de dados precisam estar no mesmo fuso horário ou sincronizados com a mesma fonte de Tempo Universal Coordenado (Hora de Greenwich).

## <a name="12-choose-a-wsus-deployment-scenario"></a>1.2. Escolher o cenário de implantação do WSUS
Esta seção descreve os recursos básicos de todas as implantações do WSUS. Use esta seção para se familiarizar com uma implantação simples com um único servidor do WSUS, além de cenários mais complexos, como uma hierarquia de servidores do WSUS ou um servidor do WSUS em um segmento de rede isolado.

### <a name="simple-wsus-deployment"></a>Implantação simples do WSUS
A implantação mais básica do WSUS consiste em um servidor dentro do firewall corporativo que serve computadores cliente em uma intranet privada. O servidor do WSUS se conecta ao Microsoft Update para baixar atualizações. Isso é conhecido como *sincronização*. Durante a sincronização, o WSUS determina se alguma atualização nova foi disponibilizada desde a última sincronização. Se for a sua primeira sincronização do WSUS, todas as atualizações serão disponibilizadas para download.

> [!NOTE]
> A sincronização inicial pode levar uma hora. Todas as sincronizações posteriores devem ser significativamente mais rápidas.

Por padrão, o servidor do WSUS usa a porta 80 para o protocolo HTTP e a porta 443 para o protocolo HTTPS para obter atualizações da Microsoft. Se houver um firewall corporativo entre a rede e a Internet, você precisará abrir essas portas no servidor que se comunica diretamente com o Microsoft Update. Entretanto, se você pretende usar portas personalizadas para essa comunicação, você deverá abrir estas últimas. É possível configurar vários servidores do WSUS para que sejam sincronizados com um servidor pai do WSUS. Por padrão, o servidor do WSUS usa a porta 8530 para o protocolo HTTP e a porta 8531 para o protocolo HTTPS para fornecer atualizações para as estações de trabalho cliente.

### <a name="multiple-wsus-servers"></a>Vários servidores do WSUS
Os administradores podem implantar vários servidores que executam WSUS e sincronizam todo o conteúdo na intranet de sua organização. Você pode expor apenas um servidor à Internet, que seria o único servidor que baixa as atualizações do Microsoft Update. Esse servidor está configurado como servidor upstream: a fonte com a qual os servidores downstream são sincronizados. Quando aplicável, os servidores podem estar localizados em uma rede geograficamente dispersa para oferecer a melhor conectividade a todos os computadores clientes.

### <a name="disconnected-wsus-server"></a>Servidor WSUS desconectado
Se a política corporativa ou outras condições limitarem o acesso dos computadores à Internet, os administradores poderão configurar um servidor interno para executar o WSUS. Um exemplo disso é um servidor conectado à intranet porém isolado da Internet. Depois de baixar, testar e aprovar as atualizações nesse servidor, um administrador exportaria os metadados e o conteúdo das atualizações para um DVD. Os metadados e o conteúdo das atualizações são importados do DVD para os servidores que executam o WSUS na intranet.

### <a name="wsus-server-hierarchies"></a>Hierarquias de servidores do WSUS
Você pode criar complexas hierarquias dos servidores do WSUS. Como é possível sincronizar um servidor do WSUS com outro servidor do WSUS, e não com o Microsoft Update, é necessário ter um único servidor do WSUS conectado ao Microsoft Update. Ao conectar os servidores do WSUS, há um servidor upstream do WSUS e um servidor downstream do WSUS. Uma implantação de hierarquia de servidores do WSUS oferece os seguintes benefícios:

-   Baixar atualizações uma vez da Internet e distribuí-las aos computadores clientes usando servidores downstream. Esse método economiza largura de banda na conexão corporativa à Internet.

-   Baixar atualizações para um servidor do WSUS que esteja fisicamente mais próximo aos computadores clientes, por exemplo, nas filiais da empresa.

-   Configurar servidores do WSUS separados para servir computadores clientes que usam diferentes idiomas dos produtos da Microsoft.

-   Dimensionar o WSUS para uma organização de grande porte que tem mais computadores clientes do que um único servidor do WSUS pode gerenciar com eficácia.

> [!NOTE]
> É recomendável não criar uma hierarquia de servidores do WSUS com mais de três níveis de profundidade. Cada nível aumenta o tempo para propagar as atualizações nos servidores conectados. Apesar de não haver um limite teórico para uma hierarquia, somente implantações que têm uma hierarquia de cinco níveis de profundidade foram testadas pela Microsoft.
>
> Além disso, os servidores downstream devem estar na mesma versão ou em uma versão anterior do WSUS em relação à origem de sincronização do servidor upstream.

Você pode conectar servidores do WSUS em modo Autônomo (para permitir administração distribuída) ou em modo de Réplica (para permitir administração centralizada). Não é necessário implantar uma hierarquia de servidores que usa um único modo: você pode implantar uma solução do WSUS que use tanto servidores do WSUS autônomos quanto servidores de réplica.

#### <a name="autonomous-mode"></a>Modo autônomo
O modo Autônomo, também chamado administração distribuída, é a opção de instalação padrão para o WSUS. No modo Autônomo, um servidor upstream do WSUS compartilha atualizações com os servidores downstream durante a sincronização. Os servidores downstream do WSUS são administrados separadamente e não recebem status de aprovação de atualização ou informações de grupos de computadores provenientes do servidor upstream. Usando o modelo de gerenciamento distribuído, cada administrador do servidor do WSUS escolhe os idiomas das atualizações, cria grupos de computadores, atribui computadores aos grupos, testa e aprova atualizações e verifica se as atualizações corretas estão instaladas nos grupos de computadores adequados. A imagem a seguir mostra como é possível implantar servidores do WSUS autônomos em um ambiente de filial da empresa:

#### <a name="replica-mode"></a>Modo de réplica
O modo de Réplica, também chamado administração centralizada, consiste em um servidor upstream do WSUS que compartilha atualizações, status de aprovação e grupos de computadores com servidores downstream. Os servidores de réplica herdam as aprovações de atualizações e não são administrados separadamente do servidor upstream do WSUS. A imagem a seguir mostra como é possível implantar servidores do WSUS de réplica em um ambiente de filial da empresa.

> [!NOTE]
> Se você configurar vários servidores de réplica para que se conectem com um único servidor upstream do WSUS, não agende a sincronização para ser executada no mesmo horário em cada servidor de réplica. Essa prática evitará falhas repentinas no uso da largura de banda.

### <a name="branch-offices"></a>Filiais
Você pode aproveitar o recurso Filial no Windows para otimizar a implantação do WSUS. Esse tipo de implantação oferece as seguintes vantagens:

1.  ajuda a reduzir a utilização de links WAN e melhora a capacidade de resposta do aplicativo. Para permitir a aceleração BranchCache do conteúdo servido pelo servidor do WSUS, instale o recurso BranchCache no servidor e nos clientes, e verifique se o serviço BranchCache foi iniciado. Nenhuma outra etapa é necessária.

2.  Em filiais que têm conexões de baixa largura de banda com a matriz porém conexões de alta largura de banda com a Internet, o recurso Branch Office também pode ser usado. Nesse caso, convém configurar os servidores downstream do WSUS para obter informações sobre quais atualizações serão instaladas com o servidor do WSUS central, porém baixar as atualizações com o Microsoft Update.

### <a name="network-load-balancing"></a>Network Load Balancing
O NLB (Balanceamento de Carga de Rede) aumenta a confiabilidade e o desempenho da rede do WSUS. Você pode configurar vários servidores do WSUS que compartilham um único cluster de failover que executa o SQL Server, como o SQL Server 2008 R2 SP1. Nessa configuração, é necessário usar uma instalação completa do SQL Server, e não a instalação do Banco de Dados Interno do Windows fornecida pelo WSUS, e a função de banco de dados precisa estar instalada em todos os servidores front-end do WSUS. Você também pode fazer com que todos os servidores do WSUS usem um DFS (sistema de arquivos distribuídos) para armazenar seu conteúdo.

**Instalação do WSUS para NLB:** em comparação com a instalação do WSUS 3.2 para NLB, não são mais necessários parâmetros e uma chamada de instalação especial para configurar o WSUS para NLB. Você precisa apenas configurar cada servidor do WSUS, mantendo as seguintes considerações em mente.

-   O WSUS deve ser configurado usando a opção de banco de dados SQL em vez de WID.

-   Se armazenar as atualizações localmente, a mesma pasta de conteúdo deve ser compartilhada entre os servidores do WSUS que compartilham o mesmo banco de dados SQL.

-   A instalação do WSUS deve ser feita em série. As tarefas pós-instalação não podem ser executadas em mais de um servidor ao mesmo tempo quando o mesmo banco de dados SQL é compartilhado.

### <a name="wsus-deployment-with-roaming-client-computers"></a>A implantação do WSUS com computadores clientes móveis
Se a rede incluir usuários móveis que fazem logon na rede de locais diferentes, você poderá configurar o WSUS para permitir que os usuários móveis atualizem seus computadores clientes do servidor do WSUS mais próximo a eles em termos geográficos. Por exemplo, você pode implantar um servidor do WSUS em cada região e usar uma sub-rede DNS diferente para cada região. Todos os computadores cliente podem ser direcionados ao mesmo servidor do WSUS, que resolve em cada sub-rede com o servidor do WSUS físico mais próximo.

## <a name="13-choose-a-wsus-storage-strategy"></a>1.3. Escolher a estratégia de armazenamento do WSUS
O WSUS (Windows Server Update Services) usa dois tipos de sistemas de armazenamento: um banco de dados para armazenar a configuração do WSUS e metadados de atualização, e um sistema de arquivos local opcional para armazenar os arquivos de atualização. Para instalar o WSUS, você deve decidir como quer implementar o armazenamento.

As atualizações consistem em duas partes: os metadados que descrevem a atualização; e os arquivos necessários para instalar a atualização. Os metadados de atualização normalmente são bem menores do que a atualização real e são armazenados no banco de dados do WSUS. Os arquivos de atualização são armazenados em um servidor do WSUS local ou em um servidor Web do Microsoft Update.

### <a name="wsus-database"></a>Banco de dados do WSUS
O WSUS requer um banco de dados para cada servidor do WSUS. O WSUS dá suporte ao uso de um banco de dados que reside em um computador diferente do servidor do WSUS, com algumas restrições. Para obter uma lista contendo os bancos de dados compatíveis e as limitações de bancos de dados remotos, confira a seção 1.1 Examinar considerações iniciais e requisitos do sistema neste guia.

O banco de dados do WSUS armazena as seguintes informações:

-   Informações de configuração do servidor do WSUS

-   Metadados que descrevem cada atualização

-   Informações sobre computadores clientes, atualizações e manipulações

Se você instalar vários servidores do WSUS, precisará manter um banco de dados separado para cada servidor do WSUS, seja ele um servidor autônomo ou de réplica. Você não pode armazenar vários bancos de dados do WSUS em uma única instância do SQL Server, a não ser em clusters NLB (Balanceamento de Carga de Rede) que usam failover do SQL Server.

O SQL Server, o SQL Server Express e o Banco de Dados Interno do Windows fornecem as mesmas características de desempenho para uma configuração de um único servidor, em que o banco de dados e o serviço do WSUS estão localizados no mesmo computador. Uma configuração de um único servidor pode dar suporte a vários milhares de computadores clientes do WSUS.

> [!NOTE]
> Não tente gerenciar o WSUS acessando o banco de dados diretamente. A manipulação direta do banco de dados pode fazer com que ele fique corrompido. A corrupção pode não ser imediatamente óbvia, mas pode impedir atualizações para a próxima versão do produto. Você pode gerenciar o WSUS usando o console do WSUS ou APIs (interfaces de programação de aplicativos) do WSUS.

#### <a name="wsus-with-windows-internal-database"></a>WSUS com Banco de Dados Interno do Windows
Por padrão, o assistente de instalação cria e usa um Banco de Dados Interno do Windows chamado SUSDB.mdf. Esse banco de dados está localizado na pasta %windir%\wid\data\, onde %windir% é a unidade local em que o software servidor do WSUS está instalado.

> [!NOTE]
> OWID (Banco de Dados Interno do Windows) foi introduzido no Windows Server 2008.

O WSUS dá suporte à autenticação do Windows somente para o banco de dados. Você não pode usar a autenticação do SQL Server com o WSUS. Se você usar o Banco de Dados Interno do Windows para o banco de dados do WSUS, a Instalação do WSUS criará uma instância do SQL Server chamada server\Microsoft##WID, onde server é o nome do computador. Com qualquer uma das opções de banco de dados, a Instalação do WSUS criará um banco de dados chamado SUSDB. O nome desse banco de dados não é configurável.

É recomendável usar o Banco de Dados Interno do Windows nos seguintes casos:

-   A organização ainda não comprou e não precisa de um produto SQL Server para nenhuma outra aplicação.

-   A organização não precisa de uma solução do WSUS NLB.

-   Você pretende implantar vários servidores do WSUS (por exemplo, nas filiais da empresa). Nesse caso, você deve avaliar o uso do Banco de Dados Interno do Windows nos servidores secundários, mesmo se usar o SQL Server para o servidor do WSUS raiz. Como cada servidor do WSUS exige uma instância separada do SQL Server, você rapidamente terá problemas de desempenho no banco de dados se somente uma instância do SQL Server manipular vários servidores do WSUS.

O Banco de Dados Interno do Windows não fornece uma interface do usuário nem ferramentas de gerenciamento de banco de dados. Se você selecionar esse banco de dados para o WSUS, use ferramentas externas para gerenciar o banco de dados. Para obter mais informações, consulte:

-   [Executar backup e restauração dos dados do WSUS e Executando backup do servidor](/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/dd939904(v=ws.10))

-   [Reindexar o banco de dados do WSUS](/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/dd939795(v=ws.10))

#### <a name="wsus-with-sql-server"></a>WSUS com SQL Server
É recomendável usar o SQL Server no WSUS nos seguintes casos:

1.  Você precisa de uma solução do WSUS NLB.

2.  Você já tem pelo menos uma instância do SQL Server instalada.

3.  Você não pode executar o serviço SQL Server em uma conta local fora do sistema ou usando autenticação do SQL Server. O WSUS dá suporte somente à autenticação do Windows.

### <a name="wsus-update-storage"></a>Armazenamento de atualizações do WSUS
Quando as atualizações são sincronizadas com o servidor do WSUS, os metadados e os arquivos de atualização são armazenados em dois locais separados. Os metadados são armazenados no banco de dados do WSUS. Os arquivos de atualização podem ser armazenados em seu servidor do WSUS ou em servidores do Microsoft Update, dependendo de como você configurou suas opções de sincronização. Se você optar por armazenar os arquivos de atualização no servidor do WSUS, os computadores cliente baixarão as atualizações aprovadas do servidor do WSUS local. Se não, os computadores cliente baixarão as atualizações aprovadas diretamente do Microsoft Update. A opção que faz mais sentido para sua organização dependerá da largura de banda da rede para a Internet, da largura de banda da rede na intranet e da disponibilidade do armazenamento local.

Você pode escolher uma solução diferente de armazenamento de atualizações para cada servidor do WSUS que implantar.

#### <a name="local-wsus-server-storage"></a>Armazenamento local no servidor do WSUS
O armazenamento local dos arquivos de atualizações é a opção padrão ao instalar e configurar o WSUS. Essa opção pode economizar largura de banda na conexão corporativa com a Internet porque os computadores clientes baixam as atualizações diretamente do servidor do WSUS local.

Essa opção exige que o servidor tenha espaço em disco suficiente para armazenar todas atualizações necessárias. O WSUS exige pelo menos 20 GB para armazenar atualizações localmente; entretanto, é recomendável 30 GB com base em variáveis testadas.

#### <a name="remote-storage-on-microsoft-update-servers"></a>Armazenamento remoto nos servidores do Microsoft Update
Você pode armazenar atualizações remotamente nos servidores do Microsoft Update. Essa opção é útil se a maioria dos computadores clientes se conecta ao servidor do WSUS por uma conexão WAN lenta porém se conecta à Internet por uma conexão de alta largura de banda.

Nesse caso, o servidor do WSUS raiz é sincronizado com o Microsoft Update e recebe os metadados de atualizações. Depois de você aprovar as atualizações, os computadores clientes baixarão as atualizações aprovadas dos servidores do Microsoft Update.

## <a name="14-choose-wsus-update-languages"></a>1.4. Escolher os idiomas de atualização do WSUS
Ao implantar uma hierarquia de servidores do WSUS, você deve determinar quais atualizações de idioma são necessárias na organização. Você deve configurar o servidor do WSUS raiz para baixar atualizações em todos os idiomas usados na organização inteira.

Por exemplo, a matriz pode exigir atualizações nos idiomas inglês e francês, mas uma filial pode exigir atualizações nos idiomas alemão, francês e inglês, e outra filial pode exigir atualizações nos idiomas espanhol e inglês. Nessa situação, você configuraria o servidor do WSUS raiz para baixar atualizações em alemão, espanhol, francês e inglês. Depois configuraria o servidor do WSUS da primeira filial para baixar atualizações somente em alemão, francês e inglês; e a segunda filial para baixar atualizações somente em espanhol e inglês.

A página **Escolher Idiomas** do Assistente de Configuração do WSUS permite obter atualizações de todos os idiomas ou de um subconjunto de idiomas. A seleção de um subconjunto de idiomas fará economizar espaço em disco, mas é IMPORTANTE escolher todos os idiomas necessários a todos os servidores downstream e computadores cliente de um servidor do WSUS.

A seguir estão algumas observações IMPORTANTES sobre o idioma de atualização que você deve lembrar antes de configurar essa opção:

-   Sempre inclua inglês além de quaisquer outros idiomas necessários em toda a organização. Todas as atualizações se baseiam nos pacotes de idiomas em inglês.

-   Os servidores downstream e os computadores clientes não receberão todas as atualizações necessárias se você não tiver escolhido todos os idiomas necessários para o servidor upstream. Verifique se escolheu todos os idiomas necessários a todos os computadores clientes associados a todos os servidores downstream.

-   Você geralmente deve baixar atualizações em todos os idiomas no servidor do WSUS raiz que é sincronizado com o Microsoft Update. Essa seleção garante que todos os servidores downstream e computadores clientes receberão atualizações nos idiomas necessários.

Se você estiver armazenando atualizações localmente e tiver configurado um servidor do WSUS para baixar atualizações em um número limitado de idiomas, poderá perceber que há atualizações em idiomas diferentes daqueles que especificou. Muitos arquivos de atualização são pacotes de vários idiomas diferentes, que incluem pelo menos um dos idiomas especificados no servidor.

**Servidores upstream**

> [!NOTE]
> Configure servidores upstream para sincronizar atualizações em todos os idiomas necessários para os servidores de réplica de downstream. Você não será notificado de atualizações necessárias nos idiomas não sincronizados.

As atualizações serão exibidas como **Não Aplicável** em computadores cliente que exigem o idioma. Para evitar isso, certifique-se de que todos os idiomas do sistema operacional estejam incluídos nas opções de sincronização do servidor do WSUS. Você pode ver todos os idiomas do sistema operacional acessando a exibição **computadores** do Console de Administração do WSUS e classificar os computadores por idioma do sistema operacional. No entanto, você poderá incluir mais idiomas se houver aplicativos da Microsoft em mais de um idioma (por exemplo, se a versão francesa do Microsoft Word está instalada em alguns computadores que usam a versão em inglês do Windows 8).

A escolha de idiomas para um servidor upstream não é igual à escolha de idiomas para um servidor downstream. Os procedimentos a seguir explicam as diferenças.

#### <a name="to-choose-update-languages-for-a-server-synchronizing-from-microsoft-update"></a>Para escolher os idiomas de atualização para um servidor sincronizando no Microsoft Update

1.  No Assistente de Configuração do WSUS:

    -   Para obter atualizações em todos os idiomas, clique em **Baixar atualizações em todos os idiomas, inclusive novos idiomas**.

    -   Para obter atualizações somente para idiomas específicos, clique em **Baixar atualizações somente nestes idiomas**e, em seguida, selecione os idiomas para os quais deseja as atualizações.

#### <a name="to-choose-update-languages-for-a-downstream-server"></a>Para escolher os idiomas de atualização para um servidor downstream

1.  Se o servidor upstream tiver sido configurado para baixar os arquivos de atualização em um subconjunto de idiomas: No Assistente de Configuração do WSUS, clique em **Baixar atualizações somente nestes idiomas (somente os idiomas marcados com um asterisco têm suporte pelo servidor upstream)** e, em seguida, selecione os idiomas para os quais deseja as atualizações.

> [!NOTE]
> Você deve fazer isso mesmo que deseje que o servidor downstream baixe os mesmos idiomas do servidor upstream.

2. Se o servidor upstream tiver sido configurado para baixar os arquivos de atualização em todos os idiomas: No Assistente de Configuração do WSUS, clique em **Baixar atualizações em todos os idiomas com suporte pelo servidor upstream**.

> [!NOTE]
> Você deve fazer isso mesmo que deseje que o servidor downstream baixe os mesmos idiomas do servidor upstream. Essa configuração faz com que o servidor upstream baixe atualizações em todos os idiomas, incluindo idiomas que não foram originalmente configurados para o servidor upstream. Se você adicionar idiomas ao servidor upstream, deve copiar as novas atualizações para seus servidores de réplica.
>
> Alterar as opções de idioma apenas no servidor upstream pode causar uma incompatibilidade entre o número de atualizações aprovadas no servidor central e o número de atualizações aprovadas nos servidores de réplica.

## <a name="15-plan-wsus-computer-groups"></a>1.5. Planejar grupos de computadores do WSUS
O WSUS permite direcionar atualizações a grupos de computadores clientes, assim é possível garantir que computadores específicos sempre obterão as atualizações certas na hora certa. Por exemplo, se todos os computadores de um departamento (por exemplo, a equipe de Contabilidade) tiver uma configuração específica, você poderá configurar um grupo para essa equipe, decidir quais atualizações são necessárias aos computadores e em qual horário deverão ser instaladas, e usar os relatórios do WSUS para avaliar as atualizações para a equipe.

> [!NOTE]
> Se um servidor do WSUS estiver em execução em modo de réplica, não será possível criar grupos de computadores nesse servidor. Todos os grupos de computadores necessários para computadores clientes do servidor de réplica precisam ser criados no servidor do WSUS que é a raiz da hierarquia de servidores do WSUS. Para obter mais informações sobre o modo de réplica, consulte Gerenciar servidores de réplica do WSUS [Gerenciar servidores de réplica do WSUS](/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/dd939893(v=ws.10)) no Guia de Operações do WSUS 3.0 SP2.

Os computadores sempre são atribuídos ao grupo **Todos os computadores** e permanecem atribuídos ao grupo **Computadores não atribuídos** até você atribuí-los a outro grupo. Os computadores podem pertencer a mais de um grupo.

Os grupos de computadores podem ser configurados em hierarquias (por exemplo, o grupo Folha de Pagamento e o grupo Contas a Pagar no grupo Contabilidade). Atualizações aprovadas para um grupo superior serão implantadas automaticamente nos grupos inferiores, além do grupo superior. Neste exemplo, se você aprovar Update1 para o grupo Contabilidade, a atualização será implantada em todos os computadores do grupo Contabilidade, todos os computadores do grupo Folha de Pagamento e todos os computadores do grupo Contas a Pagar.

Como os computadores podem ser atribuídos a vários grupos, é possível que uma única atualização seja aprovada mais de uma vez para o mesmo computador. Entretanto, a atualização será implantada somente uma vez, e quaisquer conflitos serão resolvidos pelo servidor do WSUS. Para continuar com o exemplo anterior, se o computadorA for atribuído ao grupo Folha de Pagamento e ao grupo Contas a Pagar, e Update1 for aprovada para ambos os grupos, ela será implantada somente uma vez.

É possível atribuir computadores a grupos de computadores usando um destes dois métodos: direcionamento do lado do servidor ou direcionamento do lado do cliente. A seguir estão as definições de cada método:

-   **Direcionamento do lado do servidor**: Você atribui manualmente um ou mais computadores cliente a vários grupos simultaneamente.

-   **Direcionamento do lado do cliente**: Você usa a Política de Grupo ou edita as configurações do Registro nos computadores cliente para habilitar esses computadores para serem adicionados automaticamente aos grupos de computadores criados anteriormente.

### <a name="conflict-resolution"></a>Resolução de conflitos
O servidor aplica as seguintes regras para resolver conflitos e determinar a ação resultante nos clientes:

1.  Prioridade

2.  Instalar/Desinstalar

3.  Data limite

#### <a name="priority"></a>Prioridade
As ações associadas ao grupo de maior prioridade substituem as ações dos outros grupos. Quanto maior a profundidade de um grupo na hierarquia de grupos, maior é a sua prioridade. A prioridade é atribuída com base apenas na profundidade; todas as ramificações têm a mesma prioridade. Por exemplo, um grupo dois níveis abaixo da ramificação Desktops tem prioridade maior que o grupo um nível abaixo da ramificação Servidor.

No exemplo de texto a seguir do painel de hierarquias do console do Update Services, para um servidor do WSUS nomeado WSUS-01, os grupos de computadores nomeados Computadores desktop e Servidor foram adicionados ao grupo padrão **Todos os computadores**. Os dois grupos, Computadores desktop e Servidor, estão no mesmo nível hierárquico.

-   **Update Services**

    -   **WSUS-01**

        -   **Atualizações**

        -   **computadores**

            -   **Todos os computadores**

                -   **Computadores não atribuídos**

                -   **Computadores desktop**

                    -   **Desktops-L1**

                        -   **Desktops-L2**

                -   **Servidores**

                    -   **Servidores-L1**

        -   **Servidores Downstream**

        -   **Sincronizações**

        -   **Relatórios**

        -   **Opções**

Neste exemplo, o grupo dois níveis abaixo da ramificação Computadores desktop (Desktops L2) tem maior prioridade do que o grupo um nível abaixo da ramificação Servidor (Servidores L1). Dessa forma, para um computador associado aos dois grupos, Desktops-L2 e Servidores-L1, todas as ações referentes ao grupo Desktops-L2 têm prioridade sobre as ações especificadas para o grupo Servidores-L1.

#### <a name="priority-of-install-and-uninstall"></a>Prioridade de instalação e desinstalação
As ações de instalação substituem as ações de desinstalação. As instalações necessárias substituem as instalações opcionais (as instalações opcionais só estão disponíveis via API e, se uma aprovação de atualização for alterada via Console de Administração do WSUS, todas as aprovações opcionais serão limpas.)

#### <a name="priority-of-deadlines"></a>Prioridade dos prazos finais
As ações que têm uma data limite substituem aquelas que não têm.  As ações com data limite anterior substituem aquelas com datas limite posteriores.

## <a name="16-plan-wsus-performance-considerations"></a>1.6. Planejar considerações de desempenho do WSUS
Há algumas áreas que você deve planejar cuidadosamente antes de implantar o WSUS para ter um desempenho otimizado. As áreas cruciais são:

-   Configuração da rede

-   Download adiado

-   Filtros

-   Instalação

-   Implantações de grandes atualizações

-   BITS

### <a name="network-setup"></a>Configuração da rede
Para otimizar o desempenho em redes do WSUS, avalie as seguintes sugestões:

1.  Configurar redes do WSUS em um topologia hub e spoke em vez de uma topologia hierárquica.

2.  Usar classificação de máscaras de rede DNS para computadores clientes móveis e configurar estes para obter atualizações do servidor do WSUS local.

### <a name="deferred-download"></a>Download adiado
Você pode aprovar atualizações e baixar os metadados de atualização antes de baixar os arquivos de atualização. Esse método é chamado *downloads adiados*. Ao adiar downloads, uma atualização é baixada somente depois de ser aprovada. É recomendável adiar downloads porque isso otimiza a largura de banda da rede e o espaço em disco.

Em uma hierarquia de servidores do WSUS, o WSUS define automaticamente todos os servidores downstream que usarão a configuração de download adiado do servidor do WSUS raiz. Você pode alterar essa configuração padrão. Por exemplo, é possível configurar um servidor upstream para que execute sincronizações imediatas completas e configurar um servidor downstream para adiar os downloads.

Se você implantar uma hierarquia de servidores do WSUS conectados, nossa recomendação é não aninhar profundamente os servidores. Se você habilitar downloads adiados e um servidor downstream solicitar uma atualização que não está aprovada no servidor upstream, a solicitação do servidor downstream forçará um download no servidor upstream. O servidor downstream baixa a atualização em uma sincronização subsequente. Em uma hierarquia profunda de servidores do WSUS, atrasos podem ocorrer à medida que as atualizações são solicitadas, baixadas e passadas pela hierarquia de servidores. Por padrão, os downloads adiados são habilitados quando você armazena as atualizações localmente. É possível alterar essa opção manualmente.

### <a name="filters"></a>Filtros
O WSUS permite filtrar sincronizações de atualização por idioma, produto e classificação. Em uma hierarquia de servidores do WSUS, o WSUS define automaticamente todos os servidores downstream que usarão as opções de filtragem de atualizações escolhidas no servidor do WSUS raiz. Você pode reconfigurar os servidores de download para que recebam somente um subconjunto dos idiomas.

Por padrão, os produtos que serão atualizados são o Windows e o Office, e as classificações padrão são as atualizações Críticas, de Segurança e de Definição. Para conservar a largura de banda e o espaço em disco, é recomendável limitar os idiomas aos que você realmente usa.

### <a name="installation"></a>Instalação
As atualizações normalmente consistem em novas versões de arquivos que já existem no computador que está sendo atualizado. Em um nível binário, esses arquivos existentes podem não ser muito diferentes das versões atualizadas. O recurso de arquivos de instalação expressa identifica os bytes exatos entre as versões, cria e distribui atualizações somente dessas diferenças e mescla o arquivo existente juntamente com os bytes atualizados.

Esse recurso às vezes é chamado de entrega delta porque ele baixa somente o delta (diferença) entre duas versões de um arquivo. Os arquivos de instalação expressa são maiores do que as atualizações distribuídas aos computadores clientes porque o arquivo de instalação expressa contém todas as versões possíveis de cada arquivo que será atualizado.

Você pode usar arquivos de instalação expressa para limitar a largura de banda consumida na rede local, porque o WSUS transmite apenas o delta aplicável a uma versão específica de um componente atualizado. No entanto, isso ocorre à custa de largura de banda adicional entre o servidor do WSUS, quaisquer servidores do WSUS upstream e Microsoft Update e requer espaço adicional no disco local. Por padrão, o WSUS não usa arquivos de instalação expressa.

Nem todas as atualizações são ótimas candidatas para distribuição usando os arquivos de instalação expressa. Se você escolher essa opção, obterá os arquivos de instalação expressa para todas as atualizações. Se você não armazenar as atualizações localmente, o Windows Update Agent decidirá se deseja baixar os arquivos de instalação expressa ou as distribuições de atualização completas.

### <a name="large-update-deployment"></a>Implantações de grandes atualizações
Ao implantar grandes atualizações (como os service packs), é possível evitar a saturação da rede seguindo estas práticas:

1.  Usar a otimização do BITS. As limitações de largura de banda do BITS podem ser controladas por horário no dia, mas elas se aplicam a todos os aplicativos que estão usando o BITS. Para saber como controlar a limitação do BITS, consulte [Políticas de Grupo](/windows/win32/bits/group-policies).

2.  Usar a otimização do IIS (Serviços de Informações da Internet) para limitar a otimização a um ou mais serviços Web.

3.  Usar grupos de computadores para controlar a distribuição. Um computador cliente se identifica como membro de um determinado grupo de computadores quando envia informações ao servidor do WSUS. O servidor do WSUS usa essas informações para determinar quais atualizações devem ser implantadas nesse computador. Você pode configurar vários grupos de computadores e aprovar em sequência grandes downloads de service packs para um subconjunto desses grupos.

### <a name="background-intelligent-transfer-service"></a>BITS
O WSUS usa o protocolo BITS para todas as tarefas de transferência de arquivos. Isso inclui downloads para computadores clientes e sincronizações de servidores. O BITS permite que os programas baixem arquivos usando a largura de banda sobressalente. O BITS mantém transferências de arquivos através de desconexões da rede e reinicializações do computador. Para obter mais informações, consulte: [Serviço de Transferência Inteligente em Segundo Plano](/windows/win32/bits/background-intelligent-transfer-service-portal).

## <a name="17-plan-automatic-updates-settings"></a>1.7. Planejar definições das atualizações automáticas
Você pode especificar um prazo limite para aprovar atualizações no servidor do WSUS. O prazo limite faz com que os computadores clientes instalem a atualização em um horário específico, mas há inúmeras situações diferentes, dependendo de se o prazo limite expirou, se há outras atualizações na fila para o computador instalar e se a atualização (ou outra atualização na fila) exige uma reinicialização.

Por padrão, as Atualizações Automáticas sondam o servidor do WSUS para verificar se há atualizações aprovadas a cada 22 horas menos um deslocamento aleatório. Se for necessário instalar as novas atualizações, elas serão baixadas. O tempo entre cada ciclo de detecção pode ser manipulado de 1 a 22 horas.

Você pode manipular as opções de notificação da seguinte maneira:

1.  Se as Atualizações Automáticas estiverem configuradas para notificar o usuário das atualizações prontas para ser instaladas, a notificação será enviada ao log de Sistema e à área de notificação do computador cliente.

2.  Quando um usuário com credenciais apropriadas clica no ícone da área de notificação, as Atualizações Automáticas exibem as atualizações disponíveis que serão instaladas. O usuário precisa clicar em **Instalar** para iniciar a instalação. Uma mensagem aparecerá se a atualização que o computador seja reiniciado para concluir a atualização. Se for solicitada uma reinicialização, as Atualizações Automáticas não poderão detectar atualizações adicionais enquanto o computador não for reiniciado.

Se as Atualizações Automáticas estiverem configuradas para instalar atualizações em uma agenda definida, as atualizações aplicáveis serão baixadas e marcadas como prontas para ser instaladas. As Atualizações Automáticas notificam os usuários que têm credenciais apropriadas usando um ícone de área de notificação, e um evento é registrado no log de Sistema.

No dia e horário agendado, as Atualizações Automáticas instalam a atualização e reinicias o computador (se necessário), mesmo se nenhum administrador local estiver conectado. Se um administrador local estiver conectado e o computador exigir uma reinicialização, as Atualizações Automáticas exibirão um aviso e uma contagem regressiva para a reinicialização. Caso contrário, a instalação ocorrerá em segundo plano.

Se for necessário reiniciar o computador, e qualquer usuário estiver conectado, uma caixa de diálogo de contagem regressiva será exibida, o que avisa o usuário sobre a reinicialização iminente. Você pode manipular as reinicializações do computador com a Política de Grupo.

Depois que as novas atualizações forem baixadas, as Atualizações Automáticas sondarão no servidor do WSUS se há a lista de pacotes aprovados para confirmar se os pacotes que ele baixou ainda estão válidos e aprovados. Isso significa que, se um administrador do WSUS remover as atualizações da lista de atualizações aprovadas enquanto as Atualizações Automáticas estão baixando as atualizações, somente as atualizações que ainda estão aprovadas na realidade serão instaladas.