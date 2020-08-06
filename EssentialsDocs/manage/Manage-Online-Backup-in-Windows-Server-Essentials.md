---
title: Gerenciar backup online no Windows Server Essentials
description: Descreve como usar o Windows Server Essentials
ms.date: 10/03/2016
ms.topic: article
ms.assetid: 95a9f593-fad7-4335-bd4d-c7bb8c033efb
author: nnamuhcs
ms.author: coreyp
manager: dongill
ms.openlocfilehash: e5c8a274a8e012ffd24ce6c6c819fa240c9f1095
ms.sourcegitcommit: 04637054de2bfbac66b9c78bad7bf3e7bae5ffb4
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/06/2020
ms.locfileid: "87837885"
---
# <a name="manage-online-backup-in-windows-server-essentials"></a>Gerenciar backup online no Windows Server Essentials

>Aplica-se a: Windows Server 2016 Essentials, Windows Server 2012 R2 Essentials, Windows Server 2012 Essentials

 Após a integração com o Backup do Microsoft Azure, a página Gerenciamento de **backup online** é exibida no painel do Windows Server Essentials. A página **Backup Online** torna possível realizar tarefas administrativas comuns. Os recursos na página de Backup Online incluem:

- As seguintes páginas subseção:

  -   **Backup Online** Depois de registrar o servidor para backup online, esta seção exibe o status atual de backup, status de armazenamento e informações da conta.

  -   **Pastas protegidas** Esta seção lista todas as pastas compartilhadas e todas as pastas de histórico de arquivos no servidor e quaisquer outras pastas que você tenha escolhido para fazer backup no Azure. A lista inclui o nome de pasta, o caminho da pasta e o status para cada pasta compartilhada.

  -   **Histórico de backups** Esta seção exibe uma lista de backups online recentes. A lista inclui o tipo de operação, o tempo e o status de cada backup.

- Um painel de detalhes com informações adicionais sobre um trabalho de backup ou de restauração selecionado.

- Um painel de tarefas que inclui um conjunto de tarefas administrativas que você pode executar.

  Como prática recomendada, você deve fazer backup dos dados de negócios e de usuário mais importantes. Por exemplo, você deve fazer backup de pastas do servidor que contêm arquivos de dados críticos. Você também deve fazer backup do Histórico de Arquivos para os computadores da rede que contêm informações críticas.

  Ao decidir em quais dados você fará o backup online, considere a frequência de backup e a política de retenção que você deseja implantar. Em seguida, examine seu orçamento e determine a quantidade de espaço de armazenamento que você precisa ter. Equilibre o custo e o volume de armazenamento em relação às suas necessidades e, em seguida, configure o backup online para proteger a maior quantidade de dados importantes possível. Para informações sobre preço, consulte [detalhes de preços de backup do Microsoft Azure](https://azure.microsoft.com/pricing/details/backup/).

  As seções a seguir descrevem várias tarefas de backup online que podem aparecer no Painel do Windows Server Essentials.

## <a name="online-backup-tasks-in-the-dashboard"></a>Tarefas de backup online no Painel

-   [Faça upload de um certificado para o cofre de Backup do Azure](Manage-Online-Backup-in-Windows-Server-Essentials.md#BKMK_1)

-   [Configure o backup online](Manage-Online-Backup-in-Windows-Server-Essentials.md#BKMK_2)

-   [Iniciar um backup online](Manage-Online-Backup-in-Windows-Server-Essentials.md#BKMK_3)

-   [Restaure arquivos e pastas de um backup online](Manage-Online-Backup-in-Windows-Server-Essentials.md#BKMK_4)

-   [Registre este servidor para backup](Manage-Online-Backup-in-Windows-Server-Essentials.md#BKMK_5)

-   [Cancelar o registro do servidor](Manage-Online-Backup-in-Windows-Server-Essentials.md#BKMK_6)

###  <a name="upload-a-certificate-to-the-azure-backup-vault"></a><a name="BKMK_1"></a>Carregar um certificado no cofre de backup do Azure
 Antes de usar o backup do Azure para backups online no Windows Server Essentials, você deve carregar um certificado público para se registrar no cofre de backup. O certificado é usado para autenticar a implantação do backup do Azure (o agente), agindo em nome do proprietário da assinatura do Microsoft Online Services para gerenciar os recursos associados à assinatura.

> [!NOTE]
>  Antes de fazer upload do certificado, conclua os procedimento em [Inscrever-se no Serviço de Backup do Azure](Manage-Online-Backup-in-Windows-Server-Essentials.md#BKMK_16).

##### <a name="to-upload-a-certificate-to-use-with-the-azure-backup-service"></a>Para fazer upload de um certificado para uso com o Serviço de Backup do Azure

1. Faça logon no Painel do Windows Server Essentials com uma conta de administrador.

2. No página de **Início** do Painel, clique em **BACKUP ONLINE**.

3. Na área **BACKUP ONLINE**, clique em **Fazer upload do certificado ao Cofre do Windows Azure Backup**.

    Isso abre os **serviços de recuperação** no portal de gerenciamento do Azure. Se você ainda não tiver entrado no Azure, precisará entrar usando seu conta Microsoft.

4. Clique no nome do cofre de backup que você usará para backups online para abrir a página **Início Rápido** do Cofre de backup.

5. Clique em **Gerenciar Certificado**.

6. Na caixa de diálogo **Gerenciar Certificado**, cole o caminho do certificado público gerado pelo Windows Server Essentials. Para obter o caminho do certificado público, faça o seguinte:

   1.  No Painel do Windows Server Essentials, clique na guia **Backup Online**.

   2.  Na página **Backup Online**, copie o caminho do certificado gerado.

   3.  Alterne para o Portal de Gerenciamento do Azure e, na caixa de diálogo **gerenciar certificado** , Cole o caminho para carregar o certificado público gerado.

   > [!NOTE]
   >  Você também pode usar seu próprio certificado público. Para saber qual certificado é necessário, na página **Início Rápido**, clique no link **Adquirir Certificado**.

   > [!NOTE]
   >   O Azure requer um certificado x. 509 V2 com uma chave pública. Para obter mais informações, consulte [Gerenciar contas online](/previous-versions/azure/dn169036(v=azure.100)).

7. Depois de selecionar o certificado, clique em **OK** (marca de seleção).

8. Você retorna para a página **Início Rápido**. Clique em **Painel** e verifique se o certificado foi carregado com êxito. Depois que o certificado é carregado com êxito, o painel exibe a impressão digital do certificado e a data de vencimento.

   Depois de concluir este procedimento, faça o seguinte:

9. Registre o servidor com o serviço de backup do Azure. Para obter mais informações, veja [Registrar esse servidor para backup](Manage-Online-Backup-in-Windows-Server-Essentials.md#BKMK_5).

10. Configure o backup online do servidor. Para obter mais informações, veja [Configurar o Backup Online](Manage-Online-Backup-in-Windows-Server-Essentials.md#BKMK_2).

###  <a name="configure-online-backup"></a><a name="BKMK_2"></a>Configurar o backup online
 Depois de registrar o servidor com o backup do Azure, você pode definir as configurações de backup online no Windows Server Essentials.

##### <a name="to-configure-online-backup"></a>Configurar o Backup Online

1.  A partir do Assistente Registrar seu Servidor ou da página **Backup Online** do Painel do Windows Server Essentials, clique em **Configurar o Backup Online**. O Assistente para Configurar o Backup Online é exibido.

2.  Na página **Configurar backup online** do assistente, marque a caixa de seleção de cada pasta do servidor que você deseja fazer backup no backup do Azure. Desmarque a caixa de seleção para cada pasta de servidor que você não deseja incluir no backup. Para adicionar pastas que não são mostradas na lista, clique em **Adicionar Pastas**. Quando você concluir a seleção, clique em **Avançar**.

    > [!NOTE]
    >  Você não pode selecionar uma pasta que não esteja localizada no servidor local ou uma pasta que esteja em uma unidade formatada como ReFS.

3.  Na página **Adicionar Backups de Histórico de Arquivos** do assistente, você pode selecionar para incluir Backups de Histórico de Arquivos para os computadores da rede que têm suporte ao recurso de Histórico de Arquivos. Em seguida, clique em **Avançar** para continuar.

4.  Na página **Especificar o agendamento de backup** do assistente, configure o seguinte e clique **Avançar** para continuar:

    -   Selecione ou aceite a hora do dia em que deseja executar o backup online. A hora padrão é 22H. Você também pode especificar o horário para executar um segundo backup.

    -   Selecione ou aceite os dias quando você deseja executar o backup online. Por padrão, o backup online é executado de segunda à sexta.

5.  Na página **Especificar a Política de Retenção de Backup** do assistente, selecione o número de dias que você deseja manter os backups online e, em seguida, clique em **Avançar**. O padrão é 7 dias. Você também pode optar por manter os backups online para 15 ou 30 dias.

    > [!NOTE]
    >   O backup do Azure sempre mantém o backup mais recente. Se o destino do backup não tem espaço suficiente disponível para armazenar o backup, o processo de backup não terá êxito. Para evitar essa situação, compre espaço de armazenamento adicional ou reduza o período de retenção de dados. Para obter informações sobre preços, consulte [detalhes de preços](https://azure.microsoft.com/pricing/details/backup/) para backup do Microsoft Azure.

6.  Na página **Escolher o uso de Largura de Banda** do assistente, se você deseja restringir a quantidade de largura de banda da Internet que está alocada para backup online, selecione **Habilitar o Uso de Largura de Banda da Internet**. Use as opções na página para especificar quanta largura de banda da Internet permitir que o backup online use durante o horário de trabalho horas e horas ociosas. Em seguida, defina o horário comercial e os dias úteis.

    > [!NOTE]
    >   O backup do Azure permite que você personalize como o software de integração utiliza a largura de banda da rede ao fazer backup ou restaurar informações. Usando uma técnica comumente conhecida como limitação, você pode controlar a quantidade de largura de banda de rede disponível para uso pelos processos de backup e restauração durante intervalos de dia e hora específicos. Depois de selecionar a caixa de seleção **Habilitar o Uso de Largura de Banda da Internet** no Assistente para Configurar o Backup Online, você pode especificar os dias e horas de trabalho durante o qual aplicar o limite de largura de banda de horas de trabalho. O limite de horas ociosas é usado em todas as outras vezes. Os intervalos de largura de banda válidos vão de 256 Kbps a ilimitado para ambos os limites.

7.  Quando a configuração do backup online for concluída, clique em **Fechar**.

    > [!TIP]
    >  Após uma configuração de backup com êxito, a página de **Backup Online** mostra o status do backup online mais recente e a quantidade de espaço de armazenamento usado pelo Cofre de backup. Para ver o status dos backups anteriores, clique em **Histórico de Backup**.

###  <a name="start-an-online-backup"></a><a name="BKMK_3"></a>Iniciar um backup online

> [!NOTE]
>  Antes de iniciar um backup online, você deve primeiro [Registrar este servidor para backup](Manage-Online-Backup-in-Windows-Server-Essentials.md#BKMK_5) e em seguida, [Configurar o Backup Online](Manage-Online-Backup-in-Windows-Server-Essentials.md#BKMK_2).

##### <a name="to-start-an-online-backup-immediately"></a>Para iniciar um backup online imediatamente

1.  Faça logon no Painel como administrador.

2.  Na barra de navegação, clique em **BACKUP ONLINE**.

3.  No painel **Tarefas de Backup Online**, clique em **Iniciar backup agora**.

###  <a name="restore-files-and-folders-from-an-online-backup"></a><a name="BKMK_4"></a>Restaurar arquivos e pastas de um backup online
 O Assistente para Restauração de Arquivos e Pastas orienta você no processo de localização, seleção e restauração de arquivos e pastas de backup online. À medida que você avança através do assistente, você executará as seguintes tarefas:

1.  **Escolher um backup online do qual restaurar arquivos e pastas**

     Quando você executa o Assistente para Restauração de Arquivos e Pastas, a primeira coisa que você deve fazer é especificar se você deseja restaurar arquivos de um backup online do servidor que você está conectado no momento ou de um backup de um servidor diferente.

2.  **Escolher um servidor do qual restaurar arquivos e pastas**

     Se você optar por restaurar arquivos e pastas de um servidor diferente, selecione o servidor a partir do qual você deseja restaurar da lista de servidores disponíveis e clique em **Avançar**.

3.  **Escolha um volume que contém os arquivos e pastas a serem restaurados**

     Os backups online contêm volumes correspondentes aos volumes no servidor de origem do backup. Depois de selecionar o volume, selecione a data e hora do backup do qual restaurar e clique em **Avançar**.

4.  **Selecionar arquivos e pastas para restaurar**

     Na página **Selecionar Itens para Restaurar** do assistente, você pode abrir vários locais de pasta e marcar a caixa de seleção para cada arquivo ou pasta que você deseja restaurar. Quando terminar de selecionar os arquivos, clique em **Avançar**.

5.  **Especificar o local de destino para arquivos e pastas restaurados**

     A página **Especificar Opções de Restauração** do assistente permite que você especifique onde restaurar arquivos e pastas. Há duas opções:

    -   **Restaurar ao local original**. Selecione esta opção para restaurar arquivos e pastas para o local exato de origem do backup.

    -   **Restaurar para um local diferente**.  Escolha esta opção para especificar um local diferente no servidor em que deseja restaurar os arquivos.

         Na instalação padrão, se já existir um arquivo com o mesmo nome de um arquivo de restauração no local de destino, o assistente cria uma cópia do arquivo original. Além disso, a lista de controle de acesso (ACL) é atualizada para refletir as permissões do arquivo atual. Você pode clicar em **Avançado** para personalizar as configurações padrão.

6.  **Confirmar as informações de restauração**

     A página **Confirmar suas Informações de Restauração** fornece um resumo das instruções de restauração que você especificou. Para continuar com a restauração do arquivo, você deve digitar a senha correta da sua conta de backup online.

###  <a name="register-this-server-for-backup"></a><a name="BKMK_5"></a>Registrar este servidor para backup
 Para fazer backup ou restaurar arquivos, pastas e histórico de arquivos no servidor do Windows Server Essentials para o backup do Azure, primeiro você deve registrar o servidor com o serviço de Backup do Microsoft Azure.

> [!NOTE]
>  Antes de registrar seu servidor, você deve fazer upload de um certificado para uso com o Cofre de backup que armazenará os backups online. Para obter mais informações, consulte [Carregar um certificado no cofre do Microsoft Azure Backup](Manage-Online-Backup-in-Windows-Server-Essentials.md#BKMK_1).

##### <a name="to-register-your-server-with-azure-backup"></a>Para registrar o servidor no Windows Azure Backup

1.  Faça logon no servidor como administrador e abra o Painel.

2.  Na barra de navegação, clique em **BACKUP ONLINE**.

3.  Na subseção **Backup Online**, clique em **Registrar**.

4.  Selecione o certificado que você deseja usar com o Cofre de backup para seus backups online. O certificado padrão é selecionado por padrão. Se você deseja escolher um certificado diferente, use **Procurar**. Em seguida, clique em **Próximo**.

5.  Siga as instruções do Assistente para criar uma senha e em seguida, concluir o registro.

###  <a name="unregister-server"></a><a name="BKMK_6"></a>Cancelar registro do servidor

> [!CAUTION]
>  Se você cancelar o registro do servidor do Windows Server Essentials do serviço de Backup do Microsoft Azure, o backup do Azure não poderá mais fazer backup do servidor. Além disso, os dados do servidor que foram carregados anteriormente serão apagados. Para retomar os backups online, você deve registrar o servidor novamente.

##### <a name="to-unregister-your-server-with-azure-backup"></a>Para cancelar o registro do servidor no Windows Azure Backup

1.  Entre no [Portal de Gerenciamento do Azure](https://manage.windowsazure.com).

2.  Clique em **Serviços de Recuperação**.

3.  Em **Serviços de Recuperação**, clique no nome do Cofre de backup.

4.  Na página **Início Rápido** do cofre, clique em **Servidores**.

5.  Selecione o servidor, clique em **Excluir** e clique em **Sim** no prompt de confirmação.

## <a name="related-tasks"></a>Tarefas relacionadas

-   [Alterar a política de backup online](Manage-Online-Backup-in-Windows-Server-Essentials.md#BKMK_7)

-   [Exibir o uso de armazenamento de backup online](Manage-Online-Backup-in-Windows-Server-Essentials.md#BKMK_8)

-   [Incluir uma nova pasta no backup online](Manage-Online-Backup-in-Windows-Server-Essentials.md#BKMK_9)

-   [Remover ou excluir backups de histórico do arquivo de política de backup online](Manage-Online-Backup-in-Windows-Server-Essentials.md#BKMK_10)

-   [Desabilitar ou habilitar novamente o backup do servidor online](Manage-Online-Backup-in-Windows-Server-Essentials.md#BKMK_11)

-   [Interromper um backup de servidor online em andamento](Manage-Online-Backup-in-Windows-Server-Essentials.md#BKMK_12)

-   [Exibir status de backup online](Manage-Online-Backup-in-Windows-Server-Essentials.md#BKMK_13)

-   [Exibir e gerenciar os alertas de backup online](Manage-Online-Backup-in-Windows-Server-Essentials.md#BKMK_14)

-   [Redefinir backup online para as configurações padrão](Manage-Online-Backup-in-Windows-Server-Essentials.md#BKMK_15)

-   [Assine o serviço de Backup do Azure](Manage-Online-Backup-in-Windows-Server-Essentials.md#BKMK_16)

-   [Integrar o Backup do Windows Azure com o Windows Server Essentials](Manage-Online-Backup-in-Windows-Server-Essentials.md#BKMK_17)

-   [Proteger pastas para backup online no Windows Server Essentials](Manage-Online-Backup-in-Windows-Server-Essentials.md#BKMK_18)

-   [Histórico de backup online no Windows Server Essentials](Manage-Online-Backup-in-Windows-Server-Essentials.md#BKMK_19)

###  <a name="change-the-online-backup-policy"></a><a name="BKMK_7"></a>Alterar a política de backup online
 É fácil de fazer alterações à política de backup online usando o Painel do Windows Server Essentials.

##### <a name="to-change-the-online-backup-policy"></a>Para alterar a política de backup online

1. Faça logon no Painel como administrador.

2. Na barra de navegação, clique em **Backup Online**.

3. No painel **Tarefas de Backup Online**, clique em **Configurar o Backup Online**.

4. Siga as instruções no Assistente para personalizar a política de backup online.

   Para obter mais informações sobre as configurações que você pode personalizar,veja [Configurar o Backup Online](Manage-Online-Backup-in-Windows-Server-Essentials.md#BKMK_2).

###  <a name="view-online-backup-storage-usage"></a><a name="BKMK_8"></a>Exibir o uso de armazenamento de backup online

##### <a name="to-view-the-amount-of-storage-space-that-online-backup-uses"></a>Para exibir a quantidade de espaço de armazenamento que o Online Backup usa

1.  Faça logon no Painel como administrador.

2.  Na barra de navegação, clique em **BACKUP ONLINE**.

3.  Na guia **Backup Online**, o **Status do armazenamento** é exibido no painel de informações.

###  <a name="include-a-new-folder-in-online-backup"></a><a name="BKMK_9"></a>Incluir uma nova pasta no backup online

##### <a name="to-include-a-new-folder-in-the-online-backup-policy"></a>Para incluir uma nova pasta na política de backup online

1. Faça logon no Painel como administrador.

2. Na barra de navegação, clique em **Backup Online**.

3. No painel **Tarefas de Backup Online**, clique em **Configurar o Backup Online**.

4. Na página **Alterar ou Remover a Política de Backup**, clique em **Personalizar a Política de Backup Online**.

5. Na página **Configurar o Backup Online**, se a pasta que você deseja incluir não aparece na lista, clique em **Adicionar pastas**.

6. Na janela **Adicionar pastas**, procure e selecione a pasta que você deseja incluir no backup online e em seguida, clique em **OK**.

7. Na página **Configurar o Backup Online** , selecione outras pastas para incluir conforme desejado e clique em **Avançar**.

8. Siga as instruções no Assistente para concluir a personalização da política de backup online.

   Para obter informações detalhadas sobre as configurações que você pode personalizar, veja [Configurar o Backup Online](Manage-Online-Backup-in-Windows-Server-Essentials.md#BKMK_2).

###  <a name="remove-or-exclude-file-history-backups-from-the-online-backup-policy"></a><a name="BKMK_10"></a>Remover ou excluir backups de histórico de arquivo da política de backup online

##### <a name="to-remove-or-exclude-a-folder-from-the-online-backup-policy"></a>Para remover ou excluir uma pasta da política de backup online

1.  Faça logon no Painel como administrador.

2.  Na barra de navegação, clique em **Backup Online**.

3.  Clique na guia **pastas protegidas** . O painel de detalhes exibe uma lista de pastas com seu status de backup online.

4.  Selecione a pasta que você deseja excluir da política de backup online e em seguida, no painel de tarefas, clique em **Remover a Pasta de Backup Online**.

###  <a name="disable-or-re-enable-online-server-backup"></a><a name="BKMK_11"></a>Desabilitar ou reabilitar o backup do servidor online
 Para obter instruções sobre como usar o backup do Azure para fazer backup ou restaurar dados do servidor, consulte [registrar este servidor para backup](Manage-Online-Backup-in-Windows-Server-Essentials.md#BKMK_5).

 Para obter instruções sobre como parar de usar o backup do Azure para fazer backup ou restaurar dados do servidor, consulte [Cancelar registro do servidor](Manage-Online-Backup-in-Windows-Server-Essentials.md#BKMK_6).

###  <a name="stop-an-online-server-backup-in-progress"></a><a name="BKMK_12"></a>Parar um backup do servidor online em andamento

##### <a name="to-stop-an-online-server-backup-in-progress"></a>Para interromper um backup de servidor online em andamento

1.  Faça logon no Painel como administrador.

2.  Na barra de navegação, clique em **BACKUP ONLINE**.

3.  No painel **Tarefas de Backup Online**, clique em **Interromper o backup**.

     Após interromper o backup, um status de **Cancelado** é exibido para o backup na lisa **Histórico de Backup**.

###  <a name="view-online-backup-status"></a><a name="BKMK_13"></a>Exibir status do backup online

##### <a name="to-view-the-backup-status"></a>Para exibir o status do backup

1.  Faça logon no Painel como administrador.

2.  Na barra de navegação, clique em **BACKUP ONLINE**.

3.  Clique na guia **histórico de backup** . A exibição de lista exibe o status de cada trabalho de backup. Selecione um trabalho de backup para exibir detalhes adicionais sobre o trabalho.

###  <a name="view-and-manage-online-backup-alerts"></a><a name="BKMK_14"></a>Exibir e gerenciar alertas de backup online
 Como muitos outros alertas, os alertas para o backup do Azure são exibidos no Visualizador de alertas.

##### <a name="to-view-online-backup-alerts-in-the-alert-viewer"></a>Para exibir alertas de backup online no Visualizador de Alertas

1. Abra o Painel.

2. Realize um dos seguintes procedimentos:

     Windows Server Essentials: no painel de navegação, clique no ícone alertas \( pode ser crítico, de aviso ou informativo \) . Isso abre o Visualizador de alertas.

     Windows Server Essentials: na **Home** Page do, clique na guia **monitoramento de integridade** .

3. Examine a lista de alertas para problemas relacionados ao backup do Azure.

   Para obter mais informações sobre como usar o Visualizador de alertas ou a guia monitoramento de integridade para gerenciar alertas, consulte [gerenciar a integridade do sistema](Manage-System-Health-in-Windows-Server-Essentials.md).

###  <a name="reset-online-backup-to-default-settings"></a><a name="BKMK_15"></a>Redefinir o backup online para as configurações padrão
 O Windows Server Essentials fornece um assistente que ajuda você a configurar as definições de backup online. Se você deseja restaurar as configurações padrão, execute a tarefa**Configurar o Backup Online** e escolha a opção **Remover a Política de Backup Online**. Em seguida, execute a tarefa **Configurar o Backup Online** novamente. Os dados previamente carregados permanecem inalterados.

###  <a name="sign-up-for-azure-backup-service"></a><a name="BKMK_16"></a>Inscrever-se no serviço de backup do Azure
 Para se preparar para integrar o Backup do Microsoft Azure com o Windows Server Essentials, você fará logon no Azure Portal de Gerenciamento com sua conta do Microsoft Online Services e, em seguida, criará um cofre de backup para armazenar seus backups online no Azure. Em seguida, você baixará o módulo de integração do backup do Azure e usará o arquivo baixado para instalar o suplemento de backup do Azure no servidor do Windows Server Essentials. Se você não tiver uma conta da Microsoft, você pode se inscrever para uma avaliação gratuita.

 Para realizar esta configuração, conclua as seguintes tarefas:

1.  Inscreva-se para uma Conta de Serviços Online da Microsoft e a Visualização de Backup.

2.  Crie um cofre de backup para armazenar os backups online.

3.  Baixe o agente de backup do Azure.

4.  Instale o suplemento de backup do Azure no servidor.

####  <a name="sign-up-for-a-microsoft-online-services-account-and-the-backup-preview"></a><a name="BKMK_SignupforaMicrosoftOnlineServiceAccount"></a>Inscreva-se para uma conta do Microsoft Online Services e a visualização de backup

1.  Faça logon no Painel do Windows Server Essentials

2.  No página de **Início** do Painel, clique na categoria **SUPLEMENTOS**, clique em **Integração com o Backup do Window Azure** e em seguida, clique em **Clique para registrar-se no Backup do Windows Azure**.

3.  Na página **serviços de recuperação** do Azure, na seção **backup (visualização)** , examine os detalhes.

4.  Se você não tiver uma assinatura do Azure, clique em **avaliação gratuita**e siga as instruções para obter uma assinatura do Azure.

     Se você já tiver uma assinatura do Azure, clique em **portal** no canto superior direito da página da Web para ir para a Portal de gerenciamento do Azure.

5.  Na página Portal de Gerenciamento do Azure, você verá **serviços de recuperação** no painel esquerdo. É aqui que você gerenciará os cofres de backup que armazenam seus backups online do Windows Server Essentials.

####  <a name="create-a-backup-vault-to-store-online-backups"></a><a name="BKMK_Createabackupvaulttostoreonlinebackups"></a>Criar um cofre de backup para armazenar backups online

1.  Entre no [Portal de Gerenciamento do Microsoft Azure](https://manage.windowsazure.com)pelo navegador da Web no seu servidor Windows Server Essentials.

2.  Na Portal de Gerenciamento do Azure, clique em **novo**, em **serviços de dados**, em serviços de **recuperação**, em **cofre de backup**e em **criação rápida**.

3.  Insira um nome para seu Cofre de backup, escolha a região em que você deseja armazenar seus backups e em seguida, clique em **Criação Rápida**.

     A área **Serviços de Recuperação** se abre com seu novo cofre de backup exibido.

####  <a name="download-the-azure-backup-agent"></a><a name="BKMK_DownloadtheWindowsAzureBackupAgent"></a>Baixar o agente de backup do Azure

1.  Abra o Painel do Windows Server Essentials.

2.  Na página de **Início**, clique na guia **Começar**, clique na categoria **SUPLEMENTOS**, clique em **Integração com o Backup do Windows Azure** e em seguida, clique em **Clique para baixar o módulo de integração do Backup do Windows Azure**.

####  <a name="install-the-azure-backup-add-in-on-the-server"></a><a name="BKMK_InstalltheWindowsAzureBackupAddIn"></a>Instalar o suplemento de backup do Azure no servidor

1. Entre no seu servidor usando uma conta de administrador e execute o arquivo **OnlineBackupAddin.wssx** baixado na etapa anterior.

2. Os **Termos de Licença de Software** são exibidos. Se você concorda com os termos e condições, clique em **Aceitar** para continuar a instalação.

3. Na página **Instalar o suplemento**, clique em **Instalar o suplemento**.

   > [!NOTE]
   >  Talvez seja necessário que você reinicie o servidor para instalar os softwares pré-solicitados.

    A página de **Instalação** é exibida. Um indicador de progresso exibe quando a instalação começa e mostra o andamento da instalação. Quando a instalação for concluída, você receberá uma mensagem informando que o suplemento de backup do Azure foi instalado com êxito.

4. Clique em **Concluir**.

5. Feche e reabra o Painel.

    Uma nova guia, **Backup Online**, é adicionada ao Painel. Nessa guia, você pode registrar o servidor, definir as configurações de backup e abrir os **serviços de recuperação** na portal de gerenciamento do Azure para gerenciar os cofres de backup para seus servidores.

   Depois de concluir essas etapas, faça o seguinte:

6. No Windows Server Essentials, carregue um certificado a ser usado para backups online. Para obter mais informações, consulte [Carregar um certificado no cofre do Microsoft Azure Backup](Manage-Online-Backup-in-Windows-Server-Essentials.md#BKMK_1).

7. Registre o servidor com o cofre de backup do Azure. Para obter mais informações, veja [Registrar esse servidor para backup](Manage-Online-Backup-in-Windows-Server-Essentials.md#BKMK_5).

8. Configure o backup online do servidor. Para obter mais informações, veja [Configurar o Backup Online](Manage-Online-Backup-in-Windows-Server-Essentials.md#BKMK_2).

###  <a name="integrate-azure-backup-with-windows-server-essentials"></a><a name="BKMK_17"></a>Integrar o backup do Azure com o Windows Server Essentials
 O módulo integração do Backup do Microsoft Azure é um novo recurso do Windows Server Essentials que permite criptografar e fazer backup de arquivos e pastas do seu servidor para um sistema de armazenamento hospedado pelo Azure fornecido pela Microsoft. Usando o backup do Azure para criptografar e fazer backup dos dados no servidor, você pode ajudar a evitar a perda catastrófica de dados comerciais críticos devido a incêndios, inundações, roubo ou outros desastres. Quando você usa o backup do Azure para fazer backup de dados do servidor, as informações são criptografadas usando sua senha antes de serem carregadas em um datacenter seguro na Internet. Para acessar dados de um backup online, você deve ter um servidor autenticado por um certificado e deve fornecer a senha.

 Depois de integrar e registrar o servidor com o backup do Azure, você pode definir as configurações de backup online para executar backups agendados regularmente. Você também pode iniciar um backup online a qualquer momento clicando na tarefa **Iniciar backup agora** no Painel de Backup Online.

 A configuração de backup online envolve estas etapas:

1.  [Inscrever-se no serviço de backup do Azure](Manage-Online-Backup-in-Windows-Server-Essentials.md#BKMK_16) -depois de se inscrever no serviço de backup, você criará um cofre de backup, baixará e instalará o módulo de integração de backup do Azure.

2.  [Faça upload de um certificado para o cofre de Backup do Azure](Manage-Online-Backup-in-Windows-Server-Essentials.md#BKMK_1)

3.  [Registre este servidor para backup](Manage-Online-Backup-in-Windows-Server-Essentials.md#BKMK_5)

4.  [Configure o backup online](Manage-Online-Backup-in-Windows-Server-Essentials.md#BKMK_2)

> [!NOTE]
>   O backup do Azure usa a senha para criptografar arquivos e pastas para o backup online. Alterar a senha de criptografia substituirá a senha que você especificou ao registrar o servidor. A senha aceita somente caracteres codificados ASCII.

###  <a name="protect-folders-for-online-backup-in-windows-server-essentials"></a><a name="BKMK_18"></a>Proteger pastas para backup online no Windows Server Essentials
 A subseção **Pastas Protegidas** na seção de Backup Online do Painel exibe uma lista de todas as pastas compartilhadas no servidor. A tabela a seguir descreve as informações que estão incluídas na lista.

|Coluna|Descrição|
|------------|-----------------|
|**Nome da pasta:**|O nome da pasta que está incluído no backup online.<br /><br /> Para adicionar ou excluir uma pasta, execute a tarefa **Configurar o Backup Online**.|
|**Caminho da pasta:**|O local da pasta.|
|**Status:**|Há três tipos de status **protegido**, **não protegido**e **desconhecido**.|

###  <a name="online-backup-history-in-windows-server-essentials"></a><a name="BKMK_19"></a>Histórico de backup online no Windows Server Essentials
 A subseção **Histórico de Backup** na seção de Backup Online do Painel exibe uma lista de backups online recentes. Você pode usar os backups com êxito para restaurar arquivos e pastas. A tabela a seguir descreve as informações que estão incluídas na lista.

|Coluna|Descrição|
|------------|-----------------|
|**Operacional**|Há dois tipos de operações - **Backup** e **Restauração**.|
|**Momento**|Esta é a hora registrada para o status mais recente.|
|**Status:**|Há cinco tipos de status- **êxito**, **em andamento**, **cancelado**, **aviso**e **sem êxito**.|

## <a name="additional-references"></a>Referências adicionais

-   [Gerenciar backup e restauração](Manage-Backup-and-Restore-in-Windows-Server-Essentials.md)

-   [Gerenciar serviços online da Microsoft](Manage-Microsoft-Online-Services-in-Windows-Server-Essentials.md)

-   [Gerenciar o Windows Server Essentials](Manage-Windows-Server-Essentials.md)

-   [Utilizar o Windows Server Essentials](../use/Use-Windows-Server-Essentials.md)