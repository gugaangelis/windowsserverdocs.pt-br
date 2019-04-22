---
title: Planejar a implantação do WSUS
description: Tópico do Windows Server Update Service (WSUS) - uma visão geral da implantação do processo com links para os tópicos relacionados de planejamento
ms.prod: windows-server-threshold
ms.reviewer: na
ms.technology: manage-wsus
ms.topic: article
ms.assetid: 35865398-b011-447a-b781-1c52bc0c9e3a
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 05/24/2018
ms.openlocfilehash: 73fd1d83d82da1694d90a2b3cf3f39717536606b
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59822117"
---
# <a name="plan-your-wsus-deployment"></a>Planejar sua implantação do WSUS

>Aplica-se a: Windows Server (canal semestral), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

A primeira etapa na implantação do WSUS (Windows Server Update Services) é tomar decisões importantes, por exemplo decidir o cenário de implantação do WSUS, escolher uma topologia de rede e entender os requisitos do sistema. A lista de verificação a seguir resume as etapas envolvidas na preparação para a implantação.

|Tarefa|Descrição|
|----|--------|
|[1.1. Examinar as considerações iniciais e os requisitos do sistema](plan-your-wsus-deployment.md#BKMK_1.1)|Examine a lista de considerações e requisitos do sistema para garantir que você tenha todos os componentes de hardware e software necessários para implantar o WSUS.|
|[1.2. Escolha um cenário de implantação do WSUS](plan-your-wsus-deployment.md#BKMK_1.2)|Decida qual cenário de implantação do WSUS será usado.|
|[1.3. Escolha uma estratégia de armazenamento do WSUS](plan-your-wsus-deployment.md#BKMK_1.3.)|Decida qual estratégia de armazenamento do WSUS é mais adequada à sua implantação.|
|[1.4. Escolha que idiomas de atualização do WSUS](plan-your-wsus-deployment.md#BKMK_1.4.)|Decida quais idiomas de atualização do WSUS serão instalados.|
|[1.5. Planejar grupos de computadores do WSUS](plan-your-wsus-deployment.md#BKMK_1.5)|Planeje a abordagem de grupo de computadores do WSUS que será usada para sua implantação.|
|[1.6. Planeje considerações de desempenho do WSUS: Serviço de transferência inteligente em segundo plano](plan-your-wsus-deployment.md#BKMK_1.6.)|Planeje um design do WSUS para garantir desempenho otimizado.|
|[1.7. Planejar configurações de atualizações automáticas](plan-your-wsus-deployment.md#BKMK_1.7.)|Planeje como você configurará as definições de atualizações automáticas para o seu cenário.|

## <a name="BKMK_1.1"></a>1.1. Examinar as considerações iniciais e os requisitos do sistema

### <a name="system-requirements"></a>Requisitos do sistema

Para habilitar a função de servidor do WSUS, confirme se o servidor atende aos requisitos do sistema e se você tem as permissões necessárias para concluir a instalação seguindo estas diretrizes:

-   Os requisitos de hardware do servidor para habilitar a função do WSUS são associados aos requisitos de hardware. Os requisitos mínimos de hardware do WSUS são:

    -   **Processador:** 1,4 GHz (gigahertz) x64 processador (2 Ghz ou mais recomendados)

    -   **Memória:** O WSUS requer um adicional de 2 GB de RAM, mais do que é exigido pelo servidor e todos os outros serviços ou software.

    -   **Espaço em disco disponível:** 10 GB (40 GB ou superior é recomendado)

    -   **Adaptador de rede:** 100 megabits por segundo (Mbps) ou maior

-   Requisitos de software:

    -   Para exibir relatórios, o WSUS requer o [Microsoft Report Viewer Redistributable 2008](https://www.microsoft.com/download/details.aspx?id=6576). No Windows Server 2016, o WSUS requer [Microsoft relatório Visualizador de tempo de execução de 2012](https://www.microsoft.com/download/details.aspx?id=35747)

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

1.  Funções de servidor que tiveram suas cargas removidas usando os Recursos sob Demanda não podem ser instaladas sob demanda pelo Microsoft Update. Você deve fornecer uma origem de instalação no momento em que você tentar instalar tais funções de servidor ou configurar uma origem para os recursos sob demanda na política de grupo.

2.  As edições do cliente Windows não poderão instalar o .NET 3.5 sob demanda por meio da Web. Aplicam-se ao .NET 3.5 as mesmas considerações que para as funções de servidor.

    > [!NOTE]
    > Configurando um recursos na origem de instalação de demanda não envolve WSUS. Para obter informações sobre como configurar os Recursos, consulte [Configurar Recursos sob Demanda no Windows Server](https://technet.microsoft.com/library/jj127275.aspx).

3. Dispositivos da empresa que executam o Windows 10, versão 1709 ou versão 1803, não é possível instalar todos os recursos sob demanda diretamente do WSUS. Para instalar recursos sob demanda, [criar um arquivo de recurso (side-by-side repositório)](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2012-R2-and-2012/jj127275%28v=ws.11%29#create-a-feature-file-or-side-by-side-store) ou obtenha o recurso no pacote de demanda de uma das seguintes fontes:
    - [Volume Licensing Service Center](https://www.microsoft.com/licensing/servicecenter) (VLSC) - é necessário acesso de VL
    - Portal de OEM - é necessário acesso de OEM
    - Download do MSDN – é necessária uma assinatura do MSDN

    Recurso obtido individualmente em pacotes de demanda pode ser instalado usando [opções de linha de comando DISM](https://docs.microsoft.com/windows-hardware/manufacture/desktop/dism-operating-system-package-servicing-command-line-options).

### <a name="BKM_1.1.1."></a>Requisitos de banco de dados do WSUS
O WSUS requer um dos seguintes bancos de dados:

-   WID (Banco de Dados Interno do Windows)

-   Microsoft SQL Server 2017

-   Microsoft SQL Server 2016

-   Microsoft SQL Server 2014

-   Microsoft SQL Server 2012

-   Microsoft SQL Server 2008 R2

As edições a seguir do SQL Server têm suporte pelo WSUS:

-   Standard

-   Enterprise

-   Express

> [!NOTE]
> O SQL Server Express 2008 R2 tem uma limitação de tamanho máximo do banco de dados de 10 GB. Esse tamanho de banco de dados provavelmente é suficiente para o WSUS, apesar de não haver nenhum benefício apreciável em usá-lo no lugar do WID. Banco de dados WID tem um requisito de memória RAM mínimo de 2 GB além os requisitos de sistema do Windows Server standard.

Você pode instalar a função do WSUS em um computador separado do computador do servidor de banco de dados. Nesse caso, os seguintes critérios adicionais se aplicam:

1.  O servidor de banco de dados não pode ser configurado como controlador de domínio.

2.  O servidor do WSUS não pode executar os Serviços de Área de Trabalho Remota.

3.  O servidor de banco de dados deve estar no mesmo domínio do Active Directory como o servidor do WSUS, ou ele deve ter uma relação de confiança com o domínio do Active Directory do servidor do WSUS.

4.  O servidor do WSUS e o servidor de banco de dados devem estar no mesmo fuso horário ou sincronizados com a mesma fonte de tempo de Universal Coordenado (hora de Greenwich).

## <a name="BKMK_1.2"></a>1.2. Escolher o cenário de implantação do WSUS
Esta seção descreve os recursos básicos de todas as implantações do WSUS. Use esta seção para se familiarizar com uma implantação simples com um único servidor do WSUS, além de cenários mais complexos, como uma hierarquia de servidores do WSUS ou um servidor do WSUS em um segmento de rede isolado.

### <a name="simple-wsus-deployment"></a>Implantação simples do WSUS
A implantação mais básica do WSUS consiste em um servidor dentro do firewall corporativo que serve computadores clientes em uma intranet privada. O servidor do WSUS se conecta ao Microsoft Update para baixar atualizações. Isso é conhecido como *sincronização*. Durante a sincronização, o WSUS determina se alguma atualização nova foi disponibilizada desde a última sincronização. Se for a sua primeira sincronização do WSUS, todas as atualizações serão disponibilizadas para download.

> [!NOTE]
> A sincronização inicial pode levar uma hora. Todas as sincronizações posteriores devem ser significativamente mais rápidas.

Por padrão, o servidor do WSUS usa a porta 80 para o protocolo HTTP e a porta 443 para o protocolo HTTPS para obter atualizações da Microsoft. Se houver um firewall corporativo entre a rede e a Internet, você precisará abrir essas portas no servidor que se comunica diretamente com o Microsoft Update. Se você estiver planejando usar portas personalizadas para essa comunicação, você deve abrir essas portas. É possível configurar vários servidores do WSUS para que sejam sincronizados com um servidor pai do WSUS. Por padrão, o servidor do WSUS usa a porta 8530 para o protocolo HTTP e a porta 8531 para o protocolo HTTPS para fornecer atualizações para as estações de trabalho cliente.

### <a name="multiple-wsus-servers"></a>Vários servidores do WSUS
Os administradores podem implantar diversos servidores WSUS que sincronizam todo o conteúdo dentro da intranet da empresa. Você pode expor somente um servidor com a Internet, o que seria o único servidor que baixa as atualizações do Microsoft Update. Este servidor está configurado como servidor upstream a origem para o qual os servidores downstream são sincronizados. Quando aplicável, os servidores podem estar localizados em uma rede geograficamente dispersa para oferecer a melhor conectividade a todos os computadores clientes.

### <a name="disconnected-wsus-server"></a>Servidor WSUS desconectado
Se a política corporativa ou outras condições limitarem o acesso dos computadores à Internet, os administradores poderão configurar um servidor interno para executar o WSUS. Um exemplo disso é um servidor conectado à intranet porém isolado da Internet. Depois de baixar, testar e aprovar as atualizações nesse servidor, um administrador exportaria os metadados e o conteúdo das atualizações para um DVD. Os metadados e o conteúdo das atualizações são importados do DVD para os servidores que executam o WSUS na intranet.

### <a name="wsus-server-hierarchies"></a>Hierarquias de servidores do WSUS
Você pode criar complexas hierarquias dos servidores do WSUS. Como é possível sincronizar um servidor do WSUS com outro servidor do WSUS, e não com o Microsoft Update, é necessário ter um único servidor do WSUS conectado ao Microsoft Update. Ao conectar os servidores do WSUS, há um servidor upstream do WSUS e um servidor downstream do WSUS. Uma implantação de hierarquia de servidores do WSUS oferece os seguintes benefícios:

-   Baixar atualizações uma vez da Internet e distribuí-las aos computadores clientes usando servidores downstream. Esse método economiza largura de banda na conexão corporativa à Internet.

-   Baixar atualizações para um servidor do WSUS que esteja fisicamente mais próximo aos computadores clientes, por exemplo, nas filiais da empresa.

-   Configurar servidores do WSUS separados para servir computadores clientes que usam diferentes idiomas dos produtos da Microsoft.

-   Dimensionar o WSUS para uma organização de grande porte que tem mais computadores clientes do que um único servidor do WSUS pode gerenciar com eficácia.

> [!NOTE] 
> É recomendável não criar uma hierarquia de servidores do WSUS com mais de três níveis de profundidade. Cada nível aumenta o tempo para propagar as atualizações nos servidores conectados. Embora não haja nenhum limite teórico para uma hierarquia, somente implantações que têm uma hierarquia de cinco níveis de profundidade foram testadas pela Microsoft.
>
> Além disso, os servidores downstream devem ser a mesma versão ou uma versão anterior do WSUS como a origem de sincronização do servidor upstream.

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

### <a name="network-load-balancing"></a>Balanceamento de Carga de Rede
O NLB (Balanceamento de Carga de Rede) aumenta a confiabilidade e o desempenho da rede do WSUS. Você pode configurar vários servidores WSUS que compartilham um único cluster de failover que executa o QL Server como o SQL Server 2008 R2 SP1. Nessa configuração, é necessário usar uma instalação completa do SQL Server, e não a instalação do Banco de Dados Interno do Windows fornecida pelo WSUS, e a função de banco de dados precisa estar instalada em todos os servidores front-end do WSUS. Você também pode fazer com que todos os servidores do WSUS usem um DFS (sistema de arquivos distribuídos) para armazenar seu conteúdo.

**Instalação do WSUS para NLB:** em comparação com a instalação do WSUS 3.2 para NLB, uma chamada de instalação especiais são mais necessários parâmetros e para configurar o WSUS para NLB. Você precisa apenas configurar cada servidor do WSUS, mantendo as seguintes considerações em mente.

-   O WSUS deve ser configurado usando a opção de banco de dados SQL em vez de WID.

-   Se armazenar as atualizações localmente, a mesma pasta de conteúdo deve ser compartilhada entre os servidores do WSUS que compartilham o mesmo banco de dados SQL.

-   A instalação do WSUS deve ser feita em série. As tarefas pós-instalação não podem ser executadas em mais de um servidor ao mesmo tempo quando o mesmo banco de dados SQL é compartilhado.

### <a name="wsus-deployment-with-roaming-client-computers"></a>A implantação do WSUS com computadores clientes móveis
Se a rede incluir usuários móveis que fazem logon na rede de locais diferentes, você poderá configurar o WSUS para permitir que os usuários móveis atualizem seus computadores clientes do servidor do WSUS mais próximo a eles em termos geográficos. Por exemplo, você pode implantar um servidor do WSUS em cada região e usar uma sub-rede DNS diferente para cada região. Todos os computadores cliente pode ser direcionados para o mesmo servidor do WSUS, que resolve em cada sub-rede para o servidor do WSUS físico mais próximo.

## <a name="BKMK_1.3."></a>1.3. Escolher a estratégia de armazenamento do WSUS
O WSUS (Windows Server Update Services) usa dois tipos de sistemas de armazenamento: um banco de dados para armazenar a configuração do WSUS e metadados de atualização, e um sistema de arquivos local opcional para armazenar os arquivos de atualização. Para instalar o WSUS, você deve decidir como quer implementar o armazenamento.

As atualizações consistem em duas partes: os metadados que descrevem a atualização; e os arquivos necessários para instalar a atualização. Os metadados de atualização normalmente são bem menores do que a atualização real e são armazenados no banco de dados do WSUS. Os arquivos de atualização são armazenados em um servidor do WSUS local ou em um servidor Web do Microsoft Update.

### <a name="wsus-database"></a>Banco de dados do WSUS
O WSUS requer um banco de dados para cada servidor do WSUS. O WSUS dá suporte ao uso de um banco de dados que reside em um computador diferente do servidor do WSUS, com algumas restrições. Para obter uma lista de bancos de dados com suporte e limitações do banco de dados remoto, consulte a seção "Considerações iniciais de revisão 1.1 e requisitos do sistema," neste guia.

O banco de dados do WSUS armazena as seguintes informações:

-   Informações de configuração do servidor do WSUS

-   Metadados que descrevem cada atualização

-   Informações sobre computadores clientes, atualizações e manipulações

Se você instalar vários servidores do WSUS, precisará manter um banco de dados separado para cada servidor do WSUS, seja ele um servidor autônomo ou de réplica. Você não pode armazenar vários bancos de dados do WSUS em uma única instância do SQL Server, a não ser em clusters NLB (Balanceamento de Carga de Rede) que usam failover do SQL Server.

O SQL Server, o SQL Server Express e o Banco de Dados Interno do Windows fornecem as mesmas características de desempenho para uma configuração de um único servidor, em que o banco de dados e o serviço do WSUS estão localizados no mesmo computador. Uma configuração de um único servidor pode dar suporte a vários milhares de computadores clientes do WSUS.

> [!NOTE]
> Não tente gerenciar o WSUS acessando o banco de dados diretamente. manipular diretamente o banco de dados pode causar corrupção de banco de dados. A corrupção pode não ser imediatamente óbvia, mas pode impedir atualizações para a próxima versão do produto. Você pode gerenciar o WSUS usando o console do WSUS ou APIs (interfaces de programação de aplicativos) do WSUS.

#### <a name="wsus-with-windows-internal-database"></a>WSUS com Banco de Dados Interno do Windows
Por padrão, o assistente de instalação cria e usa um Banco de Dados Interno do Windows chamado SUSDB.mdf. Esse banco de dados está localizado na pasta %windir%\wid\data\, onde %windir% é a unidade local em que o software servidor do WSUS está instalado.

> [!NOTE]
> Banco de dados interno do Windows (WID) foi introduzido no Windows Server 2012.

O WSUS dá suporte à autenticação do Windows somente para o banco de dados. Você não pode usar a autenticação do SQL Server com o WSUS. Se você usar o Banco de Dados Interno do Windows para o banco de dados do WSUS, a Instalação do WSUS criará uma instância do SQL Server chamada server\Microsoft##WID, onde server é o nome do computador. Com qualquer uma das opções de banco de dados, a Instalação do WSUS criará um banco de dados chamado SUSDB. O nome desse banco de dados não é configurável.

É recomendável usar o Banco de Dados Interno do Windows nos seguintes casos:

-   A organização ainda não comprou e não precisa de um produto SQL Server para nenhuma outra aplicação.

-   A organização não precisa de uma solução do WSUS NLB.

-   Você pretende implantar vários servidores do WSUS (por exemplo, nas filiais da empresa). Nesse caso, você deve avaliar o uso do Banco de Dados Interno do Windows nos servidores secundários, mesmo se usar o SQL Server para o servidor do WSUS raiz. Como cada servidor do WSUS exige uma instância separada do SQL Server, você rapidamente terá problemas de desempenho no banco de dados se somente uma instância do SQL Server manipular vários servidores do WSUS.

O Banco de Dados Interno do Windows não fornece uma interface do usuário nem ferramentas de gerenciamento de banco de dados. Se você selecionar esse banco de dados para o WSUS, use ferramentas externas para gerenciar o banco de dados. Para obter mais informações, consulte:

-   [Backup e restauração de dados do WSUS e fazendo backup do servidor](https://technet.microsoft.com/library/dd939904(WS.10).aspx)

-   [Reindexar o banco de dados do WSUS](https://technet.microsoft.com/library/dd939795(WS.10).aspx)

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

Essa opção exige que o servidor tenha espaço em disco suficiente para armazenar todas atualizações necessárias. no mínimo, o WSUS requer 20 GB armazenar atualizações localmente; No entanto, é recomendável 30 GB com base em variáveis testadas.

#### <a name="remote-storage-on-microsoft-update-servers"></a>armazenamento remoto nos servidores do Microsoft Update
Você pode armazenar atualizações remotamente nos servidores do Microsoft Update. Essa opção é útil se a maioria dos computadores clientes se conecta ao servidor do WSUS por uma conexão WAN lenta porém se conecta à Internet por uma conexão de alta largura de banda.

Nesse caso, o servidor do WSUS raiz é sincronizado com o Microsoft Update e recebe os metadados de atualizações. Depois de você aprovar as atualizações, os computadores clientes baixarão as atualizações aprovadas dos servidores do Microsoft Update.

## <a name="BKMK_1.4."></a>1.4. Escolher os idiomas de atualização do WSUS
Ao implantar uma hierarquia de servidores do WSUS, você deve determinar quais atualizações de idioma são necessárias na organização. Você deve configurar o servidor do WSUS raiz para baixar atualizações em todos os idiomas usados na organização inteira.

Por exemplo, a matriz pode exigir atualizações nos idiomas inglês e francês, mas uma filial pode exigir atualizações nos idiomas alemão, francês e inglês, e outra filial pode exigir atualizações nos idiomas espanhol e inglês. Nessa situação, você configuraria o servidor do WSUS raiz para baixar atualizações em alemão, espanhol, francês e inglês. Depois configuraria o servidor do WSUS da primeira filial para baixar atualizações somente em alemão, francês e inglês; e a segunda filial para baixar atualizações somente em espanhol e inglês.

A página **Escolher Idiomas** do Assistente de Configuração do WSUS permite obter atualizações de todos os idiomas ou de um subconjunto de idiomas. Selecionar um subconjunto de idiomas economiza espaço em disco, mas é importante escolher todos os idiomas que são necessários para todos os servidores downstream e computadores cliente de um servidor WSUS.

A seguir estão algumas observações importantes sobre a linguagem de atualização que você deve ter em mente antes de configurar essa opção:

-   Sempre inclua inglês além de quaisquer outros idiomas necessários em toda a organização. Todas as atualizações se baseiam nos pacotes de idiomas em inglês.

-   Os servidores downstream e os computadores clientes não receberão todas as atualizações necessárias se você não tiver escolhido todos os idiomas necessários para o servidor upstream. Verifique se escolheu todos os idiomas necessários a todos os computadores clientes associados a todos os servidores downstream.

-   Você geralmente deve baixar atualizações em todos os idiomas no servidor do WSUS raiz que é sincronizado com o Microsoft Update. Essa seleção garante que todos os servidores downstream e computadores clientes receberão atualizações nos idiomas necessários.

Se você estiver armazenando atualizações localmente e tiver configurado um servidor do WSUS para baixar atualizações em um número limitado de idiomas, poderá perceber que há atualizações em idiomas diferentes daqueles que especificou. Muitos arquivos de atualização são pacotes de vários idiomas diferentes, que incluem pelo menos um dos idiomas especificados no servidor.

**Servidores upstream**

> [!NOTE]
> Configure servidores upstream para sincronizar atualizações em todos os idiomas necessários para os servidores de réplica de downstream. Você não será notificado de atualizações necessárias nos idiomas não sincronizados.

As atualizações serão exibidas como **Não Aplicável** em computadores cliente que exigem o idioma. Para evitar isso, certifique-se de que todos os idiomas do sistema operacional estejam incluídos nas opções de sincronização do servidor do WSUS. Você pode ver todos os idiomas do sistema operacional, vá para o **computadores** exibição do Console de administração do WSUS e classificar os computadores por idioma do sistema operacional. No entanto, você talvez queira incluir mais idiomas se houver aplicativos da Microsoft em mais de um idioma (por exemplo, se a versão francesa do Microsoft Word está instalada em alguns computadores que usam a versão em inglês do Windows 8.

A escolha de idiomas para um servidor upstream não é igual à escolha de idiomas para um servidor downstream. Os procedimentos a seguir explicam as diferenças.

#### <a name="to-choose-update-languages-for-a-server-synchronizing-from-microsoft-update"></a>Para escolher os idiomas de atualização para um servidor sincronizando no Microsoft Update

1.  No Assistente de Configuração do WSUS:

    -   Para obter atualizações em todos os idiomas, clique em **Baixar atualizações em todos os idiomas, inclusive novos idiomas**.

    -   Para obter atualizações somente para idiomas específicos, clique em **Baixar atualizações somente nestes idiomas**e, em seguida, selecione os idiomas para os quais deseja as atualizações.

#### <a name="to-choose-update-languages-for-a-downstream-server"></a>Para escolher os idiomas de atualização para um servidor downstream

1.  Se o servidor upstream tiver sido configurado para baixar os arquivos de atualização em um subconjunto de idiomas: No Assistente de Configuração do WSUS, clique em **Baixar atualizações somente nestes idiomas (somente os idiomas marcados com um asterisco têm suporte pelo servidor upstream)** e, em seguida, selecione os idiomas para os quais deseja as atualizações.

> [!NOTE]
> Você deve fazer isso mesmo que deseje que o servidor downstream baixe os mesmos idiomas do servidor upstream.

2.  Se o servidor upstream tiver sido configurado para baixar os arquivos de atualização em todos os idiomas: No Assistente de Configuração do WSUS, clique em **Baixar atualizações em todos os idiomas com suporte pelo servidor upstream**.

> [!NOTE]
> Você deve fazer isso mesmo que deseje que o servidor downstream baixe os mesmos idiomas do servidor upstream. Essa configuração faz com que o servidor upstream baixe atualizações em todos os idiomas, incluindo idiomas que não foram originalmente configurados para o servidor upstream. Se você adicionar idiomas ao servidor upstream, deve copiar as novas atualizações para seus servidores de réplica.
>
> Alterar as opções de idioma apenas no servidor upstream pode causar uma incompatibilidade entre o número de atualizações aprovadas no servidor central e o número de atualizações aprovadas nos servidores de réplica.

## <a name="BKMK_1.5"></a>1.5. Planejar grupos de computadores do WSUS
O WSUS permite direcionar atualizações a grupos de computadores clientes, assim é possível garantir que computadores específicos sempre obterão as atualizações certas na hora certa. Por exemplo, se todos os computadores de um departamento (por exemplo, a equipe de Contabilidade) tiver uma configuração específica, você poderá configurar um grupo para essa equipe, decidir quais atualizações são necessárias aos computadores e em qual horário deverão ser instaladas, e usar os relatórios do WSUS para avaliar as atualizações para a equipe.

> [!NOTE]
> Se um servidor do WSUS estiver em execução em modo de réplica, não será possível criar grupos de computadores nesse servidor. Todos os grupos de computadores necessários para computadores clientes do servidor de réplica precisam ser criados no servidor do WSUS que é a raiz da hierarquia de servidores do WSUS. Para obter mais informações sobre o modo de réplica, consulte gerenciar servidores de réplica do WSUS [gerenciar servidores de réplica do WSUS](https://technet.microsoft.com/library/dd939893(WS.10).aspx) no guia de operações do WSUS 3.0 SP2.

Computadores sempre são atribuídos para o **todos os computadores** grupo e eles permanecem atribuídos ao **computadores não atribuídos** até você atribuí-los para outro grupo de grupo. Os computadores podem pertencer a mais de um grupo.

Os grupos de computadores podem ser configurados em hierarquias (por exemplo, o grupo Folha de Pagamento e o grupo Contas a Pagar no grupo Contabilidade). Atualizações aprovadas para um grupo superior serão implantadas automaticamente nos grupos inferiores, além do grupo superior. Neste exemplo, se você aprovar Update1 para o grupo Contabilidade, a atualização será implantada em todos os computadores do grupo Contabilidade, todos os computadores do grupo Folha de Pagamento e todos os computadores do grupo Contas a Pagar.

Como os computadores podem ser atribuídos a vários grupos, é possível que uma única atualização seja aprovada mais de uma vez para o mesmo computador. Entretanto, a atualização será implantada somente uma vez, e quaisquer conflitos serão resolvidos pelo servidor do WSUS. Para continuar com o exemplo anterior, se o ComputadorA for atribuído ao grupo de folha de pagamento e o grupo de contas a pagar, e Update1 for aprovada para ambos os grupos, ela será implantada somente uma vez.

É possível atribuir computadores a grupos de computadores usando um destes dois métodos: direcionamento do lado do servidor ou direcionamento do lado do cliente. A seguir estão as definições de cada método:

-   **Direcionamento do lado do servidor**: Você atribui manualmente um ou mais computadores cliente a vários grupos simultaneamente.

-   **Direcionamento do lado do cliente**: Você usa a Política de Grupo ou edita as configurações do Registro nos computadores cliente para habilitar esses computadores para serem adicionados automaticamente aos grupos de computadores criados anteriormente.

### <a name="conflict-resolution"></a>Resolução de conflitos
O servidor aplica as seguintes regras para resolver conflitos e determinar a ação resultante nos clientes:

1.  Priority

2.  Instalar/Desinstalar

3.  Data limite

#### <a name="BKMK_Priority"></a>prioridade
As ações associadas ao grupo de maior prioridade substituem as ações dos outros grupos. Quanto maior a profundidade de um grupo na hierarquia de grupos, maior é a sua prioridade. A prioridade é atribuída com base apenas na profundidade; todas as ramificações têm a mesma prioridade. Por exemplo, um grupo dois níveis abaixo da ramificação Desktops tem prioridade maior que o grupo um nível abaixo da ramificação Servidor.

No seguinte exemplo de texto do painel de hierarquia do console do Update Services, para um servidor do WSUS nomeado WSUS-01, os grupos de computadores nomeados computadores Desktop e servidor foram adicionados para o padrão **todos os computadores** grupo. Os computadores Desktop e grupos de servidores estão no mesmo nível hierárquico.

-   **Serviços de atualização**

    -   **WSUS-01**

        -   **atualizações**

        -   **Computadores**

            -   **Todos os computadores**

                -   **Computadores não atribuídos**

                -   **Computadores desktop**

                    -   **Desktops-L1**

                        -   **Desktops-L2**

                -   **Servidores**

                    -   **Servers-L1**

        -   **Servidores downstream**

        -   **Sincronizações**

        -   **Relatórios**

        -   **Opções**

Neste exemplo, o grupo dois níveis abaixo da ramificação computadores Desktop (Desktops L2) tem uma prioridade maior que o grupo um nível abaixo da ramificação servidor (servidores L1). Da mesma forma, para um computador que tenha associação em Desktops-L2 e os grupos de servidores-L1, todas as ações para o grupo Desktops-L2 têm prioridade sobre as ações especificadas para o grupo de servidores-L1.

#### <a name="BKMK_Install"></a>Prioridade de instalação e desinstalação
As ações de instalação substituem as ações de desinstalação. As instalações necessárias substituem as instalações opcionais (as instalações opcionais só estão disponíveis via API e, se uma aprovação de atualização for alterada via Console de Administração do WSUS, todas as aprovações opcionais serão limpas.)

#### <a name="BKMK_Deadline"></a>Prioridade dos prazos finais
As ações que têm uma data limite substituem aquelas que não têm.  As ações com data limite anterior substituem aquelas com datas limite posteriores.

## <a name="BKMK_1.6."></a>1.6. Planejar considerações de desempenho do WSUS
Há algumas áreas que você deve planejar cuidadosamente antes de implantar o WSUS para ter um desempenho otimizado. As áreas cruciais são:

-   Configuração da rede

-   Download adiado

-   Filtros

-   Instalação

-   Implantações de grandes atualizações

-   Serviço de Transferência Inteligente em Segundo Plano (BITS)

### <a name="BKMK_1.6.Network"></a>Configuração de rede
Para otimizar o desempenho em redes do WSUS, avalie as seguintes sugestões:

1.  Configurar redes do WSUS em um topologia hub e spoke em vez de uma topologia hierárquica.

2.  Usar classificação de máscaras de rede DNS para computadores clientes móveis e configurar estes para obter atualizações do servidor do WSUS local.

### <a name="BKMK_1.6.Deferred"></a>Download adiado
Você pode aprovar atualizações e baixar os metadados de atualização antes de baixar os arquivos de atualização. Esse método é chamado *downloads adiados*. Ao adiar downloads, uma atualização é baixada somente depois de ser aprovada. É recomendável adiar downloads porque isso otimiza a largura de banda da rede e o espaço em disco.

Em uma hierarquia de servidores do WSUS, o WSUS define automaticamente todos os servidores downstream que usarão a configuração de download adiado do servidor do WSUS raiz. Você pode alterar essa configuração padrão. Por exemplo, é possível configurar um servidor upstream para que execute sincronizações imediatas completas e configurar um servidor downstream para adiar os downloads.

Se você implantar uma hierarquia de servidores do WSUS conectados, nossa recomendação é não aninhar profundamente os servidores. Se você habilitar downloads adiados e um servidor downstream solicita uma atualização que não está aprovada no servidor upstream, a solicitação do servidor downstream forçará um download no servidor upstream. O servidor downstream baixa a atualização em uma sincronização subsequente. Em uma hierarquia profunda de servidores do WSUS, atrasos podem ocorrer à medida que as atualizações são solicitadas, baixadas e passadas pela hierarquia de servidores. Por padrão, os downloads adiados são habilitados quando você armazena as atualizações localmente. É possível alterar essa opção manualmente.

### <a name="BKMK_1.6.Filters"></a>Filtros
O WSUS permite filtrar sincronizações de atualização por idioma, produto e classificação. Em uma hierarquia de servidores do WSUS, o WSUS define automaticamente todos os servidores downstream que usarão as opções de filtragem de atualizações escolhidas no servidor do WSUS raiz. Você pode reconfigurar os servidores de download para que recebam somente um subconjunto dos idiomas.

Por padrão, os produtos que serão atualizados são o Windows e o Office, e as classificações padrão são as atualizações Críticas, de Segurança e de Definição. Para conservar a largura de banda e o espaço em disco, é recomendável limitar os idiomas aos que você realmente usa.

### <a name="BKMK_1.6.Installation"></a>Instalação
As atualizações normalmente consistem em novas versões de arquivos que já existem no computador que está sendo atualizado. Em um nível binário, esses arquivos existentes podem não ser muito diferentes das versões atualizadas. O recurso de arquivos de instalação expressa identifica os bytes exatos entre as versões, cria e distribui atualizações somente dessas diferenças e mescla o arquivo existente juntamente com os bytes atualizados.

Às vezes, esse recurso é chamado de "entrega delta" porque ele baixa somente a diferença (delta) entre duas versões de um arquivo. Os arquivos de instalação expressa são maiores do que as atualizações distribuídas aos computadores clientes porque o arquivo de instalação expressa contém todas as versões possíveis de cada arquivo que será atualizado.

Você pode usar arquivos de instalação expressa para limitar a largura de banda consumida na rede local, porque o WSUS transmite apenas o delta aplicável a uma versão específica de um componente atualizado. No entanto, isso ocorre à custa de largura de banda adicional entre o servidor do WSUS, quaisquer servidores do WSUS upstream e Microsoft Update e requer espaço adicional no disco local. Por padrão, o WSUS não usa arquivos de instalação expressa.

Nem todas as atualizações são ótimas candidatas para distribuição usando os arquivos de instalação expressa. Se você escolher essa opção, obterá os arquivos de instalação expressa para todas as atualizações. Se você não armazenar as atualizações localmente, o Windows Update Agent decidirá se deseja baixar os arquivos de instalação expressa ou as distribuições de atualização completas.

### <a name="BKMK_1.6.LargeUpdates"></a>Implantações de grandes atualizações
Ao implantar grandes atualizações (como os service packs), é possível evitar a saturação da rede seguindo estas práticas:

1.  Usar a otimização do BITS. As limitações de largura de banda do BITS podem ser controladas por horário no dia, mas elas se aplicam a todos os aplicativos que estão usando o BITS. Para saber como controlar a limitação do BITS, consulte [Group Policies (Políticas de Grupo)](https://msdn.microsoft.com/library/windows/desktop/aa362844(v=vs.85).aspx)

2.  Usar a otimização do IIS (Serviços de Informações da Internet) para limitar a otimização a um ou mais serviços Web.

3.  Usar grupos de computadores para controlar a distribuição. Um computador cliente se identifica como membro de um determinado grupo de computadores quando envia informações ao servidor do WSUS. O servidor do WSUS usa essas informações para determinar quais atualizações devem ser implantadas nesse computador. Você pode configurar vários grupos de computadores e aprovar em sequência grandes downloads de service packs para um subconjunto desses grupos.

### <a name="BKMK_1.6.BITS"></a>Serviço de transferência inteligente em segundo plano
O WSUS usa o protocolo BITS para todas as tarefas de transferência de arquivos. Isso inclui downloads para computadores clientes e sincronizações de servidores. O BITS permite que os programas baixem arquivos usando a largura de banda sobressalente. O BITS mantém transferências de arquivos através de desconexões da rede e reinicializações do computador. Para obter mais informações, consulte: [Serviço de transferência inteligente em segundo plano](https://msdn.microsoft.com/library/bb968799.aspx).

## <a name="BKMK_1.7."></a>1.7. Planejar definições das atualizações automáticas
Você pode especificar um prazo limite para aprovar atualizações no servidor do WSUS. O prazo limite faz com que os computadores clientes instalem a atualização em um horário específico, mas há inúmeras situações diferentes, dependendo de se o prazo limite expirou, se há outras atualizações na fila para o computador instalar e se a atualização (ou outra atualização na fila) exige uma reinicialização.

Por padrão, as Atualizações Automáticas sondam o servidor do WSUS para verificar se há atualizações aprovadas a cada 22 horas menos um deslocamento aleatório. Se for necessário instalar as novas atualizações, elas serão baixadas. O tempo entre cada ciclo de detecção pode ser manipulado de 1 a 22 horas.

Você pode manipular as opções de notificação da seguinte maneira:

1.  Se as Atualizações Automáticas estiverem configuradas para notificar o usuário das atualizações prontas para ser instaladas, a notificação será enviada ao log de Sistema e à área de notificação do computador cliente.

2.  Quando um usuário com credenciais apropriadas clica no ícone da área de notificação, as Atualizações Automáticas exibem as atualizações disponíveis que serão instaladas. O usuário precisa clicar em **Instalar** para iniciar a instalação. Uma mensagem aparecerá se a atualização que o computador seja reiniciado para concluir a atualização. Se for solicitada uma reinicialização, as Atualizações Automáticas não poderão detectar atualizações adicionais enquanto o computador não for reiniciado.

Se as Atualizações Automáticas estiverem configuradas para instalar atualizações em uma agenda definida, as atualizações aplicáveis serão baixadas e marcadas como prontas para ser instaladas. As Atualizações Automáticas notificam os usuários que têm credenciais apropriadas usando um ícone de área de notificação, e um evento é registrado no log de Sistema.

No dia e horário agendado, as Atualizações Automáticas instalam a atualização e reinicias o computador (se necessário), mesmo se nenhum administrador local estiver conectado. Se um administrador local estiver conectado e o computador exigir uma reinicialização, as Atualizações Automáticas exibirão um aviso e uma contagem regressiva para a reinicialização. Caso contrário, a instalação ocorrerá em segundo plano.

Se for necessário reiniciar o computador, e qualquer usuário estiver conectado, uma caixa de diálogo de contagem regressiva será exibida, o que avisa o usuário sobre a reinicialização iminente. Você pode manipular as reinicializações do computador com a Política de Grupo.

Depois que as novas atualizações forem baixadas, as Atualizações Automáticas sondarão no servidor do WSUS se há a lista de pacotes aprovados para confirmar se os pacotes que ele baixou ainda estão válidos e aprovados. Isso significa que, se um administrador do WSUS remover as atualizações da lista de atualizações aprovadas enquanto as Atualizações Automáticas estão baixando as atualizações, somente as atualizações que ainda estão aprovadas na realidade serão instaladas.

