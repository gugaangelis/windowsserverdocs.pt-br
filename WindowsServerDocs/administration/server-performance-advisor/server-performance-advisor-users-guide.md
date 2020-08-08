---
title: Guia do usuário das Supervisor de desempenho do servidor
description: Guia do usuário das Supervisor de desempenho do servidor
ms.assetid: be99a5a9-b9c0-436b-912c-e5c5839a533d
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.topic: article
ms.openlocfilehash: 1a7ea3b902793f281156930a8e666c1d32d05cbe
ms.sourcegitcommit: 68444968565667f86ee0586ed4c43da4ab24aaed
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/07/2020
ms.locfileid: "87993117"
---
# <a name="server-performance-advisor-users-guide"></a>Guia do usuário das Supervisor de desempenho do servidor

>Aplica-se a: Windows Server (canal semestral), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012, Windows 10, Windows 8

Este guia do usuário para o Microsoft Server performance Advisor (SPA) fornece diretrizes sobre como usar o SPA para identificar gargalos de desempenho em sistemas implantados em várias funções de servidor.

SPA pode ajudá-lo com as seguintes ações:

* Gerencie o desempenho do servidor e solucione problemas de desempenho do servidor.

* Forneça relatórios de dados e recomendações sobre problemas comuns de configuração e desempenho.

* Fornecer recomendações de práticas recomendadas com base nos dados coletados.

> [!NOTE]
> O Console do SPA não faz quaisquer alterações aos servidores.

Para obter mais informações sobre como desenvolver pacotes do SPA Advisor, consulte [Server Performance Advisor Pack Development Guide](server-performance-advisor-pack-development-guide.md).

## <a name="server-performance-advisor-overview"></a>Visão geral do Server Performance Advisor


Esta seção explica o histórico do SPA, descreve seu público-alvo e os cenários e apresenta uma visão geral da arquitetura da ferramenta.

### <a name="history-of-spa"></a>Histórico do SPA

A versão original do Microsoft Server Performance Advisor (SPA) foi lançada em 2005-2006. Ele destina-se principalmente ao Windows Server 2003, incluindo o sistema operacional principal e as funções de servidor, como Serviços de Informações da Internet (IIS) e Active Directory. O objetivo da ferramenta é para ajudá-lo a avaliar o desempenho do servidor e solucionar problemas de desempenho do servidor.

SPA fornece recomendações e relatórios de dados para os administradores do sistema sobre problemas de desempenho e de configuração comuns. SPA coleta dados relacionados ao desempenho de várias fontes em servidores, por exemplo, contadores de desempenho, as chaves do registro, WMI consultas, arquivos de configuração e Event Tracing for Windows (ETW). Com base nos dados de desempenho de servidor que coleta, SPA pode fornecer uma visão detalhada a situação atual de desempenho de servidor e recomendações de problema sobre o que pode ser melhorado.

Foram mais de um milhão de downloads da ferramenta SPA original e ajudou muitos administradores de sistema a gerenciar o desempenho de seus servidores. Ele tem sido uma ferramenta de solução de problemas de desempenho muito bem-sucedida.

Desde o lançamento do SPA original, há várias alterações importantes no mundo de desempenho do servidor. Notavelmente, a versão do Windows Server 2012 R2, Windows Server 2012, Windows Server 2008 R2 e Windows Server 2008, com novas versões correspondentes de funções de servidor, como os serviços de informações da Internet (IIS) 8.5. Versões mais recentes do Server Performance Advisor estendem a capacidade do SPA original de recentes sistemas operacionais de servidor e funções de servidor.

Embora o SPA 3.1 compartilha os mesmos objetivos de design e o paradigma de análise de desempenho como a ferramenta original, ele foi reescrito com a tecnologia mais recente. Ele também tem os seguintes aprimoramentos principais:

* Pode executar análises em servidores que executam o Windows Server 2012 R2, Windows Server 2012, Windows Server 2008 R2 e Windows Server 2008.

* Oferece suporte ao recurso de análise remota, que permite que o SPA 3.1 coletar e analisar dados de um console central, sem precisar instalar o código nos servidores a serem analisados.

* Usa um Microsoft SQL Server para análise de dados e armazenamento, que permite processar e armazenar grandes volumes de dados.

* Separa a ferramenta de pacotes de Supervisor. Lógica de análise de desempenho para cada função de servidor é liberada na forma de um pacote de Supervisor, que pode ser liberado ou atualizado separadamente da ferramenta.

* Fornece o Supervisor de pacotes que são desenvolvidos em scripts SQL com uma arquitetura aberta. Partes não-Microsoft podem desenvolver pacotes advisor ou estender os pacotes existentes do advisor da Microsoft para abordar necessidades especiais.

* Oferece suporte a novos recursos, como relatórios de comparação lado a lado e tendências e históricos gráficos para ajudá-lo a encontrar anormalidades.

* Fornece recursos como modificam, importação e exportação de limites para ajudá-lo bem ajustar os relatórios e notificações e compartilham esses tunings com outros usuários do SPA.

* Oferece suporte a vários projetos, que podem ser usados para agrupar os servidores de destino.

* Fornece a coleção recorrente de dados no console do SPA.

* Permite consultas personalizadas e geração de relatórios por meio do Microsoft SQL Server (para usuários avançados).

* Cmdlets do Windows PowerShell personalizados estão disponíveis para uso com SPA

* Compatível com versões anteriores para importar e exibir relatórios a partir de um banco de dados do SPA 3.0

### <a name="target-audience"></a>Público-alvo

A ferramenta SPA é projetada principalmente para administradores de sistema que gerenciam a menos de 100 servidores em várias funções de servidor. Ele também pode ser usado pelos engenheiros de suporte para coletar dados de desempenho e solucionar problemas de desempenho para os clientes.

SPA oferece recomendações para ajudá-lo a resolver problemas de desempenho; No entanto, ele se baseia suposições que podem não ser aplicáveis ao seu ambiente de servidor específico. As recomendações oferecidas pelo SPA devem ser consideradas sugestões. Esperamos que os usuários do SPA ter um bom entendimento de suas configurações de sistema, casos de uso e o impacto de ajuste do sistema no comportamento geral do sistema. Os administradores de sistema devem decidir se as recomendações do SPA devem ser aplicadas ao seu ambiente.

### <a name="what-s-new-in-spa-31"></a><a href="" id="what-s-new-in-spa-3-1-"></a>Quais s novidades no SPA 3,1?

Os seguintes recursos e aprimoramentos foram adicionados no SPA 3.1:

* O Active Directory Advisor Pack ajuda a analisar o desempenho geral da função de servidor dos serviços de domínio do Active Directory do servidor.

* Suporte para os desenvolvedores adicionar alerta perdida de notificações de eventos ETW nos pacotes do Supervisor e tornar visível o alerta foi gerado um relatório e eventos foram perdidos devido à alta frequência log lento ou muito quando sustentados disco

### <a name="target-scenarios"></a>Cenários de destino

A seguir é os cenários de destino para SPA:

* **Ambiente de servidor**

    SPA é projetado para configurar e manter facilmente. Aplica-se aos ambientes de servidor que usam servidores de 1 a 100. O SPA não será bem dimensionado se você estiver tentando gerenciar o desempenho de mais de 100 servidores. Para ambientes maiores ou mais sofisticados, considere usar o System Center Operations Manager.

* **Solucionar problemas de desempenho**

    Você pode usar o SPA como um ferramenta de solução de problemas de desempenho. Ele fornece a capacidade de coletar dados de desempenho de alto nível, ele executa uma postagem completa de processamento nos dados para fornecer uma melhor compreensão sobre seu comportamento geral do sistema e ele sinaliza todas as anomalias. Quando houver suspeita de um problema de desempenho por um cliente, você pode usar o SPA para coletar e analisar dados de desempenho do servidor.

    SPA gera um relatório que você pode exibir. SPA relatórios fornecem uma lista de notificação que realça os potenciais problemas e uma seção de dados que inclui várias configurações para o servidor, configurações e o índice de desempenho. Você pode usar esse relatório para identificar o problema de desempenho específicos e usar as recomendações para encontrar soluções para o problema. Relatórios podem ser comparados com outros relatórios que foram gerados em um momento diferente ou por um servidor diferente. Usando essa comparação lado a lado, você pode determinar as diferenças entre o comportamento de linha de base normal versus anormal.

    **Observação** O SPA não foi projetado para ser uma ferramenta de depuração ou de medição. Além disso, da perspectiva dos servidores, ele pode ser considerado uma ferramenta somente leitura e não modifica as configurações dos servidores.



* **Monitoramento de desempenho do índice**

    Você pode usar o SPA para monitorar o índice de desempenho de servidores. Você pode optar por executar o SPA rotineiramente em servidores para coletar dados de desempenho e, em seguida, executar um gráfico de tendências ou um gráfico de histórico para identificar as anormalidades. Você pode exibir o relatório para uma determinada análise, obter mais informações sobre o problema de desempenho e use as recomendações ou outros dados de relatório para resolver o problema.

### <a name="spa-architecture"></a>Arquitetura do SPA

A lógica de coleta de dados do SPA baseia-se um protocolo no Windows chamado Logs de desempenho e alertas (CCP). CCP permite que os programas coletar dados de desempenho de servidores locais ou remotos, como contadores de desempenho, consultas WMI, rastreamentos ETW, chaves do registro e arquivos de configuração. Quando o SPA executa a análise de desempenho em um servidor de destino, ele cria um conjunto de Coletores de dados CCP baseado no pacote do Supervisor específico do SPA que você selecionou. O pacote do advisor contém a fonte de dados a serem coletados e o compartilhamento de arquivo onde os logs serão armazenados. Coleta de dados do SPA armazena apenas uma única conta de usuário; a mesma conta de usuário usada pelo CCP também é usada para gravar os logs.

SPA usa um banco de dados do SQL Server para armazenar os logs de desempenho são coletados dos servidores de destino. SPA importa todos os logs de desempenho do compartilhamento de arquivos para o banco de dados e, em seguida, usa a lógica de análise de dados dentro de cada pacote advisor para processar os dados e gerar relatórios. Um pacote advisor analisa os dados de desempenho coletados dos servidores de destino e gera relatórios SPA. Para obter mais informações sobre a criação de um pacote do advisor, consulte [Server Performance Advisor Pack Development Guide](server-performance-advisor-pack-development-guide.md).

Interfaces de usuário do console SPA e interações são criadas como parte de SPAConsole.exe. Você pode usar o console para criar um banco de dados, adicionar ou remover pacotes de Supervisor, gerenciar servidores de destino, execute a análise de desempenho e exibir relatórios de desempenho.

O console do SPA pode ser executado nos seguintes sistemas operacionais:

* Windows 8.1

* Windows 8

* Windows 7

* Windows Server 2012 R2

* Windows Server 2012

* Windows Server 2008 R2

* Windows Server 2008

Em um aplicativo de negócios típico, existem três camadas: a camada de apresentação, a camada de lógica comercial e a camada de armazenamento. O SPA é projetado como um produto de duas camadas do console e do banco de dados. O console serve como uma camada de apresentação com determinados lógica relacionados ao processo incluída e serve o banco de dados como a camada de armazenamento e a camada de lógica de negócios. O console de entrada do usuário de captura e controla as etapas para a coleta de dados, processamento de dados e geração de relatórios. SPA não depende de serviços de sistema do Windows.

O diagrama a seguir mostra a arquitetura de alto nível do sistema SPA. O processo é:

1.  No console do SPA, executar a análise de desempenho nos servidores especificados.

2.  Quando os dados de desempenho são coletados, CCP nos servidores de destino grava os logs para o compartilhamento de arquivo especificado pelo conjunto de Coletores de dados.

3.  Após a coleta de dados completa em um computador de destino, o console do SPA importa os logs no banco de dados do SQL Server.

4.  O console invoca a lógica de processamento de dados do pacote advisor específico.

5.  O pacote de Supervisor processa os dados e chama as APIs do SPA para inserir registros em tabelas de relatório do sistema que são definidas pela estrutura do SPA.

6.  Você pode usar o Visualizador de relatório dentro do console para exibir os relatórios gerados. Para ajudá-lo a solucionar problemas de desempenho, o SPA fornece três tipos de relatórios: único relatórios, relatórios de lado a lado e tendência ou gráficos históricos. Para obter mais informações sobre relatórios, consulte [Exibindo relatórios](#bkmk-viewingreports).

![arquitetura do SPA](../media/server-performance-advisor/spa-user-manual-architecture.png)

## <a name="getting-started-with-spa"></a>Introdução ao SPA


### <a name="requirements"></a>Requisitos

O console do SPA pode ser instalado no Windows 8.1, Windows 8, Windows 7, Windows Server 2012 R2, Windows Server 2012, Windows Server 2008 R2 e Windows Server 2008. Não há suporte para a execução SPA em versões anteriores do sistema operacional Windows Server. SPA é executada em x86 ou x64, mas ele não oferece suporte as arquiteturas IA64 ou ARM.

SPA é baseado no Windows Presentation Foundation (WPF) 2.0, que faz parte do Microsoft .NET Framework 4, então o .NET Framework 4 é necessário.

Você também precisa instalar o SQL Server 2008 R2 Express no mesmo computador onde o SPA está instalado. SQL Server 2008 R2 Express é um mecanismo de banco de dados gratuito que é lançado pela Microsoft. Você pode baixar o Microsoft SQL Server 2008 R2 Express do Microsoft Download Center. Embora, sugerimos Express do SQL Server 2008 R2, as versões mais recentes do SQL Server também podem ser compatíveis com o SPA.

**Observação** O SPA não inclui SQL Server ou o .NET Framework como parte do pacote de instalação do SPA. Depois de instalar o Microsoft SQL Server 2008 R2 e o .NET Framework 4.0, é recomendável que você execute o Windows Update antes de instalar o SPA.



Como os usuários podem criar e gerenciar bancos de dados com SPA, a conta de usuário usada para executar o SPA deve ter os mesmos privilégios de administrador do SQL Server.

### <a name="setting-up-spa"></a><a href="" id="bkmk-setupspa"></a>Configurar o SPA

O SPA é empacotado como um arquivo. cab que inclui todos os binários da estrutura SPA, os cmdlets do Windows PowerShell que são usados em cenários avançados e os seguintes pacotes do Advisor: sistema operacional principal, Hyper-V, Active Directory e IIS. Após extrair o arquivo. cab para uma pasta, nenhuma instalação adicional é necessária. No entanto, para executar o SPA, você precisa habilitar a coleta de dados de servidores de destino da seguinte maneira:

* Para executar a coleta de dados do CCP, a conta de usuário que você usa para executar o console do SPA deve ser parte do grupo de segurança Administradores no servidor de destino. Se o servidor de destino e o console estão no mesmo domínio, a conta de usuário de domínio deve ser parte do grupo de segurança Administradores no servidor de destino. Se o servidor de destino e o console não estão no mesmo domínio, crie uma conta de usuário administrativo no servidor de destino com o mesmo nome de usuário e a senha da conta de usuário que você usa para executar o console do SPA.

* Crie uma pasta compartilhada para obter os resultados no servidor.

* Certifique-se de que a conta de usuário usada para executar o console do SPA leu e permissões de gravação para a pasta compartilhada. A PLA usa essa conta para gravar logs na pasta.
O console do SPA usa a mesma conta para ler os logs e importá-los para o banco de dados.

    **Observação** O ETW implementa um buffer circular para armazenar o rastreamento e o move para a pasta compartilhada quando possível. Se o servidor está ocupado ou a operação de gravação estiver lenta, o ETW descarta os rastreamentos quando o buffer estiver cheio. É importante que a pasta compartilhada esteja localizada em um servidor com acesso de e/s rápido. É recomendável que cada servidor de destino tem uma pasta compartilhada para minimizar a perda de dados causada por e/s de arquivo lenta.



* Para CCP acessar servidores de destino, defina o Firewall do Windows para permitir acesso de alertas e Logs de desempenho remoto nos servidores de destino. CCP usa a porta TCP 139.

* Verifique se os **logs de desempenho & serviço alertas** está em execução.

* Se o console e o servidor de destino estiverem localizados em sub-redes diferentes, você também precisará definir o campo endereço IP remoto nas regras de firewall de entrada nas configurações de **escopo** na página **logs e alertas de desempenho** , conforme mostrado aqui.

    ![Propriedades do CCP](../media/server-performance-advisor/spa-user-manual-pla-firewall.png)

* Ative descoberta de rede no console e em cada um dos servidores de destino.

* Se o servidor de destino não estiver ingressado em um domínio, habilite a seguinte configuração do registro: **HKLM \\ software \\ Microsoft \\ Windows \\ CurrentVersion \\ Policies \\ System \\ LocalAccountTokenFilterPolicy**.

**Observação** Por padrão, o SPA grava os logs de diagnóstico na pasta onde SpaConsole.exe está localizado. Se o SPA estiver instalado na pasta arquivos de programas, o SPA só poderá gravar o log quando SpaConsole.exe for executado como um administrador.

Se você quiser executar a análise de dados no computador do console, será necessário executar o SPA como administrador. CCP passa por um caminho de código diferente ao executar em um computador local, o que requer privilégios de administrador.

Se você quiser executar o pacote do supervisor do SPA do IIS em seus servidores, será necessário habilitar a consulta WMI e o rastreamento ETW para o servidor IIS. Você pode fazer isso habilitando **rastreamento** sob o **integridade e diagnóstico** serviço de função e **Ferramentas e Scripts de gerenciamento do IIS** em **Ferramentas de gerenciamento** do **servidor Web (IIS)** função de servidor.



### <a name="creating-your-first-project"></a>Criando seu primeiro projeto

Depois que tudo estiver configurado, você pode criar seu primeiro projeto SPA. Conforme descrito na seção anterior, o SPA armazena tudo que estiver relacionado à análise de dados de desempenho dentro de um banco de dados. Cada projeto SPA corresponde a um único banco de dados. A **primeira vez** que o assistente de uso o orienta durante as etapas a seguir.

**Para criar um projeto do SPA**

1.  Inicie SpaConsole.exe. O console entra em um modo desconectado, onde o SPA não está conectado a qualquer banco de dados, e a janela principal está em branco.

2.  Para criar um novo projeto, clique em **arquivo**, e, em seguida, clique em **novo projeto**. Isso é iniciado pela primeira vez que o assistente de uso. A primeira página mostra as etapas que você seguirá ao usar o assistente:

    * Criar um banco de dados

    * Provisionar os pacotes do advisor

    * Adicionar servidores à lista de servidores de destino

3.  Clique em **Próximo**. A página **criar banco de dados do projeto** solicita que você forneça o nome da instância de Microsoft SQL Server onde você deseja criar seu banco de dados. Por exemplo, se ele estiver no mesmo computador que o console, você poderá usar **localhost \\ &lt; &gt; seu nome do SQL Server**.

    **Observação** O nome de instância padrão para uma instalação expressa do SQL Server 2008 R2 é SQLExpress. Para uma instância do SQL Server 2008 R2 Express instalado no computador local, o banco de dados normalmente por padrão para **localhost\\SQLExpress**. No entanto, ele pode foram alterado durante a instalação do SQL Server, então você precisa certificar-se de que você use o nome da instância do SQL Server à direita.



4.  Forneça o nome do banco de dados. Somente letras, dígitos e sublinhados (\_) são permitidos como caracteres válidos para um nome de banco de dados. Uma sugestão razoável para o nome de banco de dados do SPA seria **SPA**. Se você inserir um nome inválido, um ícone vermelho de erro é exibida. A dica de ferramenta associada fornece o motivo da falha de validação.

    **Observação** É importante lembrar o nome do banco de dados e o nome da instância do servidor, pois esses são os únicos identificadores do seu projeto. Você precisa fornecer essas informações se você desejar alternar para esse banco de dados.



5.  Depois de fornecer o nome da instância do servidor e o nome do banco de dados, o assistente de primeiro uso gera o local do arquivo de banco de dados.

6.  Na página **criar banco de dados do projeto** , clique em **Avançar**. A primeira vez que o assistente de uso cria um banco de dados e gera todos os esquemas de banco de dados, funções e procedimentos armazenados relacionados ao SPA no banco de dados. Esta etapa pode levar vários segundos dependendo da velocidade de hardware e de rede.

    **Observação** se essa etapa falhar, uma mensagem de erro será exibida. Alguns dos problemas mais comuns são: Console não pode se conectar à instância do SQL Server, privilégios insuficientes para criar o banco de dados, ou o nome do banco de dados já existe.



7.  Quando a etapa anterior for realizada com sucesso, você verá a página **provisionar pacote do Advisor** . Ela lista todos os pacotes de supervisor que estão disponíveis em seu computador. SPA verifica automaticamente a pasta denominada **APs** sob o diretório raiz do SPA. Ele lista o nome completo, versão e autor de cada pacote de Supervisor.

    **Observação** para obter mais informações sobre como o nome completo e a versão são usados no Spa, consulte [Gerenciando pacotes do Advisor](#bkmk-manageadvisorpacks)



8.  Escolha quais pacotes de supervisor que deseja provisionar o banco de dados do projeto e, em seguida, clique em **próxima**. Ou você pode clicar em **Ignorar** para mover para a próxima etapa sem o provisionamento de todos os pacotes de Supervisor.

    **Observação** Você pode provisionar os pacotes do Advisor sempre que estiver usando a ferramenta. Para obter mais informações, consulte [Gerenciando pacotes advisor](#bkmk-manageadvisorpacks).



9.  Na página **adicionar servidores** , para cada servidor a ser adicionado à lista servidor de destino, há dois campos obrigatórios a serem preenchidos: **nome do servidor** e **local de compartilhamento de arquivos**.

    **Observação** Há também um campo de **Comentário** , que é usado principalmente para classificar ou localizar o servidor. Em casos onde você tem vários servidores, você pode importar um arquivo de valor (. csv) separada por vírgulas que contém o nome do servidor, a pasta de resultado e o campo de comentário opcional. O campo de **Comentário** é usado para descrever o servidor e o termo pode ser usado para filtrar servidores para coleta de dados. Se você estiver inicializando os servidores por meio do arquivo. csv, um erro de análise dentro do arquivo não carregará os servidores.



10. Várias configurações precisam ser definidas para habilitar a coleta de dados do CCP, conforme descrito em [Configurando SPA](#bkmk-setupspa). A página **Adicionar servidor** fornece uma funcionalidade de configuração de teste para ajudá-lo a solucionar problemas de configuração. Marque a caixa de seleção associada ao computador e clique em **testar conectividade**. SPA tenta gerar um coletor de dados definidas em servidores de destino, e ele tenta importar que os resultados de volta para o banco de dados. Se tudo estiver correto, o **Status** mostra **passar**. Se ele falhar, será exibida uma dica de ferramenta que descreve o motivo da falha.

11. Cada um dos servidores é adicionado automaticamente ao banco de dados, mesmo se ele não passar no teste de configuração. Para remover os servidores da lista, selecione o nome do servidor e clique em **remover**.

12. Quando tudo for concluído, clique em **concluir** para fechar o assistente pela primeira vez. Se você fechar o assistente pela primeira vez para usar o antes de concluir, todas as etapas anteriores persistirão e nenhum deles será revertido automaticamente. Você precisa fazer alterações futuras manualmente.

## <a name="running-analysis"></a>Análise de execução


Depois de configurar o banco de dados, você pode executar a análise de desempenho nos servidores.

Sempre que inicia o console do SPA, o último projeto que foi usado pelo usuário atual será aberta automaticamente. A janela principal contém uma lista de servidores. Cada servidor tem quatro propriedades: nome do servidor, resultado da análise, status atual e comentário.

* **Nome do servidor** o nome do servidor, que é o identificador do servidor. Não há nomes duplicados são permitidos.

* **Resultado da análise** por padrão, ele mostra o resultado da análise de desempenho mais recente execução no servidor. Se não houve alguma análise de desempenho executado no servidor, ele mostra **Nenhum relatório**. Se houver um aviso gerado pelo relatório, ele mostra **Aviso** e o carimbo de hora quando o relatório mais recente foi gerado. Se nenhum problema foi encontrado durante a análise mais recente no servidor, ele mostra **OK** e o carimbo de hora.

    **Observação** se você alterou recentemente uma configuração do sistema, recomendamos executar a análise novamente para avaliar o impacto geral da alteração e obter um relatório atualizado do estado do sistema. SPA não controla as alterações de configuração para o sistema em teste.



* **Status atual** mostra o status de desempenho tarefas de análise atualmente em execução no servidor. Você pode cancelar uma tarefa em execução clicando o **Cancelar** ícone, que é designado por um X vermelho.

* **Comentário** Descreve o servidor de destino atual. Por exemplo, você pode descrever o servidor usando a função de servidor (por exemplo, SQL Server) ou um local (por exemplo, Kent). SPA usa o **nome do servidor** e o **Comentário** para ajudar a Pesquisar e localizar o servidor adequado. Você pode digitar na caixa de texto de pesquisa. Se o **nome do servidor** ou as colunas de **Comentário** contiverem a cadeia de caracteres exata inserida na caixa de pesquisa, o servidor será exibido na lista de servidores.

Os seguintes controles também estão disponíveis no console:

* **Repita** uma caixa de seleção que descreve a capacidade de repetir regularmente uma coleção com base em um intervalo de tempo. Para a maioria das instalações de servidor, convém ter uma coleção de SPA repita por hora para que o histórico suficiente para análise. Se você quiser executar a coleta apenas uma vez, você não deve selecionar o **Repetir** caixa de seleção.

* **remover recorrência** Um botão que permite cancelar um trabalho de coleta de repetição em andamento. Cancela a coleção de repetição, mas não a coleção atual (se houver) está em andamento. Esta opção permite que você redefinir um novo intervalo de repetição de coleção ou execute a coleção manualmente.

Antes de iniciar a análise de desempenho, selecione a duração da coleta de dados. Embora coletar mais dados ajuda a fornecer uma imagem mais precisa da situação de desempenho do servidor, ele também gera um número maior de logs e ele poderia ter mais impacto potencial no servidor. Escolha a duração da coleta de dados apropriados com base em suas necessidades específicas. Cada pacote advisor define um mínimo de duração válido. A duração da coleta de dados que você escolher deve ser maior que a duração mínima dos pacotes de Supervisor selecionado.

Para executar a análise de desempenho em servidores de destino, selecione os servidores nos quais você deseja executar a análise de desempenho e clique em **executar análise** uma caixa de diálogo é exibida e solicita que você selecione os pacotes do Advisor a serem executados nos servidores. Os pacotes selecionados advisor executados simultaneamente. Os relatórios são gerados se baseiam os dados de desempenho coletados durante o mesmo período de tempo.

**Observação** se você selecionar um servidor que tenha uma análise de desempenho recorrente em execução, o botão **remover recorrência** permitirá que você cancele a coleta de dados recorrentes. SPA não permite várias sessões de coleta de dados ao mesmo tempo no mesmo computador.



## <a name="viewing-reports"></a><a href="" id="bkmk-viewingreports"></a>Exibindo relatórios


SPA, há três tipos de relatório de análise de desempenho: um único relatório, relatório lado a lado e tendências e gráficos históricos.

Após a análise de desempenho em execução, um relatório é gerado para cada um dos pacotes de Supervisor executados no computador de destino. Na lista server na janela principal, você pode expandir **resultado da análise** para ver todos os pacotes de supervisor que foram executados em um servidor específico. Você pode clicar em um nome de relatório para exibir um único relatório.

Há três ícones ao lado do nome do pacote de supervisor que mostram o status de análise mais recente executado no servidor:

* O **mais recente** ícone mostra o relatório foi gerado pela análise de desempenho mais recente no servidor para o pacote de Supervisor.

* O ícone **Localizar** mostra a lista de relatórios de análise de desempenho, que permite que você escolha o relatório correto. O **pacote Advisor** e **servidor de destino** campos são já preenchidos com as atuais advisor pack e destino informações do servidor. O intervalo de tempo padrão é definido como uma semana e a data de término é definida como hoje. Se você clicar no **pesquisa** botão no canto superior direito, você pode obter uma lista de todos os relatórios de análise de desempenho para o pacote de servidor e Supervisor selecionado dentro do intervalo de tempo.

* O **exibir gráficos** ícone abre a tendência e a exibição de gráfico de histórico.

A figura a seguir mostra os ícones **mais recentes**, **Localizar**e **exibir gráficos** após cada pacote do Advisor:

![ícones de relatório](../media/server-performance-advisor/spa-user-manual-report-icons.png)

### <a name="searching-for-and-within-reports"></a>Pesquisar e nos relatórios

Procurando relatórios é feita usando **Explorer relatório**. Isso permite que você procure relatórios, intervalo de datas, o nome do servidor e o pacote advisor. Essa é a maneira recomendada para localizar um relatório de interesse que não seja o último relatório de execução. O último relatório de execução está disponível por meio de **Exibir relatório** para o servidor.

Ao exibir um relatório específico, você pode facilmente navegue até o relatório próximo e anterior por tempo ou examinar um relatório relacionado, como um ponto de acesso diferente em execução ao mesmo tempo. Essas opções estão disponíveis em **ações**.

Também é possível pesquisar dentro de um relatório. Um número de relatórios tem uma caixa de pesquisa de **Localizar** cadeia de caracteres disponível para pesquisa de cadeia de caracteres de texto rápido no relatório. Para remover a caixa de texto, você pode descartá-la. Para ativar uma caixa de pesquisa (no windows com o texto de pesquisa), você pode usar CTRL + F atalho. A caixa **Localizar** permite que o usuário especifique uma pesquisa que diferencia maiúsculas de minúsculas, conforme apropriado, com a opção **corresponder caso** .

A figura a seguir mostra a caixa **Localizar** pesquisa com a **potência** da cadeia de caracteres na guia **relatório** .

![a caixa de pesquisa localizar](../media/server-performance-advisor/spa-user-manual-find-search-box.png)

### <a name="single-report"></a>Relatório simples

Um único relatório mostra os resultados da análise de desempenho do tempo de execução único de um pacote de supervisor em um único computador. O relatório exibe o nome do pacote de Supervisor, o nome do servidor de destino, o tempo que o relatório gerado e a duração de coleta de dados.

Um único relatório contém uma seção de notificação e as seções de dados.

### <a name="notification-section"></a>Seção de notificação

A seção notificação consiste em um conjunto de regras de análise de desempenho. Cada notificação contém algumas fontes de dados, alguns valores de limite e alguma lógica de negócios. Quando você executa a análise de desempenho, a lógica de negócios avalia as fontes de dados em relação aos limites para determinar se a regra passa ou não. Caso contrário, será exibido um aviso para informá-lo sobre um possível problema de desempenho. Ele também fornece recomendações para ajudá-lo a solucionar o problema. A seção de notificação é sempre a primeira guia no modo de exibição único relatório.

A seção de notificação é dividida em duas partes: **Aviso** e **outras notificações**.

se a fonte de dados de uma regra atender a determinadas condições com base nas configurações de lógica e limite, um aviso aparecerá na área de **aviso** . Um aviso inclui as seguintes partes:

* Um ícone de aviso indica a existência de um problema em potencial.

* O nome da regra. Por exemplo, **rede receber descartes de pacote** é um link que aponta para a página de detalhes da regra, conforme descrito em [Gerenciar pacotes advisor](#bkmk-manageadvisorpacks).

* Uma descrição simple sobre o problema potencial.

* Uma recomendação para uma possível solução para o problema de desempenho potencial.

Servidores diferentes podem ter configuração drasticamente diferente e padrões de uso e não é possível definir limites e regras que são aplicáveis a todos os servidores em todas as condições. SPA fornece a capacidade de modificar os limites. Você também pode optar por desabilitar uma regra se a regra não se aplica à sua situação. Por padrão, todas as regras são habilitadas. Uma regra desabilitada não aparece na área de notificação. Para obter mais informações, consulte [Gerenciando pacotes advisor](#bkmk-manageadvisorpacks).

O **outras notificações** área contém todas as outras regras, onde nenhum aviso é gerado ou a regra não é aplicável. Ele contém partes semelhante àquela encontrada no **Aviso** área. A maior diferença é que se nenhum aviso é gerado ou a regra não é aplicável, geralmente nenhuma recomendação é fornecida.

### <a name="data-sections"></a>Seções de dados

Seções de dados contêm os dados de desempenho que o pacote de Supervisor gera com base em dados brutos coletados de servidores de destino. As seções de dados incluem um conjunto de seções de nível superior e vários níveis de subseções. As seções de nível superior são apresentadas como guias. Subseções sob as seções de nível superior são apresentadas em áreas expansíveis. Você pode recolher ou expandir cada uma das seções para ajudar a focar na área de interesses, conforme mostrado na figura a seguir.

![seções de dados](../media/server-performance-advisor/spa-user-manual-data-sections.png)

O pacote de Supervisor principal SPA do sistema operacional e o IIS SPA Advisor Pack contêm um **Visão geral do sistema** seção. Esta seção inclui as informações de nível superior sobre o uso de recursos e a configuração. Outras seções de nível superior representam áreas de dados de desempenho. SPA apresenta os dados de relatório das seguintes maneiras:

* **Valor único** um par chave/valor. A chave é uma cadeia de caracteres que representa o significado do valor. O valor pode ser uma cadeia de caracteres, um valor numérico ou um valor booleano. Isso geralmente é usado para mostrar informações estáticas, como configuração, por exemplo, a arquitetura da CPU, o tamanho total da memória e a versão do BIOS, que não mudam com o passar do tempo.

* **valor da lista** Isso, às vezes, é um par chave/valor, mas o valor da lista pode conter vários campos. Por exemplo, o atributo da CPU pode ser mostrado em uma tabela com várias colunas e várias linhas. Cada linha representa uma CPU, e cada coluna representa um atributo da CPU.

* **Estatísticas de** pode ser considerado um tipo especial de valor único. Ele só pode conter dados numéricos. Durante o tempo de coleta de dados, muitos dos pontos de dados numéricos flutuam em vez de permanecer constante. Por exemplo, o uso da CPU muda sempre que o CCP coleta o contador de desempenho. Mostrar apenas um único valor não é possível refletir com precisão a situação de desempenho. Em vez de mostrar apenas um valor, o valor máximo, média, mínimo e 90% são usados para tais pontos de dados numéricos dinâmico. O valor de 90% representa a atividade ou acima do 90º percentil em todos os eventos desse contador naquele intervalo de determinada coleção.

* **Lista superior** geralmente contém os principais consumidores de um determinado recurso ou as principais entidades que apresentou a determinados eventos. Por exemplo, **10 principais processos em termos de uso de CPU médio** inclui os processos de dez principais com maior utilização de CPU média durante o tempo de coleta de dados. Como o uso da CPU é também um ponto de dados numéricos dinâmico, outras estatísticas, como o valor de 90%, mínimo e máximo também estão incluídos na lista para fornecer ao usuário uma visão mais completa do consumo de CPU.

Conforme mencionado nas seções anteriores, o SPA depende CCP coletar ETW rastreamento WMI consultas, contadores de desempenho, as chaves do registro e arquivos de configuração para gerar o relatório. É importante entender a fonte de dados por trás de cada ponto de dados no relatório. SPA fornece essas informações por meio de dicas de ferramenta. Você pode passar por colunas de chave ou linhas para exibir a dica de ferramenta de fonte de dados. Por exemplo, **WMI:Win32\_DisDrive: legenda** significa que a fonte de dados é de uma consulta do WMI, o nome da classe WMI é Win32\_é a unidade de disco e a propriedade **legenda**.

### <a name="side-by-side-report"></a><a href="" id="side-by-side-report-"></a>Relatório de lado a lado

Relatórios únicos fornecem notificações e uma seção de dados para ajudar os usuário localizar possíveis problemas de desempenho, mas geralmente é difícil identificar um possível problema de desempenho examinando diretamente um único relatório. Um único relatório pode conter muitos pontos de dados, o que torna difícil encontrar os problemas potenciais.

Para resolver esse problema, o SPA fornece a capacidade de comparar dois relatórios. Você pode comparar um relatório com um problema potencial para um relatório de linha de base para ajudar a localizar as diferenças.

Relatórios de lado a lado podem ser lançamento de um visualizador de relatório único. Os usuários podem clicar em **ações**e, em seguida, clicar em **comparar relatórios** para selecionar os relatórios. Só é significativo para comparar os relatórios do mesmo pacote de Supervisor. Você pode escolher comparar o relatório com um relatório anterior no tempo, o próximo relatório no tempo ou um relatório arbitrário que é selecionado por meio de recursos de pesquisa. Por exemplo, para isolar um comportamento anormal, você pode comparar um relatório do servidor de linha de base para um relatório que foi gerado em um momento diferente no mesmo computador ou para um relatório que foi gerado em um computador diferente que tem uma função de servidor semelhante e carregar.

Um relatório de lado a lado é muito parecido com um único relatório. Ele contém uma seção de notificação e seções de dados. Ele contém o mesmo número de notificações e seções de dados que o Visualizador de relatório único. A única diferença é que os relatórios são exibidos de maneira lado a lado. Cada seção contém os dados do relatório de origem (relatório 1) e o relatório de destino (relatório 2). O relatório de lado a lado exibe o nome do pacote do Supervisor, o nome do servidor de destino (relatório 1 à esquerda) e 2 do relatório à direita, a hora em que o relatório foi gerado e a duração da coleta de dados para cada relatório.

Se você descartar a caixa de diálogo **Localizar** , poderá reativá-la digitando Control + F. Essa caixa de diálogo localiza e realça cadeias de caracteres de texto na seção atual.

Na seção notificação, se qualquer um dos resultados dos dois relatórios comparados for um aviso, ele será listado na área de **aviso** . Caso contrário, os resultados serão listados na área **outras notificações** . Como a chave para um relatório de lado a lado é identificar as diferenças entre relatórios, não há informações detalhadas sobre uma regra são exibidas. Os usuários podem clicar para abrir o formulário de detalhes da regra para obter mais informações sobre a regra o nome da regra.

Na seção de dados, os dados são apresentados de uma maneira de lado a lado com dados de relatório 1 à esquerda e dados de relatório 2 à direita. SPA mostra valores únicos na mesma tabela, mas em vez de rótulos de colunas **valor**, eles são nomeados **Report 1** e **2 relatório** respectivamente. O relatório de lado a lado mostra todas as outras formas de dados nas tabelas de lado a lado.

O Visualizador de relatórios lado a lado também fornece dicas sobre a fonte de dados.

### <a name="historical-and-trend-charts"></a>Gráficos de histórico e tendências

Ele só faz sentido para mostrar tendências e históricos gráficos para um servidor específico e pacote advisor específico. Você precisa escolher o intervalo de tempo (que é padronizado para a última semana) e, em seguida, clique em **OK** para abrir o Visualizador de gráfico de histórico e tendências.

O gráfico de histórico e tendência visualizador tem três guias: gráfico de histórico, gráfico de tendências de 24 horas e gráfico de tendência de 7 dias.

### <a name="historical-chart"></a>Gráfico de histórico

O gráfico de histórico mostra uma série de valores para um ponto de dados numéricos por período de tempo especificado. Por exemplo, o **latência média de solicitação** para o IIS em um servidor único para os últimos 15 dias. Cada ponto de dados em um gráfico de histórico representa o valor de uma fonte de dados específicos em uma sessão de análise de desempenho.

Há algumas formas de usar um gráfico Histórico:

1.  Para ajudar a encontrar anormalidades em um determinado momento para um ponto de dados, por exemplo, às 2:00 A.M. por dia, a **latência média de solicitação** do IIS vai de cerca de 200 ms a 500 ms.

2.  Para ajudar a correlacionar os vários pontos de dados. Por exemplo, mostrando **latência média de solicitação** e **Contagem média de solicitação** para os últimos 15 dias. O relatório pode mostrar que a latência de solicitação e a solicitação de aumento da contagem ao mesmo tempo especial, que pode indicar que o aumento de latência de solicitação é causado por um aumento na contagem de solicitação.

Em um gráfico de histórico, os usuários podem fazer o seguinte:

* Mostra várias séries de dados na área de gráfico. Cada série de dados é mostrado como um gráfico de linhas no Visualizador de relatórios. Cada gráfico de linhas é reduzido automaticamente para caber no Visualizador de relatórios.

* Adicionar ou remover uma série de dados na lista série de dados na parte inferior do Visualizador gráfico Histórico.

* Mostrar ou ocultar uma série de dados na lista série de dados. Os usuários podem clicar uma série de dados específico na lista para destacar o gráfico de linha correspondente na área do gráfico.

* Nível de zoom um determinado período de tempo selecionando o período de tempo dentro da área do gráfico. Para reduzir, clique no botão localizado no canto inferior esquerdo do gráfico.

* Investigar um único relatório clicando duas vezes em um determinado ponto de dados.

* Copie os dados e torná-lo disponível para outros programas, como o Microsoft Excel. Isso permite que você utilize recursos, quando apropriado gráficos do Microsoft Excel.

### <a name="trend-charts"></a>Gráficos de tendência

Muitos problemas de desempenho repetitivas são causados por tarefas periódicas executadas em ou em servidores de destino. Por exemplo, um site orientado a diversão pode obter mais ocorrências no final de semana ou uma tarefa de backup de disco agenda pode prejudicar o desempenho de um servidor, todos os dias às 2:00.

Um gráfico de tendência é projetado para ajudá-lo a localizar esses problemas de desempenho. Problemas de desempenho podem ocorrer repetidamente em vários padrões. Os padrões mais comuns são padrões diárias e semanais padrões onde os problemas de desempenho ocorrem durante a mesma hora de um dia ou mesmo dia da semana. Portanto, o SPA fornece um gráfico de tendências de 24 horas e um gráfico de tendência de 7 dias.

O gráfico de análise de tendência fornece um nível mais profundo da investigação em um conjunto de dados, e ele procura tendências com base na hora do dia. O eixo x é definido como um período de 24 horas, começando em 0:00 (meia-noite) e terminar às 23:59. SPA não mostra as tendências de várias séries de dados ao mesmo tempo. Você pode clicar em **Selecione a série de dados** para selecionar uma série de dados para exibir.

Para processar os dados, o SPA procura todos os instantâneos tirados entre 0:00 e 0:59 para cada hora. SPA determina o mínimo, máximo, média e valores de sigma do conjunto de instantâneos tirados durante essa hora e gráficos-los como gráficos de velas. SPA repete o processo para instantâneos tirados entre 1:00 a 1:59, e 2:00 a 2:59 e assim por diante. Se nenhum existir para determinada hora, SPA deixa a hora em branco no gráfico e passa para a próxima hora.

Um gráfico de tendência de 7 dias é muito semelhante ao gráfico de tendência de 24 horas. A única diferença é que ela agrupa uma série de dados com base no dia da semana em vez de horas de um dia.

A série de dados selecionada em tendências e históricos gráficos é armazenada como uma preferência de usuário. Na próxima vez que o Visualizador de gráfico de tendência e histórico for aberto para o mesmo pacote do Advisor, o mesmo conjunto de séries de dados será listado como padrão.

## <a name="managing-reports"></a>Gerenciando relatórios


### <a name="deleting-reports"></a>excluindo relatórios

Relatórios podem ser removidos para minimizar o número de relatórios que precisam ser gerenciados pelo SPA. Dependendo da frequência de relatórios e o número de servidores, é recomendável excluir relatórios desnecessários. Ao SPA não tem um limite em que ele pode gerenciar relatórios, banco de dados subjacente pode ter um limite de tamanho.

**Observação** os relatórios excluídos não podem ser reexcluídos.



### <a name="exporting-and-importing-reports"></a>Exportando e importando relatórios

Relatórios podem ser exportados para um arquivo XML ao transporte para outro console SPA ou email para outro usuário. Exportar o relatório não exclui o relatório. Para exportar o relatório exibido no momento, de **Visualizador de relatórios**, clique em **ações**, e, em seguida, clique em **exportar**. Para exportar vários relatórios, no **Gerenciador de relatórios**, clique em **habilitar seleção múltipla**, selecione vários relatórios na caixa de seleção e clique em **Exportar**. Isso exporta os relatórios no formato XML para o diretório de destino selecionado.

Um relatório exportado pode ser exibido no SPA. os relatórios importados não são adicionados ao banco de dados SPA. Eles são destinados principalmente para servir como um aplicativo de Visualizador XML para o relatório exportado. O servidor de relatório importado não precisa ter os mesmos pacotes advisor instalados como o console SPA original do relatório exportado.

## <a name="managing-advisor-packs"></a><a href="" id="bkmk-manageadvisorpacks"></a>Pacotes de gerenciamento do advisor


O SPA inclui os pacotes do Advisor para o sistema operacional principal, o Hyper-V, o Active Directory e o IIS. SPA fornece uma arquitetura aberta para desenvolver pacotes advisor usando SQL, portanto, também é possível para os desenvolvedores não são da Microsoft criar versões dos pacotes do Supervisor. Há quatro opções para gerenciar um pacote advisor: provisionar, personalizar, redefinir ou remover.

### <a name="provision-new-advisor-packs"></a>Provisionar novos pacotes de Supervisor

Novos pacotes de advisor podem ser liberados pela Microsoft ou por desenvolvedores de terceiros. Um pacote de Supervisor inclui um provisionMetaData.xml e um conjunto de scripts SQL que descrevem a lógica.

**Para provisionar um novo pacote de Supervisor**

1.  Copie todo o conteúdo do Advisor Pack no diretório *% SpaRoot%* \\ APS.

2.  Na janela principal, clique em **configuração**, e, em seguida, clique em **configurar pacotes Advisor**. O **configurar pacotes Advisor** caixa de diálogo é aberta.

    **Observação** Essa caixa de diálogo é semelhante à página do **pacote do supervisor de provisionamento** no primeiro assistente de uso. Ele mostra uma lista de pacotes de supervisor que estão disponíveis para gerenciar. Cada pacote de Supervisor na lista tem propriedades, como nome, instalado a versão, a versão e autor. Nome é o nome completo do pacote de Supervisor e a versão instalada é a versão deste pacote de advisor já foi provisionado no projeto. Se o pacote do Advisor não for provisionado no banco de dados atual, a caixa de texto versão instalada exibirá **não instalado**. O campo versão indica a versão deste pacote de Supervisor, sob a pasta de pacotes do Supervisor é arquivado.



3.  Selecione o pacote do Advisor na lista. Se o pacote do Advisor não tiver sido provisionado ou se houver uma versão mais recente na pasta supervisor packs do que aquela no banco de dados, o botão **provisionar** será habilitado. Clique o **provisionar** botão.

4.  Quando o provisionamento for concluído, o campo **versão instalada** do pacote Advisor selecionado conterá as novas informações de versão.

### <a name="customize-advisor-packs"></a>Personalizar pacotes advisor

SPA define uma arquitetura aberta que permite aos usuários modificar os pacotes do advisor. Os usuários podem modificar os arquivos do pacote advisor alterar limites, compartilhamento de limites e habilitando ou desabilitando regras.

para obter mais informações sobre como alterar e criar pacotes do Advisor, consulte [Guia de desenvolvimento do Server performance Advisor Pack](server-performance-advisor-pack-development-guide.md).

### <a name="changing-thresholds"></a>Alterar limites

Os limites são usados em SPA para determinar se a condição de disparo de uma regra é atendida. Os limites reais cenários de cliente para o real podem variar muito devido a carga de trabalho, o ambiente de hardware e negócios precisa. Os limites padrão podem não ser adequados para o caso de usuário atual, portanto SPA fornece a capacidade de modificar o limite existente.

Você pode modificar os valores de limite, clicando no nome de regra em um relatório único ou lado a lado. Ou para gerenciar todas as regras de um pacote específico advisor, eles podem modificar os valores de limite do **configuração** menu.

**Para alterar um valor de limite**

1.  No **configuração** menu, clique em **configurar pacotes de Supervisor**, clique no nome do pacote a ser modificada e, em seguida, clique em Supervisor de **Configurar**.

    **Observação** Você verá uma lista de todas as regras incluídas no pacote do Advisor. A caixa de seleção à esquerda do nome do pacote advisor indica se a regra está habilitada. Se uma regra estiver desabilitada, ela ficará oculta em todos os relatórios.



2.  Clique na regra específica que você deseja modificar. O **detalhes de regra** formam a regra selecionada é aberta.

O formulário de detalhes da regra contém informações detalhadas sobre uma regra específica. Ele inclui nome, descrição, status, resultados possíveis e limites. SPA oferece suporte a dois tipos de resultados da regra, **Aviso** e **OK**. Para cada tipo, há texto recomendações e uma recomendação.

Algumas das regras não têm limites definidos. Por exemplo, o **HTTP Keep Alive** regra verifica uma configuração booleana para IIS. Portanto, a lista de limites pode ser vazia. Caso contrário, todos os limites usados pela regra atual serão listados. Uma descrição detalhada sobre como um limite é usado na regra é incluída como parte da descrição.

Se o formulário **detalhes da regra** for iniciado no menu **configuração** , a lista de limites terá três colunas: nome, configuração original e alterar configuração. Se ele for iniciado a partir de um relatório único ou lado a lado, os valores de limite usados pelo relatório também serão incluídos. Os usuários podem modificar os valores de limite atuais alterando o valor na coluna **Alterar configuração** e, em seguida, clicando em **salvar** para salvar as alterações no banco de dados.

Todas as alterações feitas aos limites só são aplicadas a relatórios gerados após as alterações. Os relatórios existentes não são afetados por essas alterações.

### <a name="sharing-thresholds"></a>Compartilhamento de limites

Se você gerenciar seus servidores em situações semelhantes, poderá optar por usar o mesmo conjunto de limites. Você pode exportar e importar os limites de um pacote específico advisor usando o **configuração** menu. Selecione o pacote de Supervisor específico e, em seguida, clique em **Configurar**. O arquivo exportado de limite está em um formato XML.

Ao importar um limite, o SPA valida o formato de arquivo XML e verifica se o arquivo corresponde o pacote selecionado advisor. Se for bem-sucedida, SPA importa todos os valores do arquivo de limite para o banco de dados do projeto atual. Semelhante ao cenário de limites de alteração anterior, todas as alterações de valor de limite só têm efeito em relatórios gerados no futuro. Os relatórios existentes não são afetados.

### <a name="enable-or-disable-rules"></a>Habilitar ou desabilitar regras

Uma regra pode ser habilitada ou desabilitada da **regra detalhes** formulário. Você precisa clicar **Salvar** para manter as alterações feitas. Se uma regra estiver desabilitada, ela não será exibida em nenhum dos relatórios. Mas a lógica de negócios subjacente é disparada ao gerar o relatório, portanto, quando você opta por reabilitar a regra, ela aparece nos relatórios novamente.

### <a name="reset-advisor-packs-to-original-state"></a>redefinir os pacotes do Advisor para o estado original

Você pode decidir modificar um pacote de Supervisor provisionado no banco de dados. Diferente de alterar os limites, você também pode alterar os scripts SQL. SPA não suporta alteração metadados do relatório, ou adicionando ou removendo regras para um pacote de Supervisor provisionado. Modificar manualmente essas áreas pode provocar comportamento inesperado.

para obter mais informações sobre como alterar um pacote Advisor provisionado, consulte o [Guia de desenvolvimento do Server performance Advisor Pack](server-performance-advisor-pack-development-guide.md).

Se você quiser reverter as alterações feitas em um pacote do Advisor provisionado, poderá optar por redefinir o pacote do Advisor. Isso substitui todos os scripts SQL relacionados ao pacote do Advisor e redefine todos os valores de limites padrão. Isso mantém todos os relatórios existentes.

a redefinição do pacote do Advisor pode ser feita usando o formulário **configurar pacotes do Advisor** . Você precisa selecionar o pacote do Advisor a ser redefinido e clicar em **Redefinir**.

### <a name="remove-advisor-packs"></a>remover os pacotes do Advisor

Quando um pacote advisor não é mais necessária, os usuários poderá ser removido do banco de dados. a remoção do pacote Advisor remove tudo sobre o pacote do Advisor do banco de dados, incluindo as regras e os limites, todos os scripts SQL e todos os relatórios. Nenhuma das ações pode ser revertida.

a remoção do pacote do Advisor pode ser feita usando o formulário **configurar pacotes do Advisor** . Você precisa selecionar o pacote advisor a ser removido e, em seguida, clique em **desprovisionamento**.

### <a name="update-existing-advisor-packs"></a>Atualizar os pacotes existentes do Supervisor

Pacotes existentes do Supervisor de atualização é muito semelhante ao redefinir o pacote de supervisor ao seu estado original. Para atualizar o pacote advisor para uma versão mais recente, copie o novo pacote de Supervisor para a pasta de pacote advisor. Um pacote de Supervisor é considerado uma atualização para um pacote existente no Supervisor apenas quando não há alterações de metadados de relatório. O relatório e as IDs de regras devem ser idênticos. Caso contrário, eles devem ser tratados como dois pacotes de supervisor diferente.

Se houver apenas alterações de lógica de negócios e nenhum metadado de relatório for alterado para um pacote do Advisor, ele deverá receber um novo número de versão, por exemplo, de 1,0 a 2,0. Se não houver alteração de metadados do relatório, o pacote de Supervisor deve receber um nome completo diferente. Por exemplo, Microsoft.ServerPerformanceAdvisor.IIS.V1 pode ser alterado para Microsoft.ServerPerformanceAdvisor.IIS.V2.

Se existir uma versão mais recente do Advisor Pack, a lista no formulário **configurar pacotes do Advisor** preencherá automaticamente a coluna **versão** com a versão mais recente do Advisor Pack. Você pode selecionar o pacote do Advisor e, em seguida, clicar em **Redefinir**. As atualizações de pacote advisor com a nova lógica de negócios e limites. Todos os relatórios para este pacote advisor são preservados.

## <a name="managing-servers"></a>Gerenciamento de servidores


SPA fornece recursos básicos para gerenciamento de servidores de destino. Você pode optar por adicionar novos servidores à lista de servidores de destino, remover servidores da lista ou modificar comentários para servidores.

**Para adicionar ou remover servidores ou modificar as informações do servidor**

1.  Clique o **configuração** menu, clique em **Configurar servidores**.

2.  Na lista de servidores que existem atualmente no banco de dados do projeto, a última linha está vazia. Clique na linha e preencha os campos. O **nome do servidor** e **local de compartilhamento de arquivo** pasta campos são obrigatórios, e o nome do servidor deve ser exclusivo.

3.  Insira outras informações do servidor na coluna de **comentários** para cada servidor.

    **Observação** Esse campo usa um formato de texto livre, portanto, você pode usá-lo como um campo de descrição. Ou usar este campo para marcar os servidores para que eles possam ser encontrados facilmente na janela principal, ou para servidores do grupo, por exemplo, por local ou função de servidor.



4.  Se você quiser usar o SPA com um grande número de servidores, o SPA oferecerá suporte a um formato de valor separado por vírgula (. csv) para importação. O arquivo deve conter pelo menos dois campos: **Server** e **local de compartilhamento de arquivo**. O terceiro campo, **Mark** é opcional, mas é recomendável organizar seus servidores. Você também pode exportar a lista de servidores para um arquivo. csv para determinar o formato apropriado ou fazer backup da configuração do servidor.

### <a name="searching-and-filtering"></a>Pesquisando e filtrando

Se você gerenciar mais de alguns servidores, o SPA fornecerá suporte básico para localizar rapidamente os servidores na janela principal. Você pode clicar na coluna cabeçalho para classificar com base no nome do servidor, resultados da análise, status da tarefa atual ou comentários. Você também pode optar por usar a funcionalidade de pesquisa. No canto superior direito da janela principal, você pode digitar uma cadeia de caracteres para pesquisar. A lista **servidor de destino** na janela principal usa a cadeia de caracteres para filtrar os servidores e para exibir apenas os servidores com campos de nome ou de comentário que contêm a cadeia de caracteres de pesquisa.

A figura a seguir mostra como a cadeia de caracteres a **Dell** corresponde aos servidores com a cadeia de caracteres **Dell** ou servidores com o campo de **Comentário** que contém a **Dell**.

![exemplo de filtragem e pesquisa](../media/server-performance-advisor/spa-user-manual-find-search-example.png)

## <a name="advanced-functionality"></a>Funcionalidade avançada


### <a name="working-with-spa-windows-powershell-cmdlets"></a>Trabalhar com os cmdlets do Windows PowerShell do SPA

O console do SPA tem suporte por meio da interface do Usuário recorrente para coleta de dados. Se essa funcionalidade não é suficiente para o seu ambiente, há cmdlets do Windows PowerShell que um administrador avançado pode usar para personalizar a coleta de dados. Esses cmdlets do Windows PowerShell permitem que os administradores de sistema executar a análise de desempenho em servidores de destino e usar SPA para suporte ao cliente remoto automaticamente. Por exemplo, os administradores do sistema podem escrever scripts para chamar SPA cmdlets em determinados intervalos de tempo de amostragem periodicamente a condição de desempenho de servidores de destino.

É recomendável fechar o aplicativo SPAConsole.exe antes de usar esses cmdlets.

Antes de executar cmdlets do Windows PowerShell, você precisa registrar cmdlets no console do computador.

**Para registrar cmdlets do Windows PowerShell**

1.  Em um prompt de comando com privilégios elevados do Windows PowerShell, digite **registerSpaCmdlets. cmd**. A mensagem **registrar cmdlets spa com êxito** é exibida.

2.  Executar **SPA PowerShell.cmd**. Se você passar o caminho para um arquivo de script do Windows PowerShell, ele executará os scripts automaticamente. Caso contrário, ele abre um prompt de comando do Windows PowerShell, que está pronto para executar os cmdlets do Windows PowerShell SPA.

A tabela a seguir descreve os cmdlets do Windows PowerShell do SPA:

| Nome do cmdlet | Parâmetros | Descrição |
| ------ | ------- | ------ |
| Início SpaAnalysis | **-ServerName** Nome do servidor de destino.<br>**-AdvisorPackName** Nome completo do supervisor Pack a ser colocado na fila no servidor. Quando mais de um pacote está agendado para ser executado ao mesmo tempo, o valor do parâmetro deve ser formatado como AP1name, AP2name.<br>**-Duração** Duração da coleta de dados.<br>**-Credential** Credenciais do usuário para a conta que executa a coleta de dados no servidor de destino.<br>**-SQLINSTANCENAME** Nome da instância de SQL Server.<br>**-Sqldatabasename** Nome do banco de dados do projeto SPA. | Inicia uma sessão de coleta de dados do SPA no servidor especificado. |
| Stop-SpaAnalysis | **-SQLINSTANCENAME** Nome da instância de SQL Server.<br>**-Sqldatabasename** Nome do banco de dados do projeto SPA.<br>**-ServerName** Nome do servidor de destino. | Tenta interromper uma sessão em execução do SPA. Se uma sessão já estiver concluída, ela retornará sem fazer nada. |
| Get-SpaServer | **-SQLINSTANCENAME** Nome da instância de SQL Server.<br>**-Sqldatabasename** Nome do banco de dados do projeto SPA. | Obtém a lista de servidores no banco de dados. Retorna uma lista de objetos, inclusive estas propriedades: nome, Status, compartilhamento de arquivos e comentário. |
| Get-SpaAdvisorPacks | **-SQLINSTANCENAME** Nome da instância de SQL Server<br>**-Sqldatabasename** Nome do banco de dados do projeto SPA | Obtém a lista de pacotes de Supervisor no banco de dados. Retorna uma lista de objetos, inclusive estas propriedades: nome, DisplayName, autor e versão. |

O Windows PowerShell oferece a capacidade de passar credenciais por meio de arquivos criptografados para habilitar cenários de automação. Para obter mais informações sobre como usar arquivos criptografados para passar credenciais para um cmdlet, consulte [criar scripts do Windows PowerShell que aceitam credenciais](/previous-versions/technet-magazine/ff714574(v=msdn.10)).

### <a name="automating-spa-report-collection-by-using-windows-powershell"></a>Automatizar a coleta de relatório SPA usando o Windows PowerShell

Os procedimentos a seguir representam um exemplo de como automatizar uma coleção de relatório SPA usando cmdlets do Windows PowerShell do SPA.

As credenciais do usuário podem ser criptografadas e armazenadas em cache por meio do Windows PowerShell. Essas credenciais são usadas para fazer logon servidores remotos. Embora não seja específico ao SPA, o exemplo a seguir demonstra essa técnica:

``` syntax
$fileName = 'D:\temp\operator.txt'
$userName = 'domainname\operator'

# save credential to file
$(Get-Credential).Password | convertFrom-SecureString | Set-Content $fileName

# load credential from file
$credential = New-Object System.Management.Automation.PsCredential $userName, $(Get-Content $fileName | convertTo-SecureString)

# run command
.\start-SpaAnalysis  ServerName: Server1  Credential: $credential  AdvisorPackName:Microsoft.ServerPerformanceAdvisor.CoreOS.V1 10  Duration:10  SqlInstanceName: .\SQLExpress  SqlDatabaseName:SPA8294
```

Antes de executar esse arquivo, você deve executar **Set-ExecutionPolicy RemoteSigned**

Você pode executá-lo de um arquivo em lotes usando o comando a seguir:

``` syntax
PowerShell -command "& '.\RunSpa.ps1' "
```

### <a name="use-t-sql-to-generate-reports"></a>Usar o T-SQL para gerar relatórios

Dados de relatório do SPA podem ser extraídos usando SQL para criar relatórios personalizados não são fornecidos dentro do SPAConsole.exe.

por exemplo, o comando T-SQL a seguir fornece uma lista dos 10 principais servidores por CPU para o período de tempo abrangendo os últimos três dias:

``` syntax
select TOP 10
   MachineName,
   AverageCpu
FROM (
   select
      __MachineName MachineName,
      AVG(AverageValue) AverageCpu
   FROM [SPA].[Microsoft.ServerPerformanceAdvisor.CoreOS.V1].vwCpuPerformance
   WHERE __Creationtime > dateadd(DAY, -3, GETUTcdate())
      AND Name = N'% Processor time' AND CpuId = N'_Total'
   GROUP BY __MachineName
) t
OrdER BY t.AverageCpu DESC
```

### <a name="working-with-multiple-projects"></a>Trabalhar com vários projetos

Em alguns casos, você pode desejar particionar os bancos de dados do SQL Server que são usados pelo SPA. Isso pode ajudar a reduzir o tamanho dos dados do banco de dados do SQL Server ou ajudá-lo a partição lógica de servidores. Nesses casos, o console do SPA oferece suporte a vários projetos. Você pode criar um novo banco de dados do projeto clicando em **arquivo**, e, em seguida, clicando em **novo projeto**. O assistente de primeira vez usado é exibido para ajudar a criar o novo banco de dados do projeto.

Após a criação de projetos, você pode clicar em **arquivo**, e, em seguida, clique em **Abrir projeto** para alternar entre projetos. O console do SPA não permite a alternância de bancos de dados quando a análise de desempenho está em execução dentro do projeto aberto atualmente. Isso é para proteger a integridade dos dados de desempenho.

Quando você opta por criar um novo banco de dados do projeto ou abrir um banco de dados de outro projeto, o projeto atual seja fechado. Todos os relatórios abertos são fechados quando o projeto atual seja fechado.

**Observação** SQL Server 2008 R2 Express tem um limite de banco de dados de 10 GB. Usando vários projetos, você pode usar um ou mais bancos de dados do SQL Server e permanecer no limite de 10 GB do SQL Server 2008 R2 Express.



### <a name="logging-and-debugging"></a>Registro em log e depuração

SPA fornece a funcionalidade de log básico. Ele permite apenas registros a serem gravados em um arquivo de log, que está localizado na mesma pasta SPAConsole.exe. A conta de usuário que executa o console do SPA precisa ter permissão de gravação para a pasta onde o SPA foi instalado para certificar-se de que os logs podem ser gravados para o arquivo de log.

SPA contém os seguintes níveis de log válido:

* **Informativo** Despeja registros de cada ação que usa o console do SPA e é projetado principalmente para fins de depuração.

* **Aviso** registra todas as falhas e exceções que ocorrem dentro do console do SPA. Algumas das falhas são simplesmente falhas de validação que podem ser manipuladas pelo console do SPA.

* **Crítico** registra apenas falhas e exceções que não podem ser tratadas pelo console do SPA. Essas falhas fazem com que o console do SPA falhe. Os logs fornecem as informações de contexto para tais falhas.

Por padrão, o nível de log é aviso, o que significa que o SPA registra apenas falhas e exceções que ocorrem no SPA. O nível de log pode ser alterado editando o **SpaConsole.exe.config** arquivo na mesma pasta SpaConsole.exe está localizado. Todos os logs são gravados para o arquivo de log. txt na mesma pasta.

SPA também fornece alguns recursos básicos para depuração. Para ativar a depuração para SPA, os usuários precisam modificar manualmente o banco de dados de projeto do SPA. A configuração é armazenada em uma tabela de configurações. Usuário precisa executar o seguinte script SQL para alterar o projeto do SPA para o modo de depuração:

``` syntax
UPdate [Configurations] SET Value = N'true' WHERE Name = N'Debugmode'.
```

No modo de depuração, a estrutura do SPA preserva todos os dados temporários que são gerados durante a análise de desempenho para que você pode solucionar problemas nos pacotes do Supervisor. Esse script é projetado principalmente para ser usado pelos desenvolvedores de pacote de Supervisor.

para obter mais informações sobre os recursos de depuração no SPA, consulte o [Guia de desenvolvimento do Server performance Advisor Pack](server-performance-advisor-pack-development-guide.md).

### <a name="managing-the-database"></a>Gerenciando o banco de dados

### <a name="backup-and-restore-the-database"></a>Fazer backup e restaurar o banco de dados

O console do SPA não fornece funcionalidade para fazer backup e restaurar o banco de dados do SPA. Você deve usar scripts e ferramentas padrão do Microsoft SQL Server para fazer backup e restaurar bancos de dados.

### <a name="clean-up-the-database"></a>limpar o banco de dados

Os bancos de dados de projeto do SPA podem crescer em tamanho, como análise de desempenho mais é executada. Por padrão, o SPA mantém todos os relatórios. Você talvez queira remover relatórios antigos para ajudar a melhorar o desempenho. Você pode excluir relatórios por meio de **Gerenciador de relatório** interface.

## <a name="privacy-and-security"></a>Privacidade e segurança


SPA só oferece suporte a coleta de dados de servidores de destino para o console do SPA. Ele não é projetado para enviar informações à Microsoft ou desenvolvedores de terceiros. Para obter mais informações sobre privacidade do SPA, consulte os termos de licença de Software da Microsoft para o Server Performance Advisor.

Todos os dados coletados pelo SPA é armazenado nos bancos de dados do projeto. Devido à natureza de alguns dos rastreamentos ETW, SPA pode coletar informações confidenciais que podem ter a importância dos negócios alta. Você deve estar ciente do risco potencial associado compartilhando o acesso a bancos de dados de projeto do SPA. Arquivos de log temporários são salvos em pastas compartilhadas que são especificadas em cada computador de destino. Embora o SPA tente excluir esses arquivos de log temporários quando a importação de dados for concluída, não haverá nenhuma garantia de que esses arquivos de log sempre sejam excluídos. Você deve certificar-se de que as pastas compartilhadas estão localizadas em locais seguros.

Pacotes do SPA advisor contém scripts SQL para examinar e analisar logs de desempenho para gerar relatórios de desempenho. SPA tenta limitar o privilégio esses scripts executados em. No entanto, ainda há uma possibilidade de que os scripts podem coletar informações confidenciais por meio do SPA de servidores de destino, ou obter ou modificar informações confidenciais são armazenadas no banco de dados do projeto SPA mesmo. Você precisa certificar-se de que todos os pacotes de Supervisor provisionados no banco de dados de projeto do SPA são provenientes de fontes confiáveis.

## <a name="errors-and-troubleshooting"></a>Erros e solução de problemas


### <a name="spaconsoleexe-does-not-start-or-write-log-file"></a>SPAConsole.exe não iniciar ou gravar o arquivo de log

Quando você tenta executar o SPAConsole.exe pela primeira vez, se o .NET Framework não estiver instalado, o aplicativo não iniciará ou gravará um arquivo de log. Certifique-se de que uma compatible.NET que Framework está instalado e funcionando corretamente antes de iniciar o SPA.

### <a name="locating-log-information"></a>Localizando informações de log

SPA armazena informações de falha no arquivo de log. txt na pasta SPA. mensagens de erro detalhadas e informações de pilha de chamadas são gravadas nessa pasta. Se houver falhas que precisam de mais informações para interpretar, você pode abrir o arquivo de log. txt para ver os detalhes do erro.

### <a name="database-size-limitations-for-sql-server-express"></a>Limitações de tamanho do banco de dados do SQL Server Express

SQL Server Express tem um limite de tamanho de 10 GB para um banco de dados do usuário. Este banco de dados de tamanho tem a capacidade de armazenar relatórios 20.000 30.000. Para reduzir a possibilidade de se atingir esse limite de banco de dados, é recomendável excluir relatórios regularmente e/ou usando outra versão do SQL Server que não tem a limitação de banco de dados de usuário de 10 GB.

### <a name="sql-server-express-log-size-and-disk-capacity"></a>SQL Server Express disco e tamanho de capacidade do log

Se você estiver usando SQL Server Express, o banco de dados do usuário será limitado a 10 GB, mas o arquivo de log correspondente poderá exceder 70 GB. Por esses motivos, é recomendável 100 GB ou mais de espaço livre em disco para o SQL Server Express. Esse espaço em disco deve ser suficiente para armazenar cerca de 20.000 para 30.000 relatórios. Esse arquivo de log é denominado SPADB \_ log. ldf e é encontrado em **% Program Files% \\ Microsoft SQL Server \\ nó MSSQL10. \\Sqlmssql \\ Data**.

### <a name="failure-to-connect-to-target-server"></a>Falha ao conectar ao servidor de destino

Quando você executa a análise de desempenho em servidores de destino, a conta de usuário que executa o console do SPA precisa ter certos privilégios para o coletor de dados definido como CCP nos servidores de destino da fila. Configurações do Firewall do Windows também precisam ser alterada para permitir comunicações CCP de passagem. Para obter instruções de configuração detalhadas, consulte [Configurando SPA](#bkmk-setupspa).

### <a name="failure-to-create-pla-data-collection-set-on-the-target-server"></a>Falha ao criar o conjunto de coleta de dados do CCP no servidor de destino

Se você receber uma mensagem não é possível criar conjunto de coleta de dados PLA na servidor de destino, certifique-se de fazer o seguinte:

* Verifique se o **Logs de desempenho e alertas** serviço está em execução

* A configuração de segurança **acesso à rede: não permitir o armazenamento de senhas e credenciais para autenticação de rede** está desabilitado. A configuração de segurança deve ser desabilitada porque SPA precisa usar as credenciais do usuário para criar o conjunto de coleta de dados no servidor de destino.

### <a name="running-spa-against-the-console"></a><a href="" id="running-spa-against-the-console-"></a>Executando o SPA contra o console

CCP passa por um canal diferente se o servidor de destino é o mesmo que o console do SPA. Mesmo que a conta de usuário esteja executando o console do SPA com privilégios de administrador, a PLA falhará. Se o console do SPA é instalado em um servidor de destino, os usuários precisam iniciar SPA como um administrador para verificar se que pode executar a tarefa de análise de desempenho no console.

### <a name="running-multiple-consoles-at-the-same-time"></a>Executando vários consoles ao mesmo tempo

SPA não oferece suporte a vários consoles em execução no mesmo banco de dados de projeto SPA ao mesmo tempo. SPA também fornece o mecanismo de sincronização e bloqueio para impedir que isso aconteça. Se estiver executando dois consoles SPA ao mesmo tempo, o console irá se comportar inconsistentemente dependendo da sequência de tempo que executam esses consoles SPA. Para evitar isso, todas as sessões de análise de desempenho na fila devem ser removidas da lista antes de ser processada pelo console que inicia a análise.

SPA protege a integridade de cada relatório que é gerado com êxito pelo SPA. ao mesmo tempo, o SPA não garante que todas as tarefas de análise em fila sejam concluídas. Se você estiver vendo inconsistente alterações de status de sessões de análise de desempenho ou erros que o sistema de declaração não é possível localizar o conjunto de logs de desempenho gerados pelo coletor de dados, é provável que seja causado por várias instâncias do console SPA em execução no mesmo banco de dados de projeto SPA.

Executar os cmdlets do Windows PowerShell do SPA também pode ser afetado por um console SPA que esteja em execução no mesmo banco de dados do SPA. É recomendável que você feche o console do SPA antes de executar os cmdlets do Windows PowerShell do SPA.

### <a name="spaconsoleexe-recurring-collection-is-disrupted"></a>Coleção recorrente SPAConsole.exe é interrompida

Quando você executa o SPAConsole.exe e você usar uma coleção de dados recorrente (por exemplo, uma coleção por hora), o servidor que executa o SPAConsole.exe não deve estar no modo de economia de energia, de modo que ele pode suspender. O SPA não verifica se há uma política de economia de energia. Essa atividade suspensa pode interromper a coleta de dados recorrente regular.

### <a name="lost-etw-events"></a>Eventos ETW de perdidos

Para criar o impacto de desempenho mínimo nos servidores de destino, CCP foi projetado para ser executado com baixa prioridade, enquanto ele está coletando informações de desempenho. Se o servidor de destino estiver ocupado, a PLA poderá descartar algumas das tarefas de coleta de dados para gerar tarefas de alta prioridade em execução nos servidores de destino. Você deve considerar a configuração da pasta compartilhada em que os eventos são gravados em um disco que não está em conflito com a e/s de carga de trabalho ou uma unidade mais rápida, como um SSD. Quando os eventos são removidos, são relatados na exibição do relatório como eventos perdidos podem afetar a confiabilidade das métricas de desempenho gerados.

Alguns problemas de permissão também pode causar o registro ou consultas WMI deve ser ignorada. No entanto, isso é muito menos provável de acontecer de perda de eventos ETW. Como resultado, os resultados de conjunto de Coletores de dados, às vezes, não contêm todos os valores que são solicitados. Você precisa certificar-se de que a situação é tratada pelos scripts T-SQL para todos os pacotes de Supervisor. Se os dados não existirem no resultado da coleta de dados, eles serão marcados como sem dados nos relatórios.

Como a perda de eventos ETW é comum CCP, pontos de dados que são gerados com base em um rastreamento ETW pode não ser consistente com pontos de dados que são gerados com base em, por exemplo, os contadores de desempenho. Por exemplo, é possível ver que a utilização total da CPU pelo IIS é 80% (que vêm de contadores de desempenho) e que a parte superior URLs usam somente 10% de todo o tempo de CPU (que é um ponto de dados de rastreamento ETW). Normalmente, a fonte de dados para um ponto de dados pode ser visualizada por meio de dica de ferramenta do ponto de dados. Você deve estar ciente do impacto da perda desses dados.

Para evitar essa perda de evento, a pasta de resultados deve ser fechada para o servidor de destino.

Se os resultados do coletor de dados contiverem dados incompletos diferentes da perda de rastreamento ETW e o desenvolvedor do pacote Advisor tiver adicionado suporte à notificação de perda de evento ETW, uma barra de informações será mostrada na parte superior do relatório único para notificar o usuário sobre o relatório potencialmente inconsistente causado pela perda de dados. informações detalhadas de perda de dados podem ser encontradas no arquivo log.txt.

## <a name="glossary"></a>Glossário


Aqui estão alguns dos termos usados com SPA:

* **Pacote Advisor** uma coleção de metadados e os scripts T-SQL que processam os logs de desempenho são coletados do servidor de destino. O pacote de Supervisor e gera relatórios dos dados de log de desempenho. Os metadados no pacote do Supervisor definem os dados a serem coletados do servidor de destino para as medidas de desempenho. Os metadados também definem o conjunto de regras, os limites e o formato de relatório. Geralmente, um pacote advisor foi criado especificamente para uma função de servidor único, por exemplo, Internet Information Services (IIS).

* **Console do SPA** SpaConsole.exe, que é a parte central do SPA. SPA não precisa executar no servidor de destino que você está testando. O console do SPA contém todas as interfaces de usuário para SPA, configuração de projeto para executar a análise e exibir relatórios. Por design, o SPA é um aplicativo de duas camadas. O console do SPA contém a camada de interface do Usuário e a parte da camada de lógica de negócios. O console do SPA agenda e processa as solicitações de análise de desempenho.

* **Framework do SPA** fornece todas as interfaces de usuário, processamento de log de desempenho, configuração, tratamento de erros e procedimentos de gerenciamento e APIs de banco de dados.

* **Projeto do SPA** relatórios de um banco de dados que contém todas as informações sobre os servidores de destino, os pacotes de Supervisor e análise de desempenho que são gerados nos servidores de destino para os pacotes do advisor. Você pode comparar e exibir gráficos de histórico e tendências dentro do mesmo projeto do SPA. Você pode criar mais de um projeto. Os projetos do SPA são independentes um do outro e nenhum dado compartilhada entre projetos.

* **Servidor de destino** o computador físico ou máquina virtual que executa o Windows Server com certas funções de servidor, como o IIS.

* **Sessão de análise de dados** uma análise de desempenho em um servidor de destino específico. Uma sessão de análise de dados pode incluir vários pacotes de Supervisor. Os conjuntos de Coletores de dados desses pacotes de Supervisor são mesclados em um conjunto de Coletores de dados único. Todos os logs de desempenho para uma sessão de análise de dados são coletados durante o mesmo período de tempo. Analisar relatórios que são gerados por pacotes de supervisor em execução na mesma sessão de análise de dados pode ajudar os usuários a entender a situação geral de desempenho e identificar as causas de problemas de desempenho.

* **Rastreamento de eventos do Windows** um sistema de rastreamento de alto desempenho, baixa sobrecarga e dimensionável que é fornecido no Windows. Ele fornece perfis e depuração de recursos, que podem ser usados para solucionar uma variedade de cenários. SPA usa eventos ETW como uma fonte de dados para gerar os relatórios de desempenho. Para obter informações gerais sobre o ETW, consulte [melhorar a depuração e ajuste de desempenho com o ETW](/archive/msdn-magazine/2007/april/event-tracing-improve-debugging-and-performance-tuning-with-etw).

* **Windows Management Instrumentation (WMI)** a infra-estrutura para gerenciamento de dados e operações no Windows. Você pode escrever scripts WMI ou aplicativos para automatizar tarefas administrativas em computadores remotos. WMI também fornece dados de gerenciamento para outras partes do sistema operacional e produtos. SPA usa informações de classe WMI e pontos de dados como fontes para geração de relatórios de desempenho.

* **Contadores de desempenho** usado para fornecer informações sobre como o sistema operacional ou um aplicativo, serviço ou driver está sendo executado. Os dados do contador de desempenho podem ajudar a determinar gargalos do sistema e ajustar o desempenho do sistema e do aplicativo. O sistema operacional, rede e dispositivos fornecem dados de contador que um aplicativo pode utilizar para fornecer aos usuários uma exibição gráfica de como o sistema está sendo executado. SPA usa informações do contador de desempenho e pontos de dados como fontes para gerar relatórios de desempenho.

* **Logs de desempenho e alertas (CCP)** desempenho coleta logs e rastreia e gera alertas de desempenho quando determinados disparadores são atendidos. CCP pode ser usado para coletar contadores de desempenho, rastreamento de eventos para Windows (ETW), consultas WMI, chaves do registro e configuração de arquivos. CCP também oferece suporte a coleta de dados remotos por meio de chamadas de procedimento remoto (RPC). O usuário define um conjunto de Coletores de dados, que inclui informações sobre os dados a serem coletados, a frequência da coleta de dados, duração da coleta de dados, filtros e um local para salvar os arquivos de resultados. SPA usa PLA para coletar todos os dados de desempenho de servidores de destino.

* **Um único relatório** relatório um SPA que é gerado com base em sessão de análise de dados para o pacote de um supervisor em um servidor de destino único. Ela pode conter várias seções de dados e notificações.

* **Relatório de lado a lado** relatório de um SPA que compara dois relatórios únicos para o mesmo pacote de Supervisor. Os dois relatórios podem ser gerados a partir de servidores de destino diferentes ou execuções de análise de desempenho separados no mesmo servidor de destino. O relatório side-by-side cria a capacidade de comparar dois relatórios para ajudar os usuários a identificar comportamentos anormais ou configurações em um dos relatórios. Um relatório de lado a lado contém várias seções de dados e notificações. Em cada seção, dados de ambos os relatórios são listada lado a lado.

* **Gráfico de tendência** relatório um SPA que é usado para investigar padrões repetitivos dos problemas de desempenho. Muitos problemas de desempenho repetitivas são causados por alterações de carga do servidor agendado do servidor ou de computadores cliente, que podem ocorrer diariamente ou semanalmente. SPA fornece um gráfico de tendências de 24 horas e um gráfico de tendência de 7 dias para identificar esses problemas.

    O usuário pode escolher uma ou mais séries de dados por vez, que é um valor numérico no relatório único, como **média de utilização de CPU total**. mais especificamente, um valor numérico é um valor escalar de um único servidor gerado por um único AP em uma determinada instância de tempo. SPA agrupa esses valores em 24 grupos, um para cada hora do dia (sete para um relatório de 7 dias, um para cada dia da semana). SPA calcula a média, mínimo, máximo e desvios padrão para cada grupo.

* **Gráfico de histórico** relatório um SPA que é usado para mostrar as alterações em determinadas numérico valores dentro de relatórios únicos para um determinado servidor e Supervisor pack par ao longo do tempo. O usuário pode escolher várias séries de dados e mostrá-los juntos no gráfico de histórico para entender a correlação entre a série de dados diferente.

* **Série de dados** dados numéricos que são coletados da mesma fonte de dados durante um período de tempo. A mesma fonte significa que os dados tem que vêm do mesmo servidor de destino, como o comprimento da fila de média de solicitação do IIS em um servidor.

* **Regras** combinações de lógica, limites e descrições. Eles representam um possível problema de desempenho. Cada pacote advisor contém várias regras. Cada regra é acionada por um processo de geração de relatórios. Uma regra se aplica a lógica e os limites para os dados em um único relatório. Se os critérios forem atendidos, uma notificação de aviso será gerada. Se não, a notificação é definida como o **OK** estado. Se a regra não se aplica, a notificação é definida como não aplicável (**NA**) estado.

* **Notificações** informações que exibe uma regra para os usuários. Ele inclui o status da regra (**OK**, **NA**, ou um **Aviso**), o nome da regra e recomendações possíveis para resolver os problemas de desempenho.