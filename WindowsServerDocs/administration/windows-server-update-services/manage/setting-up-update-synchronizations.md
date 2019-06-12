---
title: Configurar sincronizações de atualização
description: Tópico do Windows Server Update Service (WSUS) - como instalar e configurar sincronizações de atualização
ms.custom: na
ms.prod: windows-server-threshold
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
ms.openlocfilehash: 5fdfaaf1af2b74fe15530095700005a422b64986
ms.sourcegitcommit: a3958dba4c2318eaf2e89c7532e36c78b1a76644
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/05/2019
ms.locfileid: "66719629"
---
# <a name="setting-up-update-synchronizations"></a>Configurar sincronizações de atualização

>Aplica-se a: Windows Server (canal semestral), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Durante a sincronização, um servidor WSUS baixa atualizações (atualização de metadados e arquivos) de uma fonte de atualização. Ele também baixa novas classificações de produto e categorias, se houver. Quando o servidor do WSUS sincroniza pela primeira vez, ele baixará todas as atualizações que você especificou ao configurar as opções de sincronização. Após a primeira sincronização, seu servidor do WSUS baixa somente as atualizações da origem da atualização, bem como as revisões em metadados de atualizações existentes e expirações de atualizações.

Na primeira vez que um servidor WSUS baixa atualizações pode levar muito tempo. Se você estiver configurando vários servidores do WSUS, você pode acelerar o processo até certo ponto baixar todas as atualizações em um servidor WSUS e, em seguida, copiando as atualizações para os diretórios de conteúdo de outros servidores do WSUS.

Você pode copiar o conteúdo do diretório de conteúdo de um servidor do WSUS para outro. O local do diretório de conteúdo é especificado quando você executar o procedimento de instalação do WSUS post. Você pode usar a ferramenta wsusutil.exe para exportar metadados de atualização de um servidor WSUS para um arquivo. Você pode importá-lo em outros servidores do WSUS.

## <a name="setting-up-update-synchronizations"></a>Configurar sincronizações de atualização
O **opções** página é o ponto de acesso central no Console de administração do WSUS para personalizar como o servidor do WSUS sincroniza as atualizações. Você pode especificar quais atualizações são sincronizadas automaticamente, em que o servidor obtém atualizações, configurações de conexão e a agenda de sincronização. Você também pode usar o Assistente de configuração do **opções** página para configurar ou reconfigurar o servidor do WSUS a qualquer momento.

### <a name="synchronizing-update-by-product-and-classification"></a>Sincronização de atualização por produto e classificação
Um servidor WSUS baixa atualizações com base em produtos ou famílias de produtos (por exemplo, Windows ou Windows Server 2008, Datacenter edition) e classificações (por exemplo, atualizações críticas ou atualizações de segurança) que você especificar. na primeira sincronização, o servidor do WSUS baixa todas as atualizações disponíveis nas categorias que você especificou. Em sincronizações subsequentes, seus downloads somente as atualizações mais recentes do servidor WSUS (ou alterações em atualizações já disponíveis no servidor do WSUS) para as categorias de você ter especificado.

Você pode especificar os produtos de atualização e classificações na **opções** página sob **produtos e classificações**. Produtos são listados em uma hierarquia, agrupada por família de produtos. Se você selecionar o Windows, você seleciona automaticamente todos os produtos que se enquadra nessa hierarquia de produto. Marcando a caixa de seleção pai é selecionar todos os itens abaixo dele, bem como todas as versões futuras. marcando as caixas de seleção filho não selecionará as caixas de seleção pai. A configuração padrão para os produtos é todos os produtos Windows e a configuração padrão para classificações é crítica e atualizações de segurança.

Se um servidor WSUS estiver em execução no modo de réplica, você não poderá executar essa tarefa. Para obter mais informações sobre o modo de réplica, consulte [modo de réplica do WSUS em execução](running-wsus-replica-mode.md), e [etapa 1: Preparar para implantação do WSUS](../plan/plan-your-wsus-deployment.md).

##### <a name="to-specify-update-products-and-classifications-for-synchronization"></a>Para especificar os produtos de atualização e classificações para sincronização

1.  No Console de administração do WSUS, clique o **opções** nó.

2.  Clique em **produtos e classificações**e, em seguida, clique no **produtos** guia.

3.  Marque as caixas de seleção dos produtos e famílias de produtos que você deseja atualizar com o WSUS e, em seguida, clique em **Okey**.

4.  Sobre o **classificações** guia, marque as caixas de seleção das classificações de atualização que você deseja que o servidor do WSUS para sincronizar e, em seguida, clique em **Okey**.

> [!NOTE]
> Você pode remover os produtos ou classificações da mesma maneira. O servidor do WSUS interromperá a sincronização de novas atualizações para os produtos que você tenha desmarcado. No entanto, as atualizações que foram sincronizadas para esses produtos antes de você desmarcou-los permanecerão no seu servidor do WSUS e serão listadas como disponíveis.
> 
> Para remover esses produtos, recusar a atualização, conforme documentado em [operações de atualizações](updates-operations.md)e, em seguida, usar o [Assistente de limpeza de servidor a](the-server-cleanup-wizard.md) para removê-los.

### <a name="synchronizing-updates-by-language"></a>Sincronização de atualizações por linguagem
O servidor WSUS baixa atualizações com base nas linguagens que você especificar. Você pode sincronizar as atualizações em todos os idiomas nos quais eles estão disponíveis, ou você pode especificar um subconjunto de idiomas. Se você tiver uma hierarquia de servidores do WSUS, e você precisa baixar atualizações em idiomas diferentes, certifique-se de que você tenha especificado todos os idiomas necessários no servidor upstream. Em um servidor downstream, você pode especificar um subconjunto de idiomas especificados no servidor upstream.

### <a name="synchronizing-updates-from-the-microsoft-update-catalog"></a>Sincronização de atualizações do catálogo do Microsoft Update
Para obter detalhes sobre a sincronização de atualizações do site do catálogo do Microsoft Update, consulte: [WSUS e o Site do catálogo](wsus-and-the-catalog-site.md).

## <a name="configuring-proxy-server-settings"></a>Definir configurações de servidor Proxy
Você pode configurar seu servidor do WSUS para usar um servidor proxy durante a sincronização com um servidor upstream ou o Microsoft Update. Essa configuração será aplicada apenas quando o servidor do WSUS executar sincronizações. Por padrão o servidor do WSUS tentará se conectar diretamente ao servidor upstream ou Microsoft Update.

#### <a name="to-specify-a-proxy-server-for-synchronization"></a>Para especificar um servidor proxy para sincronização

1.  No Console de administração do WSUS, clique em **opções**e, em seguida, clique em **origem da atualização e servidor Proxy**.

2.  Sobre o **servidor Proxy** guia, selecione o **usar um servidor proxy ao sincronizar** caixa de seleção e, em seguida, digite o nome do servidor e número da porta do servidor proxy.

    > [!NOTE]
    > Configurar o WSUS com o mesmo número de porta que o servidor proxy está configurado para usar.

    -   Se você deseja se conectar ao servidor proxy com as credenciais de usuário específico, selecione o **usar as credenciais do usuário para se conectar ao servidor proxy** caixa de seleção e, em seguida, insira o nome de usuário, domínio e senha do usuário nas caixas correspondentes .

    -   Se você quiser habilitar a autenticação básica para o usuário se conectar ao servidor proxy, selecione a **permitir autenticação básica (senha enviada em texto não criptografado)** caixa de seleção.

3.  Clique em **OK**.

    > [!NOTE]
    > Como o WSUS inicia todo seu tráfego de rede, não é necessário configurar o Firewall do Windows em um servidor do WSUS conectado diretamente ao Microsoft update.

## <a name="configuring-the-update-source"></a>Configurando a origem de atualização
A origem de atualização é o local do qual o servidor WSUS obtém suas atualizações e atualizar os metadados. Você pode especificar que a origem de atualização deve ser Microsoft Update ou outro servidor do WSUS (servidor do WSUS que atua como a origem de atualização é o servidor upstream e seu servidor é o servidor downstream).

Opções para personalizar como o seu servidor do WSUS sincroniza com a origem de atualização incluem o seguinte:

-   Você pode especificar uma porta personalizada para sincronização. Para obter informações sobre como configurar portas, consulte [etapa 3: Configurar o WSUS](../deploy/2-configure-wsus.md) no guia de implantação do WSUS.

-   Você pode usar camadas de soquete seguro (SSL) para a sincronização segura de informações de atualização entre os servidores do WSUS. Para obter mais informações sobre como usar SSL, consulte a seção "3.5. Proteger o WSUS com o protocolo SSL"de [etapa 3: Configurar o WSUS](../deploy/2-configure-wsus.md) no guia de implantação do WSUS.

## <a name="synchronizing-manually-or-automatically"></a>Sincronizar manualmente ou automaticamente
Você pode sincronizar seu servidor do WSUS manualmente ou especificar uma hora para que ele sincronize automaticamente.

#### <a name="to-manually-synchronize-the-wsus-server"></a>Para sincronizar manualmente o servidor do WSUS

1.  No Console de administração do WSUS, clique em **opções**e, em seguida, clique em **agenda de sincronização**.

2.  Clique em **sincronizar manualmente**e, em seguida, clique em **Okey**.

#### <a name="to-set-up-an-automatic-synchronization-schedule"></a>Para configurar uma agenda de sincronização automática

1.  No Console de administração do WSUS, clique em **opções**, em seguida, clique em **agenda de sincronização**.

2.  Clique em **sincronizar automaticamente**.

3.  Para **primeira sincronização**, selecione a hora em que você deseja que cada dia de início da sincronização.

4.  para **sincronizações por dia**, selecione o número de sincronizações que você deseja fazer a cada dia. Por exemplo, se você quiser quatro sincronizações de partida do dia às 3 da manhã, em seguida, as sincronizações ocorrerão em 3h00, 9h00, 3: 12h e às 21h cada dia. (Observe que um deslocamento de tempo aleatório será adicionado ao tempo de sincronização agendada para espaçá as conexões de servidor para o Microsoft Update.)

5.  Clique em **OK**.

#### <a name="to-synchronize-your-wsus-server-immediately"></a>Para sincronizar imediatamente o seu servidor do WSUS

1.  No Console de administração do WSUS, selecione o nó de servidor superior.

2.  No **visão geral** painel, em **Status da sincronização**, clique em **sincronizar agora**.

> [!NOTE]
> A sincronização é iniciada pelo servidor downstream.
