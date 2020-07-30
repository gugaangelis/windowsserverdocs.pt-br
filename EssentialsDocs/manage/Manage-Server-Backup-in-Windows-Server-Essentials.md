---
title: Gerenciar o backup do servidor no Windows Server Essentials
description: Descreve como usar o Windows Server Essentials
ms.date: 10/03/2016
ms.topic: article
ms.assetid: 0302d070-c58a-40f2-b56d-7e7842813d02
author: nnamuhcs
ms.author: coreyp
manager: dongill
ms.openlocfilehash: 611f3824493acd6047c55f83cf17ba81daf8c923
ms.sourcegitcommit: d99bc78524f1ca287b3e8fc06dba3c915a6e7a24
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/27/2020
ms.locfileid: "87180922"
---
# <a name="manage-server-backup-in-windows-server-essentials"></a>Gerenciar o backup do servidor no Windows Server Essentials

>Aplica-se a: Windows Server 2016 Essentials, Windows Server 2012 R2 Essentials, Windows Server 2012 Essentials

 Os tópicos a seguir incluem informações sobre tarefas comuns de backup que você pode realizar usando o Painel do Windows Server Essentials:

-   [Qual backup eu devo escolher?](Manage-Server-Backup-in-Windows-Server-Essentials.md#BKMK_WhichBackup)

-   [Configurar ou personalizar o backup do servidor](Manage-Server-Backup-in-Windows-Server-Essentials.md#BKMK_1)

-   [Interromper um backup de servidor em andamento](Manage-Server-Backup-in-Windows-Server-Essentials.md#BKMK_2)

-   [Gerenciar backups remotamente](Manage-Server-Backup-in-Windows-Server-Essentials.md#BKMK_3)

-   [Desabilitar o backup do servidor](Manage-Server-Backup-in-Windows-Server-Essentials.md#BKMK_4)

-   [Saiba mais sobre como configurar o backup do servidor](Manage-Server-Backup-in-Windows-Server-Essentials.md#BKMK_5)

-   [Reparticionar um disco rígido no servidor](Manage-Server-Backup-in-Windows-Server-Essentials.md#BKMK_6)

-   [Restaurar arquivos e pastas de um backup do servidor](Manage-Server-Backup-in-Windows-Server-Essentials.md#BKMK_7)

##  <a name="which-backup-should-i-choose"></a><a name="BKMK_WhichBackup"></a>Qual backup devo escolher?
 A escolha de um backup pode ser simples se você tiver realizado um backup bem-sucedido recente e souber que ele contém todos os seus dados essenciais. Se você estiver tentando restaurar o servidor ou um computador de um backup mais antigo, escolher um bom backup para restaurar pode exigir alguma pesquisa e, possivelmente, algum comprometimento.

#### <a name="to-choose-a-backup"></a>Para escolher um backup

1.  Consulte o proprietário dos arquivos ou pastas e anote as datas e horários em que eles foram adicionados ou editados. Use essas datas e horários como um ponto de partida.

2.  Na página **Escolha uma opção de restauração** do Assistente de Restauração de Arquivos ou Pastas, clique em **Restaurar de um backup selecionado (avançado)**.

3.  Se desejar restaurar uma versão mais antiga ou mais recente dos arquivos ou pastas, selecione o backup que melhor corresponder às datas e horários anotados na etapa 1.

4.  Como prática recomendada, você pode restaurar arquivos e pastas para um local alternativo e, em seguida, permitir que o proprietário dos arquivos e pastas mova aquelas de que ele precisa para o local original. Quando ele terminar, os arquivos e pastas que permanecerem no local alternativo poderão ser excluídas.

##  <a name="set-up-or-customize-server-backup"></a><a name="BKMK_1"></a>Configurar ou personalizar o backup do servidor
 O backup do servidor não é configurado automaticamente durante a instalação. Você deve proteger seu servidor e seus dados automaticamente com o agendamento de backups diários. É recomendável que você mantenha um plano de backup diário, porque a maioria das organizações não pode perder dados criados ao longo de vários dias. Para obter mais informações, consulte [Configurar ou personalizar o backup do servidor](Set-up-or-customize-server-backup.md).

##  <a name="stop-server-backup-in-progress"></a><a name="BKMK_2"></a>Parar backup do servidor em andamento
 Se um backup do servidor for iniciado em um horário agendado regularmente ou se você iniciar um backup do servidor manualmente, é possível interromper o backup em andamento.

#### <a name="to-stop-a-backup-in-progress"></a>Para interromper um backup em andamento

1.  Abra o Painel.

2.  Na barra de navegação, clique em **Dispositivos**.

3.  Na lista de computadores, clique no servidor e, em seguida, clique em **Parar backup para o servidor**, no painel **Tarefas**.

4.  Clique em **Sim** para confirmar a ação.

##  <a name="remotely-manage-your-backups"></a><a name="BKMK_3"></a>Gerenciar seus backups remotamente
 Quando estiver fora do escritório, você pode usar o Acesso via Web Remoto do Windows Server Essentials para acessar o painel do Windows Server Essentials e gerenciar o servidor.

#### <a name="to-use-remote-web-access-to-manage-your-server"></a>Para usar o Acesso via Web Remoto para gerenciar o servidor

1. Abra um navegador da Web.

2. Na caixa de endereços, digite o nome de domínio do Windows Server Essentials.

3. Quando solicitado, digite o nome de usuário e senha.

4. Quando você clica no nome do servidor no Acesso via Web remoto, a página de logon do painel é exibida.

5. Faça logon no Painel como administrador e, em seguida, clique em **Dispositivos**.

   Para obter mais informações sobre Acesso via Web remotos, consulte [visão geral de acesso via Web remoto](Manage-Remote-Web-Access-in-Windows-Server-Essentials.md#BKMK_Overview).

##  <a name="disable-server-backup"></a><a name="BKMK_4"></a>Desabilitar backup do servidor
 Você deve proteger seu servidor e seus dados automaticamente com o agendamento de backups diários. É recomendável que você mantenha um plano de backup diário, porque a maioria das organizações não pode perder dados criados ao longo de vários dias.

 Se já tiver configurado o servidor de backup e mais tarde desejar usar um aplicativo de terceiros para fazer backup do servidor, você pode desabilitar o backup do Windows Server Essentials.

#### <a name="to-disable-server-backup"></a>Para desabilitar o backup do servidor

1.  Faça logon no Painel do Windows Server Essentials usando uma conta de administrador e senha.

2.  Clique na guia **Dispositivos** e, em seguida, clique no nome do servidor.

3.  No painel de Tarefas, clique em **Personalizar Backup para o servidor**.

    > [!NOTE]
    >  A tarefa **Personalizar Backup para o servidor** é exibida depois que você tiver configurado o backup do servidor usando o Assistente de Configuração de Backup do Servidor. Para obter mais informações sobre como configurar o backup do servidor, consulte [Configurar ou personalizar o backup do servidor](Manage-Server-Backup-in-Windows-Server-Essentials.md#BKMK_1).

4.  O  Assistente de Personalização de Backup do Servidor é exibido.

5.  Na página **Opções de Configuração**, clique em **Desabilitar o Backup do Servidor**. Siga as instruções no assistente.

##  <a name="learn-more-about-setting-up-server-backup"></a><a name="BKMK_5"></a>Saiba mais sobre como configurar o backup do servidor
 O backup do servidor não é habilitado durante a instalação do servidor.

> [!NOTE]
>  Quando configurar o backup do servidor, você deverá conectar pelo menos um disco rígido externo ao servidor para usar como o disco rígido de destino do backup.

###  <a name="backup-destination-drive"></a><a name="BKMK_Target"></a>Unidade de destino de backup
 Você pode usar várias unidades de armazenamento externo para backups, e pode alternar as unidades entre locais de armazenamento externo e no local. Isso pode melhorar o planejamento de preparação para desastres, ajudando a recuperar seus dados caso haja danos físicos ao hardware no local.

 Ao escolher uma unidade de armazenamento para o backup do servidor, considere o seguinte:

-   Escolha uma unidade que contém espaço suficiente para armazenar os dados. As unidades de armazenamento devem conter pelo menos 2,5 vezes a capacidade de armazenamento de dados de que você deseja fazer backup. As unidades também devem ser grandes o suficiente para acomodar o crescimento futuro de seus dados de servidor.

-   Se a unidade de destino de backup contiver unidades offline, a configuração de backup não terá êxito. Para concluir a configuração, ao selecionar o destino do backup, desmarque a caixa de seleção para excluir as unidades que estiverem offline.

-   Se você escolher uma unidade que contém backups anteriores como o destino do backup, o assistente permitirá que você escolha se desejar manter os backups anteriores. Se você mantiver os backups, o assistente não formatará a unidade.

-   Ao reutilizar uma unidade de armazenamento externo, certifique-se de que a unidade está vazia ou contém apenas dados que não são necessários.

-   Visite o site do fabricante da unidade de armazenamento externo para garantir que sua unidade de backup tenha suporte em computadores que executam o Windows Server Essentials.

> [!CAUTION]
>  O Assistente de Configuração de Servidor de Backup formata as unidades de armazenamento quando ele os configura para backup.

### <a name="server-backup-schedule"></a>Agendamento de backup do servidor
 Você deve proteger seu servidor e seus dados automaticamente com o agendamento de backups diários. É recomendável que você mantenha um plano de backup diário, porque a maioria das organizações não pode perder dados criados ao longo de vários dias.

 Quando usa o Assistente de Configuração de Backup do Servidor do Windows Server Essentials, você pode optar por fazer backup de dados do servidor várias vezes durante o dia. Como o assistente agenda de backups baseados em diferencial, o backup é executado rapidamente e o desempenho do servidor não é afetado significativamente. Por padrão, **Configurar Backup do Servidor** agenda um backup para executar diariamente às 12:00 e às 23:00. No entanto, você pode ajustar o cronograma de backup de acordo com as necessidades da sua organização. Ocasionalmente, você deve avaliar a eficácia do seu plano de backup e alterar o plano conforme necessário.

> [!NOTE]
>  Na instalação padrão do Windows Server Essentials, o servidor está configurado para executar automaticamente uma desfragmentação uma vez por semana. Isso pode resultar em backups maiores que o normal se você usar um software de imagem que não for da Microsoft. Se não for necessário desfragmentar o servidor regularmente, você pode seguir estas etapas para desativar o agendamento de desfragmentação:
>
> 1. Pressione a tecla Windows + W para abrir a **Pesquisa**.
>    2. Na caixa de texto Pesquisar, digite **Defragment**.
>    3. Na seção de resultados, clique em **Desfragmentar e Otimizar Unidades**.
>    4. Na página **Otimizar Unidades**, selecione uma unidade e, em seguida, clique em **Alterar configurações**.
>    5. Na janela **Agendamento de otimização**, desmarque a caixa de seleção **Executar seguindo um agendamento (recomendado)** e clique em **OK** para salvar a alteração.

### <a name="items-to-be-backed-up"></a>Itens para backup
 Por padrão, todos os arquivos do sistema operacional e pastas estão selecionados para backup. Você pode optar por fazer backup de todos os discos rígidos, arquivos e pastas no servidor ou selecionar apenas discos rígidos, arquivos ou pastas individuais para backup. Para adicionar ou remover itens para backup, siga um destes procedimentos:

-   Para incluir uma unidade de dados no backup do servidor, marque a caixa de seleção adjacente.

-   Para excluir uma unidade de dados do backup do servidor, desmarque a caixa de seleção adjacente.

> [!NOTE]
>  Se deseja excluir o item **Sistema Operacional** do backup, você deve primeiro desmarcar a caixa de seleção **Backup de sistema (recomendado)**.

 Para minimizar a quantidade de espaço em disco de backup que os backups do servidor usam, convém excluir todas as pastas que contêm arquivos que não sejam considerados valiosos ou particularmente importantes.

 Por exemplo, você pode ter uma pasta que contém programas de TV gravados que usam muito espaço em disco rígido. Você pode optar por não fazer backup desses arquivos porque normalmente eles são excluídos após serem assistidos. Como alternativa, você pode ter uma pasta com os arquivos temporários que não deseja manter.

##  <a name="repartition-a-hard-drive-on-the-server"></a><a name="BKMK_6"></a>Reparticionar uma unidade de disco rígido no servidor
 Quando uma unidade de disco rígido interna não formatada é detectada no servidor do Windows Server Essentials, é gerado um alerta de integridade que contém um link para adicionar um novo Assistente de unidade de disco rígido. O Assistente de Adição de Novo Disco Rígido descreve as várias opções para formatar o disco rígido. Quando o assistente for concluído, um ou mais discos rígidos lógicos formatados, dependendo do tamanho da unidade de disco, serão criados no disco rígido e formatados como NTFS.

 Se for necessário reparticionar uma unidade de disco rígido, siga estas instruções:

#### <a name="to-repartition-a-hard-disk-drive"></a>Para reparticionar uma unidade de disco rígido

1.  Na tela **Iniciar**, clique em **Ferramentas Administrativas** e clique duas vezes em **Gerenciamento do Computador**.

2.  Em Gerenciamento do Computador, clique em **Armazenamento** e clique duas vezes em **Gerenciamento de Disco**.

3.  Clique com o botão direito do mouse na unidade que você deseja reparticionar, clique em **Excluir Volume** e clique em **Sim**.

    > [!NOTE]
    >  Repita essa etapa para cada partição da unidade de disco rígido.

4.  Clique com o botão direito do mouse na unidade de disco rígido **Não alocado** e clique em **Novo Volume Simples**.

5.  No Assistente para Novas Partições Simples, crie e formate um volume que seja de 16 TB (16.000.000 MB) ou menos.

    > [!NOTE]
    >  Repita essa etapa até que todo o espaço não alocado na unidade de disco rígido seja usado.

##  <a name="restore-files-and-folders-from-a-server-backup"></a><a name="BKMK_7"></a>Restaurar arquivos e pastas de um backup do servidor
 Você pode procurar e restaurar arquivos e pastas individuais de um backup do servidor.

#### <a name="to-restore-files-and-folders-from-a-server-backup"></a>Para restaurar arquivos e pastas de um backup do servidor

1.  Abra o Painel e clique na guia **Dispositivos**.

2.  Clique no nome do servidor e, em seguida, clique em **Restaurar arquivos ou pastas do servidor** no painel **Tarefas**.

3.  O Assistente de Restauração de Arquivos ou Pastas é aberto. Siga as instruções no assistente para restaurar os arquivos ou pastas.

## <a name="additional-references"></a>Referências adicionais

-   [Gerenciar backup e restauração](Manage-Backup-and-Restore-in-Windows-Server-Essentials.md)

-   [Gerenciar o Windows Server Essentials](Manage-Windows-Server-Essentials.md)

-   [Utilizar o Windows Server Essentials](../use/Use-Windows-Server-Essentials.md)
