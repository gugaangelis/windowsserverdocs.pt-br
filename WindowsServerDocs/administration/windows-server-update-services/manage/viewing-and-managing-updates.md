---
title: Exibir e gerenciar atualizações
description: Tópico Windows Server Update Service (WSUS)-como exibir e gerenciar atualizações no console do WSUS
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: manage-wsus
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: ac70192b-0309-4385-b697-2e8eda51911c
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: de2d12ad34ba11f948baa390546747a6bf4b635c
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71361487"
---
# <a name="viewing-and-managing-updates"></a>Exibir e gerenciar atualizações

>Aplica-se a: Windows Server (canal semestral), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Você pode usar o console do WSUS para exibir e gerenciar atualizações.

## <a name="viewing-updates"></a>Exibindo atualizações
Na página **atualizações** , você pode fazer o seguinte:

-   Exibir atualizações. A visão geral de atualização exibe as atualizações que foram sincronizadas da origem de atualização para o servidor do WSUS e estão disponíveis para aprovação.

-   Filtrar atualizações. No modo de exibição padrão, você pode filtrar as atualizações por status de aprovação e status de instalação. A configuração padrão é para atualizações não aprovadas que são necessárias para alguns clientes ou que tiveram falhas de instalação em alguns clientes. Você pode alterar essa exibição alterando o status de aprovação e os filtros de status de instalação e, em seguida, clicando em **Atualizar**.

-   Criar novas exibições de atualização. No painel **ações** , clique em **nova exibição de atualização**. Você pode filtrar as atualizações por classificação, produto, o grupo para o qual foram aprovadas e a data de sincronização. Você pode classificar a lista clicando no título de coluna apropriado na barra de título.

-   Procure atualizações. Você pode procurar uma atualização individual ou um conjunto de atualizações por título, descrição, artigo da base de dados de conhecimento ou número do Microsoft Security Response Center para a atualização.

-   Exiba detalhes, status e histórico de revisão para cada atualização.

-   Aprovar e recusar atualizações.

#### <a name="to-view-updates"></a>Para exibir atualizações

1.  No console de administração do WSUS, expanda o nó atualizações e clique em todas as atualizações.

2.  Por padrão, as atualizações são exibidas com seu título, classificação, porcentagem instalada/não aplicável e status de aprovação. Se você quiser exibir propriedades de atualização mais ou diferentes, clique com o botão direito do mouse na barra de título da coluna e selecione as colunas apropriadas.

3.  Para classificar por critérios diferentes, como status de download, título, classificação, data de lançamento ou status de aprovação, clique no título de coluna apropriado.

#### <a name="to-filter-the-list-of-updates-displayed-on-the-updates-page"></a>Para filtrar a lista de atualizações exibidas na página atualizações

1.  No console de administração do WSUS, expanda **atualizações**e clique em **todas as atualizações**.

2.  No painel central ao lado de **aprovação**, selecione o status de aprovação desejado e, ao lado de **status** , selecione o status de instalação desejado. Clique em **Atualizar**.

#### <a name="to-create-a-new-update-view-on-wsus"></a>Para criar uma nova exibição de atualização no WSUS

1.  No console de administração do WSUS, expanda **atualizações**e clique em **todas as atualizações**.

2.  No painel **ações** , clique em **nova exibição de atualização**.

3.  Na janela **Adicionar exibição de atualização** , em **etapa 1: selecionar Propriedades**, selecione as propriedades necessárias para filtrar a exibição de atualização:

    -   As atualizações selecionadas estão em uma classificação específica para filtrar as atualizações que pertencem a uma ou mais classificações de atualização.

    -   Selecione atualizações para um produto específico filtrar as atualizações de um ou mais produtos ou famílias de produtos.

    -   Selecione atualizações são aprovadas para um grupo específico filtrar as atualizações aprovadas para um ou mais grupos de computadores.

    -   As atualizações selecionadas foram sincronizadas em um período de tempo específico para filtrar as atualizações sincronizadas em um momento específico.

    -   Selecione atualizações são atualizações do WSUS para filtrar as atualizações do WSUS.

4.  Em **etapa 2: editar as propriedades**, clique nas palavras sublinhadas para escolher os valores desejados.

5.  Em **Step 3: Especifique um nome @ no__t-0, dê um nome à sua nova exibição.

6.  Clique em **OK**.

O novo modo de exibição será exibido no painel exibição de árvore em atualizações. Ele será exibido, como os modos de exibição padrão, no painel central quando você o seleciona.

#### <a name="to-search-for-an-update"></a>Para procurar uma atualização

1.  Selecione o nó **atualizações** (ou qualquer nó sob ele).

2.  No painel **ações** , clique em **Pesquisar**.

3.  Na janela **Pesquisar** , na guia **atualizações** , insira seus critérios de pesquisa. Você pode usar o texto dos campos **título**, **Descrição**e **número de artigo da base de dados de conhecimento Microsoft (KB)** . Cada um desses itens é uma propriedade listada na guia **detalhes** nas propriedades da atualização.

#### <a name="to-view-the-properties-for-an-update"></a>Para exibir as propriedades de uma atualização

1.  No console de administração do WSUS, expanda **atualizações**e clique em **todas as atualizações**.

2.  Na lista de atualizações, clique na atualização que você deseja exibir.

3.  No painel inferior, você verá as diferentes seções de propriedade:

    -   A barra de título exibe o título da atualização; por exemplo, atualização de segurança para Windows Media Player 9 (KB911565).

    -   A seção status exibe o status da instalação da atualização (os computadores nos quais ele precisa ser instalado, computadores nos quais ele foi instalado com erros, computadores nos quais ele foi instalado ou não é aplicável e computadores que não foram relatados status da atualização), bem como informações gerais (KB e números MSRC data de lançamento, etc.).

    -   A seção Descrição exibe uma breve descrição da atualização.

    -   A seção detalhes adicionais exibe as seguintes informações:

        -   O comportamento da instalação da atualização (seja ou não removível, solicita uma reinicialização, requer entrada do usuário ou deve ser instalado exclusivamente).

        -   Se a atualização tem termos de licença para software Microsoft

        -   Os produtos aos quais a atualização se aplica

        -   As atualizações que substituem essa atualização

        -   As atualizações substituídas por esta atualização

        -   Os idiomas com suporte da atualização

        -   A ID da atualização

Observe que você pode executar esse procedimento em apenas uma atualização por vez. Se você selecionar várias atualizações, somente a primeira atualização da lista será exibida no painel **Propriedades** .

## <a name="managing-updates-with-wsus"></a>Gerenciando atualizações com o WSUS
As atualizações são usadas para atualizar ou fornecer uma substituição completa de arquivos para o software instalado em um computador. Cada atualização que está disponível em Microsoft Update é composta de dois componentes:

-   Los Fornece informações sobre a atualização. Por exemplo, os metadados fornecem informações para as propriedades de uma atualização, permitindo que você descubra o que a atualização é útil. Os metadados também incluem termos de licença para software Microsoft. O pacote de metadados baixado para uma atualização normalmente é muito menor do que o pacote de arquivo de atualização real.

-   Arquivos de atualização: Os arquivos reais necessários para instalar uma atualização em um computador.

Quando as atualizações são sincronizadas com o servidor do WSUS, os metadados e os arquivos de atualização são armazenados em dois locais separados. Os metadados são armazenados no banco de dados do WSUS. Os arquivos de atualização podem ser armazenados no servidor do WSUS ou em servidores Microsoft Update, dependendo de como você configurou suas opções de sincronização. Se você optar por armazenar arquivos de atualização em servidores Microsoft Update, somente os metadados serão baixados no momento da sincronização; Você aprova as atualizações por meio do console do WSUS e, em seguida, os computadores cliente obtêm os arquivos de atualização diretamente do Microsoft Update no momento da instalação. Para obter mais informações sobre as opções de armazenamento de atualizações, consulte a seção ,3. Escolha uma estratégia de armazenamento do WSUS @ no__t-0 da etapa 1: Prepare-se para a implantação do WSUS no guia de implantação do WSUS.

Você irá configurar e executar sincronizações, adicionar computadores e grupos de computador e implantar atualizações regularmente. A lista a seguir fornece exemplos de tarefas gerais que podem ser responsabilizadas na atualização de computadores com o WSUS.

-   Determine um plano de gerenciamento de atualizações geral com base na topologia de rede e na largura de banda, nas necessidades da empresa e na estrutura organizacional.

    -   Se é necessário configurar uma hierarquia de servidores do WSUS e como a hierarquia deve ser estruturada.

    -   Quais grupos de computadores criar e como atribuir computadores a eles (direcionamento do lado do servidor ou do cliente).

    -   Qual banco de dados usar para atualizar metadados (por exemplo, banco de dados interno do Windows, SQL Server).

    -   Se as atualizações devem ser sincronizadas automaticamente e em que hora.

-   Defina opções de sincronização, como fonte de atualização, classificação de produto e atualização, idioma, configurações de conexão, local de armazenamento e agendamento de sincronização.

-   Obtenha as atualizações e os metadados associados no servidor do WSUS por meio da sincronização de Microsoft Update ou de um servidor WSUS upstream.

-   Aprovar ou recusar atualizações. Você tem a opção de permitir que os usuários instalem as atualizações em si (se forem administradores locais em seus computadores cliente).

-   Configurar aprovações automáticas. Você também pode configurar se deseja habilitar a aprovação automática de revisões para as atualizações existentes ou aprovar as revisões manualmente. Se você optar por aprovar as revisões manualmente, o servidor do WSUS continuará usando a versão mais antiga até que você aprove manualmente a nova revisão.

-   Verifique o status das atualizações. Você pode exibir o status da atualização, imprimir um relatório de status ou configurar o email para relatórios de status regulares.

## <a name="update-products-and-classifications"></a>Atualizar produtos e classificações
As atualizações disponíveis em Microsoft Update são diferenciadas por produto (ou família de produtos) e classificação.

Um produto é uma edição específica de um aplicativo ou sistema operacional, por exemplo, o Windows Server 2012. Uma família de produtos é o sistema operacional base ou o aplicativo do qual os produtos individuais são derivados. Um exemplo de uma família de produtos é o Microsoft Windows, do qual o Windows Server 2012 é membro. Você pode selecionar os produtos ou as famílias de produtos para os quais deseja que o servidor sincronize as atualizações. Você pode especificar uma família de produtos ou produtos individuais dentro da família. Selecionar qualquer produto ou família de produtos obterá atualizações para as versões atuais e futuras do produto.

As classificações de atualização representam o tipo de atualização. Para qualquer produto ou família de produtos específico, as atualizações podem estar disponíveis entre várias classificações de atualização (por exemplo, atualizações críticas da família do Windows 7 e atualizações de segurança). A tabela a seguir lista as classificações de atualização.

| Classificações de atualização  | Descrição   |
|--|--|
|Atualizações Críticas|Correções liberadas amplamente para problemas específicos que abordam bugs críticos e não relacionados à segurança.|
|Atualizações de definição|Atualizações para vírus ou outros arquivos de definição.|
|Drivers|Componentes de software projetados para dar suporte a novos hardwares.|
|Pacotes de recursos|Novas versões de recursos, geralmente distribuídas em produtos na próxima versão.|
|Atualizações de segurança|Correções liberadas em larga escala para produtos específicos, abordando problemas de segurança.|
|Service packs|Conjuntos cumulativos de todos os hotfixes, atualizações de segurança, atualizações críticas e atualizações criadas desde o lançamento do produto. Os service packs também podem conter um número limitado de recursos ou alterações de design solicitadas pelo cliente.|
|Ferramentas|Utilitários ou recursos que ajudam a realizar uma tarefa ou um conjunto de tarefas.|
|Pacotes cumulativos de atualizações|Um conjunto cumulativo de hotfixes, atualizações de segurança, atualizações críticas e outras atualizações que são reunidas para facilitar a implantação. Um ROLLUP geralmente tem como alvo uma área específica, como segurança, ou um componente específico, como Serviços de Informações da Internet (IIS).|
|Atualizações|Correções liberadas amplamente para problemas específicos que abordam bugs não críticos relacionados à segurança.|

## <a name="icons-used-for-updates-in-windows-server-update-services"></a>Ícones usados para atualizações no Windows Server Update Services
 As atualizações no WSUS são representadas por um dos ícones a seguir.  
 Para exibir esses ícones, você precisa habilitar a coluna substituição no console do Update Services.
 
### <a name="no-icon"></a>Sem ícone
 A atualização não tem nenhuma relação de substituição com nenhuma outra atualização.

 **Problemas operacionais:**  

 Não há preocupações operacionais.  
 
### <a name="superseding-icon"></a>Ícone de substituição
 ![ícone](../../media/wsus/wsus-superseding.png) Esta atualização substitui outras atualizações.

 **Problemas operacionais:**  

 Não há preocupações operacionais.  

### <a name="superseded--superseding-icon"></a>Ícone de substituição de & substituído
 ![ícone](../../media/wsus/wsus-superseded.png) Essa atualização é substituída por outra atualização e substitui outras atualizações.

 **Problemas operacionais:**  

 Substitua essas atualizações com as atualizações substitutas quando possível.
 
### <a name="superseded-icon"></a>Ícone substituído
 ![ícone](../../media/wsus/wsus-superseded-leaf.png) Esta atualização foi substituída por outra atualização.

 **Problemas operacionais:**  

 Substitua essas atualizações com as atualizações substitutas quando possível.
