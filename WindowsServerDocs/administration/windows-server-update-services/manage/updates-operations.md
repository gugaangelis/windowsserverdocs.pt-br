---
title: Operações de atualizações
description: Tópico do Windows Server Update Service (WSUS) - como gerenciar atualizações, incluindo o processo de aprovação
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-wsus
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 4cb7ff54-3014-4e91-842a-a7b831ea59ff
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 4d99e006a03e12d7201390748aec8671236cf297
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59822547"
---
# <a name="updates-operations"></a>Operações de atualizações

>Aplica-se a: Windows Server (canal semestral), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Depois que as atualizações forem sincronizadas com o servidor WSUS, eles serão verificados automaticamente para relevância para os computadores de cliente do servidor. No entanto, você deve aprovar as atualizações antes de serem implantadas para os computadores em sua rede. Quando você aprova uma atualização, você está basicamente informando WSUS o que fazer com ele (as opções são **instale** ou **declínio** para uma nova atualização). Você pode aprovar atualizações para o **todos os computadores** grupo ou de subgrupos. Se você aprovar uma atualização, seu status de aprovação permanece **não aprovado**, e o servidor do WSUS permite que os clientes avaliar se elas precisam de atualização.

Se o servidor do WSUS estiver em execução no modo de réplica, você não poderá aprovar as atualizações no servidor do WSUS. Para obter mais informações sobre o modo de réplica, consulte [modo de réplica do WSUS em execução](running-wsus-replica-mode.md).

## <a name="approving-updates"></a>Aprovar atualizações
Você pode aprovar a instalação das atualizações para todos os computadores na rede do WSUS ou para grupos de computadores diferentes. Após aprovar uma atualização, você pode fazer um (ou mais) das seguintes opções:

-   Aplica esta aprovação para grupos filho, se houver.

-   Defina um prazo para instalação automática. Quando você seleciona essa opção, você definir horários específicos e as datas de instalar atualizações, substituindo quaisquer configurações nos computadores cliente. Além disso, você pode especificar uma data passada para o prazo final para aprovar uma atualização imediatamente (para ser instalado na próxima vez em que computadores cliente entre em contato com o servidor do WSUS).

-   Remova uma atualização instalada se essa atualização oferece suporte à remoção.

Há duas considerações importantes que você deve ter em mente:

-   Primeiro, é possível definir um prazo para instalação automática de uma atualização se a entrada do usuário for necessária (por exemplo, definir uma configuração relevante para a atualização). Para determinar se uma atualização exigirá a entrada do usuário, veja o **podem solicitar a entrada do usuário** campo nas propriedades de atualização para uma atualização exibido na **atualiza** página. Além disso, procure uma mensagem na **aprovar atualizações** caixa que diz: "**a atualização selecionada requer entrada do usuário e não oferece suporte a um prazo de instalação**."

-   Se houver atualizações para o componente de servidor do WSUS, você não pode aprovar outras atualizações nos sistemas cliente até que a atualização do WSUS seja aprovada. Você verá esta mensagem de aviso na caixa de diálogo Aprovar atualizações: "Há atualizações do WSUS que não foram aprovadas. Você deve aprovar as atualizações do WSUS antes de aprovar essa atualização." Nesse caso, você deve clique no nó atualizações do WSUS e certifique-se de que todas as atualizações nessa exibição foi aprovadas antes de retornar para as atualizações gerais.

#### <a name="to-approve-updates"></a>Para aprovar atualizações

1.  No console administrativo do WSUS, clique em **atualizações** e, em seguida, clique em **todas as atualizações**.

2.  Na lista de atualizações, selecione a atualização que você deseja aprovar e com o botão direito (ou vá para o painel de ações), na caixa de diálogo Aprovar atualizações, selecione o grupo de computadores para o qual você deseja aprovar a atualização e clique na seta ao lado dele.

3.  Selecione **aprovadas para instalação**e, em seguida, clique em **aprovar**.

4.  O **progresso da aprovação** janela exibirá o progresso para concluir a aprovação. Quando o processo for concluído, o **fechar** botão é exibido. Clique em **Fechar**.

5.  Você pode selecionar um prazo final, clicando duas vezes a atualização, selecionando o grupo de computadores apropriado, clicando na seta ao lado dele e, em seguida, clicando em **prazo**.

    -   Você pode selecionar um dos prazos padrão (uma semana, duas semanas, um mês), ou você pode clicar em **personalizado** para especificar uma data e hora.

    -   Se você quiser uma atualização para ser instalado assim que o contato de computadores cliente do servidor, clique em **personalizado**e, em seguida, defina uma data e hora para a data e hora atuais ou para um no passado.

#### <a name="to-approve-multiple-updates"></a>Para aprovar várias atualizações

1.  No console administrativo do WSUS, clique em **atualizações** e, em seguida, clique em **todas as atualizações**.

2.  Para selecionar várias atualizações contíguas, pressione **shift** enquanto seleciona as atualizações. Para selecionar várias atualizações não contíguas, pressione e segure **CTRL** enquanto seleciona as atualizações.

3.  A seleção de mouse e clique em **aprovar**. O **aprovar atualizações** caixa de diálogo é aberta com o **status de aprovação** definido como **Manter aprovações existentes** e o **Okey** botão desabilitado.

4.  Você pode alterar as aprovações para grupos individuais, mas fazer então não afeta as aprovações de filho. Selecione o grupo para o qual você deseja alterar a aprovação e clique na seta à esquerda. No menu de atalho, clique em **aprovadas para instalação**.

5.  A aprovação para o grupo selecionado é alterado para **instalar**. Se não houver nenhum grupo filho, sua aprovação permanece **Manter aprovação existente**. Para alterar a aprovação para os grupos filho, clique no grupo e clique na seta à esquerda. No menu de atalho, clique em **aplicar aos filhos**.

6.  Para definir um filho específicos ao herdar todos os seu aprovação do pai, clique o filho e clique na seta à esquerda. No menu de atalho, clique em **mesmo que pai**. Se você definir um filho herdam as aprovações, mas não estiver alterando as aprovações de pai, filho herda as aprovações existentes do pai.

7.  Se você quiser o comportamento de aprovação para alterar para todos os filhos, aprove **todos os computadores**e, em seguida, escolha **aplicar aos filhos**.

8.  Clique em **Okey** depois de definir todas as suas aprovações. O **progresso da aprovação** janela exibirá o progresso para concluir a aprovação. Quando o processo for concluído, o **fechar** botão estará disponível. Clique em **Fechar**.

## <a name="declining-updates"></a>Recusar atualizações
Se você selecionar essa opção, a atualização é removida da lista padrão de atualizações disponíveis e o servidor do WSUS não oferecerá a atualização para clientes, para avaliação ou instalação. Você pode acessar essa opção selecionando-se uma atualização ou um grupo de atualizações e clicando duas vezes ou painel de ações. As atualizações recusadas aparecerá na lista de atualizações somente se você selecionar **recusadas** na lista de aprovação ao especificar o filtro para a lista de atualização sob **exibição**.

#### <a name="to-decline-updates"></a>Para recusar atualizações

1.  No console administrativo do WSUS, clique em **atualizações**e, em seguida, clique em **todas as atualizações**.

2.  Na lista de atualizações, selecione uma ou mais atualizações que você deseja recusar.

3.  Selecione **declínio**e, em seguida, clique em **Sim** na mensagem de confirmação.

## <a name="cleaning-up-declined-updates"></a>Limpeza de atualizações recusadas
As atualizações recusadas continuam a consumir alguns recursos de servidor do WSUS. Você deve executar a limpeza de servidor o Assistente para remover as atualizações recusadas do banco de dados do WSUS. Consulte: [A Assistente de limpeza do servidor](the-server-cleanup-wizard.md), para obter mais detalhes.

## <a name="reinstating-declined-updates"></a>Reinstaurando atualizações recusadas
Depois que uma atualização foi recusada, você ainda poderá restabelecê-lo.

#### <a name="to-reinstate-declined-updates"></a>Para reabilitar atualizações recusadas

1.  No console administrativo do WSUS, clique em **atualizações** e, em seguida, clique em **todas as atualizações**.

2.  alterar **aprovação** à **recusadas** e clique em **atualizar**. Carrega a lista de atualizações recusadas.

3.  Na lista de atualizações, selecione um ou mais atualizações recusadas que você deseja restabelecer.

4.  Para restabelecer uma atualização específica, clique com botão direito na atualização e selecione **aprovar**. No **aprovar atualizações** caixa de diálogo, clique em **Okey** aplicar novamente o status de aprovação padrão "Aprovado". A atualização aparecerá na lista como **não aprovado** em vez de recusadas.

Depois que uma atualização recusada foram limpos usando a Assistente de limpeza de servidor do WSUS, ele será excluído do servidor do WSUS e não aparecerá mais no modo de exibição de todas as atualizações. Você pode importar novamente recusadas, atualizações limpas do catálogo do Microsoft Update. Para obter mais informações, consulte [WSUS e o Site do catálogo](wsus-and-the-catalog-site.md).

## <a name="change-an-approved-update-to-not-approved"></a>alterar uma atualização aprovada não aprovadas
Se uma atualização foi aprovada e você optar por não instalá-lo no momento e, em vez disso, deseja salvá-la para uma hora futura, você pode alterar a atualização com o status de não aprovado. Isso significa que a atualização permanecerá na lista padrão de atualizações disponíveis e irá relatar a conformidade do cliente, mas não será instalada nos clientes.

#### <a name="to-change-an-update-from-approved-to-not-approved"></a>Para alterar uma atualização aprovada não aprovadas

1.  No console administrativo do WSUS, clique em **atualizações**e, em seguida, clique em **todas as atualizações**.

2.  Na lista de atualizações, selecione um ou mais atualizações aprovadas que você deseja alterar não aprovadas.

3.  No menu de atalho ou o **ações** painel, selecione **não aprovado**e, em seguida, clique em **Sim** na mensagem de confirmação.

## <a name="approving-updates-for-removal"></a>Aprovar atualizações para remoção
Você pode aprovar uma atualização para remoção (ou seja, para desinstalar uma atualização já instalado). Essa opção está disponível somente se a atualização já está instalada e dá suporte à remoção. Você pode especificar um prazo final para a atualização a ser desinstalado ou especificar uma data passada para o prazo final, se você quiser remover a atualização imediatamente (na próxima vez que os computadores cliente entre em contato com o servidor do WSUS).

É importante mencionar que nem todas as atualizações oferece suporte a remoção. Você pode ver se uma atualização dá suporte à remoção selecionando uma atualização individual e observando os **detalhes** painel. Sob **detalhes adicionais**, você verá o **removível** categoria. Se a atualização não pode ser removida por meio do WSUS, em alguns casos ela pode ser removida com **adicionar ou remover programas** partir **painel de controle**.

#### <a name="to-approve-updates-for-removal"></a>Para aprovar as atualizações para remoção

1.  No console administrativo do WSUS, clique em **atualizações** e, em seguida, clique em **todas as atualizações**.

2.  Na lista de atualizações, selecione uma ou mais atualizações que você deseja aprovar a remoção e com o botão direito-los (ou vá para o **ações** painel).

3.  No **aprovar atualizações** caixa de diálogo, selecione o grupo de computador do qual você deseja remover a atualização e clique na seta ao lado dele.

4.  Selecione **aprovadas para remoção**e, em seguida, clique no **remover** botão.

5.  Após a aprovação de remoção, você pode selecionar um prazo clicando duas vezes a atualização mais uma vez, selecionando o grupo de computadores apropriado e, em seguida, clicando na seta ao lado dele. Em seguida, selecione **prazo**. Você pode selecionar um entre os prazos padrão (uma semana, duas semanas, um mês), ou você pode clicar em **personalizado** para selecionar uma data e hora específicas.

6.  Se você quiser uma atualização a ser removido, assim que o contato de computadores cliente do servidor, clique em **personalizado**e definir uma data no passado.

## <a name="approving-updates-automatically"></a>Aprovar atualizações automaticamente
Você pode configurar seu servidor do WSUS para aprovação automática de certas atualizações. Você também pode especificar a aprovação automática das revisões de atualizações existentes como eles se tornam disponíveis. Essa opção é selecionada por padrão. Uma revisão é uma versão de uma atualização que tenha tido alterações feitas nele (por exemplo, ele pode ter expirado ou suas regras de aplicabilidade podem ter alterado). Se você não optar por aprovar a versão revisada de uma atualização automaticamente, o WSUS usará a versão mais antiga e é necessário aprovar manualmente a revisão da atualização.

Você pode criar regras que o servidor do WSUS será aplicada automaticamente durante a sincronização. Você especifica quais atualizações para instalação, por classificação da atualização, por produto e por grupo de computadores que você deseja aprovar automaticamente. Isso se aplica somente a novas atualizações, em vez de atualizações revisadas. Você também pode especificar um prazo de aprovação de atualização, que define um número de dias e um horário específico da oferta antes da atualização aprovada data limite instalado. Essas configurações estão disponíveis na **opções** painel, em **aprovações automáticas**.

#### <a name="to-automatically-approve-updates"></a>Para aprovar automaticamente as atualizações

1.  No console de administração do WSUS, clique em **opções**e, em seguida, clique em **aprovações automáticas**.

2.  Em **Regras de Atualização**, clique em **Nova Regra**.

3.  No **Adicionar regra** caixa de diálogo, em **etapa 1: selecionar propriedades**, selecione se deseja usar **quando uma atualização está em uma classificação específica** ou **quando uma atualização está em um produto específico** (ou ambos) como critérios. Opcionalmente, selecione se deseja **definir um prazo** para a aprovação.

4.  Na **etapa 2: editar as propriedades** clique nas propriedades de sublinhado para selecionar as classificações, os produtos e os grupos de computadores para o qual você deseja aprovações automáticas, conforme aplicável. Como opção, escolha a aprovação de atualização de dia e hora do prazo.

5.  No **etapa 3: Especifique uma caixa de nome**, digite um nome exclusivo para a regra.

6.  Clique em **OK**.

Regras de aprovação automática não se aplicará às atualizações que exigem um contrato de licença de usuário final (EULA) que ainda não foi aceito no servidor. Se você achar que a aplicação de uma regra de aprovação automática não faz com que todas as atualizações relevantes a serem aprovadas, você deve aprovar essas atualizações manualmente.

## <a name="automatically-approving-revisions-to-updates-and-declining-expired-updates"></a>Aprovar automaticamente revisões de atualizações e recusar atualizações de expiradas
A seção de aprovações automáticas do painel de opções contém uma opção padrão para aprovar automaticamente revisões de atualizações aprovadas. Você também pode definir seu servidor do WSUS para recusar automaticamente as atualizações expiradas. Se você optar por não aprovar a versão revisada de uma atualização automaticamente, o servidor do WSUS usará a revisão mais antiga e é necessário aprovar manualmente a revisão da atualização.

> [!NOTE]
> Uma revisão é uma versão de uma atualização que foi alterado (por exemplo, ele pode ter expirado ou atualizar regras de aplicabilidade).

#### <a name="to-automatically-approve-revisions-to-updates-and-decline-expired-updates"></a>Para aprovar revisões de atualizações automaticamente e recusar atualizações expiradas

1.  No console de administração do WSUS, clique em **opções**e, em seguida, clique em **aprovações automáticas**.

2.  Sobre o **avançado** guia, certifique-se de que ambos **aprovar automaticamente novas revisões de atualizações aprovadas** e **recusar atualizações automaticamente quando uma nova revisão faz com que eles expirem** estão selecionados.

3.  Clique em OK.

    > [!NOTE]
    > Manter os valores padrão para essas opções permite que você manter um bom desempenho em sua rede do WSUS. Se você não quiser que as atualizações expiradas recusa automaticamente, você deve garantir recusá-las manualmente em intervalos periódicos.

## <a name="automatically-declining-superseded-updates"></a>Recusar automaticamente atualizações substituídas
Quando você aprova uma nova atualização que substitui uma atualização existente que é aprovada automaticamente, a atualização substituída se torna "Não aplicável" em um computador ou dispositivo depois que a atualização mais recente foi instalada. Você pode verificar no console do WSUS que não é se aplica uma atualização para todos os computadores. Quando esse for o caso, a atualização pode ser recusada com segurança. Além disso, a atualização pode ser automaticamente recusada quando você executa a Assistente de limpeza de servidor do WSUS.

Para procurar atualizações substituídas, você pode selecionar a coluna do sinalizador "Substituído" no modo de exibição de todas as atualizações e classifique essa coluna. Haverá quatro grupos:

-   Atualizações do que nunca foram substituídas (um ícone em branco).

-   As atualizações que foram substituídas, mas têm nunca substituído outra atualização (um ícone com um quadrado azul na parte inferior).

-   Atualizações que foram substituídas e tiveram substituído outra atualização (um ícone com um quadrado azul no meio).

-   Atualizações que têm substitui outra atualização (um ícone com um quadrado azul na parte superior).

Não há nenhum recurso no Windows Server Update Services que automaticamente recusa atualizações substituídas após a aprovação de uma atualização mais recente. É recomendável primeiro definir a aprovação para "Aprovado" e, em seguida, use a Assistente de limpeza do servidor para recusar a atualização automaticamente quando todas as condições relevantes tiverem sido atendidas. Para obter mais informações, consulte: [A Assistente de limpeza do servidor](the-server-cleanup-wizard.md).

## <a name="approving-superseding-or-superseded-updates"></a>Aprovando atualizações substituídas ou substitutas
Normalmente, uma atualização que substitui outras atualizações faz uma ou mais das seguintes opções:

-   Melhora, melhora ou acrescenta à correção fornecida por um ou mais atualizações lançadas anteriormente.

-   Melhora a eficiência do seu pacote de arquivos de atualização, que é instalado em computadores cliente, se a atualização for aprovada para instalação. Por exemplo, a atualização substituída pode conter arquivos que não são mais relevantes para a correção ou para os sistemas operacionais agora compatíveis com a nova atualização, portanto, esses arquivos não são incluídos no pacote de arquivos da atualização substituta.

-   Atualiza versões mais recentes dos sistemas operacionais. Também é importante observar que a atualização substituta pode não oferecer suporte a versões anteriores dos sistemas operacionais.

Por outro lado, uma atualização que é substituída por outra atualização faz o seguinte:

-   Corrige um problema semelhante da atualização que substitui-lo. No entanto, a atualização que a substitui pode melhorar a correção que fornece a atualização substituída.

-   Atualiza versões anteriores dos sistemas operacionais. Em alguns casos, essas versões de sistemas operacionais não serão mais atualizadas pela atualização substituta.

No painel de detalhes de uma atualização individual, um ícone de informação e uma mensagem na parte superior indica que ele substitui ou é substituído por outra atualização. Além disso, você pode determinar quais atualizações substituem ou são substituídas pela atualização examinando os **atualizações que substituem essa atualização** e **atualizações substituídas por essa atualização** entradas na **detalhes adicionais** seção de **propriedades**. Painel de detalhes de uma atualização é exibida abaixo da lista de atualizações.

O WSUS não automaticamente recusa atualizações substituídas e é recomendável que você não pressuponha que atualizações substituídas devam ser recusadas em favor a nova atualização de substituição. Antes de recusar uma atualização substituída, certifique-se de que não é necessária por qualquer um dos seus computadores cliente. A seguir está exemplos de cenários em que você talvez precise instalar uma atualização substituída:

-   Se uma atualização substituta oferece suporte a mais recentes somente versões de um sistema operacional, e alguns de seus computadores cliente executam versões anteriores do sistema operacional.

-   Se uma atualização substituta tem aplicabilidade mais restrita que a atualização que está substituindo, que a tornaria inadequada para alguns computadores cliente.

-   Se uma atualização não substitui mais uma atualização anteriormente lançada devido a novas alterações. É possível que as alterações a cada versão, por meio de uma atualização não substitui mais uma atualização que anteriormente substituída em uma versão anterior. Nesse cenário, você ainda verá uma mensagem sobre a atualização substituída, mesmo que a atualização que a substitui por uma atualização que não foi substituída.


