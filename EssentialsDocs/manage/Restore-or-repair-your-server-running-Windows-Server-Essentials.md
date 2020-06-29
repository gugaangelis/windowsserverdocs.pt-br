---
title: Restaurar ou reparar o servidor que executa o Windows Server Essentials
description: Descreve como usar o Windows Server Essentials
ms.date: 10/03/2016
ms.prod: windows-server
ms.topic: article
ms.assetid: 27bf6f24-30c4-4935-9b24-069eb43e22f4
author: nnamuhcs
ms.author: coreyp
manager: dongill
ms.openlocfilehash: cfb78f6b451783bec4cc6f20f36a65f0731162e2
ms.sourcegitcommit: 771db070a3a924c8265944e21bf9bd85350dd93c
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/27/2020
ms.locfileid: "85470351"
---
# <a name="restore-or-repair-your-server-running-windows-server-essentials"></a>Restaurar ou reparar o servidor que executa o Windows Server Essentials

>Aplica-se a: Windows Server 2016 Essentials, Windows Server 2012 R2 Essentials, Windows Server 2012 Essentials

 Este tópico fornece uma visão geral e procedimentos de suporte para restaurar ou reparar um servidor que executa o Windows Server Essentials e inclui as seguintes seções:

-   [Visão geral de restauração do sistema de servidor](Restore-or-repair-your-server-running-Windows-Server-Essentials.md#BKMK_Overview)

-   [Restaurar ou reparar a unidade do sistema](Restore-or-repair-your-server-running-Windows-Server-Essentials.md#BKMK_Restore)

-   [Restaurar arquivos e pastas no servidor](Restore-or-repair-your-server-running-Windows-Server-Essentials.md#BKMK_RestoreFilesAndFolders)

##  <a name="overview-of-server-system-restores"></a><a name="BKMK_Overview"></a>Visão geral das restaurações do sistema de servidor
 O estado do servidor quando você executa uma restauração afeta o método de restauração que está disponível e a abrangência de uma restauração que você pode executar.

 Os motivos mais comuns para restaurar um servidor são:

- Um vírus no servidor não pode ser inoculado ou excluído.

- As definições de configuração do servidor são inválidas e não é possível iniciar o servidor.

- Você substituiu a unidade do sistema.

- Você está desativando o servidor e você deseja restaurar para um novo servidor.

  Você pode restaurar o servidor de um backup ou restaurar o servidor para as configurações padrão de fábrica.

- [Restaurando o servidor a partir de um backup](Restore-or-repair-your-server-running-Windows-Server-Essentials.md#BKMK_RestoreFromBackup)

- [Redefinindo o servidor para as configurações padrão de fábrica](Restore-or-repair-your-server-running-Windows-Server-Essentials.md#BKMK_FactoryReset)

###  <a name="restoring-the-server-from-a-backup"></a><a name="BKMK_RestoreFromBackup"></a>Restaurando o servidor a partir de um backup
 Esta seção fornece orientação sobre que tipo de backup escolher.

 Se um backup estiver disponível, sua melhor opção para restaurar o servidor será usar a mídia de instalação do fabricante para restaurar a partir de um backup externo. A restauração irá recuperar pastas e configurações do servidor do backup que você escolher. Você somente precisa definir as configurações e restaurar os dados criados após o backup.

 Quando você escolhe recuperar o servidor por meio da restauração de um backup anterior, você deve escolher o backup específico que você deseja restaurar e você deve ter um arquivo de backup válido em um disco rígido externo que está conectado diretamente ao servidor:

- **Se você tiver um backup bem-sucedido mais recente do servidor** e você souber que ele contém todos os seus dados críticos, sua opção é bastante simples. Você precisará apenas recriar os dados que foram criados após o último bom backup e reconfigurar mudanças de configurações feitas após o backup.

- **Se você estiver restaurando o servidor devido a um vírus**, selecione um backup que você sabe que ocorreu antes de receber o vírus. Talvez seja necessário voltar vários dias para selecionar um backup que está limpo.

- **Se você estiver restaurando o servidor devido às configurações inválidas**, selecione um backup que você sabe que ocorreu antes da alteração que está causando o problema no servidor.

  Quando você restaura a partir de um backup, o processo exato e o acompanhamento necessário dependem do número de discos rígidos no servidor e se a unidade do sistema é substituída:

- **Se o servidor tem um único disco rígido e a unidade não é substituída**, as informações de partição de unidade permanecem intactas quando você restaura o servidor. O volume do sistema é restaurado e os dados no volume restante são preservados.

- **Se o servidor tem um único disco rígido e a unidade é substituída**, o volume do sistema é restaurado e, em seguida, você deve restaurar manualmente pastas para o volume de dados. As pastas compartilhadas não padrão precisam ser criadas porque elas não são criadas quando o armazenamento do servidor é recriado.

- **Se o servidor tiver vários discos rígidos e a unidade 0 (contém o volume do sistema) não for substituída**, as informações de partição de unidade permanecem intactas quando você restaura o servidor. O volume do sistema é restaurado e os dados em todos os volumes restantes são preservados.

- **Se o servidor tiver vários discos rígidos e a unidade 0 (contém o volume do sistema) for substituída**, o volume do sistema é restaurado e, em seguida, você deve restaurar manualmente as pastas compartilhadas que foram armazenadas anteriormente no disco 0.

###  <a name="resetting-the-server-to-factory-default-settings"></a><a name="BKMK_FactoryReset"></a>Redefinindo o servidor para as configurações padrão de fábrica
 Se você não tiver um backup que possa restaurar ou por algum outro motivo queira ou precise executar uma restauração completa do sistema sem restaurar a configuração anterior do servidor, você pode realizar uma restauração que redefina o servidor para as configurações padrão de fábrica usando a instalação ou a mídia de recuperação a partir do fabricante do hardware de servidor.

 Quando você restaurar o servidor reconfigurando-o com as configurações padrão de fábrica, todas as configurações existentes e aplicativos instalados no servidor serão excluídos e você deve configurar seu servidor novamente. Depois de redefinição de fábrica, o servidor é reinicializado.

 Quando você executa uma redefinição de fábrica, você pode optar por manter os dados ou excluí-los, com os seguintes efeitos:

-   Se você optar por manter todos os seus dados, todos os dados no volume do sistema serão excluídos, mas os dados em outros volumes serão mantidos.

    > [!CAUTION]
    >  Se as configurações de disco não corresponderem às configurações padrão, todos os dados em um disco serão excluídos. Se você substituiu o disco do sistema, o novo disco deve ser maior do que o volume do sistema do disco original.
    >
    >  Se as informações da partição para uma unidade do sistema forem ilegíveis ou se você substituir a unidade do sistema, todos os dados na unidade do sistema serão removidos, mesmo se você optar por manter todos os seus dados.

-   Se você planeja encerrar ou realocar o servidor, opte por excluir todos os seus dados. Além da configuração do servidor, outras configurações e os dados no volume do sistema, todos os outros dados são excluídos e todos os discos rígidos no servidor são reformatados.

> [!NOTE]
>  Se os espaços de armazenamento forem configurados no servidor, antes de executar uma redefinição de fábrica, você deve usar a seção **Avançado** do console **Gerenciar Espaços de Armazenamento** para remover manualmente todos os espaços de armazenamento.

 Depois da redefinição de fábrica, você precisará executar as seguintes tarefas:

-   **Reconfigure o servidor.** No servidor, use o Assistente Configurar o Servidor para redigitar as definições de configuração. Para configurar um servidor do Windows Server Essentials gerenciado remotamente de um computador cliente, abra um navegador da Web e digite **http://** _<nomedoservidor \> _ na barra de endereços.

-   **Reconecte computadores cliente ao servidor.** Se um computador tiver sido conectado anteriormente ao servidor, você deverá desinstalar o software do conector do Windows Server Essentials do computador antes de conectar o computador ao servidor novamente. Para obter mais informações, consulte [Desinstalar o software Connector](../use/Get-Connected-in-Windows-Server-Essentials.md#BKMK_13) e [Conectar computadores ao servidor](../use/Get-Connected-in-Windows-Server-Essentials.md#BKMK_9).

##  <a name="restore-or-repair-the-system-drive"></a><a name="BKMK_Restore"></a>Restaurar ou reparar a unidade do sistema
 A primeira etapa da restauração do servidor é restaurar ou reparar a unidade de sistema do servidor. Depois que você restaurar a unidade do sistema, você fará o que for necessário para restaurar as unidades de dados no servidor e qualquer compartilhamento que foi perdido na restauração.

 Três métodos estão disponíveis para executar a restauração:

-   [Restaurar ou reparar seu servidor usando a mídia de instalação](Restore-or-repair-your-server-running-Windows-Server-Essentials.md#BKMK_Restore_1). Use a mídia de instalação do fabricante do servidor para restaurar a partir de um backup.

-   **Usar a mídia de instalação para restaurar o servidor para configurações padrão de fábrica**. Para saber como fazer isso no seu servidor, consulte a documentação do fabricante do servidor.

-   [Restaurar ou redefinir o servidor a partir de um computador cliente usando o DVD de recuperação](Restore-or-repair-your-server-running-Windows-Server-Essentials.md#BKMK_Restore_2). Se precisar restaurar um servidor administrado remotamente que esteja executando o Windows Server Essentials, você deverá executar a restauração de um computador cliente usando o DVD de restauração do fabricante do servidor.

###  <a name="restore-or-repair-your-server-using-installation-media"></a><a name="BKMK_Restore_1"></a>Restaurar ou reparar o servidor usando a mídia de instalação
 O procedimento a seguir descreve como restaurar a unidade do sistema do servidor de um backup usando a mídia de instalação do Windows Server Essentials. (Para saber como usar a mídia de instalação para restaurar as configurações padrão de fábrica, consulte a documentação do fabricante do servidor).

> [!NOTE]
>  Se o servidor usar espaços de armazenamento e você estiver restaurando os dados para um novo servidor, você deverá recuperar a unidade do sistema primeiro e, em seguida, fazer logon no painel do Windows Server Essentials, configurar os espaços de armazenamento de forma semelhante ao servidor antigo e, em seguida, recuperar os volumes de dados.

##### <a name="to-restore-the-server-system-drive-from-a-backup-using-installation-media"></a>Para restaurar a unidade de sistema do servidor de um backup usando a mídia de instalação

1.  Insira o DVD de instalação do Windows Server Essentials na unidade de DVD do servidor, reinicie o servidor e pressione qualquer tecla para iniciar a partir do DVD.

    > [!NOTE]
    >  Se o processo de restauração não for iniciado automaticamente, verifique as configurações de BIOS para o seu servidor para garantir que a unidade de DVD apareça primeiro no menu de inicialização.

     -Ou-

     Se o fabricante tiver pré-carregado a mídia de instalação no servidor, pressione F8 durante a inicialização para iniciar a partir da mídia de instalação.

2.  Após os arquivos do Windows Server serem carregados, escolha seu idioma e outras preferências e, em seguida, clique em **Avançar**.

3.  Na página seguinte do assistente, clique em **Reparar o computador**.

    > [!CAUTION]
    >  Não escolha a opção **Instalar agora**. Essa opção orientará você em uma instalação completa do sistema que exclui todas as configurações e todos os dados na unidade do sistema.

4.  Na página **Escolher uma opção**, clique em **Solucionar**.

5.  Na página **recuperação de imagem do sistema** , selecione o sistema atual? o **Windows Server Essentials** ou o **Windows Server Essentials**.

     O Assistente para Recriar a Imagem do Computador é aberto.

6.  Na página **Selecionar um backup de imagem do sistema**, você pode optar por usar o backup mais recente ou você pode selecionar um backup anterior. O sistema será restaurado para o estado que estava no momento do backup que você escolher para restaurar ou reparar o servidor. Dados que foram adicionados ou alterações nas configurações feitas depois que o backup foi salvo devem ser recriados.

     Selecione uma das opções a seguir e clique em **Avançar**:

    -   **Usar a imagem de sistema disponível mais recente (recomendada)**

    -   **Selecione uma imagem do sistema**

    > [!NOTE]
    >  Se você tiver um backup bem-sucedido mais recente do servidor e você souber que ele contém todos os seus dados críticos, sua opção é bastante simples. Você precisará apenas recriar os dados que foram criados após o último bom backup e reconfigurar mudanças de configurações feitas após o backup.
    >
    >  Se você estiver restaurando o servidor devido a um vírus, selecione um backup que você sabe que ocorreu antes de receber o vírus. Talvez seja necessário voltar vários dias para selecionar um backup que está limpo.
    >
    >  Se você estiver restaurando o servidor devido às configurações inválidas, selecione um backup que você sabe que ocorreu antes da alteração que está causando o problema no servidor.

7.  Siga as instruções do assistente para concluir a restauração do sistema.

8.  Depois que o servidor for restaurado com êxito, remova o DVD de instalação se você tiver usado um e, em seguida, reinicie o servidor.

> [!NOTE]
>  Para restaurar e compartilhar pastas no servidor, talvez seja necessário executar etapas adicionais. Para obter mais informações, consulte [Restaurar arquivos e pastas no servidor](Restore-or-repair-your-server-running-Windows-Server-Essentials.md#BKMK_RestoreFilesAndFolders).

###  <a name="restore-or-reset-your-server-from-a-client-computer-using-the-recovery-dvd"></a><a name="BKMK_Restore_2"></a>Restaurar ou redefinir o servidor de um computador cliente usando o DVD de recuperação
 No Windows Server Essentials, você pode iniciar o servidor a partir de uma unidade flash USB inicializável que você cria e, em seguida, recuperar o servidor de um computador cliente usando o DVD de recuperação que você recebeu do fabricante do servidor. O computador cliente deve estar na mesma rede que o servidor. Esse método não está disponível no Windows Server Essentials.

 O procedimento a seguir fornece etapas gerais para executar uma restauração de servidor. As etapas são igualmente aplicáveis para restaurar a partir de um backup ou restaurar para as configurações padrão de fábrica. Para obter instruções mais específicas, consulte a documentação do fabricante do servidor.

##### <a name="to-restore-or-reset-the-server-from-a-client-computer-using-the-recovery-dvd"></a>Para restaurar ou redefinir o servidor a partir de um computador cliente usando o DVD de recuperação

1.  Insira a mídia de recuperação do Windows Server Essentials Server que você recebeu do fabricante do servidor em um computador cliente.

     O Assistente para recuperar o servidor é aberto.

2.  Siga as instruções no Assistente para criar uma unidade flash inicializável USB que você usará para iniciar o servidor no modo de recuperação.

3.  Depois que o Assistente para recuperar o servidor prepara a unidade flash USB inicializável, insira a unidade flash no servidor e, em seguida, inicie o servidor no modo de recuperação. Para saber como iniciar o servidor no modo de recuperação, consulte a documentação do fabricante do hardware do servidor.

     Depois de você iniciar o servidor no modo de recuperação, o Assistente para recuperar o servidor localizará o servidor e estabelecerá uma conexão.

4.  Siga as instruções no Assistente para concluir a restauração do servidor.

> [!NOTE]
>  Esse método de recuperação de servidor ignora os dispositivos de armazenamento externo conectados ao servidor durante a recuperação. Se você deseja apagar os dados em um dispositivo de armazenamento externo, você deve fazer isso manualmente.

> [!NOTE]
>  Se você tiver criado pastas compartilhadas adicionais no servidor, depois que você restaurar os dados a partir do backup, as pastas compartilhadas adicionais não poderão ser reconhecidas pelo servidor. Você deve compartilhar essas pastas novamente. Para obter mais informações, consulte [Restaurar arquivos e pastas no servidor](Restore-or-repair-your-server-running-Windows-Server-Essentials.md#BKMK_RestoreFilesAndFolders).

##  <a name="restore-files-and-folders-on-the-server"></a><a name="BKMK_RestoreFilesAndFolders"></a>Restaurar arquivos e pastas no servidor
 Dependendo do método que você usou para restaurar ou reparar o servidor e o tipo de armazenamento que o servidor usa, poderá ser necessário recuperar os volumes de dados depois que você restaurar a unidade do sistema. Em alguns casos, você talvez precise compartilhar pastas existentes novamente para que o servidor as reconheça.

 A seguir estão alguns exemplos de quando você precisará restaurar arquivos e pastas:

-   [Restaurar arquivos e pastas de um backup de servidor](Restore-or-repair-your-server-running-Windows-Server-Essentials.md#BKMK_RestoreFilesFromBackup). Se você substituiu o disco do sistema ou não é possível ler as informações da partição no disco do sistema, você poderá restaurar o sistema, mas não será possível restaurar dados de outros volumes do disco. Para restaurar arquivos e pastas de outros volumes de dados, você deve usar o Assistente para restaurar pastas e arquivos.

-   [Restaurar pastas compartilhadas no servidor](Restore-or-repair-your-server-running-Windows-Server-Essentials.md#BKMK_ConfigreSharedFolders). Se você tiver criado pastas compartilhadas adicionais no servidor, depois que você restaurar a unidade do sistema do backup, as pastas compartilhadas ainda estarão na partição de dados ou foram restauradas para a partição de dados, mas não poderão ser reconhecidas pelo servidor. Você deve compartilhar essas pastas novamente.

###  <a name="restore-files-and-folders-from-a-server-backup"></a><a name="BKMK_RestoreFilesFromBackup"></a>Restaurar arquivos e pastas de um backup do servidor
 O Assistente para restauração de arquivos e pastas ajuda a proteger seus dados caso o disco rígido falhe ou os arquivos sejam apagados acidentalmente. Com o backup do Windows Server Essentials, você pode criar uma cópia de todos os dados no disco rígido e armazenar os dados em um dispositivo de armazenamento externo. Se os dados originais no disco rígido forem acidentalmente apagados, substituídos ou ficarem inacessíveis devido a falhas, você poderá restaurar os dados a partir do backup. O assistente para restauração de arquivos ou pastas ajuda a restaurar um único arquivo ou pasta, vários arquivos ou pastas ou todo o disco rígido de um backup existente.

 Após uma restauração do sistema, você talvez precise usar o Assistente para restaurar arquivos e pastas para restaurar arquivos e pastas que não foram mantidas durante a restauração. Por exemplo, se você substituiu o disco do sistema ou não é possível ler as informações da partição no disco do sistema, você não poderá restaurar os dados de outros volumes do disco do sistema.

> [!NOTE]
>  Não é possível usar o Assistente para restaurar arquivos e pastas para restaurar a unidade do sistema completo. Para obter informações sobre como restaurar o sistema completo, consulte [Restaurar ou reparar seu servidor usando a mídia de instalação](Restore-or-repair-your-server-running-Windows-Server-Essentials.md#BKMK_Restore_1) ou [Restaurar ou redefinir o servidor a partir de um computador cliente usando o DVD de recuperação](Restore-or-repair-your-server-running-Windows-Server-Essentials.md#BKMK_Restore_2).

##### <a name="to-restore-files-and-folders-from-a-server-backup"></a>Para restaurar arquivos e pastas de um backup do servidor

1.  Abra o painel do Windows Server Essentials e, em seguida, clique na guia **dispositivos** .

2.  Clique no nome do servidor e, em seguida, clique em **Restaurar arquivos ou pastas do servidor** no painel **Tarefas**.

     O assistente para restauração de arquivos e pastas é aberto.

3.  Siga as instruções no assistente para restaurar os arquivos ou pastas.

> [!WARNING]
>  Para obter mais informações sobre como fazer backup e restaurar arquivos e pastas, consulte [gerenciar backup e restauração](Manage-Backup-and-Restore-in-Windows-Server-Essentials.md).

###  <a name="restore-shared-folders-on-the-server"></a><a name="BKMK_ConfigreSharedFolders"></a>Restaurar pastas compartilhadas no servidor
 Depois de restaurar a unidade do sistema do servidor, se as pastas compartilhadas ainda estiverem na partição de dados ou forem restauradas para a partição de dados, talvez seja necessário configurar as pastas compartilhadas novamente para que o servidor reconheça as pastas. O procedimento a seguir descreve como adicionar pastas compartilhadas que foram compartilhadas antes.

##### <a name="to-add-an-existing-folder-to-the-server-shared-folders"></a>Para adicionar uma pasta existente no servidor de pastas compartilhadas

1.  No Explorador de Arquivos, localize a pasta compartilhada no disco rígido.

2.  Clique com o botão direito na pasta compartilhada, clique em **Propriedades**, clique na guia **Compartilhamento** e, em seguida, anote as permissões da pasta.

3.  Faça logon no painel do Windows Server Essentials, clique na guia **armazenamento** e, em seguida, clique em **Adicionar uma pasta** no painel de **tarefas pastas de servidor** .

     O Assistente Adicionar uma pasta é aberto.

4.  Digite um nome para o compartilhamento na caixa **Nome**.

5.  Clique em **procurar**, navegue até *<unidade \> \\<\> ServerName*\ServerFolders (por exemplo, *d:\Contoso\ServerFolders*), selecione a pasta que deseja compartilhar e clique em **OK**.

6.  Clique em **Avançar**.

7.  Especifique as permissões que você anotou na etapa 2 e clique em **Adicionar pasta**.

    > [!IMPORTANT]
    >  Essas permissões substituirão as permissões existentes que não foram adicionadas à pasta usando o painel do Windows Server Essentials.

> [!IMPORTANT]
>  Depois de concluir a adição de pastas à lista de pastas compartilhadas, certifique-se de que as pastas estejam incluídas no backup do servidor. Para obter informações sobre como adicionar pastas ao backup do servidor, consulte [Definir ou personalizar o backup do servidor](Set-up-or-customize-server-backup.md).

## <a name="additional-references"></a>Referências adicionais

-   [Gerenciar backup e restauração](Manage-Backup-and-Restore-in-Windows-Server-Essentials.md)

-   [Gerenciar o Windows Server Essentials](Manage-Windows-Server-Essentials.md)

-   [Utilizar o Windows Server Essentials](../use/Use-Windows-Server-Essentials.md)
