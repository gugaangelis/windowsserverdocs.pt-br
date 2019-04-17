---
title: Gerenciar Online Backup no Windows Server Essentials
description: Descreve como usar o Windows Server Essentials
ms.custom: na
ms.date: 10/03/2016
ms.prod: windows-server-2016-essentials
ms.reviewer: na
ms.suite: na
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 95a9f593-fad7-4335-bd4d-c7bb8c033efb
author: nnamuhcs
ms.author: coreyp
manager: dongill
ms.openlocfilehash: 37d59d500a2de1e2b98c848e7484ae13639d09b7
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 12/12/2017
---
# <a name="manage-online-backup-in-windows-server-essentials"></a>Gerenciar Online Backup no Windows Server Essentials

>Aplica-se a: Windows Server 2016 Essentials, Windows Server 2012 R2 Essentials, Windows Server 2012 Essentials
  
 Depois de integrar com o Backup do Microsoft Azure, o **Online Backup** página Gerenciamento aparece no painel Windows Server Essentials. O **Online Backup** página torna possível realizar tarefas administrativas comuns. Os recursos na página de Backup Online incluem:  
  
-   As páginas de subseção seguintes:  
  
    -   **Backup online** depois de registrar o servidor de backup online, esta seção exibe o status atual do backup, status de armazenamento e as informações da conta.  
  
    -   **Pastas protegidos** esta seção lista todas as pastas compartilhadas e todas as pastas de histórico de arquivos no servidor e outras pastas que você escolheu para fazer backup no Azure. A lista inclui o nome da pasta, o caminho da pasta e o status para cada pasta compartilhada.  
  
    -   **Cujo histórico de backup** esta seção exibe uma lista de backups on-line recentes. A lista inclui o tipo de operação e o tempo e o status para cada backup.  
  
-   Um painel de detalhes com informações adicionais sobre um trabalho de backup selecionado ou um trabalho de restauração.  
  
-   Um painel de tarefas que inclui um conjunto de tarefas administrativas que você pode executar.  
  
 Como prática recomendada, você deve fazer backup dos dados de negócios e usuário mais importantes. Por exemplo, você deve fazer backup de pastas de servidor que contêm arquivos de dados críticos. Você também deve fazer backup do histórico de arquivos para computadores de rede que contêm informações essenciais.  
  
 Ao decidir quais dados devem ser backup online, considere a política de frequência e a retenção de backup que você deseja implementar. Em seguida, analisar seu orçamento e determinar a quantidade de espaço de armazenamento, que você pode pagar. Equilibrar o custo e o volume de armazenamento em relação às suas necessidades e, em seguida, configure o backup online para proteger o máximo possível de seus dados importantes. Para informações sobre preços, consulte [detalhes de preços para Azure Backup](https://azure.microsoft.com/pricing/details/backup/).  
  
 As seções a seguir descrevem várias tarefas de backup online que podem aparecer no painel Windows Server Essentials.  
  
## <a name="online-backup-tasks-in-the-dashboard"></a>Tarefas de backup online no painel  
  
-   [Carregar um certificado para o Cofre de Backup do Azure](Manage-Online-Backup-in-Windows-Server-Essentials.md#BKMK_1)  
  
-   [Configurar o backup online](Manage-Online-Backup-in-Windows-Server-Essentials.md#BKMK_2)  
  
-   [Inicie um backup online](Manage-Online-Backup-in-Windows-Server-Essentials.md#BKMK_3)  
  
-   [Restaurar arquivos e pastas de um backup online](Manage-Online-Backup-in-Windows-Server-Essentials.md#BKMK_4)  
  
-   [Registre esse servidor para backup](Manage-Online-Backup-in-Windows-Server-Essentials.md#BKMK_5)  
  
-   [Cancelar o registro de servidor](Manage-Online-Backup-in-Windows-Server-Essentials.md#BKMK_6)  
  
###  <a name="BKMK_1"></a>Carregar um certificado para o Cofre de Backup do Azure  
 Antes que você pode usar o Backup do Azure para backups online no Windows Server Essentials, você deve carregar um certificado público para registrar com o Cofre de backup. O certificado é usado para autenticar a implantação de Backup do Azure (o agente), atuando em nome do proprietário de assinatura do Microsoft Online Services para gerenciar os recursos associados a assinatura.  
  
> [!NOTE]
>  Antes de carregar o certificado, você deve concluir os procedimentos [Inscreva-se para o serviço de Backup do Azure](Manage-Online-Backup-in-Windows-Server-Essentials.md#BKMK_16).  
  
##### <a name="to-upload-a-certificate-to-use-with-the-azure-backup-service"></a>Para carregar um certificado para usar com o serviço de Backup do Azure  
  
1.  Faça logon no painel Windows Server Essentials com uma conta de administrador.  
  
2.  No painel **Home** página, clique em **ONLINE BACKUP**.  
  
3.  No **ONLINE BACKUP** área, clique em **certificado de carregamento ao Cofre de Backup do Azure**.  
  
     Isso abre **serviços de recuperação** no Portal de gerenciamento do Azure. Se você não estiver t já entrou Azure, você precisará entrar usando sua conta da Microsoft.  
  
4.  Clique no nome do Cofre de backup que você usará para backups online para abrir o **início rápido** página para o Cofre de backup.  
  
5.  Clique em **gerenciar certificados**.  
  
6.  No **gerenciar certificados** caixa de diálogo, colar o caminho do certificado público gerados pelo Windows Server Essentials. Para obter o caminho do certificado público, faça o seguinte:  
  
    1.  No painel Windows Server Essentials, clique no **Online Backup** guia.  
  
    2.  Sobre o **Online Backup** página, copie o caminho do certificado gerado.  
  
    3.  Alterne para o Portal de gerenciamento do Azure e, em seguida, no **gerenciar certificados** caixa de diálogo caixa, cole o caminho para carregar o certificado público gerado.  
  
    > [!NOTE]
    >  Você também pode usar seu próprio certificado público. Para saber qual certificado for necessário, pela **início rápido** página, clique no **adquirir certificado** link.  
  
    > [!NOTE]
    >   Azure requer um certificado de v2 x. 509 com uma chave pública. Para obter mais informações, consulte [gerenciar certificados de cofre](https://msdn.microsoft.com/library/azure/dn169036.aspx).  
  
7.  Depois de selecionar o certificado, clique em **Okey** (marca de seleção).  
  
8.  Você é retornado para o **início rápido** página. Clique em **painel**e verifique se o certificado foi carregado com êxito. Depois que o certificado for carregado com êxito, o painel exibe a data de expiração e de impressão digital do certificado.  
  
 Depois de concluir este procedimento, faça o seguinte:  
  
1.  Registre o servidor com o serviço de Backup do Azure. Para obter mais informações, consulte [registrar esse servidor para backup](Manage-Online-Backup-in-Windows-Server-Essentials.md#BKMK_5).  
  
2.  Configure online backup do servidor. Para obter mais informações, consulte [backup online configurar](Manage-Online-Backup-in-Windows-Server-Essentials.md#BKMK_2).  
  
###  <a name="BKMK_2"></a>Configurar o backup online  
 Depois de registrar o servidor com o Backup do Azure, você pode definir as configurações de backup online no Windows Server Essentials.  
  
##### <a name="to-configure-online-backup"></a>Para configurar backup online  
  
1.  A partir do Assistente para registrar o servidor ou do **Online Backup** página do painel Windows Server Essentials, clique em **backup online configurar**. O Assistente de configuração de Backup Online é exibida.  
  
2.  Sobre o **Configurar Backup Online** página do assistente, selecione a caixa de seleção para cada pasta de servidor que você deseja fazer backup de Backup do Azure. Desmarque a caixa de seleção para cada pasta de servidor que não deseja incluir no backup. Para adicionar pastas que não são mostradas na lista, clique em **adicionar pastas**. Quando você terminar sua seleção, clique em **próxima**.  
  
    > [!NOTE]
    >  Você não pode selecionar uma pasta que não esteja localizada no servidor local ou uma pasta que esteja em uma unidade formatada como ReFS.  
  
3.  Sobre o **adicionar o arquivo de histórico de Backups** página do assistente, você pode optar por incluir Backups de histórico de arquivos para computadores de rede que dão suporte ao recurso de histórico de arquivos. Clique em **próxima** para continuar.  
  
4.  Sobre o **especificar o agendamento de Backup** página do assistente, configure o seguinte e, em seguida, clique em **próxima** para continuar:  
  
    -   Selecione ou aceite o horário do dia quando você deseja executar o backup online. O tempo padrão é 10:00 PM. Você também pode especificar um tempo para executar um segundo backup.  
  
    -   Selecione ou aceitar os dias quando quiser que o backup online para ser executado. Por padrão, o backup on-line é executado na segunda a sexta.  
  
5.  Sobre o **especificar a política de retenção de Backup** página do assistente, selecione o número de dias que você deseja manter backups on-line e, em seguida, clique em **próxima**. O padrão é 7 dias. Você também pode optar por manter backups on-line para 15 ou 30 dias.  
  
    > [!NOTE]
    >   Backup do Azure sempre retém seu backup mais recente. Se o destino do backup não tiver espaço suficiente armazenar o backup, o processo de backup não serão bem-sucedidas. Para evitar essa situação, espaço de armazenamento adicional de compra ou reduza o período de retenção de dados. Para informações sobre preços, consulte [detalhes de preço](https://azure.microsoft.com/pricing/details/backup/) para Backup do Microsoft Azure.  
  
6.  Sobre o **escolher o uso de largura de banda** página do assistente, se você quiser restringir a quantidade de largura de banda de Internet que é alocada para fazer backup online, selecione **uso de largura de banda de Internet habilitar**. Use as opções na página especificar quanta largura de banda de Internet para permitir que o backup online usar durante o trabalho horas e durante o trabalho não horas. Em seguida, defina o horário comercial e dias úteis.  
  
    > [!NOTE]
    >   Backup do Azure permite que você personalize como o software de integração utiliza a largura de banda de rede para backup ou restauração de informações. Usando uma técnica conhecida como limitação, você pode controlar a quantidade de largura de banda de rede que está disponível para uso pelo backup e restaurar processos durante um dia específico e intervalos de tempo. Depois de selecionar o **uso de largura de banda de Internet habilitar** caixa de seleção no Assistente para configurar Backup Online, você pode especificar as horas de trabalho durante o qual deseja aplicar o limite de largura de banda de horas de trabalho e dias úteis. O limite do horário de trabalho não é usado em todos os outros vezes. Intervalos de largura de banda válidos são de 256 Kbps a ilimitado para ambos os limites.  
  
7.  Quando a configuração de backup online é concluída, clique em **fechar**.  
  
    > [!TIP]
    >  Depois de uma configuração de backup bem-sucedido, o **Online Backup** página mostra o status de backup online mais recente e a quantidade de espaço de armazenamento usado pelo Cofre de backup. Para ver o status de backups anteriores, clique em **histórico de Backup**.  
  
###  <a name="BKMK_3"></a>Inicie um backup online  
  
> [!NOTE]
>  Antes de iniciar um backup online, você deve primeiro [registrar esse servidor para backup](Manage-Online-Backup-in-Windows-Server-Essentials.md#BKMK_5)e depois [backup online configurar](Manage-Online-Backup-in-Windows-Server-Essentials.md#BKMK_2).  
  
##### <a name="to-start-an-online-backup-immediately"></a>Para iniciar um backup online imediatamente  
  
1.  Faça logon no painel como administrador.  
  
2.  Na barra de navegação, clique em **ONLINE BACKUP**.  
  
3.  No **tarefas de Backup Online** painel, clique em **Iniciar backup agora**.  
  
###  <a name="BKMK_4"></a>Restaurar arquivos e pastas de um backup online  
 Restaurar arquivos e pastas assistente orienta você pelo processo de localização, selecionando e restaurando arquivos e pastas que são armazenadas em backup online. À medida que você avança através do assistente, você executará as seguintes tarefas:  
  
1.  **Escolha um backup online do qual restaurar arquivos e pastas**  
  
     Quando você executa o Assistente de pastas e restaurar arquivos, a primeira coisa que você deve fazer é especificar se você quer restaurar arquivos de um backup online do servidor que você está conectado no momento, ou de um backup de um servidor diferente.  
  
2.  **Escolha um servidor do qual restaurar arquivos e pastas**  
  
     Se você optar por restaurar arquivos e pastas de um servidor diferente, selecione o servidor que você deseja restaurar a partir de na lista de servidores disponíveis e clique em **próxima**.  
  
3.  **Escolha um volume que contém os arquivos e pastas a serem restauradas**  
  
     Backups on-line contêm volumes que correspondem aos volumes no servidor de origem de backup. Depois de selecionar o volume, selecione a data e hora do backup do qual restaurar e, em seguida, clique em **próxima**.  
  
4.  **Selecione arquivos e pastas a serem restaurados**  
  
     Sobre o **selecionar itens para restaurar** página do assistente, você pode abrir vários locais de pasta e selecione a caixa de seleção para cada arquivo ou pasta que você quer restaurar. Quando tiver terminado de selecionar arquivos, clique em **próxima**.  
  
5.  **Especifique o local de destino para arquivos e pastas restaurados**  
  
     O **especificar opções de restauração** página do assistente permite que você especifique onde restaurar arquivos e pastas. Há duas opções:  
  
    -   **Restaure para o local original**. Selecione essa opção para restaurar arquivos e pastas para o local exato que originou o backup.  
  
    -   **Restaurar em um local diferente**.  Escolha esta opção para especificar um local diferente no servidor em que deseja restaurar os arquivos.  
  
         A instalação padrão, se já existir um arquivo com o mesmo nome de um arquivo de restauração no local de destino, o assistente cria uma cópia do arquivo original. Além disso, a lista de controle de acesso (ACL) é atualizada para refletir as permissões de arquivo atual. Você pode clicar em **avançado** para personalizar as configurações padrão.  
  
6.  **Confirme as informações de restauração**  
  
     O **confirmar suas informações de restauração** página fornece um resumo das instruções restauração que você especificou. Para continuar com a restauração de arquivos, você deve digitar a senha correta para sua conta de backup online.  
  
###  <a name="BKMK_5"></a>Registre esse servidor para backup  
 Para fazer backup ou restaurar arquivos, pastas e histórico de arquivos em seu servidor Windows Server Essentials para fazer Backup do Azure, primeiro você deve registrar seu servidor com o serviço de Backup do Microsoft Azure.  
  
> [!NOTE]
>  Antes de registrar seu servidor, você deve carregar um certificado para usar com o Cofre de backup que armazenará os backups online. Para obter mais informações, consulte [carregar um certificado para o Cofre de Backup do Azure](Manage-Online-Backup-in-Windows-Server-Essentials.md#BKMK_1).  
  
##### <a name="to-register-your-server-with-azure-backup"></a>Para registrar seu servidor com o Backup do Azure  
  
1.  Fazer logon no servidor como administrador e abra o painel.  
  
2.  Na barra de navegação, clique em **ONLINE BACKUP**.  
  
3.  No **Online Backup** subseção, clique em **registrar**.  
  
4.  Escolha o certificado que você deseja usar com o Cofre de backup para seus backups online. O certificado padrão é selecionado por padrão; Se você deseja escolher um certificado diferente, use **procurar**. Clique em **próxima**.  
  
5.  Siga as instruções no Assistente para criar uma senha e, em seguida, concluir o registro.  
  
###  <a name="BKMK_6"></a>Cancelar o registro de servidor  
  
> [!CAUTION]
>  Se você cancelar o registro de seu servidor Windows Server Essentials do serviço de Backup do Microsoft Azure, Azure Backup não é mais possível fazer backup do servidor. Além disso, os dados do servidor que foi carregados anteriormente serão apagados. Para retomar backups on-line, você deve registrar o servidor novamente.  
  
##### <a name="to-unregister-your-server-with-azure-backup"></a>Para cancelar o registro de seu servidor com o Backup do Azure  
  
1.  Faça logon no [Portal de gerenciamento do Azure](https://manage.windowsazure.com).  
  
2.  Clique em **serviços de recuperação**.  
  
3.  Em **serviços de recuperação**, clique no nome do Cofre de backup.  
  
4.  Do **início rápido** para o Cofre de página, clique em **servidores**.  
  
5.  Selecione o servidor, clique em **excluir**e clique em **Sim** no prompt de confirmação.  
  
## <a name="related-tasks"></a>Tarefas relacionadas  
  
-   [Alterar a política de backup online](Manage-Online-Backup-in-Windows-Server-Essentials.md#BKMK_7)  
  
-   [Uso de armazenamento de backup online do modo de exibição](Manage-Online-Backup-in-Windows-Server-Essentials.md#BKMK_8)  
  
-   [Incluir uma nova pasta no backup online](Manage-Online-Backup-in-Windows-Server-Essentials.md#BKMK_9)  
  
-   [Remover ou excluir backups de histórico de arquivos da diretiva de backup online](Manage-Online-Backup-in-Windows-Server-Essentials.md#BKMK_10)  
  
-   [Desabilite ou habilite novamente o backup do servidor online](Manage-Online-Backup-in-Windows-Server-Essentials.md#BKMK_11)  
  
-   [Parar um backup do servidor online em andamento](Manage-Online-Backup-in-Windows-Server-Essentials.md#BKMK_12)  
  
-   [Exibir status de backup online](Manage-Online-Backup-in-Windows-Server-Essentials.md#BKMK_13)  
  
-   [Exibir e gerenciar os alertas de backup online](Manage-Online-Backup-in-Windows-Server-Essentials.md#BKMK_14)  
  
-   [Restaurar backup online para as configurações padrão](Manage-Online-Backup-in-Windows-Server-Essentials.md#BKMK_15)  
  
-   [Inscreva-se para o serviço de Backup do Azure](Manage-Online-Backup-in-Windows-Server-Essentials.md#BKMK_16)  
  
-   [Integrar o Backup do Azure com o Windows Server Essentials](Manage-Online-Backup-in-Windows-Server-Essentials.md#BKMK_17)  
  
-   [Proteger pastas para backup online no Windows Server Essentials](Manage-Online-Backup-in-Windows-Server-Essentials.md#BKMK_18)  
  
-   [Histórico de backup online no Windows Server Essentials](Manage-Online-Backup-in-Windows-Server-Essentials.md#BKMK_19)  
  
###  <a name="BKMK_7"></a>Alterar a política de backup online  
 É fácil fazer alterações na diretiva de backup online usando o painel do Windows Server Essentials.  
  
##### <a name="to-change-the-online-backup-policy"></a>Para alterar a política de backup online  
  
1.  Faça logon no painel como administrador.  
  
2.  Na barra de navegação, clique em **Online Backup**.  
  
3.  No **tarefas de Backup Online** painel, clique em **backup online configurar**.  
  
4.  Siga as instruções no Assistente para personalizar a política de backup online.  
  
 Para obter informações sobre as configurações que você pode personalizar mais detalhadas, consulte [backup online configurar](Manage-Online-Backup-in-Windows-Server-Essentials.md#BKMK_2).  
  
###  <a name="BKMK_8"></a>Uso de armazenamento de backup online do modo de exibição  
  
##### <a name="to-view-the-amount-of-storage-space-that-online-backup-uses"></a>Para ver a quantidade de espaço de armazenamento que usa Online Backup  
  
1.  Faça logon no painel como administrador.  
  
2.  Na barra de navegação, clique em **ONLINE BACKUP**.  
  
3.  Sobre o **Online Backup** guia, o **Status de armazenamento** aparece no painel de informações.  
  
###  <a name="BKMK_9"></a>Incluir uma nova pasta no backup online  
  
##### <a name="to-include-a-new-folder-in-the-online-backup-policy"></a>Para incluir uma nova pasta na política de backup online  
  
1.  Faça logon no painel como administrador.  
  
2.  Na barra de navegação, clique em **Online Backup**.  
  
3.  No **tarefas de Backup Online** painel, clique em **backup online configurar**.  
  
4.  Sobre o **alterar ou remover a política de Backup** página, clique em **personalizar a política de Backup Online**.  
  
5.  Sobre o **Configurar Backup Online** página, se a pasta que você deseja incluir não aparecer na lista, clique em **adicionar pastas**.  
  
6.  No **adicionar pastas** janela, procure e selecione a pasta que você deseja incluir no backup online e, em seguida, clique em **Okey**.  
  
7.  Sobre o **Configurar Backup Online**, selecione outras pastas para incluir conforme desejado e clique em **próxima**.  
  
8.  Siga as instruções no Assistente para concluir a personalização a política de backup online.  
  
 Para obter informações sobre outras configurações que você pode personalizar mais detalhadas, consulte [backup online configurar](Manage-Online-Backup-in-Windows-Server-Essentials.md#BKMK_2).  
  
###  <a name="BKMK_10"></a>Remover ou excluir backups de histórico de arquivos da diretiva de backup online  
  
##### <a name="to-remove-or-exclude-a-folder-from-the-online-backup-policy"></a>Para remover ou excluir uma pasta da diretiva de backup online  
  
1.  Faça logon no painel como administrador.  
  
2.  Na barra de navegação, clique em **Online Backup**.  
  
3.  Clique no **pastas protegidas** guia. O painel de detalhes, exibe uma lista de pastas com seu status de backup online.  
  
4.  Selecione a pasta que você deseja excluir da diretiva de backup online e no painel de tarefas, clique em **remover a pasta de backup online**.  
  
###  <a name="BKMK_11"></a>Desabilite ou habilite novamente o backup do servidor online  
 Para obter instruções sobre como usar o Backup do Azure para backup ou restauração de dados do servidor, veja [registrar esse servidor para backup](Manage-Online-Backup-in-Windows-Server-Essentials.md#BKMK_5).  
  
 Para obter instruções sobre como parar usando o Backup do Azure para backup ou restauração de dados do servidor, veja [Unregister servidor](Manage-Online-Backup-in-Windows-Server-Essentials.md#BKMK_6).  
  
###  <a name="BKMK_12"></a>Parar um backup do servidor online em andamento  
  
##### <a name="to-stop-an-online-server-backup-in-progress"></a>Para parar de um backup do servidor online em andamento  
  
1.  Faça logon no painel como administrador.  
  
2.  Na barra de navegação, clique em **ONLINE BACKUP**.  
  
3.  No **tarefas de Backup Online** painel, clique em **parar o backup**.  
  
     Depois que você parar o backup, o status de **Canceled** aparece para o backup no **histórico de Backup** lista.  
  
###  <a name="BKMK_13"></a>Exibir status de backup online  
  
##### <a name="to-view-the-backup-status"></a>Para exibir o status de backup  
  
1.  Faça logon no painel como administrador.  
  
2.  Na barra de navegação, clique em **ONLINE BACKUP**.  
  
3.  Clique no **histórico de Backup** guia. O modo de exibição de lista exibe o status de cada trabalho de backup. Selecione um trabalho de backup para ver mais detalhes sobre esse trabalho.  
  
###  <a name="BKMK_14"></a>Exibir e gerenciar os alertas de backup online  
 Como muitos outras alertas, alertas para Backup do Azure são exibidos no Visualizador de alerta.  
  
##### <a name="to-view-online-backup-alerts-in-the-alert-viewer"></a>Para exibir alertas de backup online no Visualizador de alerta  
  
1.  Abra o painel.  
  
2.  Siga um destes procedimentos:  
  
      Windows Server Essentials: No painel de navegação, clique no ícone de alertas \ (pode ser crítico, aviso ou Informational\). Isso abre o Visualizador de alerta.  
  
      Windows Server Essentials: Sobre o **Home** página, clique no **monitoramento de integridade** guia.  
  
3.  Examine a lista de alertas para problemas relacionados ao Backup do Azure.  
  
 Para obter mais informações sobre como usar a guia Visualizador de alerta ou monitoramento de integridade para gerenciar alertas, consulte [gerenciar a integridade do sistema](Manage-System-Health-in-Windows-Server-Essentials.md).  
  
###  <a name="BKMK_15"></a>Restaurar backup online para as configurações padrão  
 Windows Server Essentials fornece um assistente que ajuda você a configurar as configurações de backup online. Se você quiser restaurar as configurações padrão, execute o **backup online configurar** de tarefas e escolha o **remover a política de backup online** opção. Em seguida, execute o **backup online configurar** de tarefas novamente. Seus dados anteriormente atualizados permanecem inalterados.  
  
###  <a name="BKMK_16"></a>Inscreva-se para o serviço de Backup do Azure  
 Para preparar para integrar o Microsoft Azure Backup com o Windows Server Essentials, você vai fazer logon no Portal de gerenciamento do Azure com sua conta da Microsoft Online Services e, em seguida, crie um backup cofre para armazenar seus backups online no Azure. Você vai baixar o módulo de integração de Backup do Azure e usar o arquivo baixado para instalar o Backup do Azure Add-In em seu servidor Windows Server Essentials. Se você não tiver uma conta da Microsoft, você pode se inscrever para uma avaliação gratuita.  
  
 Para fazer essa configuração, conclua as seguintes tarefas:  
  
1.  Inscreva-se para uma conta da Microsoft Online Services e a visualização de Backup.  
  
2.  Crie um backup cofre para armazenar backups on-line.  
  
3.  Baixe o agente de Backup do Azure.  
  
4.  Instale o Backup do Azure Add-In no servidor.  
  
####  <a name="BKMK_SignupforaMicrosoftOnlineServiceAccount"></a>Inscreva-se para uma conta da Microsoft Online Services e a visualização de Backup  
  
1.  Logon do painel do Windows Server Essentials.  
  
2.  No painel **Home** página, clique no **ADD-INS** categoria, clique em **integrar com o Backup do Azure**e clique em **clique em Inscrever-se para o Backup do Azure**.  
  
3.  Sobre o Azure **serviços de recuperação** página, o **Backup (visualização)** seção, examine os detalhes.  
  
4.  Se você não tiver uma assinatura do Azure, clique em **avaliação gratuita**e siga as instruções para obter uma assinatura do Azure.  
  
     Se você já tiver uma assinatura do Azure, clique em **Portal** no canto superior direito da página da web para ir para o Portal de gerenciamento do Azure.  
  
5.  Na página de Portal de gerenciamento do Azure, você verá **serviços de recuperação** no painel esquerdo. Esse s onde você ll gerenciar o backup vaults loja seus backups online do Windows Server Essentials.  
  
####  <a name="BKMK_Createabackupvaulttostoreonlinebackups"></a>Criar um backup cofre para armazenar backups on-line  
  
1.  Faça logon no [Portal de gerenciamento do Azure](https://manage.windowsazure.com)do navegador da web em seu servidor Windows Server Essentials.  
  
2.  No Portal de gerenciamento do Azure, clique em **nova**, clique em **serviços de dados**, clique em **serviços de recuperação**, clique em **Backup cofre**e clique em **criação rápida**.  
  
3.  Insira um nome para o Cofre de backup, escolha a região onde deseja armazenar seus backups e, em seguida, clique em **criação rápida**.  
  
     O **serviços de recuperação** área abre com o novo cofre backup exibido.  
  
####  <a name="BKMK_DownloadtheWindowsAzureBackupAgent"></a>Baixar o agente de Backup Azure  
  
1.  Abra o painel do Windows Server Essentials.  
  
2.  No painel **Home** página, clique no **começar**, clique no **ADD-INS** categoria, clique em **integrar com o Backup do Azure**e clique em **clique aqui para baixar o módulo de integração do Azure Backup**.  
  
####  <a name="BKMK_InstalltheWindowsAzureBackupAddIn"></a>Instalar o Backup do Azure Add-In no servidor  
  
1.  Faça logon servidor usando uma conta de administrador e execute o **OnlineBackupAddin.wssx** arquivo que você baixou na etapa anterior.  
  
2.  O **termos de licença para Software** são exibidos. Se você concorda com os termos e condições, clique em **aceitar** para continuar a instalação.  
  
3.  Sobre o **instalar o suplemento** página, clique em **instalar o suplemento**.  
  
    > [!NOTE]
    >  Você pode ser solicitado a reiniciar o servidor para instalar qualquer software obrigatório.  
  
     O **instalação** página é exibida. Um indicador de progresso exibe quando a instalação inicia e mostra o progresso da instalação. Quando a instalação for concluída, você receberá uma mensagem de que o Backup do Azure Add-in foi instalado com êxito.  
  
4.  Clique em **concluir **.  
  
5.  Feche e reabra o painel.  
  
     Uma nova guia, **Online Backup**, é adicionado ao painel. Nessa guia, você pode registrar seu servidor, definir configurações de backup e abra **serviços de recuperação** no Portal de gerenciamento do Azure para gerenciar os backup cofres para seus servidores.  
  
 Depois de concluir essas etapas, faça o seguinte:  
  
1.  No Windows Server Essentials, carrega um certificado a ser usado para backups on-line. Para obter mais informações, consulte [carregar um certificado para o Cofre de Backup do Azure](Manage-Online-Backup-in-Windows-Server-Essentials.md#BKMK_1).  
  
2.  Registre o servidor com o Cofre de Backup do Azure. Para obter mais informações, consulte [registrar esse servidor para backup](Manage-Online-Backup-in-Windows-Server-Essentials.md#BKMK_5).  
  
3.  Configure online backup do servidor. Para obter mais informações, consulte [backup online configurar](Manage-Online-Backup-in-Windows-Server-Essentials.md#BKMK_2).  
  
###  <a name="BKMK_17"></a>Integrar o Backup do Azure com o Windows Server Essentials  
 Módulo de integração do Microsoft Azure Backup é um novo recurso do Windows Server Essentials que permite criptografar e fazer backup de arquivos e pastas do seu servidor para um sistema de armazenamento hospedados pelo Azure fornecido pela Microsoft. Usando o Backup do Azure para criptografar e fazer backup dos dados no servidor, você pode ajudar a evitar a perda de catastrófica de dados comerciais essenciais devido a fogo, inundação, roubo ou outros desastres. Quando você usa o Backup do Azure para fazer backup dos dados do servidor, as informações são criptografadas usando sua senha antes que ela é carregada em um datacenter seguro na Internet. Para acessar dados de um online backup, você deve ter um servidor é autenticado por um certificado e deve fornecer a senha.  
  
 Depois de integração e registrando o servidor com o Backup do Azure, você pode configurar as configurações de backup online para fazer backups regular agendados. Você também pode iniciar um backup online a qualquer momento clicando o **Iniciar backup agora** tarefa no painel de Backup Online.  
  
 A configuração de backup online envolve estas etapas:  
  
1.  [Inscreva-se para o serviço de Backup do Azure](Manage-Online-Backup-in-Windows-Server-Essentials.md#BKMK_16) - depois que você entrar pronto para o serviço de Backup, você vai criar um backup cofre, baixar e instalar o módulo de integração do Backup do Azure.  
  
2.  [Carregar um certificado para o Cofre de Backup do Azure](Manage-Online-Backup-in-Windows-Server-Essentials.md#BKMK_1)  
  
3.  [Registre esse servidor para backup](Manage-Online-Backup-in-Windows-Server-Essentials.md#BKMK_5)  
  
4.  [Configurar o backup online](Manage-Online-Backup-in-Windows-Server-Essentials.md#BKMK_2)  
  
> [!NOTE]
>   Backup do Azure usa a senha para criptografar arquivos e pastas para o backup online. Alterar a senha de criptografia substituirá a senha que você especificou ao se registrar com o servidor. A senha aceita apenas caracteres ASCII embutidas em código.  
  
###  <a name="BKMK_18"></a>Proteger pastas para backup online no Windows Server Essentials  
 O **pastas protegidas** subseção na seção de Backup Online do painel exibe uma lista de todas as pastas compartilhadas no servidor. A tabela a seguir descreve as informações que estão incluídas na lista.  
  
|Coluna|Descrição|  
|------------|-----------------|  
|**Nome de pasta:**|O nome da pasta que está incluído no backup online.<br /><br /> Para adicionar ou excluir uma pasta, execute o **backup online configurar** tarefa.|  
|**Caminho da pasta:**|O local da pasta.|  
|**Status:**|Há três tipos de status **protegidos**, **não protegidas**, e **desconhecido**.|  
  
###  <a name="BKMK_19"></a>Histórico de backup online no Windows Server Essentials  
 O **histórico de Backup** subseção na seção de Backup Online do painel exibe uma lista de backups on-line recentes. Você pode usar backups bem-sucedidos para restaurar arquivos e pastas. A tabela a seguir descreve as informações que estão incluídas na lista.  
  
|Coluna|Descrição|  
|------------|-----------------|  
|**Operação:**|Existem dois tipos de operações - **Backup** e **restaurar**.|  
|**Tempo:**|Este é o momento registrado para o status mais recente.|  
|**Status:**|Há cinco tipos de status **sucesso**, **em andamento**, **Canceled**, **aviso**, e **tentativas**.|  
  
## <a name="see-also"></a>Consulte também  
  
-   [Gerenciar o Backup e restauração](Manage-Backup-and-Restore-in-Windows-Server-Essentials.md)  
  
-   [Gerenciar Serviços Online da Microsoft](Manage-Microsoft-Online-Services-in-Windows-Server-Essentials.md)  
  
-   [Gerenciar o Windows Server Essentials](Manage-Windows-Server-Essentials.md)  
  
-   [Usar o Windows Server Essentials](../use/Use-Windows-Server-Essentials.md)
