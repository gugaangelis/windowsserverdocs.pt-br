---
title: Gerenciar o backup do computador cliente no Windows Server Essentials
description: Descreve como usar o Windows Server Essentials
ms.date: 10/03/2016
ms.prod: windows-server
ms.topic: article
ms.assetid: 1b4776e8-9504-4b98-ae80-11da797d9819
author: nnamuhcs
ms.author: coreyp
manager: dongill
ms.openlocfilehash: ae59fb050b41cef866a8f0e97d8d07d28b54ca52
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80852819"
---
# <a name="manage-client-computer-backup-in-windows-server-essentials"></a>Gerenciar o backup do computador cliente no Windows Server Essentials

>Aplica-se a: Windows Server 2016 Essentials, Windows Server 2012 R2 Essentials, Windows Server 2012 Essentials
 
 Este tópico inclui informações sobre tarefas comuns de backup para os computadores cliente que podem ser realizadas usando o Painel do Windows Server Essentials, e inclui as seguintes seções:  
  
-   [Como funciona o assistente para reparar banco de dados de backup](Manage-Client-Computer-Backup-in-Windows-Server-Essentials.md#BKMK_1)  
  
-   [Entender as configurações de backup do computador](Manage-Client-Computer-Backup-in-Windows-Server-Essentials.md#BKMK_2)  
  
-   [Configurar o backup para um computador cliente](Manage-Client-Computer-Backup-in-Windows-Server-Essentials.md#BKMK_3)  
  
-   [Alterar a hora em que o backup está agendado para ser executado](Manage-Client-Computer-Backup-in-Windows-Server-Essentials.md#BKMK_4)  
  
-   [Alterar a política de retenção de backup do computador](Manage-Client-Computer-Backup-in-Windows-Server-Essentials.md#BKMK_5)  
  
-   [Redefinir backup para as configurações padrão](Manage-Client-Computer-Backup-in-Windows-Server-Essentials.md#BKMK_6)  
  
-   [Usar ferramentas de reparo e recuperação](Manage-Client-Computer-Backup-in-Windows-Server-Essentials.md#BKMK_7)  
  
-   [Reparar o banco de dados de backup](Manage-Client-Computer-Backup-in-Windows-Server-Essentials.md#BKMK_8)  
  
-   [Desabilitar o backup de um computador](Manage-Client-Computer-Backup-in-Windows-Server-Essentials.md#BKMK_9)  
  
-   [Executar a tarefa de limpeza de backup](Manage-Client-Computer-Backup-in-Windows-Server-Essentials.md#BKMK_10)  
  
-   [Exibir alertas na barra de tarefas em um computador cliente](Manage-Client-Computer-Backup-in-Windows-Server-Essentials.md#BKMK_11)  
  
-   [Exibir status de backup no Launchpad](Manage-Client-Computer-Backup-in-Windows-Server-Essentials.md#BKMK_12)  
  
-   [Parar um backup em andamento no Launchpad](Manage-Client-Computer-Backup-in-Windows-Server-Essentials.md#BKMK_13)  
  
-   [Iniciar um backup no Launchpad](Manage-Client-Computer-Backup-in-Windows-Server-Essentials.md#BKMK_14)  
  
-   [Como funciona o backup do computador](Manage-Client-Computer-Backup-in-Windows-Server-Essentials.md#BKMK_15)  
  
-   [Dicas para ajudar a evitar a perda de dados devido à corrupção do banco de dado de backup do cliente](Manage-Client-Computer-Backup-in-Windows-Server-Essentials.md#BKMK_16)  
  
-   [Restaurar arquivos ou pastas de um computador cliente backup](Manage-Client-Computer-Backup-in-Windows-Server-Essentials.md#BKMK_17)  
  
-   [Compreendendo o histórico de arquivos](Manage-Client-Computer-Backup-in-Windows-Server-Essentials.md#BKMK_FileHistory)  
  
##  <a name="how-the-repair-the-backup-database-wizard-works"></a><a name="BKMK_1"></a>Como funciona o assistente para reparar banco de dados de backup  
 Se o Windows Server Essentials detectar erros no backup do banco de dados, envia uma notificação de integridade e o status de alerta muda para vermelho, que indica uma condição crítica.  
  
 No Painel do Windows Server Essentials, clique no ícone de status de alerta para ver a notificação de erro de backup do banco de dados. A notificação inclui um **reparar** botão, que inicia o Assistente de Backup de Banco de Dados de reparo. O assistente pode levar horas para ser concluído.  
  
 O assistente examina o  backup do banco de dados de para o computador selecionado e tenta reparar o banco de dados se forem detectados erros. Em alguns casos, o assistente não pode reparar o banco de dados de backup inteiro. Antes de iniciar o assistente, verifique se todos os discos rígidos externos que usam no seu servidor estão conectados ao servidor, ligado e funcionando adequadamente. O erro de backup do banco de dados pode ser corrigido automaticamente se reconectar a uma unidade de disco rígida ausente.  
  
> [!CAUTION]
>  Você deve fazer backup do banco de dados antes de tentar repará-la. Dependendo do tipo de erros encontrados no backup do banco de dados, o assistente não poderá recuperar alguns backups. Alguns ou todos os backups poderão ser perdidos permanentemente.  
  
##  <a name="understand-the-computer-backup-settings"></a><a name="BKMK_2"></a>Entender as configurações de backup do computador  
 Depois que o backup está configurado para computadores cliente, você pode especificar uma janela diferente de tempo para o realização do backup. Da mesma forma, você pode especificar um tempo de retenção de backup maior ou menor que o padrão.  
  
 A opção**restaurar padrões** permite que você redefina a política de retenção e a janela de backup para o padrão fornecido durante a configuração inicial do backup.  
  
 Os padrões são:  
  
|Configuração de backup|Padrão|Descrição|  
|--------------------|-------------|-----------------|  
|Hora de início|18:00 Horas|Especifica a hora que o backup diário será iniciado. É recomendável que você defina isso para um horário quando os usuários não estão usando seus computadores.|  
|Hora de término|9h00|Especifica a hora que o backup deverá ser concluído. Se o backup não exigir uma duração específica, ele usa apenas o tempo necessário para fazer o backup do seu computador com êxito.|  
|Armazenamento de backups diários|5 dias|Especifica o número de dias que os backups diários são armazenados. Por exemplo, 5 backups diários usando a configuração padrão, são armazenados. No sexto dia e a cada dia posteriormente, o arquivo de backup diário mais antigo será excluído.|  
|Retenção de backups semanais|4 semanas|Especifica o número de semanas que o último backup da semana dura. Por exemplo, 4 backups semanais usando as configurações padrão, são armazenados. Na quinta semana e cada semana posteriormente, o backup semanal mais antigo será excluído.|  
|Retenção de backups mensais|6 meses|Especifica o número de meses que o último backup do mês dura. Por exemplo, usando a configuração padrão, 6 meses de backups serão armazenados. Sobre o quarto e cada mês posteriormente, o backup mensal mais antigo será excluído.|  
  
##  <a name="set-up-backup-for-a-client-computer"></a><a name="BKMK_3"></a>Configurar o backup para um computador cliente  
 Se o backup é desabilitado, você pode configurar o backup do computador no Painel. Quando você configura o backup de um computador, você pode escolher fazer backup total do computador, ou selecione os volumes e as pastas que você deseja fazer backup.  
  
> [!NOTE]
>  O computador deve estar online para definir o backup.  
  
> [!IMPORTANT]
>   O Windows Server Essentials não dá suporte ao backup e à restauração de discos dinâmicos em computadores cliente.  
  
#### <a name="to-set-up-backup-for-a-client-computer"></a>Para configurar o backup de um computador cliente  
  
1.  Abra o **Painel** e clique na guia **Dispositivos**  
  
2.  Clique no nome do computador cliente que você deseja configurar o backup para e, em seguida, no painel **Tarefas**, clique em **Configurar o Backup para este computador**.  
  
    > [!NOTE]
    >  Se o backup já está configurado para o computador cliente, **Personalizar Backup para este Computador** listados no painel de **tarefas** em vez de **Configurar Backup para este Computador**.  
  
3.  No Assistente de configuração de Backup, você pode optar por fazer backup de todas as pastas ou selecionar determinadas pastas que você deseja fazer backup. Siga as instruções no assistente.  
  
4.  Clique em **Fechar** quando o backup estiver definido para o computador.  
  
### <a name="critical-system-files"></a>Arquivos essenciais do sistema  
 Quando você instala o sistema operacional Windows, o programa de instalação cria pastas na unidade do sistema para coloca os arquivos que o sistema exige para iniciar e executar. Arquivos essenciais do sistema incluem arquivos com extensões de arquivo. sys, .exe,. ocx e. dll. Alguns desses arquivos são fontes True Type. Além disso, os arquivos de estado do sistema, como o registro do sistema, são necessários para que o sistema operacional seja executado corretamente.  
  
### <a name="find-the-file-you-are-looking-for"></a>Localizar o arquivo que você está procurando  
 Você pode restaurar todas as pastas para um computador, vários arquivos e pastas, ou um único arquivo ou pasta de um backup existente.  
  
 Depois de selecionar o backup que você deseja usar para restaurar, o arquivo de backup é lido e todos os arquivos e pastas são exibidas. Você pode analisar o arquivo ou pasta específica para restaurar clicando duas vezes na pasta de nível superior e, em seguida, busca detalhada por meio da hierarquia de pastas até localizar o arquivo que você está procurando.  
  
### <a name="why-am-i-unable-to-select-some-items"></a>Por que não é possível selecionar alguns itens?  
 A caixa de seleção no menu seleção do **Selecione os itens para backup página** pode indicar status diferente para cada pasta. Quando a caixa de seleção é:  
  
- **Selecionada**, a pasta associada e o conteúdo da pasta está selecionado para backup.  
  
- **Não selecionada**, a pasta associada e o conteúdo da pasta é excluído do backup.  
  
- **Sólido**, a pasta associada é selecionada para backup, mas um ou mais itens nessa pasta são excluídos do backup.  
  
  Se você não pode selecionar uma pasta específica:  
  
- O volume pode ser configurado para backup, mas pode estar offline. Isso é comum para unidades de USB removíveis. Volumes que estão offline são mostrados em texto cinza.  
  
- Você só pode fazer backup dos dados de uma unidade local formatada no sistema de arquivos NTFS. Unidades formatadas como FAT (incluindo FAT32) ou sistemas de arquivos ReFS não aparecem na lista de unidades de backup.  
  
> [!IMPORTANT]
>  O Volume Shadow Copy Service (VSS) não oferece suporte à criação de uma cópia fantasma de um volume virtual e o volume de hospedagem do mesmo conjunto istantâneo. O VSS oferece suporte à criação de instantâneos de volumes em um disco rígido virtual (VHD), se o backup do volume virtual for necessário. Para obter mais informações, consulte [Fazendo manutenção e backup de discos rígidos virtuais](https://go.microsoft.com/fwlink/p/?LinkId=256577).  
  
##  <a name="change-the-time-that-backup-is-scheduled-to-run"></a><a name="BKMK_4"></a>Alterar a hora em que o backup está agendado para ser executado  
 O processo de backup deve ser agendado durante um período quando o menor número possível de pessoas estiver usando os computadores em rede. Normalmente acontece durante as primeiras horas do dia ou tarde da noite. A hora padrão de backup é de 6:00 até 9:00 Horas. O servidor tenta fazer backup de computadores cliente somente durante o período de tempo agendado.  
  
 No entanto, se sua empresa está ativa tradicionalmente no peróido fora do horário comercial, convém alterar essas configurações padrão.  
  
#### <a name="to-change-the-time-that-backup-is-scheduled-to-run"></a>Para alterar o tempo de execução do backup agendado.  
  
1.  Abra o servidor **Painel**e clique em **Dispositivos**.  
  
2.  Em **Tarefas de dispositivos**, clique em **Personalizar Configurações de Backup do computador e Configurações do Histórico de arquivos**.  
  
    > [!NOTE]
    >  No Windows Server Essentials, essa tarefa foi renomeada como **tarefas de backup do computador cliente**.  
  
3.  Em **ferramentas e configurações de backup do computador cliente**, na guia **Backup do Computador**, você pode alterar os horários de início e término para atender às suas necessidades.  
  
4.  Clique em **Aplicar**e clique em **OK**.  
  
##  <a name="change-the-computer-backup-retention-policy"></a><a name="BKMK_5"></a>Alterar a política de retenção de backup do computador  
 Backups diários de todos os computadores se acumulam no seu servidor ao longo do tempo. Para ajudar a gerenciar esses backups, o Windows Server Essentials pode ajudá-lo a gerenciar os backups do banco de dados do computador. Você pode configurar qualquer número de backups para armazenar para todos os computadores.  
  
 A política de armazenamento de backup determina quanto tempo um backup será mantido antes de ser excluído durante o processo de limpeza de backup. A limpeza do backup é executado às 23:59 Horas todos os sábados. Ele exclui todos os backups que estão fora da política de armazenamento do backup. Os padrões para a política de armazenamento de backup são:  
  
-   **Manter backups diários por 5 dias.** O primeiro backup do dia é mantido como o backup diário. Após 5 dias, o backup diário mais antigo será excluído no processo de limpeza.  
  
-   **Manter o backup semanal por 4 semanas.** O primeiro backup da semana é mantido como backup semanal. Depois de 4 semanas, o backup semanal mais antigo será excluído no processo de limpeza.  
  
-   **Armazenamento de backups mensalmente por 6 meses.** O primeiro backup do mês é armazenado como backup mensal. Depois de 6 meses, o backup mensal mais antigo será excluído no processo de limpeza  
  
#### <a name="to-change-the-computer-backup-retention-policy"></a>Para alterar a política de armazenamento do backup do computador  
  
1.  Abra o **Dashboard**.  
  
2.  Clique em **Dispositivos**.  
  
3.  No painel **Tarefas**, clique em **Personalizar Backup do Computador e Configurações do Histórico do Arquivo**.  
  
    > [!NOTE]
    >  No Windows Server Essentials, essa tarefa foi renomeada como **tarefas de backup do computador cliente**.  
  
4.  Em **Ferramentas e Configurações de backup do computador cliente**, em **Política de armazenamento de backup do computador cliente** seção, faça as alterações de política de armazenamento que atendem às suas necessidades.  
  
5.  Quando terminar, clique em **OK** para aplicar as alterações e fechar a caixa de diálogo. A política de armazenamento atualizada será aplicada na próxima vez que executar a limpeza de backup.  
  
    > [!NOTE]
    >  A política de armazenamento atualizada se aplica a todos os computadores cliente na sua rede que estão configurados para backup.  
  
##  <a name="reset-backup-to-default-settings"></a><a name="BKMK_6"></a>Redefinir backup para as configurações padrão  
 Depois que o backup está configurado para os computadores cliente, o administrador de rede pode ter especificado um período diferente de tempo. Da mesma forma, o administrador pode ter especificado um tempo de armazenamento de backup maior ou menor que o padrão. O botão **Redefinir padrões** permite redefinir a período de backup para o padrão fornecido durante a configuração inicial do backup.  
  
 Os padrões são:  
  
-   Hora de início de backup: 18h00  
  
-   Hora de término: 9h00  
  
-   Número de dias que os backups diários são mantidos: 5 dias  
  
-   Número de semanas que o último backup da semana será mantido: 4 semanas  
  
-   Número de meses que o último backup do mês será mantido: 6 meses  
  
#### <a name="to-reset-computer-backup-to-the-default-settings"></a>Para redefinir o Backup do computador para as configurações padrão  
  
1.  Abra o **Painel**e, em seguida, abra a página de**Dispositivos** .  
  
2.  Em **Tarefas de dispositivos**, clique em **Personalizar Configurações de Backup do computador e Configurações do Histórico de arquivos**.  
  
    > [!NOTE]
    >  No Windows Server Essentials, clique em **tarefas de backup do computador cliente**.  
  
3.  Na guia **Backup do computador** na página **ferramentas e configurações de backup do computador cliente**, clique em **Redefinir padrões**.  
  
    > [!NOTE]
    >  Procure anotar o agendamento personalizado e as configurações de política de armazenamento porque elas serão excluídas quando você restaurar as configurações padrão.  
  
4.  Clique em **Aplicar**e clique em **OK**.  
  
##  <a name="use-repair-and-recovery-tools"></a><a name="BKMK_7"></a>Usar ferramentas de reparo e recuperação  
 **Reparar backups:** se o banco de dados de backups do computador ficar corrompido ou inutilizável por algum motivo, você poderá tentar reparar o banco de dados usando o Assistente de reparo do banco de dados de backup. O assistente analisa os arquivos de backup para determinar se há problemas que podem ser corrigidos. Em seguida, o assistente tenta corrigir todos os problemas encontrados.  
  
> [!WARNING]
>  Se um problema não pode ser corrigido, o assistente pode excluir permanentemente um arquivo de backup do computador.  
  
 **Recuperação do computador:** você pode criar uma unidade flash USB inicializável para usar para restaurar um computador de um backup existente. Você deve usar uma unidade flash USB de 1 GB ou maior. Depois que a unidade flash USB inicializável for criada, insira-a no computador que você deseja restaurar e, em seguida, inicie a unidade flash USB para iniciar o processo de restauração completa do sistema.  
  
##  <a name="repair-the-backup-database"></a><a name="BKMK_8"></a>Reparar o banco de dados de backup  
 Se você receber um alerta informando que o banco de dados do backup do computador tem problemas, você pode tentar repará-la.  
  
#### <a name="to-repair-the-backup-database"></a>Para reparar o banco de dados de backup  
  
1.  Abra o **Dashboard**.  
  
2.  Clique em **Dispositivos**.  
  
3.  No painel **Tarefas**, clique em **Personalizar Backup do Computador e Configurações do Histórico do Arquivo**.  
  
    > [!NOTE]
    >  No Windows Server Essentials, clique em **tarefas de backup do computador cliente**.  
  
4.  Em **ferramentas e configurações de backup docomputador cliente**, clique na guia **ferramentas**.  
  
5.  Na seção**Reparar backups** , clique em **Reparar agora**. O Assistente de reparo do banco de dados do Backup abre.  
  
6.  Siga as instruções da reparar o Assistente de banco de dados de Backup.  
  
7.  Dependendo do tamanho do banco de dados do backup, reparar o banco de dados pode levar horas. Clique em **Fechar**e clique em **OK** para fechar a página **Personalizar configurações de Backup do computador e histórico de arquivos** .  
  
    > [!NOTE]
    >  No Windows Server Essentials, clique em **configurações e ferramentas de backup do computador cliente**.  
  
#### <a name="to-review-the-results-of-the-backup-database-repair"></a>Para analisar os resultados de reparo do banco de dados do backup  
  
1.  Abra o **Dashboard**.  
  
2.  Clique em **Dispositivos**.  
  
3.  No painel **Tarefas**, clique em **Personalizar Backup do Computador e Configurações do Histórico do Arquivo**.  
  
    > [!NOTE]
    >  No Windows Server Essentials, clique em **tarefas de backup do computador cliente**.  
  
4.  Em **ferramentas e configurações de backup docomputador cliente**, clique na guia **ferramentas**.  
  
5.  Os resultados são exibidos na seção **Reparar backups** .  
  
##  <a name="disable-backup-for-a-computer"></a><a name="BKMK_9"></a>Desabilitar o backup de um computador  
 Use o Painel para desabilitar rapidamente backups de computadores na sua rede.  
  
#### <a name="to-disable-backup-for-a-computer"></a>Para desabilitar o backup de um computador  
  
1. Abra o **Dashboard**.  
  
2. Clique na guia **Dispositivos**.  
  
3. Clique no nome do computador para o qual você deseja desabilitar backups.  
  
4. No painel de tarefas, clique em **personalizar Backup para o computador**. O Assistente para personalizar o Backup é exibida.  
  
5. Clique em **Desabilitar o backup deste computador** e, em seguida, selecione se você deseja manter ou excluir os arquivos de backup existentes.  
  
6. Clique em **salvar alterações**e clique em **fechar**.  
  
   Para obter informações sobre como habilitar o backup de um computador depois que o backup foi desabilitado, consulte [configurar o backup de um computador cliente](Manage-Client-Computer-Backup-in-Windows-Server-Essentials.md#BKMK_3).  
  
##  <a name="run-the-backup-cleanup-task"></a><a name="BKMK_10"></a>Executar a tarefa de limpeza de backup  
 A tarefa de limpeza backup do computador cliente está agendada para ser executada às 23:59H sempre aos sábados depois que todos os backups forem concluídos. A tarefa de limpeza exclui os backups do cliente computador backup do banco de dados de acordo com a política de amarzenamento de backup. As configurações padrão para a política de armazenamento de backup são:  
  
- Número de dias que os backups diários são mantidos: 5 dias  
  
- Número de semanas que o último backup da semana será mantido: 4 semanas  
  
- Número de meses que o último backup do mês será mantido: 6 meses  
  
  Para obter informações sobre como alterar a política de retenção de backup, consulte [alterar a política de retenção de backup do computador](Manage-Client-Computer-Backup-in-Windows-Server-Essentials.md#BKMK_5).  
  
#### <a name="to-run-the-client-backup-database-cleanup-task"></a>Para executar o cliente a tarefa de limpeza de banco de dados de backup  
  
1.  Faça logon no servidor com sua conta de usuário administrador e senha.  
  
2.  No servidor, abra **Agendador de Tarefas**. Você pode acessar o Agendador de Tarefas do programa de **ferramentas administrativas** console.  
  
3.  No Agendador de Tarefas, expanda **Biblioteca do Agendador de Tarefas**, expanda **Microsoft**, expanda **Windows**e clique em **Windows Server Essentials**.  
  
4.  Clique na tarefa **Limpeza de Backup** e, em seguida, clique em **Executar** no painel de **ações** . O Status mudará para **executando** até que a tarefa seja concluída.  
  
##  <a name="view-alerts-in-the-task-bar-on-a-client-computer"></a><a name="BKMK_11"></a>Exibir alertas na barra de tarefas em um computador cliente  
 Você poderá receber um aviso de backup na barra de tarefas em seu computador pelos seguintes motivos:  
  
-   Um backup é iniciado.  
  
-   Um backup terminou.  
  
-   O backup recente não foi completado com sucesso. O número de dias desde o último backup bem-sucedido fica incluído no aviso.  
  
-    Somente Windows Server Essentials: um novo volume é adicionado ao seu computador. A pessoa que administra sua rede precisa executar o Personalizar Assistente de Backup para incluir ou excluir o volume em backups futuros.  
  
##  <a name="view-backup-status-from-the-launchpad"></a><a name="BKMK_12"></a>Exibir status de backup no Launchpad  
 Use a barra inicial para exibir o status de backup rápido para o seu computador.  
  
> [!TIP]
>  Para ver o status, mova o cursor sobre o ícone da barra inicial na área de notificação da área de trabalho. O status do backup aparece na dica de ferramenta.  
  
#### <a name="to-view-status-of-a-backup-for-your-computer"></a>Para exibir o status de um backup do computador  
  
1.  Entrar para a **Barra Inicial** com seu nome de usuário e senha.  
  
2.  Clique em **Backup**.  
  
3.  Na caixa de diálogo **Propriedades de Backup**, na seção **Status de Backup**, o status do backup é exibido.  
  
    -   **Não há backups anteriores**: o Backup ainda não foi executado ou o histórico de backup foi excluído.  
  
    -   **Backup pendente**: um backup está aguardando outro processo no servidor ser concluído.  
  
    -   **Conectando**: o backup está se conectando ao servidor.  
  
    -   **Não é possível conectar**: o backup não pode se conectar ao Serviço do provedor de backup do computador cliente do Windows Server.  
  
        > [!NOTE]
        >  No Windows Server Essentials, esse serviço foi renomeado como serviço de gerenciamento de computador cliente do Windows Server Essentials.  
  
    -   **Backup em andamento**: exibe o percentual concluído do backup.  
  
    -   **Backup cancelado**: exibe a data e hora em que o backup foi iniciado se você ou o administrador de rede cancelou o backup.  
  
    -   **Backup bem-sucedido**: exibe a data e hora em que o backup foi iniciado se ele foi concluído com sucesso.  
  
    -   **Backup incompleto**: exibe a data e hora em que o backup foi iniciado se ele não foi concluído com sucesso.  
  
    -   **Backup malsucedido**: exibe a data e hora em que o backup foi iniciado se ele não foi concluído com sucesso.  
  
4.  Clique em **OK** para fechar a caixa de diálogo **Propriedades de Backup**.  
  
##  <a name="stop-a-backup-in-progress-from-the-launchpad"></a><a name="BKMK_13"></a>Parar um backup em andamento no Launchpad  
 Você pode facilmente interromper um backup está em andamento.  
  
#### <a name="to-stop-a-backup-in-progress"></a>Para interromper um backup em andamento  
  
1.  Abra o **barra inicial**.  
  
    > [!NOTE]
    >  Se solicitado, faça logon na **barra inicial** com seu nome de usuário e senha.  
  
2.  Clique em **Backup**.  
  
3.  Na caixa de diálogo **Propriedades de Backup**, na seção **Status de Backup**, o botão **Iniciar backup** muda para **Parar backup** quando o backup está em andamento. Clique em **Parar backup**e clique em **Sim** para confirmar. O backup continuará sendo executado até você clicar em **Sim**.  
  
4.  Quando você interrompe um backup em andamento, o status mudará para **Backup Cancelado** com a data e hora em que o backup foi iniciado. Se, em vez disso, verificar o status de **realizado com sucesso**, **incompleto**, ou **falha**, o último backup já terá sido concluído.  
  
5.  Clique em **OK** para fechar a caixa de diálogo **Propriedades de Backup**.  
  
##  <a name="start-a-backup-from-the-launchpad"></a><a name="BKMK_14"></a>Iniciar um backup no Launchpad  
 Pode acontecer de você desejar fazer backup dos arquivos e pastas anteriores antes do backup regular agendado configurado no servidor. A barra inicial permite iniciar o backup do seu computador manualmente.  
  
#### <a name="to-start-a-backup"></a>Para iniciar um backup  
  
1.  Abra o **barra inicial**.  
  
    > [!NOTE]
    >  Se solicitado, faça logon na **barra inicial** com seu nome de usuário e senha.  
  
2.  Clique em **Backup**.  
  
3.  Na caixa de diálogo **Propriedades do Backup**, na seção **status de Backup**, clique em **Iniciar backup**e, em seguida, clique **OK**.  
  
4.  Digite uma descrição para o backup e, em seguida, clique **OK**. O status mudará para **iniciando backup**, e depois para **Processeando Backup** com uma porcentagem concluída.  
  
5.  Após o início do backup, o botão muda para **Parar backup**. Se o backup está em andamento e você deseja interrompe-lo, clique em **Parar backup**e clique em**Sim**. Quando interromper um backup em andamento, o status mudará para **Backup cancelado** com a data e hora em que o backup foi iniciado.  
  
6.  Quando o backup for concluído com êxito, o status mudará para **Backup realizado com sucesso** com a data e hora em que o backup realizado iniciou.  
  
7.  Clique em **OK** para fechar a caixa de diálogo **Propriedades de Backup**.  
  
##  <a name="how-computer-backup-works"></a><a name="BKMK_15"></a>Como funciona o backup do computador  
 As seguintes ações ocorrem durante o backup diariamente:  
  
-   Contam com computadores da rede até um após o outro.  
  
-   Um backup está em andamento for concluída, mesmo se você fechar a janela de backup.  
  
-   Atualizações do Windows são instaladas e o Windows Server Essentials reinicia (se necessário).  
  
-   No domingo, limpeza de Backup é executado.  
  
> [!IMPORTANT]
>  O Volume Shadow Copy Service (VSS) não oferece suporte à criação de uma cópia fantasma de um volume virtual e o volume de hospedagem do mesmo conjunto istantâneo. O VSS oferece suporte à criação de instantâneos de volumes em um disco rígido virtual (VHD), se o backup do volume virtual for necessário. Para obter mais informações, consulte [Fazendo manutenção e backup de discos rígidos virtuais](https://go.microsoft.com/fwlink/p/?LinkId=256577).  
  
##  <a name="tips-to-help-prevent-data-loss-due-to-corruption-of-the-client-backup-database"></a><a name="BKMK_16"></a>Dicas para ajudar a evitar a perda de dados devido à corrupção do banco de dado de backup do cliente  
 Se o banco de dados de backup do cliente estiver corrompido, você poderá perder dados críticos.  
  
 Para ajudar a evitar a perda de dados devido à corrupção de banco de dados de backup do cliente, considere o seguinte:  
  
-   Habilite um método adicional de backup de banco de dados de backup do cliente. Por exemplo, dependendo da versão do Windows Server Essentials que você está executando, use backup do servidor, backup online ou Backup do Microsoft Azure para fazer backup do banco de dados.  
  
    > [!IMPORTANT]
    >  Certifique-se de que você selecionar para fazer backup de todos os arquivos de **Backup de computadores cliente** pasta.  
  
-   Se você deve restaurar o banco de dados, certifique-se de restaurar todos os arquivos da pasta **Backup de computadores cliente**. Se você não restaurar todos os arquivos, o banco de dados pode se tornar inconsistente.  
  
-   Antes de limpar ou restaurar o banco de dados de backup do cliente, certifique-se de parar todos os backups do cliente que estão atualmente em andamento.  
  
     Se você estiver executando o Windows Server Essentials, também deverá interromper o serviço de backup do computador cliente do Windows Server e o serviço do provedor de backup do computador cliente do Windows Server.  
  
     Se você estiver executando o Windows Server Essentials, também deverá interromper o serviço de backup do computador com Windows Server.  
  
     Depois de concluir a operação de restauração, reinicie o serviço.  
  
##  <a name="restore-files-or-folders-from-a-client-computer-backup"></a><a name="BKMK_17"></a>Restaurar arquivos ou pastas de um computador cliente backup  
 Você pode procurar e restaurar arquivos e pastas individuais de um backup.  
  
> [!NOTE]
>  Não é possível restaurar arquivos e pastas em um computador cliente usando o Painel do servidor. Abra o Painel em um computador cliente para concluir a tarefa.  
  
> [!IMPORTANT]
>  Não é possível restaurar arquivos e pastas em um disco rígido que tenha sido configurado para ser um disco dinâmico.  
  
#### <a name="to-restore-files-and-folders"></a>Para restaurar arquivos e pastas  
  
1.  Abra o **Painel** em um computador cliente.  
  
2.  Clique na guia **Dispositivos**.  
  
3.  Clique no computador para o qual você deseja restaurar arquivos e pastas e, em seguida, clique em **restaurar arquivos ou pastas para o computador** no **tarefas** painel. O Assistente de Restauração de Arquivos ou Pastas é aberto.  
  
4.  Siga as instruções no assistente.  
  
##  <a name="understanding-file-history"></a><a name="BKMK_FileHistory"></a>Compreendendo o histórico de arquivos  
 O recurso de histórico de arquivos do Windows Server Essentials automaticamente faz backup dos arquivos que estão em pastas de bibliotecas, contatos, área de trabalho e favoritos dos computadores da rede que têm o recurso de histórico de arquivos. Se os originais forem perdidos, danificados ou excluídos, você pode restaurá-los. Você também pode encontrar versões diferentes dos arquivos em períodos determinados de tempo. Ao longo do tempo, você terá um histórico completo dos seus arquivos.  
  
 No Windows Server Essentials, você pode personalizar as configurações de histórico de arquivos na página **histórico de arquivos** de **configurações e ferramentas de backup do computador cliente**.  
  
 No Windows Server Essentials, você pode personalizar as configurações do histórico de arquivos acessando a guia **usuários** e, em seguida, selecionando a tarefa **alterar a configuração do histórico de arquivos** .  
  
 A página de histórico de arquivos fornece as seguintes opções:  
  
|Configuração de backup|Padrão|Descrição|  
|--------------------|-------------|-----------------|  
|Ativar / desativar|Ativado|O Histórico de arquivos é ativado por padrão quando você instala o Windows Server Essentials.|  
|Dados de backup|Documentos e área de trabalho|Existem três configurações pré-configuradas, que permitem que você especifique uma variedade de soluções de backup. Você pode escolher uma das seguintes opções:<br /><br /> -Documentos e área de trabalho<br /><br /> -Todas as bibliotecas, área de trabalho, contatos e favoritos<br /><br /> -Todos os dados em bibliotecas, área de trabalho, contatos e favoritos, excluindo dados nas bibliotecas música, vídeo e imagens|  
|Frequência de backup|Cada hora|Especifica a frequência com que o histórico de arquivos cria um backup dos dados selecionados. Você pode escolher entre várias opções que variam de a cada 10 minutos para diariamente.|  
|Armazenar cópias por|1 ano|Especifica a quantidade de tempo que o Histórico de arquivos armazenará a cópia de um backup.|  
  
 Para obter informações sobre problemas com o Histórico de arquivos, consulte [Solução de problemas do Histórico de arquivos](../support/Troubleshoot-File-History-in-Windows-Server-Essentials.md).  
  
## <a name="see-also"></a>Consulte também  
  
-   [Gerenciar backup e restauração](Manage-Backup-and-Restore-in-Windows-Server-Essentials.md)  
  
-   [Gerenciar o Windows Server Essentials](Manage-Windows-Server-Essentials.md)  
  
-   [Utilizar o Windows Server Essentials](../use/Use-Windows-Server-Essentials.md)
