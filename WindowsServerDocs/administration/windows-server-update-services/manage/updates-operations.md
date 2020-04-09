---
title: Operações de atualizações
description: Tópico Windows Server Update Service (WSUS)-como gerenciar atualizações, incluindo o processo de aprovação
ms.prod: windows-server
ms.technology: manage-wsus
ms.topic: article
ms.assetid: 4cb7ff54-3014-4e91-842a-a7b831ea59ff
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 327bff2e678e278dcba05ce1df807dc3842a56cd
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80828489"
---
# <a name="updates-operations"></a>Operações de atualizações

>Aplicável a: Windows Server (canal semestral), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Depois que as atualizações tiverem sido sincronizadas com o servidor do WSUS, elas serão verificadas automaticamente para fins de relevância para os computadores cliente do servidor. No entanto, você deve aprovar as atualizações antes que elas sejam implantadas nos computadores da sua rede. Ao aprovar uma atualização, você está basicamente dizendo ao WSUS o que fazer com ele (suas opções são **instalar** ou **recusar** uma nova atualização). Você pode aprovar atualizações para o grupo **todos os computadores** ou para subgrupos. Se você não aprovar uma atualização, seu status de aprovação permanecerá **não aprovado**e o servidor do WSUS permitirá que os clientes avaliem se precisam ou não da atualização.

Se o servidor do WSUS estiver sendo executado no modo de réplica, você não poderá aprovar as atualizações no servidor do WSUS. Para obter mais informações sobre o modo de réplica, consulte [executando o modo de réplica do WSUS](running-wsus-replica-mode.md).

## <a name="approving-updates"></a>Aprovar atualizações
Você pode aprovar a instalação de atualizações para todos os computadores em sua rede do WSUS ou para grupos de computadores diferentes. Depois de aprovar uma atualização, você pode fazer uma (ou mais) das seguintes opções:

-   Aplique essa aprovação a grupos filho, se houver.

-   Defina um prazo para instalação automática. Ao selecionar essa opção, você define as horas e as datas específicas para instalar atualizações, substituindo as configurações nos computadores cliente. Além disso, você pode especificar uma data passada para o prazo se quiser aprovar uma atualização imediatamente (para ser instalado na próxima vez que os computadores cliente entrarem em contato com o servidor do WSUS).

-   Remover uma atualização instalada se essa atualização der suporte à remoção.

Há duas considerações importantes que você deve ter em mente:

-   Primeiro, você não pode definir um prazo para a instalação automática para uma atualização se a entrada do usuário for necessária (por exemplo, especificando uma configuração relevante para a atualização). Para determinar se uma atualização exigirá entrada do usuário, examine o campo de **entrada do usuário solicitar solicitação** nas propriedades de atualização de uma atualização exibida na página **atualizações** . Verifique também uma mensagem na caixa **aprovar atualizações** que diz, **a atualização selecionada requer entrada do usuário e não dá suporte a um prazo de instalação**.

-   Se houver atualizações para o componente do servidor do WSUS, você não poderá aprovar outras atualizações para sistemas cliente até que a atualização do WSUS seja aprovada. Você verá essa mensagem de aviso na caixa de diálogo aprovar atualizações: há atualizações do WSUS que não foram aprovadas. Você deve aprovar as atualizações do WSUS antes de aprovar esta atualização. Nesse caso, você deve clicar no nó atualizações do WSUS e certificar-se de que todas as atualizações nesse modo de exibição foram aprovadas antes de retornar às atualizações gerais.

#### <a name="to-approve-updates"></a>Para aprovar atualizações

1.  No console administrativo do WSUS, clique em **atualizações** e em **todas as atualizações**.

2.  Na lista de atualizações, selecione a atualização que você deseja aprovar e clique com o botão direito do mouse (ou vá para o painel Ações) e, na caixa de diálogo aprovar atualizações, selecione o grupo de computadores para o qual você deseja aprovar a atualização e clique na seta ao lado dela.

3.  Selecione **aprovado para instalação**e clique em **aprovar**.

4.  A janela **progresso da aprovação** exibirá o progresso para concluir a aprovação. Quando o processo for concluído, o botão **fechar** será exibido. Clique em **Fechar**.

5.  Você pode selecionar um prazo clicando com o botão direito do mouse na atualização, selecionando o grupo de computadores apropriado, clicando na seta ao lado dele e, em seguida, clicando em **prazo**.

    -   Você pode selecionar um dos prazos padrão (uma semana, duas semanas, um mês) ou pode clicar em **personalizado** para especificar uma data e hora.

    -   Se você quiser que uma atualização seja instalada assim que os computadores cliente entrarem em contato com o servidor, clique em **personalizado**e defina uma data e hora para a data e hora atuais ou para uma no passado.

#### <a name="to-approve-multiple-updates"></a>Para aprovar várias atualizações

1.  No console administrativo do WSUS, clique em **atualizações** e em **todas as atualizações**.

2.  Para selecionar várias atualizações contíguas, pressione **Shift** enquanto seleciona as atualizações. Para selecionar várias atualizações não contíguas, pressione e mantenha pressionada a **tecla CTRL** enquanto seleciona as atualizações.

3.  Clique com o botão direito do mouse na seleção e clique em **aprovar**. A caixa de diálogo **aprovar atualizações** é aberta com o **status de aprovação** definido para **manter as aprovações existentes** e o botão **OK** desabilitado.

4.  Você pode alterar as aprovações para os grupos individuais, mas fazer isso não afetará as aprovações filhas. Selecione o grupo para o qual você deseja alterar a aprovação e clique na seta à esquerda. No menu de atalho, clique em **aprovado para instalar**.

5.  A aprovação do grupo selecionado muda para **instalar**. Se houver algum grupo filho, sua aprovação permanecerá para **manter a aprovação existente**. Para alterar a aprovação dos grupos filho, clique no grupo e clique na seta à esquerda. No menu de atalho, clique em **aplicar a filhos**.

6.  Para definir um filho específico para herdar toda sua aprovação do pai, clique no filho e clique na seta à esquerda. No menu de atalho, clique em **igual a pai**. Se você definir um filho para herdar aprovações, mas não estiver alterando as aprovações pai, o filho herdará as aprovações existentes do pai.

7.  Se você quiser que o comportamento de aprovação seja alterado para todos os filhos, aprove **todos os computadores**e, em seguida, escolha **aplicar a filhos**.

8.  Clique em **OK** depois de definir todas as suas aprovações. A janela **progresso da aprovação** exibirá o progresso para concluir a aprovação. Quando o processo for concluído, o botão **fechar** estará disponível. Clique em **Fechar**.

## <a name="declining-updates"></a>Recusando atualizações
Se você selecionar essa opção, a atualização será removida da lista padrão de atualizações disponíveis e o servidor do WSUS não oferecerá a atualização para os clientes, seja para avaliação ou instalação. Você pode acessar essa opção selecionando uma atualização ou grupo de atualizações e clicando com o botão direito do mouse ou indo para o painel Ações. As atualizações recusadas serão exibidas na lista de atualizações somente se você selecionar **recusadas** na lista aprovação ao especificar o filtro para a lista de atualizações em **exibição**.

#### <a name="to-decline-updates"></a>Para recusar atualizações

1.  No console administrativo do WSUS, clique em **atualizações**e em **todas as atualizações**.

2.  Na lista de atualizações, selecione uma ou mais atualizações que você deseja recusar.

3.  Selecione **recusar**e, em seguida, clique em **Sim** na mensagem de confirmação.

## <a name="cleaning-up-declined-updates"></a>Limpando atualizações recusadas
As atualizações recusadas continuam a consumir alguns recursos do servidor WSUS. Você deve executar o assistente de limpeza do servidor para remover atualizações recusadas do banco de dados do WSUS. Consulte: [Assistente para limpeza do servidor](the-server-cleanup-wizard.md), para obter detalhes adicionais.

## <a name="reinstating-declined-updates"></a>Atualizações recusadas do reinstaurando
Depois que uma atualização for recusada, você ainda poderá reinstalá-la.

#### <a name="to-reinstate-declined-updates"></a>Para restabelecer atualizações recusadas

1.  No console administrativo do WSUS, clique em **atualizações** e em **todas as atualizações**.

2.  Altere a **aprovação** para **recusada** e clique em **Atualizar**. A lista de carregamentos de atualizações recusadas.

3.  Na lista de atualizações, selecione uma ou mais atualizações recusadas que você deseja restabelecer.

4.  Para reabilitar uma atualização específica, clique com o botão direito do mouse na atualização e selecione **aprovar**. Na caixa de diálogo **aprovar atualizações** , clique em **OK** para aplicar novamente o status de aprovação padrão não aprovado. A atualização será mostrada na lista como **não aprovada** , em vez de recusada.

Depois que uma atualização recusada for limpa usando o assistente de limpeza do servidor do WSUS, ela será excluída do servidor do WSUS e não aparecerá mais no modo de exibição todas as atualizações. Você pode importar novamente as atualizações recusadas e removidas do catálogo de Microsoft Update. Para obter informações adicionais, consulte [WSUS e o site do catálogo](wsus-and-the-catalog-site.md).

## <a name="change-an-approved-update-to-not-approved"></a>Alterar uma atualização aprovada para não aprovada
Se uma atualização tiver sido aprovada e você decidir não instalá-la agora e, em vez disso, quiser salvá-la para um momento futuro, você poderá alterar a atualização para um status de não aprovado. Isso significa que a atualização permanecerá na lista padrão de atualizações disponíveis e relatará a conformidade do cliente, mas não será instalada em clientes.

#### <a name="to-change-an-update-from-approved-to-not-approved"></a>Para alterar uma atualização de aprovada para não aprovada

1.  No console administrativo do WSUS, clique em **atualizações**e em **todas as atualizações**.

2.  Na lista de atualizações, selecione uma ou mais atualizações aprovadas que você deseja alterar para não aprovadas.

3.  No menu de atalho ou no painel **ações** , selecione **não aprovado**e, em seguida, clique em **Sim** na mensagem de confirmação.

## <a name="approving-updates-for-removal"></a>Aprovando atualizações para remoção
Você pode aprovar uma atualização para remoção (ou seja, para desinstalar uma atualização já instalada). Essa opção só estará disponível se a atualização já estiver instalada e der suporte à remoção. Você pode especificar um prazo para a atualização a ser desinstalada ou especificar uma data anterior para o prazo se desejar remover a atualização imediatamente (na próxima vez que os computadores cliente entrarem em contato com o servidor do WSUS).

É importante mencionar que nem todas as atualizações dão suporte à remoção. Você pode ver se uma atualização dá suporte à remoção selecionando uma atualização individual e examinando o painel de **detalhes** . Em **detalhes adicionais**, você verá a categoria **removível** . Se a atualização não puder ser removida por meio do WSUS, em alguns casos, ela poderá ser removida com **Adicionar ou remover programas** do **painel de controle**.

#### <a name="to-approve-updates-for-removal"></a>Para aprovar atualizações para remoção

1.  No console administrativo do WSUS, clique em **atualizações** e em **todas as atualizações**.

2.  Na lista de atualizações, selecione uma ou mais atualizações que você deseja aprovar para remoção e clique com o botão direito do mouse nelas (ou vá para o painel **ações** ).

3.  Na caixa de diálogo **aprovar atualizações** , selecione o grupo de computadores do qual você deseja remover a atualização e clique na seta ao lado dela.

4.  Selecione **aprovado para remoção**e, em seguida, clique no botão **remover** .

5.  Depois que a aprovação de remoção for concluída, você poderá selecionar um prazo clicando com o botão direito do mouse na atualização mais uma vez, selecionando o grupo de computadores apropriado e clicando na seta ao lado dela. Em seguida, selecione **prazo**. Você pode selecionar um dos prazos padrão (uma semana, duas semanas, um mês) ou pode clicar em **personalizado** para selecionar uma data e hora específicas.

6.  Se você quiser que uma atualização seja removida assim que os computadores cliente entrarem em contato com o servidor, clique em **personalizado**e defina uma data no passado.

## <a name="approving-updates-automatically"></a>Aprovando atualizações automaticamente
Você pode configurar o servidor do WSUS para aprovação automática de determinadas atualizações. Você também pode especificar a aprovação automática de revisões para as atualizações existentes à medida que elas se tornarem disponíveis. Essa opção é selecionada por padrão. Uma revisão é uma versão de uma atualização que teve alterações feitas nela (por exemplo, ela pode ter expirado ou suas regras de aplicabilidade podem ter sido alteradas). Se você não optar por aprovar automaticamente a versão revisada de uma atualização, o WSUS usará a versão mais antiga e você deverá aprovar manualmente a revisão da atualização.

Você pode criar regras que o servidor do WSUS aplicará automaticamente durante a sincronização. Você especifica quais atualizações deseja aprovar automaticamente para instalação, por classificação de atualização, por produto e por grupo de computadores. Isso se aplica somente a novas atualizações, em oposição às atualizações revisadas. Você também pode especificar um prazo de aprovação de atualização, que define um número de dias e um tempo específico de oferta antes que a atualização aprovada seja instalada no prazo final. Essas configurações estão disponíveis no painel **Opções** , em **aprovações automáticas**.

#### <a name="to-automatically-approve-updates"></a>Para aprovar atualizações automaticamente

1.  No console de administração do WSUS, clique em **Opções**e, em seguida, clique em **aprovações automáticas**.

2.  Em **Regras de Atualização**, clique em **Nova Regra**.

3.  Na caixa de diálogo **Adicionar regra** , em **etapa 1: selecionar Propriedades**, selecione se deseja usar **quando uma atualização estiver em uma classificação específica** ou **quando uma atualização estiver em um produto específico** (ou em ambos) como critério. Opcionalmente, selecione se deseja **definir um prazo** para a aprovação.

4.  Na **etapa 2: editar as propriedades** clique nas propriedades sublinhadas para selecionar as classificações, os produtos e os grupos de computadores para os quais você deseja aprovações automáticas, conforme aplicável. Opcionalmente, escolha a data e a hora do prazo de aprovação da atualização.

5.  Na **caixa etapa 3: especificar um nome**, digite um nome exclusivo para a regra.

6.  Clique em **OK**.

As regras de aprovação automática não se aplicarão a atualizações que exigem um EULA (contrato de licença de usuário final) que ainda não foi aceito no servidor. Se você achar que a aplicação de uma regra de aprovação automática não faz com que todas as atualizações relevantes sejam aprovadas, você deve aprovar essas atualizações manualmente.

## <a name="automatically-approving-revisions-to-updates-and-declining-expired-updates"></a>Aprovando revisões automaticamente para atualizações e recusando atualizações expiradas
A seção aprovações automáticas do painel opções contém uma opção padrão para aprovar automaticamente as revisões para atualizações aprovadas. Você também pode definir o servidor do WSUS para recusar automaticamente as atualizações expiradas. Se você optar por não aprovar automaticamente a versão revisada de uma atualização, o servidor do WSUS usará a revisão mais antiga e você deverá aprovar manualmente a revisão da atualização.

> [!NOTE]
> Uma revisão é uma versão de uma atualização que foi alterada (por exemplo, ela pode ter expirado ou ter regras de aplicabilidade atualizadas).

#### <a name="to-automatically-approve-revisions-to-updates-and-decline-expired-updates"></a>Para aprovar automaticamente as revisões para atualizações e recusar atualizações expiradas

1.  No console de administração do WSUS, clique em **Opções**e, em seguida, clique em **aprovações automáticas**.

2.  Na guia **avançado** , certifique-se de que ambos **aprove automaticamente as novas revisões de atualizações aprovadas** e **recuse automaticamente as atualizações quando uma nova revisão fizer com que elas expirem** sejam selecionadas.

3.  Clique em OK.

    > [!NOTE]
    > Manter os valores padrão para essas opções permite que você mantenha um bom desempenho na sua rede do WSUS. Se você não quiser que as atualizações expiradas sejam recusadas automaticamente, lembre-se de recusá-las manualmente em uma base periódica.

## <a name="automatically-declining-superseded-updates"></a>Recusando atualizações substituídas automaticamente
Quando você aprova uma nova atualização que substitui uma atualização existente que é aprovada automaticamente, a atualização substituída se torna não aplicável a um computador ou dispositivo depois que a atualização mais recente é instalada. Você pode verificar no console do WSUS que uma atualização não é aplicável a todos os computadores. Quando esse for o caso, a atualização poderá ser recusada com segurança. Além disso, a atualização pode ser recusada automaticamente quando você executa o assistente de limpeza do servidor do WSUS.

Para procurar atualizações substituídas, você pode selecionar a coluna sinalizador substituído na exibição todas as atualizações e classificar essa coluna. Haverá quatro grupos:

-   Atualizações que nunca foram substituídas (um ícone em branco).

-   Atualizações que foram substituídas, mas nunca substituíram outra atualização (um ícone com um quadrado azul na parte inferior).

-   Atualizações que foram substituídas e substituíram outra atualização (um ícone com um quadrado azul no meio).

-   Atualizações que substituíram outra atualização (um ícone com um quadrado azul na parte superior).

Não há nenhum recurso no Windows Server Update Services que recusa automaticamente as atualizações substituídas após a aprovação de uma atualização mais recente. É recomendável primeiro definir a aprovação como não aprovada e, em seguida, usar o assistente de limpeza do servidor para recusar a atualização automaticamente quando todas as condições relevantes forem satisfeitas. Para obter mais informações, consulte: [Assistente para limpeza do servidor](the-server-cleanup-wizard.md).

## <a name="approving-superseding-or-superseded-updates"></a>Aprovando atualizações substitutas ou substituídas
Normalmente, uma atualização que substitui outras atualizações faz um ou mais dos seguintes:

-   Aprimora, melhora ou adiciona a correção fornecida por uma ou mais atualizações lançadas anteriormente.

-   Melhora a eficiência de seu pacote de arquivos de atualização, que é instalado em computadores cliente se a atualização for aprovada para instalação. Por exemplo, a atualização substituída pode conter arquivos que não são mais relevantes para a correção ou para os sistemas operacionais que agora têm suporte pela nova atualização, para que esses arquivos não sejam incluídos no pacote de arquivos da atualização substituta.

-   Atualiza as versões mais recentes dos sistemas operacionais. Também é importante observar que a atualização substituta pode não oferecer suporte a versões anteriores de sistemas operacionais.

Por outro lado, uma atualização substituída por outra atualização faz o seguinte:

-   Corrige um problema semelhante ao da atualização que o substitui. No entanto, a atualização que o substitui pode melhorar a correção que a atualização substituída fornece.

-   Atualiza versões anteriores de sistemas operacionais. Em alguns casos, essas versões de sistemas operacionais não são mais atualizadas pela atualização substituta.

Em um painel de detalhes de uma atualização individual, um ícone informativo e uma mensagem na parte superior indicam que ele é substituído ou substituído por outra atualização. Além disso, você pode determinar quais atualizações são substituídas ou substituídas pela atualização examinando as atualizações que **substituem essa atualização** e **as atualizações substituídas por essas** entradas de atualização na seção **detalhes adicionais** das **Propriedades**. O painel de detalhes de uma atualização é exibido abaixo da lista de atualizações.

O WSUS não recusa atualizações substituídas automaticamente, e é recomendável que você não assuma que as atualizações substituídas devem ser recusadas em favor da nova atualização substituta. Antes de recusar uma atualização substituída, verifique se ela não é mais necessária para nenhum dos seus computadores cliente. Veja a seguir exemplos de cenários em que você pode precisar instalar uma atualização substituída:

-   Se uma atualização substituta der suporte apenas a versões mais recentes de um sistema operacional, e alguns dos seus computadores cliente executarem versões anteriores do sistema operacional.

-   Se uma atualização substituta tiver uma aplicabilidade mais restrita do que a atualização que ela substitui, o que o tornaria inadequado para alguns computadores cliente.

-   Se uma atualização não substituir mais uma atualização lançada anteriormente devido a novas alterações. É possível que, por meio das alterações em cada versão, uma atualização não substitua mais uma atualização que foi substituída anteriormente em uma versão anterior. Nesse cenário, você ainda verá uma mensagem sobre a atualização substituída, embora a atualização que a substitua tenha sido substituída por uma atualização que não tenha.


