---
title: Exibir e gerenciar atualizações
description: Tópico do Windows Server Update Service (WSUS) - como exibir e gerenciar atualizações no console do WSUS
ms.prod: windows-server-threshold
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
ms.openlocfilehash: cd517be0de3ba6ca97ca11f4bbe8f59111a01216
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59870947"
---
# <a name="viewing-and-managing-updates"></a>Exibir e gerenciar atualizações

>Aplica-se a: Windows Server (canal semestral), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Você pode usar o console do WSUS para exibir e gerenciar atualizações.

## <a name="viewing-updates"></a>Exibir atualizações
Sobre o **atualizações** página, você pode fazer o seguinte:

-   Exibir atualizações. A visão geral de atualização exibe as atualizações que foram sincronizadas da origem de atualização para o servidor do WSUS e estão disponíveis para aprovação.

-   Filtre as atualizações. No modo de exibição padrão, você pode filtrar as atualizações por status de aprovação e o status da instalação. A configuração padrão é para atualizações não aprovadas, que são necessários para alguns clientes ou que tiveram falhas de instalação em alguns clientes. Você pode alterar essa exibição alterando os filtros de status de instalação e o status de aprovação, e, em seguida, clicando em **Refresh**.

-   Crie novas exibições de atualização. No **ações** painel, clique em **novo modo de exibição de atualização**. Você pode filtrar as atualizações por classificação, produto, o grupo para o qual eles foram aprovados e data de sincronização. Você pode classificar a lista clicando no cabeçalho da coluna apropriada na barra de título.

-   Procurar atualizações. Você pode procurar uma atualização individual ou um conjunto de atualizações por título, descrição, artigo da Base de dados de conhecimento ou o número do Microsoft Security Response Center para a atualização.

-   Exibir detalhes, status e histórico de revisão para cada atualização.

-   Aprovar e recusar atualizações.

#### <a name="to-view-updates"></a>Para exibir atualizações

1.  No console de administração do WSUS, expanda o nó de atualizações e, em seguida, clique em todas as atualizações.

2.  Por padrão, as atualizações são exibidas com seu título, classificação, porcentagem de aplicável/não instalado e status de aprovação. Se você quiser exibir as propriedades de atualização mais ou diferentes, a barra de título de coluna com o botão direito e selecione as colunas apropriadas.

3.  Para classificar por diferentes critérios, como status do download, título, classificação, data de lançamento ou status de aprovação, clique no cabeçalho da coluna apropriado.

#### <a name="to-filter-the-list-of-updates-displayed-on-the-updates-page"></a>Para filtrar a lista de atualizações exibidas na página atualizações

1.  No console de administração do WSUS, expanda **atualizações**e, em seguida, clique em **todas as atualizações**.

2.  No painel central, lado a lado **aprovação**, selecione o status de aprovação desejado e, ao lado **Status** selecione o status de instalação desejado. Clique em **Atualizar**.

#### <a name="to-create-a-new-update-view-on-wsus"></a>Para criar uma nova exibição de atualização no WSUS

1.  No console de administração do WSUS, expanda **atualizações**e, em seguida, clique em **todas as atualizações**.

2.  No **ações** painel, clique em **novo modo de exibição de atualização**.

3.  No **adicionar exibição de atualização** janela, em **etapa 1: selecionar propriedades**, selecione as propriedades que você precisa filtrar a exibição de atualização:

    -   Selecione as atualizações estão em uma classificação específica para filtrar as atualizações que pertencem a um ou mais classificações de atualização.

    -   Selecione as atualizações são de um produto específico filtrar as atualizações para um ou mais produtos ou famílias de produtos.

    -   Selecione as atualizações são aprovadas para um grupo específico filtrar as atualizações aprovadas para um ou mais grupos de computadores.

    -   Selecione as atualizações foram sincronizadas dentro de um período de tempo específico para filtrar as atualizações sincronizadas em um momento específico.

    -   Selecione as atualizações são atualizações do WSUS para filtrar as atualizações do WSUS.

4.  Sob **etapa 2: editar as propriedades**, clique nas palavras sublinhadas para escolher os valores desejados.

5.  Sob **etapa 3: Especifique um nome**, dê um nome ao novo modo de exibição.

6.  Clique em **OK**.

A nova exibição aparecerá no painel de exibição de árvore em atualizações. Ele será exibido, como modos de exibição padrão, no painel central, quando você selecioná-la.

#### <a name="to-search-for-an-update"></a>Para procurar uma atualização

1.  Selecione o **atualizações** nó (ou qualquer nó sob ele).

2.  No **ações** painel, clique em **pesquisa**.

3.  No **pesquisa** janela diante a **atualizações** , insira seus critérios de pesquisa. Você pode usar o texto a partir de **Title**, **descrição**, e **número do artigo da Base de dados de Conhecimento Microsoft (KB)** campos. Cada um desses itens é uma propriedade listada na **detalhes** guia nas propriedades de atualização.

#### <a name="to-view-the-properties-for-an-update"></a>Para exibir as propriedades de uma atualização

1.  No console de administração do WSUS, expanda **atualizações**e, em seguida, clique em **todas as atualizações**.

2.  Na lista de atualizações, clique na atualização que você deseja exibir.

3.  No painel inferior, você verá as seções de propriedade diferentes:

    -   Na barra de título exibe o título da atualização; Por exemplo, segurança atualização para o Windows Media Player 9 (KB911565).

    -   A seção de Status exibe o status da instalação da atualização (a computadores nos quais ele precisa ser instalado, no qual ele foi instalado com erros de computadores, computadores nos quais ele foi instalado ou não é aplicável e computadores que não relataram status da atualização), bem como informações gerais (KB e MSRC data de lançamento de números, etc.).

    -   A seção de descrição exibe uma breve descrição da atualização.

    -   A seção de detalhes adicionais exibe as seguintes informações:

        -   O comportamento da instalação da atualização (ou não está removível, solicita uma reinicialização, requer entrada do usuário ou deve ser instalada exclusivamente).

        -   Se tem ou não a atualização do Microsoft Software License Terms

        -   Os produtos aos quais a atualização se aplica

        -   As atualizações que substituem essa atualização

        -   As atualizações que são substituídas por essa atualização

        -   As linguagens compatíveis com a atualização

        -   A ID de atualização

Observe que você pode executar esse procedimento em apenas uma atualização por vez. Se você selecionar várias atualizações, somente a primeira atualização na lista será exibida na **propriedades** painel.

## <a name="managing-updates-with-wsus"></a>Gerenciando atualizações com WSUS
As atualizações são usadas para atualizar ou fornecendo uma substituição de arquivo completo para o software que é instalado em um computador. Cada atualização que está disponível no Microsoft Update é composta por dois componentes:

-   Metadata: Fornece informações sobre a atualização. Por exemplo, os metadados fornecem informações para as propriedades de uma atualização, permitindo que você descubra do que a atualização é útil. Os metadados também incluem o Microsoft Software License Terms. O pacote de metadados baixado para uma atualização é normalmente muito menor do que o pacote de arquivos de atualização real.

-   Arquivos de atualização: Os arquivos reais necessários para instalar uma atualização em um computador.

Quando as atualizações são sincronizadas com o servidor do WSUS, os metadados e os arquivos de atualização são armazenados em dois locais separados. Os metadados são armazenados no banco de dados do WSUS. Arquivos de atualização podem ser armazenados no servidor do WSUS ou em servidores do Microsoft Update, dependendo de como você configurou suas opções de sincronização. Se você optar por armazenar os arquivos de atualização nos servidores do Microsoft Update, somente os metadados é baixado no momento da sincronização; aprovar as atualizações por meio do console do WSUS e, em seguida, os computadores cliente obtém os arquivos de atualização diretamente do Microsoft Update no momento da instalação. Para obter mais informações sobre as opções de armazenamento de atualizações, consulte a seção [1.3. Escolha uma estratégia de armazenamento do WSUS](../plan/plan-your-wsus-deployment.md#BKMK_1.3.) da etapa 1: Prepare a implantação do WSUS, no guia de implantação do WSUS.

Você será como configurar e executar sincronizações, adição de computadores e grupos de computadores e implantar atualizações regularmente. A lista a seguir fornece exemplos de tarefas gerais que você pode executar na atualização de computadores com o WSUS.

-   Determine um plano de gerenciamento de atualização geral com base em sua topologia de rede e largura de banda, as necessidades da empresa e estrutura organizacional.

    -   Se deseja configurar uma hierarquia de servidores do WSUS e como a hierarquia deve ser estruturada.

    -   Grupos de quais computadores para criar e como atribuir computadores a eles (direcionamento do lado do servidor ou cliente).

    -   Qual banco de dados a ser usado para metadados de atualização (por exemplo, o banco de dados interno do Windows, o SQL Server).

    -   Se as atualizações devem ser sincronizadas automaticamente e o horário.

-   Definir opções de sincronização, como a origem da atualização, produto e atualização de classificação, idioma, as configurações de conexão, local de armazenamento e agendamento de sincronização.

-   Obter as atualizações e os metadados associados no servidor do WSUS por meio da sincronização do Microsoft Update ou um servidor WSUS upstream.

-   Aprovar ou recusar atualizações. Você tem a opção de permitir que os usuários instalem as próprias atualizações (se eles forem administradores locais em seus computadores cliente).

-   Configure aprovações automáticas. Você também pode configurar se deseja habilitar a aprovação automática das revisões de atualizações existentes ou aprovar revisões manualmente. Se você optar por aprovar revisões manualmente, em seguida, o servidor do WSUS continuará usando a versão mais antiga até que você aprova manualmente a nova revisão.

-   Verifique o status das atualizações. Você pode exibir o status de atualização, imprimir um relatório de status ou configurar o email para relatórios de status regulares.

## <a name="update-products-and-classifications"></a>Atualização de produtos e classificações
As atualizações disponíveis no Microsoft Update são diferenciadas por produto (ou família de produtos) e classificação.

Um produto é uma edição específica de um sistema operacional ou aplicativo, por exemplo, o Windows Server 2012. Uma família de produtos é o sistema operacional base ou aplicativo do qual os produtos individuais são derivados. Um exemplo de uma família de produtos é o Microsoft Windows, do qual o Windows Server 2012 é um membro. Você pode selecionar os produtos ou famílias de produtos para os quais você deseja que o servidor para sincronizar as atualizações. Você pode especificar uma família de produtos ou produtos individuais dentro da família. Selecionar qualquer produto ou família de produtos, você receberá atualizações para versões atuais e futuras do produto.

Classificações de atualização representam o tipo de atualização. Para qualquer produto ou família de produtos, as atualizações podem estar disponíveis em várias classificações de atualização (por exemplo, Windows 7 família atualizações críticas e atualizações de segurança). A tabela a seguir lista as classificações de atualização.

| Classificações de atualização  | Descrição   |
|--|--|
|Atualizações Críticas|Correções para problemas específicos abordar bugs relacionados críticos, não são de segurança foi lançada em larga escala.|
|Atualizações de definição|Atualizações de vírus ou outros arquivos de definição.|
|Drivers|Componentes de software projetados para dar suporte ao novo hardware.|
|Pacotes de recursos|Novas versões do recurso, geralmente atribuídos produtos na próxima versão.|
|Atualizações de segurança|Correções para os produtos específicos, resolvendo problemas de segurança foi lançada em larga escala.|
|Service packs|Conjuntos cumulativos de todos os hotfixes, atualizações de segurança, atualizações críticas e atualizações criadas desde o lançamento do produto. Os service packs também podem conter um número limitado de recursos ou alterações de design solicitados pelo cliente.|
|Ferramentas|Utilitários ou recursos que ajudam a concluir uma tarefa ou um conjunto de tarefas.|
|Pacotes cumulativos de atualização|Um conjunto cumulativo de hotfixes, atualizações de segurança, atualizações críticas e outras atualizações que são reunidas para facilitar a implantação. Um pacote cumulativo de atualizações geralmente tem como alvo uma área específica, como segurança ou um componente específico, como o Internet Information Services (IIS).|
|Atualizações|Correções para problemas específicos abordar bugs relacionados não críticos, não são de segurança foi lançada em larga escala.|

## <a name="icons-used-for-updates-in-windows-server-update-services"></a>Ícones usados para atualizações no Windows Server Update Services
 Atualizações no WSUS são representadas por um dos ícones a seguir.  
 Para exibir esses ícones, você precisa habilitar a coluna de substituição no console do Update Services.
 
### <a name="no-icon"></a>Nenhum ícone
 A atualização não tem nenhuma relação de substituição com qualquer outra atualização.

 **Problemas operacionais:**  

 Não há nenhum problemas operacionais.  
 
### <a name="superseding-icon"></a>Ícone substituta
 ![ícone](../../media/wsus/wsus-superseding.png) Esta atualização substitui outras atualizações.

 **Problemas operacionais:**  

 Não há nenhum problemas operacionais.  

### <a name="superseded--superseding-icon"></a>Ícone substituído & substituta
 ![ícone](../../media/wsus/wsus-superseded.png) Esta atualização é substituída por outra atualização e substitui outras atualizações.

 **Problemas operacionais:**  

 Substitua essas atualizações com as atualizações substitutas quando possível.
 
### <a name="superseded-icon"></a>Ícone substituído
 ![ícone](../../media/wsus/wsus-superseded-leaf.png) Essa atualização é substituída por outra atualização.

 **Problemas operacionais:**  

 Substitua essas atualizações com as atualizações substitutas quando possível.
