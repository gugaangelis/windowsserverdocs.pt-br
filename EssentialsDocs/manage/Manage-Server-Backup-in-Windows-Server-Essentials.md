---
title: Gerenciar o Backup do servidor no Windows Server Essentials
description: Descreve como usar o Windows Server Essentials
ms.custom: na
ms.date: 10/03/2016
ms.prod: windows-server-2016-essentials
ms.reviewer: na
ms.suite: na
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 0302d070-c58a-40f2-b56d-7e7842813d02
author: nnamuhcs
ms.author: coreyp
manager: dongill
ms.openlocfilehash: 2b0cd926b15d65e5cd4c784681c40df29b18a48f
ms.sourcegitcommit: 70c1b6cedad55b9c7d2068c9aa4891c6c533ee4c
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/03/2017
---
# <a name="manage-server-backup-in-windows-server-essentials"></a>Gerenciar o Backup do servidor no Windows Server Essentials

>Aplica-se a: Windows Server 2016 Essentials, Windows Server 2012 R2 Essentials, Windows Server 2012 Essentials
  
 Os tópicos a seguir incluem informações sobre tarefas comuns de backup que você pode realizar usando o painel do Windows Server Essentials:  
  
-   [Backup do qual devo escolher?](Manage-Server-Backup-in-Windows-Server-Essentials.md#BKMK_WhichBackup)  
  
-   [Configurar ou personalizar o backup do servidor](Manage-Server-Backup-in-Windows-Server-Essentials.md#BKMK_1)  
  
-   [Parar de backup do servidor em andamento](Manage-Server-Backup-in-Windows-Server-Essentials.md#BKMK_2)  
  
-   [Gerenciar remotamente seus backups](Manage-Server-Backup-in-Windows-Server-Essentials.md#BKMK_3)  
  
-   [Desativar o backup do servidor](Manage-Server-Backup-in-Windows-Server-Essentials.md#BKMK_4)  
  
-   [Saiba mais sobre como configurar o backup do servidor](Manage-Server-Backup-in-Windows-Server-Essentials.md#BKMK_5)  
  
-   [Reparticionar um disco rígido no servidor](Manage-Server-Backup-in-Windows-Server-Essentials.md#BKMK_6)  
  
-   [Restaurar arquivos e pastas de um backup do servidor](Manage-Server-Backup-in-Windows-Server-Essentials.md#BKMK_7)  
  
##  <a name="BKMK_WhichBackup"></a>Backup do qual devo escolher?  
 Escolher um backup pode ser simples se você tiver um backup bem-sucedido muito recente e você sabe que o backup contém todos os seus dados críticos. Se você estiver tentando restaurar para o servidor ou em um computador a partir de um backup mais antigo, escolher um backup bom restaurar pode exigir alguns investigação e, possivelmente, alguns comprometer.  
  
#### <a name="to-choose-a-backup"></a>Para escolher um backup  
  
1.  Verifique com o proprietário dos arquivos ou pastas e observe as datas e horas quando eles adicionado ou editado-los. Use essas datas e horas como ponto de partida.  
  
2.  Sobre o **escolha uma opção de restauração** página em Restaurar arquivos e pastas assistente, clique em **restaurar a partir de um backup que eu selecionar (Avançado)**.  
  
3.  Dependendo se você deseja restaurar uma versão mais antiga ou mais recente dos arquivos ou pastas, selecione o backup que melhor se adapta as datas e horas anotadas na etapa 1.  
  
4.  Como prática recomendada, você pode restaurar arquivos e pastas em um local alternativo e, em seguida, deixar que o proprietário dos arquivos e pastas mover aquelas que precisam para o local original. Quando eles terminarem, os arquivos e pastas que permanecem no local alternativo podem ser excluídas.  
  
##  <a name="BKMK_1"></a>Configurar ou personalizar o backup do servidor  
 Backup do servidor não está configurado automaticamente durante a instalação. Você deve proteger seu servidor e seus dados automaticamente agendando backups diários. É recomendável que você mantenha um plano de backup diário porque a maioria das organizações não pode perder os dados que foi criados por vários dias. Para obter mais informações, consulte [definido para cima e para personalizar o backup do servidor](Set-up-or-customize-server-backup.md).  
  
##  <a name="BKMK_2"></a>Parar de backup do servidor em andamento  
 Se um backup do servidor é iniciado em um horário agendado regularmente ou iniciar um backup do servidor manualmente, você pode parar o backup em andamento.  
  
#### <a name="to-stop-a-backup-in-progress"></a>Para parar de um backup em andamento  
  
1.  Abra o painel.  
  
2.  Na barra de navegação, clique em **dispositivos**.  
  
3.  Na lista de computadores, clique no servidor e, em seguida, clique em **parar backup do servidor** no **tarefas** painel.  
  
4.  Clique em **Sim** para confirmar a ação.  
  
##  <a name="BKMK_3"></a>Gerenciar remotamente seus backups  
 Quando você estiver longe do escritório, você pode usar o acesso de via Web remoto do Windows Server Essentials para acessar o painel do Windows Server Essentials para gerenciar seu servidor.  
  
#### <a name="to-use-remote-web-access-to-manage-your-server"></a>Usar o acesso via Web remoto para gerenciar seu servidor  
  
1.  Abra um navegador da web.  
  
2.  Na caixa Endereço, digite o nome do domínio do Windows Server Essentials.  
  
3.  Quando solicitado, digite seu nome de usuário e senha.  
  
4.  Quando você clica no nome do servidor no acesso via Web remoto, a página de logon para o painel é exibida.  
  
5.  Faça logon no painel como administrador e clique em **dispositivos**.  
  
 Para obter mais informações sobre o acesso via Web remoto, consulte [visão geral do acesso via Web remoto](Manage-Remote-Web-Access-in-Windows-Server-Essentials.md#BKMK_Overview).  
  
##  <a name="BKMK_4"></a>Desativar o backup do servidor  
 Você deve proteger seu servidor e seus dados automaticamente agendando backups diários. É recomendável que você mantenha um plano de backup diário porque a maioria das organizações não pode perder os dados que foi criados por vários dias.  
  
 Se você já tem o backup do servidor configurado e mais tarde quiser usar um aplicativo de terceiros para fazer backup do servidor, você pode desativar o backup do servidor Windows Server Essentials.  
  
#### <a name="to-disable-server-backup"></a>Para desativar o backup do servidor  
  
1.  Faça logon no Windows Server Essentials painel usando uma conta de administrador e uma senha.  
  
2.  Clique no **dispositivos** guia e, em seguida, clique no nome do servidor.  
  
3.  No painel de tarefas, clique em **personalizar o Backup do servidor**.  
  
    > [!NOTE]
    >  O **personalizar o Backup do servidor** tarefa é exibida depois que você configurou o backup do servidor usando o Assistente de configuração de servidor de Backup. Para obter mais informações sobre como configurar o backup do servidor, veja [definido para cima e para personalizar o backup do servidor](Manage-Server-Backup-in-Windows-Server-Essentials.md#BKMK_1).  
  
4.  O Assistente para personalizar o Backup do servidor é exibida.  
  
5.  Sobre o **opções de configuração** página, clique em **desabilitar o Backup do servidor**. Siga as instruções no assistente.  
  
##  <a name="BKMK_5"></a>Saiba mais sobre como configurar o backup do servidor  
 Backup do servidor não está habilitado durante a instalação do servidor.  
  
> [!NOTE]
>  Quando você configura um backup do servidor, você deve se conectar pelo menos um disco rígido externo para o servidor para usar como o disco rígido de destino do backup.  
  
###  <a name="BKMK_Target"></a>Unidade de destino do backup  
 Você pode usar várias unidades de armazenamento externos para backups, e você pode girar as unidades entre os locais de armazenamento externa e no local. Isso pode melhorar seu planejamento ajudando você a recuperar seus dados caso ocorra dano físico para o local de hardware de preparação para desastres.  
  
 Ao escolher uma unidade de armazenamento para o backup do servidor, considere o seguinte:  
  
-   Escolha uma unidade que contém o espaço suficiente para armazenar seus dados. Suas unidades de armazenamento devem conter no mínimo 2,5 vezes a capacidade de armazenamento dos dados que você quer fazer backup. As unidades também devem ser grandes o suficiente para acomodar o crescimento futuro de seus dados de servidor.  
  
-   Se a unidade de destino do backup contiver unidades offline, a configuração de backup não serão bem-sucedidas. Para concluir a configuração, ao selecionar o destino do backup, desmarque a caixa de seleção para excluir as unidades que estão offline.  
  
-   Se você escolher uma unidade que contém os backups anteriores como o destino do backup, o assistente permite que você escolha se você deseja manter os backups anteriores. Se você mantiver os backups, o assistente não formate a unidade.  
  
-   Quando reutilizar uma unidade de armazenamento externo, certifique-se de que a unidade está vazia ou contém apenas os dados que você não precisa.  
  
-   Você deve visitar o site do fabricante do seu armazenamento externo disco para garantir que a unidade de backup tem suporte em computadores que executam o Windows Server Essentials.  
  
> [!CAUTION]
>  Assistente de configuração de servidor Backup formata as unidades de armazenamento quando configura-los para backup.  
  
### <a name="server-backup-schedule"></a>Agendamento de backup do servidor  
 Você deve proteger seu servidor e seus dados automaticamente agendando backups diários. É recomendável que você mantenha um plano de backup diário porque a maioria das organizações não pode perder os dados que foi criados por vários dias.  
  
 Quando você usa o Windows Server Essentials Server Backup do Assistente de configuração, você pode optar por fazer backup dos dados do servidor em várias vezes durante o dia. Como o Assistente de agenda backups baseados em diferencial, Backup executa rapidamente e desempenho do servidor não é afetado significativamente. Por padrão, **definir o servidor de Backup** agenda um backup para execução diariamente em 12:00 PM e 11:00 PM. No entanto, você pode ajustar o agendamento de backup acordo com as necessidades da sua organização. Ocasionalmente, você deve avalia a eficiência do seu plano de backup e altere o plano conforme necessário.  
  
> [!NOTE]
>  Na instalação padrão do Windows Server Essentials, o servidor está configurado para executar automaticamente uma desfragmentação uma vez por semana. Isso pode resultar em maior do que backups normais se você usar o software de geração de imagens não são da Microsoft. Se não for necessária desfragmentar o servidor regularmente, você pode seguir estas etapas para desativar o agendamento de desfragmentação:  
>   
>  1.  Pressione a tecla Windows + W para abrir **pesquisa**.  
> 2.  Na caixa de texto de pesquisa, digite **desfragmentar**.  
> 3.  Na seção de resultados, clique em **desfragmentar e otimizar unidades**.  
> 4.  No **otimizar unidades** página, selecione uma unidade e clique em **alterar configurações**.  
> 5.  No **agendamento de otimização** janela, limpar o **executados em um agendamento (recomendado)** caixa de seleção e, em seguida, clique em **Okey** para salvar a alteração.  
  
### <a name="items-to-be-backed-up"></a>Itens de backup  
 Por padrão, todos os arquivos do sistema operacional e pastas estão selecionadas para backup. Você pode optar por fazer backup de todos os discos rígidos, arquivos e pastas no servidor, ou selecione apenas discos rígidos individuais, arquivos ou pastas para fazer backup. Para adicionar ou remover itens para o backup, siga um destes procedimentos:  
  
-   Para incluir uma unidade de dados no backup do servidor, marque a caixa de seleção adjacente.  
  
-   Para excluir uma unidade de dados usando o backup do servidor, desmarque a caixa de seleção adjacente.  
  
> [!NOTE]
>  Se você deseja excluir o **sistema operacional** item usando o backup, primeiro você deve limpar os **Backup de sistema (recomendado)** caixa de seleção.  
  
 Para minimizar a quantidade de espaço de disco rígido backup que usam seus backups de servidor, você pode querer excluir todas as pastas que contêm arquivos que você não considerar importantes ou particularmente importante.  
  
 Por exemplo, você pode ter uma pasta que contém os programas de TV gravados que usa muito espaço em disco rígido. Você pode optar por não fazer backup desses arquivos porque você excluí-los normalmente após exibi-los de qualquer maneira. Como alternativa, você pode ter uma pasta que contém os arquivos temporários que não deseja manter.  
  
##  <a name="BKMK_6"></a>Reparticionar um disco rígido no servidor  
 Quando uma unidade de disco rígido interna não formatada é detectada no servidor Windows Server Essentials, é gerado um alerta de saúde que contém um link para adicionar um novo Assistente de unidade de disco rígido. Adicionar um novo Assistente de unidade de disco rígido percorre as várias opções para formatar o disco rígido. Quando o assistente for concluído, uma ou mais, dependendo do tamanho da unidade, lógicos discos rígidos formatados serão criados no disco rígido e formatados como NTFS.  
  
 Se for necessário reparticionar a unidade de disco rígido, siga estas instruções:  
  
#### <a name="to-repartition-a-hard-disk-drive"></a>Para reparticionar a unidade de disco rígido  
  
1.  Sobre o **iniciar** de tela, clique em **ferramentas administrativas**e clique duas vezes em **gerenciamento do computador**.  
  
2.  No gerenciamento do computador, clique em **armazenamento**e clique duas vezes em **gerenciamento de disco**.  
  
3.  Clique com botão direito na unidade que você deseja criar uma nova partição, clique em **Excluir Volume**e clique em **Sim**.  
  
    > [!NOTE]
    >  Repita essa etapa para cada partição no disco rígido.  
  
4.  Clique com botão direito do **Unallocated** unidade de disco rígido e clique em **Novo Volume simples**.  
  
5.  No novo Assistente de Volume simples, criar e formatar um volume é de 16 TB (16,000,000 MB) ou menos.  
  
    > [!NOTE]
    >  Repita essa etapa até que todo o espaço não alocado na unidade de disco rígido é usado.  
  
##  <a name="BKMK_7"></a>Restaurar arquivos e pastas de um backup do servidor  
 Você pode procurar e restaurar arquivos e pastas individuais de um backup do servidor.  
  
#### <a name="to-restore-files-and-folders-from-a-server-backup"></a>Para restaurar arquivos e pastas de um backup do servidor  
  
1.  Abra o painel e clique no **dispositivos** guia.  
  
2.  Clique no nome do servidor e, em seguida, clique em **restaurar arquivos ou pastas para o servidor** no **tarefas** painel.  
  
3.  O Assistente de pastas ou restaurar arquivos é aberta. Siga as instruções no Assistente para restaurar os arquivos ou pastas.  
  
## <a name="see-also"></a>Consulte também  
  
-   [Gerenciar o Backup e restauração](Manage-Backup-and-Restore-in-Windows-Server-Essentials.md)  
  
-   [Gerenciar o Windows Server Essentials](Manage-Windows-Server-Essentials.md)  
  
-   [Usar o Windows Server Essentials](../use/Use-Windows-Server-Essentials.md)
