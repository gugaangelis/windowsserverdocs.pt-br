---
title: Gerenciar o Backup do computador cliente no Windows Server Essentials
description: Descreve como usar o Windows Server Essentials
ms.custom: na
ms.date: 10/03/2016
ms.prod: windows-server-2016-essentials
ms.reviewer: na
ms.suite: na
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 1b4776e8-9504-4b98-ae80-11da797d9819
author: nnamuhcs
ms.author: coreyp
manager: dongill
ms.openlocfilehash: d184c97e47f04b9d7434aaeb0d328761bcfac1c0
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 12/12/2017
---
# <a name="manage-client-computer-backup-in-windows-server-essentials"></a>Gerenciar o Backup do computador cliente no Windows Server Essentials

>Aplica-se a: Windows Server 2016 Essentials, Windows Server 2012 R2 Essentials, Windows Server 2012 Essentials
 
 Este tópico inclui informações sobre tarefas comuns de backup para computadores cliente que você pode fazer usando o painel do Windows Server Essentials e inclui as seguintes seções:  
  
-   [Como funciona o reparo do Assistente de banco de dados de Backup](Manage-Client-Computer-Backup-in-Windows-Server-Essentials.md#BKMK_1)  
  
-   [Entender as configurações de backup do computador](Manage-Client-Computer-Backup-in-Windows-Server-Essentials.md#BKMK_2)  
  
-   [Configurar o backup para um computador cliente](Manage-Client-Computer-Backup-in-Windows-Server-Essentials.md#BKMK_3)  
  
-   [Alterar a hora em que o backup está agendado para ser executado](Manage-Client-Computer-Backup-in-Windows-Server-Essentials.md#BKMK_4)  
  
-   [Alterar a política de retenção de backup do computador](Manage-Client-Computer-Backup-in-Windows-Server-Essentials.md#BKMK_5)  
  
-   [Restaurar backup para as configurações padrão](Manage-Client-Computer-Backup-in-Windows-Server-Essentials.md#BKMK_6)  
  
-   [Use as ferramentas de reparo e recuperação](Manage-Client-Computer-Backup-in-Windows-Server-Essentials.md#BKMK_7)  
  
-   [Reparar o banco de dados de backup](Manage-Client-Computer-Backup-in-Windows-Server-Essentials.md#BKMK_8)  
  
-   [Desativar o backup de um computador](Manage-Client-Computer-Backup-in-Windows-Server-Essentials.md#BKMK_9)  
  
-   [Executar a tarefa de limpeza backup](Manage-Client-Computer-Backup-in-Windows-Server-Essentials.md#BKMK_10)  
  
-   [Exibir alertas na barra de tarefas em um computador cliente](Manage-Client-Computer-Backup-in-Windows-Server-Essentials.md#BKMK_11)  
  
-   [Exibir status de backup da barra inicial](Manage-Client-Computer-Backup-in-Windows-Server-Essentials.md#BKMK_12)  
  
-   [Parar um backup em andamento da barra inicial](Manage-Client-Computer-Backup-in-Windows-Server-Essentials.md#BKMK_13)  
  
-   [Inicie um backup da barra inicial](Manage-Client-Computer-Backup-in-Windows-Server-Essentials.md#BKMK_14)  
  
-   [Como funciona o backup do computador](Manage-Client-Computer-Backup-in-Windows-Server-Essentials.md#BKMK_15)  
  
-   [Dicas para ajudar a evitar a perda de dados devido a corrupção de banco de dados de backup do cliente](Manage-Client-Computer-Backup-in-Windows-Server-Essentials.md#BKMK_16)  
  
-   [Restaurar arquivos ou pastas de um backup do computador cliente](Manage-Client-Computer-Backup-in-Windows-Server-Essentials.md#BKMK_17)  
  
-   [Noções básicas sobre o histórico de arquivos](Manage-Client-Computer-Backup-in-Windows-Server-Essentials.md#BKMK_FileHistory)  
  
##  <a name="BKMK_1"></a>Como funciona o reparo do Assistente de banco de dados de Backup  
 Se o Windows Server Essentials detectar erros em seu banco de dados de backup, ele envia uma notificação de integridade e o status de alerta muda para vermelho, que indica uma condição de crítica.  
  
 No painel Windows Server Essentials, clique no ícone de status de alerta para ver a notificação de erro de backup do banco de dados. A notificação inclui um **reparo** botão, que inicia o reparo do Assistente de banco de dados de Backup. O assistente pode levar várias horas para terminar.  
  
 O assistente examina o banco de dados de backup do computador selecionado e tenta reparar o banco de dados se forem detectados erros. Em alguns casos, o assistente não pode corrigir o banco de dados de backup inteiro. Antes de iniciar o assistente, verificar se todos os discos rígidos externos que você usa em seu servidor estão conectados ao servidor, ligado e funcionando corretamente. O erro de banco de dados de backup pode ser corrigido automaticamente ao reconectar um disco rígido ausente.  
  
> [!CAUTION]
>  Você deve fazer backup do banco de dados antes de tentar corrigi-lo. Dependendo do tipo de erros encontrados no banco de dados de backup, o assistente não poderá recuperar alguns backups. Alguns ou todos os backups podem ser perdidos permanentemente.  
  
##  <a name="BKMK_2"></a>Entender as configurações de backup do computador  
 Depois que o backup está configurado para computadores cliente, você pode especificar uma janela diferente de tempo para o backup ocorra. Da mesma forma, você pode especificar um tempo de retenção de backup maior ou menor que o padrão.  
  
 O **restaurar padrões** opção permite que você redefina a política de janela e a retenção de backup para o padrão fornecido durante a configuração inicial de backup.  
  
 Os padrões são:  
  
|Configuração de backup|Padrão|Descrição|  
|--------------------|-------------|-----------------|  
|Hora de início|6:00 PM|Especifica o tempo que o backup diário é iniciado. É recomendável que você definir isso como uma vez quando os usuários não estiver usando seus computadores.|  
|Hora de término|9H ÀS|Especifica o tempo que o backup deve ser concluído. Se o backup não exige a duração especificada, ele usa apenas o tempo necessário para fazer o backup do computador com êxito.|  
|Reter backups diários|5 dias|Especifica o número de dias backups diários são mantidos. Por exemplo, 5 backups diários usando a configuração padrão, são mantidos. O 6ª dia e cada daí em diante, o arquivo de backup diário mais antigo é excluído.|  
|Mantém backups semanais|4 semanas|Especifica o número de semanas que o último backup da semana é retido. Por exemplo, 4 backups semanais usando as configurações padrão, são mantidos. Na semana 5ª e cada semana daí em diante, o backup semanal mais antigo é excluído.|  
|Mantém backups mensais|6 meses|Especifica o número de meses que o último backup do mês é retido. Por exemplo, usando a configuração padrão, seis meses de backups são mantidas. Sobre o 7 e cada mês daí em diante, o backup mensal mais antigo é excluído.|  
  
##  <a name="BKMK_3"></a>Configurar o backup para um computador cliente  
 Se o backup está desativado, você pode definir o backup do computador no painel. Quando você define o backup para um computador, você pode optar por fazer backup de tudo no computador ou selecionar os volumes e pastas que você quer fazer backup.  
  
> [!NOTE]
>  O computador deve estar online para definir o backup.  
  
> [!IMPORTANT]
>   Windows Server Essentials não oferece suporte a backup e restauração discos dinâmicos em computadores cliente.  
  
#### <a name="to-set-up-backup-for-a-client-computer"></a>Para configurar o backup para um computador cliente  
  
1.  Abrir o **painel**e, em seguida, clique no **dispositivos** guia.  
  
2.  Clique no nome do computador cliente que você deseja configurar backup para e, em seguida, no **tarefas** painel, clique em **Configurar Backup para esse computador**.  
  
    > [!NOTE]
    >  Se o backup já está configurado para o computador cliente, **personalizar o Backup para esse computador** estiver listado no **tarefas** painel, em vez de **Configurar Backup para esse computador**.  
  
3.  No Assistente de configuração de Backup, você pode optar por fazer backup de todas as pastas ou selecione determinadas pastas que você quer fazer backup. Siga as instruções no assistente.  
  
4.  Clique em **fechar** quando o backup é configurado para o computador.  
  
### <a name="critical-system-files"></a>Arquivos importantes do sistema  
 Quando você instala o sistema operacional Windows, o programa de instalação cria pastas na unidade do sistema onde ela coloca arquivos que o sistema exija a funcionar. Arquivos importantes do sistema incluem arquivos com extensões de arquivo. sys, .exe,. ocx e. dll. Alguns desses arquivos são TrueType. Além disso, os arquivos de estado do sistema, como o registro do sistema s, são necessários para o sistema operacional a ser executado corretamente.  
  
### <a name="find-the-file-you-are-looking-for"></a>Localize o arquivo que você está procurando  
 Você poderá restaurar todas as pastas para um computador, vários arquivos e pastas, ou um único arquivo ou pasta usando um backup existente.  
  
 Depois de selecionar o backup que você deseja usar para restaurar a partir de, o arquivo de backup é lidos e todos os arquivos e pastas são exibidas. Você pode analisar o arquivo específico ou a pasta a ser restaurada por duas vezes na pasta de nível superior e, em seguida, busca detalhada por meio da hierarquia de pastas até localizar o arquivo que você está procurando.  
  
### <a name="why-am-i-unable-to-select-some-items"></a>Por que não posso selecionar alguns itens?  
 A caixa de seleção no menu de seleção a **selecionar quais itens para fazer backup de página** pode indicar o status diferente para cada pasta. Quando a caixa de seleção é:  
  
-   **Selecionado**, a pasta associada e o conteúdo da pasta é selecionado para backup.  
  
-   **Não selecionado**, a pasta associada e o conteúdo da pasta é excluído do backup.  
  
-   **Sólido**, a pasta associada será selecionada para backup, mas um ou mais itens dentro dessa pasta são excluídas do backup.  
  
 Se você não pode selecionar uma pasta específica:  
  
-   O volume pode ser configurado para o backup, mas ele pode estar offline. Isso é comum para unidades USB removíveis. Volumes que estão offline são mostrados em texto cinza.  
  
-   Você só pode fazer backup dos dados de uma unidade local formatada como um sistema de arquivos NTFS. Unidades formatadas como FAT (incluindo FAT32) ou sistemas de arquivos ReFS não aparecem na lista de unidades para fazer backup.  
  
> [!IMPORTANT]
>  Volume Shadow Copy Service (VSS) não oferece suporte à criação de uma cópia de sombra de um volume virtual e o volume do host no mesmo conjunto de instantâneo. VSS suporte criando instantâneos de volumes em um disco rígido virtual (VHD), se o backup do volume virtual é necessário. Para obter mais informações, consulte [manutenção e fazendo backup os discos rígidos virtuais](https://go.microsoft.com/fwlink/p/?LinkId=256577).  
  
##  <a name="BKMK_4"></a>Alterar a hora em que o backup está agendado para ser executado  
 O processo de backup deve ser agendado durante um tempo quando o menor número de pessoas possível estiver usando seus computadores em rede. Isso é geralmente durante a noite atrasada ou horas de início da manhã. O tempo padrão para o backup é 6:00 PM até 9 horas. O servidor tenta fazer backup de computadores cliente somente durante a janela agendada de tempo.  
  
 No entanto, se sua empresa está ativa durante esses tradicionalmente fora do horário comercial, convém alterar essas configurações padrão.  
  
#### <a name="to-change-the-time-that-backup-is-scheduled-to-run"></a>Para alterar o tempo que o backup está agendado para ser executado  
  
1.  Abra o servidor **painel**e clique em **dispositivos**.  
  
2.  Em **dispositivos tarefas**, clique em **configurações de personalizar o Backup do computador e o histórico de arquivos**.  
  
    > [!NOTE]
    >  No Windows Server Essentials, essa tarefa tenha sido renomeada **tarefas backup do computador cliente**.  
  
3.  Em **ferramentas e configurações de backup de computador cliente**diante do **Backup do computador** guia, você poderá alterar os horários de início e fim para atender às suas necessidades.  
  
4.  Clique em **aplicar**e clique em **Okey**.  
  
##  <a name="BKMK_5"></a>Alterar a política de retenção de backup do computador  
 Backups diários de todos os seus computadores acumulam em seu servidor ao longo do tempo. Para ajudá-lo a gerenciar esses backups, Windows Server Essentials pode ajudá-lo a gerenciar o banco de dados de backups do computador. Você pode configurar os backups quantos manter para todos os seus computadores.  
  
 A política de retenção de backup determina quanto tempo um backup é mantido antes que ele seja excluído durante o processo de limpeza backup. Limpeza de backup é executado em 11:59 PM cada sábado. Ele exclui todos os backups que fogem a política de retenção de backup. Os padrões para a política de retenção de backup são:  
  
-   **Reter backups diários de 5 dias.** O primeiro backup do dia é mantido como o backup diário. Após 5 dias, o backup diário mais antigo é excluído no processo de limpeza.  
  
-   **Reter backup semanal por 4 semanas.** O primeiro backup da semana é mantido como o backup semanal. Depois de 4 semanas, o backup semanal mais antigo é excluído no processo de limpeza.  
  
-   **Reter backups mensais por seis meses.** O primeiro backup do mês é mantido como o backup mensal. Após 6 meses, o backup mensal mais antigo é excluído no processo de limpeza  
  
#### <a name="to-change-the-computer-backup-retention-policy"></a>Para alterar a política de retenção de backup do computador  
  
1.  Abrir o **painel**.  
  
2.  Clique em **dispositivos**.  
  
3.  No **dispositivos tarefas** painel, clique em **configurações de personalizar o Backup do computador e o histórico de arquivos**.  
  
    > [!NOTE]
    >  No Windows Server Essentials, essa tarefa tenha sido renomeada **tarefas de backup do computador cliente**.  
  
4.  Em **ferramentas e configurações de backup de computador cliente**, além do **política de retenção de backup do computador cliente** seção, faça as alterações à política de retenção que atenda às suas necessidades.  
  
5.  Quando você terminar, clique em **Okey** para aplicar as alterações e fechar a caixa de diálogo. A política de retenção atualizados será aplicada na próxima vez que o backup é executado de limpeza.  
  
    > [!NOTE]
    >  A política de retenção atualizado se aplica a todos os computadores cliente em sua rede que estão configurados para backup.  
  
##  <a name="BKMK_6"></a>Restaurar backup para as configurações padrão  
 Depois que o backup está configurado para computadores cliente, o administrador de rede talvez tenha especificado um intervalo de tempo diferente. Da mesma forma, o administrador pode ter especificado um tempo de retenção de backup maior ou menor que o padrão. O **restaurar padrões** botão permite que você redefina a política de janela e a retenção de backup para o padrão fornecido durante a configuração inicial de backup.  
  
 Os padrões são:  
  
-   Hora de início de backup: 6:00 PM  
  
-   Hora de término: 9H às  
  
-   Número de dias backups diários são mantidos: 5 dias  
  
-   Número de semanas que o último backup da semana é retido: 4 semanas  
  
-   Número de meses que o último backup do mês é retido: 6 meses  
  
#### <a name="to-reset-computer-backup-to-the-default-settings"></a>Restaurar o Backup do computador para as configurações padrão  
  
1.  Abra o **painel**e, em seguida, abra o **dispositivos** página.  
  
2.  Em **dispositivos tarefas**, clique em **configurações de personalizar o Backup do computador e o histórico de arquivos**.  
  
    > [!NOTE]
    >  No Windows Server Essentials, clique em **tarefas backup do computador cliente**.  
  
3.  No **Backup do computador** guia do **ferramentas e configurações do computador e o backup do cliente** página, clique em **restaurar padrões**.  
  
    > [!NOTE]
    >  Você pode querer Anote o agendamento personalizado e configurações de política de retenção porque eles será excluídos quando você restaurar as configurações padrão.  
  
4.  Clique em **aplicar**e clique em **Okey**.  
  
##  <a name="BKMK_7"></a>Use as ferramentas de reparo e recuperação  
 **Reparar backups:** se o banco de dados de backups de computador ficar corrompida ou inutilizável por algum motivo, você pode tentar reparar o banco de dados usando o reparo do Assistente de banco de dados de Backup. O assistente analisa os arquivos de backup para determinar se há quaisquer problemas que podem ser reparados. O assistente, em seguida, tenta corrigir os problemas que forem encontrados.  
  
> [!WARNING]
>  Se não puder ser corrigido um problema, o assistente pode excluir permanentemente um arquivo de backup do computador.  
  
 **Recuperação do computador:** você pode criar uma unidade flash USB inicializável para usar para restaurar um computador de um backup existente. Você deve usar uma unidade flash USB que é 1 GB ou maior. Depois que a unidade flash USB inicializável é criada, insira-o no computador que você quer restaurar e, em seguida, inicialize para a unidade flash USB para iniciar o processo de restauração do sistema completo.  
  
##  <a name="BKMK_8"></a>Reparar o banco de dados de backup  
 Se você receber um alerta informando que o banco de dados de backup do computador tiver problemas, você pode tentar repará-la.  
  
#### <a name="to-repair-the-backup-database"></a>Para reparar o banco de dados de backup  
  
1.  Abrir o **painel**.  
  
2.  Clique em **dispositivos**.  
  
3.  No **dispositivos tarefas** painel, clique em **configurações de personalizar o Backup do computador e o histórico de arquivos.**  
  
    > [!NOTE]
    >  No Windows Server Essentials, clique em **tarefas backup do computador cliente**.  
  
4.  Em **ferramentas e configurações de backup de computador cliente**, clique no **ferramentas** guia.  
  
5.  No **reparar backups** seção, clique em **reparar agora**. Abre o reparo do Assistente de banco de dados de Backup.  
  
6.  Siga as instruções da reparar o Assistente de banco de dados de Backup.  
  
7.  Dependendo de qual é o tamanho do banco de dados de backup é, o reparo de banco de dados pode levar várias horas. Clique em **fechar**e clique em **Okey** para fechar o **configurações de personalizar o Backup do computador e o histórico de arquivos** página.  
  
    > [!NOTE]
    >  No Windows Server Essentials, clique em **ferramentas e configurações de backup de computador cliente**.  
  
#### <a name="to-review-the-results-of-the-backup-database-repair"></a>Para analisar os resultados do reparo de backup do banco de dados  
  
1.  Abrir o **painel**.  
  
2.  Clique em **dispositivos**.  
  
3.  No **dispositivos tarefas** painel, clique em **configurações de personalizar o Backup do computador e o histórico de arquivos.**  
  
    > [!NOTE]
    >  No Windows Server Essentials, clique em **tarefas backup do computador cliente**.  
  
4.  Em **ferramentas e configurações de backup de computador cliente**, clique no **ferramentas** guia.  
  
5.  Os resultados são exibidos no **reparar backups** seção.  
  
##  <a name="BKMK_9"></a>Desativar o backup de um computador  
 Use o painel para desabilitar rapidamente os backups de computadores em sua rede.  
  
#### <a name="to-disable-backup-for-a-computer"></a>Para desativar o backup de um computador  
  
1.  Abrir o **painel**.  
  
2.  Clique no **dispositivos** guia.  
  
3.  Clique no nome do computador para o qual você deseja desativar backups.  
  
4.  No painel de tarefas, clique em **personalizar o Backup do computador**. O Assistente de Backup personalizar aparece.  
  
5.  Clique em **desativa o backup para esse computador**e, em seguida, selecione se você deseja manter ou excluir os arquivos de backup existentes.  
  
6.  Clique em **salvar alterações**e clique em **fechar**.  
  
 Para obter informações sobre como habilitar o backup de um computador após a desativação do backup, consulte [configurar backup para um computador cliente](Manage-Client-Computer-Backup-in-Windows-Server-Essentials.md#BKMK_3).  
  
##  <a name="BKMK_10"></a>Executar a tarefa de limpeza backup  
 A tarefa de limpeza backup do computador cliente é agendada para execução em 11:59 PM cada sábado depois que todos os backups são concluídos. A tarefa de limpeza exclui backups do cliente computador backup banco de acordo com a política de retenção de backup. As configurações padrão para a política de retenção de backup são:  
  
-   Número de dias backups diários são mantidos: 5 dias  
  
-   Número de semanas que o último backup da semana é retido: 4 semanas  
  
-   Número de meses que o último backup do mês é retido: 6 meses  
  
 Para obter informações sobre como alterar a política de retenção de backup, consulte [alterar a política de retenção de backup do computador](Manage-Client-Computer-Backup-in-Windows-Server-Essentials.md#BKMK_5).  
  
#### <a name="to-run-the-client-backup-database-cleanup-task"></a>Para executar tarefas de limpeza de backup do banco de dados de cliente  
  
1.  Fazer logon no servidor com sua conta de usuário administrador e a senha.  
  
2.  No servidor, abra **Agendador de tarefas**. Você pode acessar o Agendador de tarefas do programa a **ferramentas administrativas** console.  
  
3.  No Agendador de tarefas, expanda **biblioteca do Agendador de tarefas**, expanda **Microsoft**, expanda **Windows**e clique em **Windows Server Essentials**.  
  
4.  Clique no **Backup limpeza** de tarefas e clique em **executar** no **ações** painel. O Status é alterado para **executando** até que a tarefa seja concluída.  
  
##  <a name="BKMK_11"></a>Exibir alertas na barra de tarefas em um computador cliente  
 Você pode receber um aviso de backup na barra de tarefas em seu computador pelos seguintes motivos:  
  
-   Um backup foi iniciado.  
  
-   Um backup foi encerrado.  
  
-   Não há um backup recente bem-sucedida. O número de dias desde o último backup bem-sucedido está incluído no aviso.  
  
-    Somente para o Windows Server Essentials: um novo volume é adicionado ao seu computador. A pessoa que administra sua rede precisa executar o Assistente para personalizar o Backup para incluir ou excluir o volume para backups futuros.  
  
##  <a name="BKMK_12"></a>Exibir status de backup da barra inicial  
 Use a barra inicial para exibir status rápido de backup para o seu computador.  
  
> [!TIP]
>  Para um status rápido, mova o cursor sobre o ícone da barra inicial na área de notificação da área de trabalho. O backup status aparecerá na dica de ferramenta.  
  
#### <a name="to-view-status-of-a-backup-for-your-computer"></a>Para exibir o status de um backup para o seu computador  
  
1.  Faça logon no **Launchpad** com seu nome de usuário e senha.  
  
2.  Clique em **Backup**.  
  
3.  No **propriedades Backup** na caixa o **Backup status** seção, o status do backup é exibida.  
  
    -   **Nenhum backup anterior**: Backup seja ainda não foi executado, ou o histórico de backup foi excluído.  
  
    -   **Backup pendentes**: um backup está esperando por outro processo no servidor para concluir.  
  
    -   **Conectando-se**: Backup está se conectando ao servidor.  
  
    -   **Não consigo me conectar**: Backup não consegue se conectar para o serviço de provedor do Backup do cliente computador do Windows Server.  
  
        > [!NOTE]
        >  No Windows Server Essentials, esse serviço foi renomeado o serviço de gerenciamento do computador de cliente do Windows Server Essentials.  
  
    -   **Backup em andamento**: exibe a porcentagem de conclusão de backup.  
  
    -   **Backup foi cancelado**: exibe a data e hora em que o backup iniciado se você ou o administrador da rede cancelado o backup.  
  
    -   **Backup bem-sucedido**: exibe a data e hora em que o backup iniciado se o backup foi concluída com êxito.  
  
    -   **Backup foi incompleto**: exibe a data e hora em que o backup iniciado se o backup não foi concluída com êxito.  
  
    -   **Backup não deu certo**: exibe a data e hora em que o backup iniciado se o backup não foi concluída com êxito.  
  
4.  Clique em **Okey** para fechar o **Backup Properties** caixa de diálogo.  
  
##  <a name="BKMK_13"></a>Parar um backup em andamento da barra inicial  
 Você pode facilmente parar um backup que está em andamento.  
  
#### <a name="to-stop-a-backup-in-progress"></a>Para parar de um backup em andamento  
  
1.  Abrir o **Launchpad**.  
  
    > [!NOTE]
    >  Se solicitado, faça logon no **Launchpad** com seu nome de usuário e senha.  
  
2.  Clique em **Backup**.  
  
3.  No **propriedades Backup** na caixa o **Backup status** seção, o **Iniciar backup** botão é alterado para **parar backup** quando o backup está em andamento. Clique em **parar backup**e clique em **Sim** para confirmar. O backup continuará a funcionar até você clicar em **Sim**.  
  
4.  Quando você interrompe um backup em andamento, o status é alterado para **Backup foi cancelado** com a data e hora que iniciou o backup. Se, em vez disso, você vê o status de **bem-sucedida**, **incompleto**, ou **Failed**, o último backup já foi concluído.  
  
5.  Clique em **Okey** para fechar o **Backup Properties** caixa de diálogo.  
  
##  <a name="BKMK_14"></a>Inicie um backup da barra inicial  
 Pode haver momentos quando você quer fazer backup de seus arquivos e pastas antes do tempo de backup agendado regularmente configurada no seu servidor. Barra inicial permite que você inicie manualmente o backup do computador.  
  
#### <a name="to-start-a-backup"></a>Para iniciar um backup  
  
1.  Abrir o **Launchpad**.  
  
    > [!NOTE]
    >  Se solicitado, faça logon no **Launchpad** com seu nome de usuário e senha.  
  
2.  Clique em **Backup**.  
  
3.  No **propriedades Backup** na caixa o **Backup status** seção, clique em **Iniciar backup**e clique em **Okey**.  
  
4.  Digite uma descrição para o backup e, em seguida, clique em **Okey**. O status é alterado para **a partir do backup**e, em seguida, para **Backup no processo** com uma porcentagem concluída.  
  
5.  Após o início do backup, o botão é alterado para **parar backup**. Se o backup está em andamento e você deseja interrompê-la, clique em **parar backup**e clique em **Sim**. Quando você interrompe um backup em andamento, o status é alterado para **Backup foi cancelado** com a data e hora em que o backup foi iniciado.  
  
6.  Quando o backup é concluído com êxito, o status é alterado para **Backup bem-sucedido** com a data e hora que iniciou o backup terminado.  
  
7.  Clique em **Okey** para fechar o **Backup Properties** caixa de diálogo.  
  
##  <a name="BKMK_15"></a>Como funciona o backup do computador  
 As seguintes coisas acontecem durante o backup cada dia:  
  
-   Computadores de rede são copiados para cima após o outro.  
  
-   Um backup que está em progresso é concluída, mesmo se você fechar a janela de backup.  
  
-   Atualizações do Windows são instaladas e Windows Server Essentials é reiniciado (se necessário).  
  
-   No domingo, Backup limpeza é executado.  
  
> [!IMPORTANT]
>  Volume Shadow Copy Service (VSS) não oferece suporte à criação de uma cópia de sombra de um volume virtual e o volume do host no mesmo conjunto de instantâneo. VSS suporte criando instantâneos de volumes em um disco rígido virtual (VHD), se o backup do volume virtual é necessário. Para obter mais informações, consulte [manutenção e fazendo backup os discos rígidos virtuais](https://go.microsoft.com/fwlink/p/?LinkId=256577).  
  
##  <a name="BKMK_16"></a>Dicas para ajudar a evitar a perda de dados devido a corrupção de banco de dados de backup do cliente  
 Se o banco de dados de backup do cliente forem corrompido, você poderá perder dados críticos.  
  
 Para ajudar a evitar a perda de dados devido a corrupção de banco de dados de backup do cliente, considere o seguinte:  
  
-   Permitir que um método adicional de backup do banco de dados de backup do cliente. Por exemplo, dependendo da versão do Windows Server Essentials que você está executando, use Backup do servidor, Backup Online ou Microsoft Azure Backup para fazer backup do banco de dados.  
  
    > [!IMPORTANT]
    >  Certifique-se de que você selecione fazer backup de todos os arquivos na **Backup de computadores cliente** pasta.  
  
-   Que você deve restaurar o banco de dados, certifique-se de restaurar todos os arquivos na **Backup de computadores cliente** pasta. Se você não restaurar todos os arquivos, o banco de dados pode se tornar inconsistente.  
  
-   Antes de você limpa ou restaurar o banco de dados de backup do cliente, certifique-se parar de todos os backups de cliente que estão atualmente em andamento.  
  
     Se você estiver executando o Windows Server Essentials, você também deve interromper o serviço de Backup do computador cliente do Windows Server e o serviço de provedor do Backup do cliente computador do Windows Server.  
  
     Se você estiver executando o Windows Server Essentials, você também deve interromper o serviço de Backup do computador Windows Server.  
  
     Depois de concluir a operação de restauração, reinicie o serviço.  
  
##  <a name="BKMK_17"></a>Restaurar arquivos ou pastas de um backup do computador cliente  
 Você pode procurar e restaurar arquivos e pastas individuais de um backup.  
  
> [!NOTE]
>  Você não poderá restaurar arquivos e pastas em um computador cliente usando o painel no servidor. Você deve abrir o painel em um computador cliente para concluir a tarefa.  
  
> [!IMPORTANT]
>  Você não poderá restaurar arquivos e pastas em um disco rígido que foi configurado para ser um disco dinâmico.  
  
#### <a name="to-restore-files-and-folders"></a>Para restaurar arquivos e pastas  
  
1.  Abrir o **painel** em um computador cliente.  
  
2.  Clique no **dispositivos** guia.  
  
3.  Clique no computador para o qual você deseja restaurar arquivos e pastas e clique em **restaurar arquivos ou pastas para o computador** no **tarefas** painel. O Assistente de pastas ou restaurar arquivos é aberta.  
  
4.  Siga as instruções no assistente.  
  
##  <a name="BKMK_FileHistory"></a>Noções básicas sobre o histórico de arquivos  
 O recurso de histórico de arquivos do Windows Server Essentials backup automático dos arquivos que estão nas pastas Favoritos, contatos, área de trabalho e bibliotecas de computadores de rede que têm a funcionalidade de histórico de arquivos. Se os originais forem perdidos, danificados ou excluídos, você poderá restaurá-los. Você também pode encontrar diferentes versões de seus arquivos de um ponto específico no tempo. Ao longo do tempo, você terá um histórico completo dos seus arquivos.  
  
 No Windows Server Essentials, você pode personalizar as configurações do histórico de arquivos do **histórico de arquivos** página de **ferramentas e configurações de backup de computador cliente**.  
  
 No Windows Server Essentials, você pode personalizar as configurações do histórico de arquivos, acessando o **usuários** tab e selecionando o **alterar a configuração do histórico de arquivos** tarefa.  
  
 A página Histórico de arquivos fornece as seguintes opções:  
  
|Configuração de backup|Padrão|Descrição|  
|--------------------|-------------|-----------------|  
|Ativar / desativar|Em|Histórico de arquivos é ativado por padrão quando você instala o Windows Server Essentials.|  
|Dados de backup|Documentos e área de trabalho|Há três configurações pré-configurados, que permitem que você especifique uma variedade de soluções de backup. Você pode escolher uma das seguintes opções:<br /><br /> -Documentos e área de trabalho<br /><br /> -Todas as bibliotecas, área de trabalho, contatos e Favoritos<br /><br /> -Todos os dados em bibliotecas, área de trabalho, contatos e excluir dados nas bibliotecas de música, vídeo e imagens de Favoritos|  
|Frequência de backup|A cada hora|Especifica quantas vezes o histórico de arquivos cria um backup dos dados selecionados. Você pode escolher entre várias opções que variam de cada 10 minutos para diariamente.|  
|Manter as cópias para|1 ano|Especifica a quantidade de tempo que o histórico de arquivos mantém uma cópia de um backup.|  
  
 Para obter informações sobre problemas de histórico de arquivos, consulte [solucionar histórico de arquivos](../support/Troubleshoot-File-History-in-Windows-Server-Essentials.md).  
  
## <a name="see-also"></a>Consulte também  
  
-   [Gerenciar o Backup e restauração](Manage-Backup-and-Restore-in-Windows-Server-Essentials.md)  
  
-   [Gerenciar o Windows Server Essentials](Manage-Windows-Server-Essentials.md)  
  
-   [Usar o Windows Server Essentials](../use/Use-Windows-Server-Essentials.md)
