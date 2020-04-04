---
title: Configurar sincronizações de atualização
description: Tópico Windows Server Update Service (WSUS)-como configurar e configurar sincronizações de atualização
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: manage-wsus
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: ddd5c395-451b-44a0-8e08-a05db26d2282
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 9c7bca5be7a8ec0e857cba65680fbc3b967af4f8
ms.sourcegitcommit: 3c3dfee8ada0083f97a58997d22d218a5d73b9c4
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/03/2020
ms.locfileid: "80639750"
---
# <a name="setting-up-update-synchronizations"></a>Configurar sincronizações de atualização

>Aplica-se a: Windows Server 2019, Windows Server (canal semestral), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Durante a sincronização, um servidor WSUS baixa atualizações (arquivos e metadados de atualização) de uma origem de atualização. Ele também baixa novas classificações e categorias de produtos, se houver. Quando o servidor do WSUS for sincronizado pela primeira vez, ele baixará todas as atualizações que você especificou quando configurou as opções de sincronização. Após a primeira sincronização, o servidor WSUS baixa apenas as atualizações da origem da atualização, bem como as revisões em metadados para atualizações existentes e as expirações para atualizações.

A primeira vez que um servidor WSUS baixa atualizações pode levar muito tempo. Se você estiver configurando vários servidores WSUS, poderá acelerar o processo para uma determinada extensão baixando todas as atualizações em um servidor WSUS e, em seguida, copiando as atualizações nos diretórios de conteúdo dos outros servidores WSUS.

Você pode copiar o conteúdo de um diretório de conteúdo do servidor do WSUS para outro. O local do diretório de conteúdo é especificado quando você executa o procedimento pós-instalação do WSUS. Você pode usar a ferramenta WSUSutil. exe para exportar metadados de atualização de um servidor WSUS para um arquivo. Em seguida, você pode importar esse arquivo para outros servidores WSUS.

## <a name="setting-up-update-synchronizations"></a>Configurar sincronizações de atualização
A página **Opções** é o ponto de acesso central no console de administração do WSUS para personalizar como o servidor do WSUS sincroniza as atualizações. Você pode especificar quais atualizações serão sincronizadas automaticamente, onde o servidor Obtém atualizações, configurações de conexão e a agenda de sincronização. Você também pode usar o assistente de configuração da página **Opções** para configurar ou reconfigurar o servidor do WSUS a qualquer momento.

### <a name="synchronizing-update-by-product-and-classification"></a>Sincronizando atualização por produto e classificação
Um servidor WSUS baixa atualizações com base nos produtos ou nas famílias de produto (por exemplo, Windows ou Windows Server 2008, Datacenter Edition) e classificações (por exemplo, atualizações críticas ou atualizações de segurança) que você especificar. na primeira sincronização, o servidor WSUS baixa todas as atualizações disponíveis nas categorias que você especificou. Em Sincronizações subsequentes, o servidor WSUS baixa apenas as atualizações mais recentes (ou as alterações nas atualizações já disponíveis no servidor do WSUS) para as categorias que você especificou.

Você pode especificar produtos e classificações de atualização na página **Opções** em **produtos e classificações**. Os produtos são listados em uma hierarquia, agrupados por família de produtos. Se você selecionar Windows, selecionará automaticamente todos os produtos que se enquadram na hierarquia do produto. Selecionando a caixa de seleção pai, você seleciona todos os itens sob ele, bem como todas as versões futuras. marcar as caixas de seleção filho não marcará as caixas de seleção pai. A configuração padrão para produtos é todos os produtos do Windows, e a configuração padrão de classificações é crítica e atualizações de segurança.

Se um servidor WSUS estiver sendo executado no modo de réplica, você não poderá executar essa tarefa. Para obter mais informações sobre o modo de réplica, consulte [executando o modo de réplica do WSUS](running-wsus-replica-mode.md)e [etapa 1: preparar para a implantação do WSUS](../plan/plan-your-wsus-deployment.md).

##### <a name="to-specify-update-products-and-classifications-for-synchronization"></a>Para especificar produtos e classificações de atualização para sincronização

1.  No console de administração do WSUS, clique no nó **Opções** .

2.  Clique em **produtos e classificações**e clique na guia **produtos** .

3.  Marque as caixas de seleção dos produtos ou das famílias de produtos que você deseja atualizar com o WSUS e, em seguida, clique em **OK**.

4.  Na guia **classificações** , marque as caixas de seleção das classificações de atualização que você deseja que o servidor do WSUS Sincronize e clique em **OK**.

> [!NOTE]
> Você pode remover produtos ou classificações da mesma maneira. O servidor WSUS deixará de sincronizar novas atualizações para os produtos que você limpou. No entanto, as atualizações que foram sincronizadas para esses produtos antes de serem apagadas permanecerão no servidor do WSUS e serão listadas como disponíveis.
> 
> Para remover esses produtos, recuse a atualização, conforme documentado nas [operações de atualizações](updates-operations.md)e, em seguida, use o assistente de [limpeza do servidor](the-server-cleanup-wizard.md) para removê-las.

### <a name="synchronizing-updates-by-language"></a>Sincronizando atualizações por idioma
O servidor WSUS baixa as atualizações com base nos idiomas que você especificar. Você pode sincronizar atualizações em todos os idiomas nos quais elas estão disponíveis ou pode especificar um subconjunto de idiomas. Se você tiver uma hierarquia de servidores WSUS e precisar baixar atualizações em idiomas diferentes, certifique-se de ter especificado todos os idiomas necessários no servidor upstream. Em um servidor downstream, você pode especificar um subconjunto dos idiomas especificados no servidor upstream.

### <a name="synchronizing-updates-from-the-microsoft-update-catalog"></a>Sincronizando atualizações do catálogo de Microsoft Update
para obter detalhes sobre a sincronização de atualizações do site do Microsoft Update Catalog, consulte: [WSUS e o site do catálogo](wsus-and-the-catalog-site.md).

## <a name="configuring-proxy-server-settings"></a>Definindo configurações do servidor proxy
Você pode configurar o servidor do WSUS para usar um servidor proxy durante a sincronização com um servidor upstream ou Microsoft Update. Essa configuração será aplicada somente quando o servidor WSUS executar sincronizações. Por padrão, o servidor do WSUS tentará se conectar diretamente ao servidor upstream ou Microsoft Update.

#### <a name="to-specify-a-proxy-server-for-synchronization"></a>Para especificar um servidor proxy para sincronização

1.  No console de administração do WSUS, clique em **Opções**e, em seguida, clique em **Atualizar origem e servidor proxy**.

2.  Na guia **servidor proxy** , marque a caixa de seleção **usar um servidor proxy ao sincronizar** e, em seguida, digite o nome do servidor e o número da porta do servidor proxy.

    > [!NOTE]
    > Configure o WSUS com o mesmo número de porta que o servidor proxy está configurado para usar.

    -   Se você quiser se conectar ao servidor proxy com credenciais de usuário específicas, marque a caixa de seleção **usar credenciais do usuário para se conectar ao servidor proxy** e insira o nome de usuário, o domínio e a senha do usuário nas caixas correspondentes.

    -   Se você quiser habilitar a autenticação básica para o usuário que está se conectando ao servidor proxy, marque a caixa de seleção **Permitir autenticação básica (senha enviada em texto não criptografado)** .

3.  Clique em **OK**.

    > [!NOTE]
    > Como o WSUS inicia todo o tráfego de rede, não há necessidade de configurar o Firewall do Windows em um servidor do WSUS conectado diretamente ao Microsoft Update.

## <a name="configuring-the-update-source"></a>Configurando a origem da atualização
A origem da atualização é o local do qual o servidor do WSUS obtém suas atualizações e atualiza os metadados. Você pode especificar que a origem da atualização deve ser Microsoft Update ou outro servidor do WSUS (o servidor do WSUS que atua como a origem da atualização é o servidor upstream, e o servidor é o servidor downstream).

As opções para personalizar como o servidor do WSUS sincroniza com a origem de atualização incluem o seguinte:

-   Você pode especificar uma porta personalizada para sincronização. Para obter informações sobre como configurar portas, consulte [etapa 3: configurar o WSUS](../deploy/2-configure-wsus.md) no guia de implantação do WSUS.

-   Você pode usar SSL (Secure Socket Layers) para proteger a sincronização de informações de atualização entre servidores WSUS. Para obter mais informações sobre como usar o SSL, consulte a seção "3,5. Proteger o WSUS com o protocolo protocolo SSL "da [etapa 3: configurar o WSUS](../deploy/2-configure-wsus.md) no guia de implantação do WSUS.

## <a name="synchronizing-manually-or-automatically"></a>Sincronizando manual ou automaticamente
Você pode sincronizar o servidor do WSUS manualmente ou especificar um horário para que ele seja sincronizado automaticamente.

#### <a name="to-manually-synchronize-the-wsus-server"></a>Para sincronizar manualmente o servidor do WSUS

1.  No console de administração do WSUS, clique em **Opções**e, em seguida, clique em **agenda de sincronização**.

2.  Clique em **sincronizar manualmente**e em **OK**.

#### <a name="to-set-up-an-automatic-synchronization-schedule"></a>Para configurar uma agenda de sincronização automática

1.  No console de administração do WSUS, clique em **Opções**e em **agendamento de sincronização**.

2.  Clique em **sincronizar automaticamente**.

3.  Para a **primeira sincronização**, selecione a hora em que deseja que a sincronização seja iniciada a cada dia.

4.  para **sincronizações por dia**, selecione o número de sincronizações que você deseja fazer diariamente. Por exemplo, se você quiser quatro sincronizações um dia a partir de 3:00, as sincronizações ocorrerão às 3:00 A.M., 9:00 A.M., 3:00 P.M. e 9:00 P.M. todos os dias. (Observe que um deslocamento de tempo aleatório será adicionado ao tempo de sincronização agendado para espaçar as conexões do servidor para Microsoft Update.)

5.  Clique em **OK**.

#### <a name="to-synchronize-your-wsus-server-immediately"></a>Para sincronizar o servidor do WSUS imediatamente

1.  No console de administração do WSUS, selecione o nó de servidor superior.

2.  No painel **visão geral** , em **status da sincronização**, clique em **sincronizar agora**.

> [!NOTE]
> A sincronização é iniciada pelo servidor downstream.
