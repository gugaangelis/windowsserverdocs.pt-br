---
title: Restaurar ou reparar o servidor que executa o Windows Server Essentials
description: Descreve como usar o Windows Server Essentials
ms.custom: na
ms.date: 10/03/2016
ms.prod: windows-server-2016-essentials
ms.reviewer: na
ms.suite: na
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 27bf6f24-30c4-4935-9b24-069eb43e22f4
author: nnamuhcs
ms.author: coreyp
manager: dongill
ms.openlocfilehash: e1ebc539928d13b0d34dfe5a0ee57ce6e98088e9
ms.sourcegitcommit: 70c1b6cedad55b9c7d2068c9aa4891c6c533ee4c
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/03/2017
---
# <a name="restore-or-repair-your-server-running-windows-server-essentials"></a>Restaurar ou reparar o servidor que executa o Windows Server Essentials

>Aplica-se a: Windows Server 2016 Essentials, Windows Server 2012 R2 Essentials, Windows Server 2012 Essentials
 
 Este tópico fornece uma visão geral e dando suporte a procedimentos para restaurar ou reparar um servidor que executa o Windows Server Essentials e inclui as seguintes seções:  
  
-   [Visão geral de restauração do sistema de servidor](Restore-or-repair-your-server-running-Windows-Server-Essentials.md#BKMK_Overview)  
  
-   [Restaurar ou reparar a unidade do sistema](Restore-or-repair-your-server-running-Windows-Server-Essentials.md#BKMK_Restore)  
  
-   [Restaurar arquivos e pastas no servidor](Restore-or-repair-your-server-running-Windows-Server-Essentials.md#BKMK_RestoreFilesAndFolders)  
  
##  <a name="BKMK_Overview"></a>Visão geral de restauração do sistema de servidor  
 O estado do servidor quando você executa uma restauração afeta o método de restauração disponível e uma restauração como abrangente que você pode executar.  
  
 Os motivos mais comuns para restaurar um servidor são:  
  
-   Um vírus no servidor não pode ser inoculated ou excluído.  
  
-   As definições de configuração do servidor são ruins e não for possível iniciar o servidor.  
  
-   Você substituiu a unidade do sistema.  
  
-   Desativação de servidor, e você quer restaurar para um novo servidor.  
  
 Você pode restaurar ou o servidor de um backup, ou você pode restaurar o servidor para as configurações padrão de fábrica.  
  
-   [Restaurar o servidor a partir de um backup](Restore-or-repair-your-server-running-Windows-Server-Essentials.md#BKMK_RestoreFromBackup)  
  
-   [Redefinir o servidor para as configurações padrão de fábrica](Restore-or-repair-your-server-running-Windows-Server-Essentials.md#BKMK_FactoryReset)  
  
###  <a name="BKMK_RestoreFromBackup"></a>Restaurar o servidor a partir de um backup  
 Esta seção fornece orientação sobre qual tipo de backup para escolher.  
  
 Se um backup estiver disponível, a melhor opção para restaurar seu servidor é usar a mídia de instalação do fabricante s para restaurar a partir de um backup externo. A restauração irá recuperar as configurações de servidor e pastas usando o backup que você escolher. Você só precisa definir as configurações e restaurar dados criados após o backup.  
  
 Quando você escolhe recuperar o servidor pela restauração de um backup anterior, você deve escolher o backup específico que você deseja restaurar, e você deve ter um arquivo de backup válido em uma unidade de disco rígido externa conectada diretamente ao servidor:  
  
-   **Se você tiver um backup bem-sucedido muito recente do servidor**, e você sabe que o backup contém todos os seus dados críticos, a escolha é bastante simples. Você só precisará recriar os dados que foi criados após suas última boa backup e reconfigure configurações as alterações feitas após o backup.  
  
-   **Se você for restaurar seu servidor devido a um vírus**, selecione um backup que você sabe que ocorreu antes de receber o vírus. Talvez seja necessário voltar vários dias para selecionar um backup que é limpo.  
  
-   **Se você for restaurar seu servidor devido às configurações de configuração incorreta**, selecione um backup que você sabe que ocorreu antes da alteração que está causando o problema no servidor de configuração.  
  
 Quando você restaurar a partir de um backup, o processo exato e o acompanhamento necessário dependem do número de discos rígidos no servidor e se a unidade do sistema é substituída:  
  
-   **Se o servidor tem um único disco rígido e a unidade não é substituída**, as informações de partição de unidade permanece intactas quando você restaurar o servidor. O volume do sistema é restaurado e os dados no volume restante serão preservados.  
  
-   **Se o servidor tem um único disco rígido e a unidade é substituída**, o volume do sistema é restaurado e, em seguida, você deve restaurar manualmente pastas para o volume de dados. As pastas compartilhadas padrão não precisam ser criados porque eles não são criados quando o armazenamento do servidor é recriado.  
  
-   **Se o servidor tem vários discos rígidos e unidade 0 (contém o volume do sistema) não é substituída**, as informações de partição de unidade permanece intactas quando você restaurar o servidor. O volume do sistema é restaurado e os dados em todos os volumes restantes serão preservados.  
  
-   **Se o servidor tem vários discos rígidos e unidade 0 (contém o volume do sistema) é substituída**, o volume do sistema é restaurado e, em seguida, você deverá restaurar manualmente as pastas compartilhadas que foram armazenadas anteriormente no disco 0.  
  
###  <a name="BKMK_FactoryReset"></a>Redefinir o servidor para as configurações padrão de fábrica  
 Se você não tiver um backup que você pode restaurar de, ou por algum motivo você que ou precisa executar uma restauração completa do sistema sem restaurar a configuração do servidor anterior, você pode executar uma restauração que redefine o servidor para as configurações padrão de fábrica usando a mídia de instalação ou recuperação do fabricante do hardware de servidor.  
  
 Quando você restaurar seu servidor pelo redefini-lo com as configurações padrão de fábrica, todas as configurações existentes e os aplicativos instalados em seu servidor são excluídos, e você deve configurar seu servidor novamente. Depois que uma fábrica de restauração, seu servidor é reiniciado.  
  
 Quando você executa uma redefinição de fábrica, você pode optar por manter seus dados ou excluí-lo, com os seguintes efeitos:  
  
-   Se você optar por manter todos os seus dados, todos os dados no volume do sistema é excluído, mas os dados em outros volumes são mantidos.  
  
    > [!CAUTION]
    >  Se as configurações de disco não corresponder as configurações padrão, todos os dados em um disco serão excluídos. Se você substituiu o disco do sistema, o novo disco deve ser maior do que o volume do sistema s disco original.  
    >   
    >  Se as informações de partição para uma unidade do sistema são ilegíveis ou, se você substituir a unidade do sistema, todos os dados na unidade do sistema serão removidos, mesmo se você optar por manter todos os seus dados.  
  
-   Se você pretende desativarem ou reutilizarem o servidor, optar por excluir todos os seus dados. Além de configuração do servidor, outras configurações e os dados no volume do sistema, todos os outros dados são excluídos e todos os discos rígidos no servidor são reformatados.  
  
> [!NOTE]
>  Se espaços de armazenamento é configurado no servidor, antes de executar uma restauração de fábrica, você deve usar o **avançado** seção o **gerenciar espaços de armazenamento** console para remover manualmente todos os espaços de armazenamento.  
  
 Após uma fábrica de redefinição, você precisará executar as seguintes tarefas:  
  
-   **Reconfigure o servidor.** No servidor, use o Assistente para configurar servidor para inserir as definições de configuração. Para configurar um servidor Windows Server Essentials gerenciado remotamente em um computador cliente, abra um navegador da web e digite **http://***< YourServerName\ >* na barra de endereços.  
  
-   **Reconecte computadores cliente para o servidor.** Se um computador foi conectado anteriormente para o servidor, você deve desinstalar o software do Windows Server Essentials conector do computador antes de conectar o computador para o servidor novamente. Para obter mais informações, consulte [desinstalar o software do conector](../use/Get-Connected-in-Windows-Server-Essentials.md#BKMK_13) e [conectar computadores para o servidor](../use/Get-Connected-in-Windows-Server-Essentials.md#BKMK_9).  
  
##  <a name="BKMK_Restore"></a>Restaurar ou reparar a unidade do sistema  
 A primeira etapa na restauração do servidor é restaurar ou reparar a unidade do sistema de servidor. Depois de restaurar a unidade do sistema, você vai fazer tudo o que é necessário para restaurar as unidades de dados no servidor e restaurar qualquer compartilhamento foi perdida na restauração.  
  
 Três métodos estão disponíveis para executar a restauração:  
  
-   [Restaurar ou reparar seu servidor usando a mídia de instalação](Restore-or-repair-your-server-running-Windows-Server-Essentials.md#BKMK_Restore_1). Use a mídia de instalação do fabricante do servidor para restaurar a partir de um backup.  
  
-   **Usar a mídia de instalação para restaurar o servidor para as configurações padrão de fábrica**. Para saber como fazer isso em seu servidor, consulte a documentação do fabricante do servidor.  
  
-   [Restaurar ou restaurar seu servidor de um computador cliente usando a DVD de recuperação](Restore-or-repair-your-server-running-Windows-Server-Essentials.md#BKMK_Restore_2). Se você precisar restaurar um servidor de administração remota que está executando o Windows Server Essentials, você deve executar a restauração de um computador cliente usando a DVD de restauração do fabricante do servidor.  
  
###  <a name="BKMK_Restore_1"></a>Restaurar ou reparar seu servidor usando a mídia de instalação  
 O procedimento a seguir descreve como restaurar a unidade do sistema de servidor a partir de um backup usando a mídia de instalação do Windows Server Essentials. (Para saber como usar a mídia de instalação para restaurar as configurações padrão de fábrica, consulte a documentação do fabricante do servidor).  
  
> [!NOTE]
>  Se o servidor usa espaços de armazenamento, e você está restaurando os dados para um novo servidor, você deve recuperar a unidade do sistema pela primeira vez e faça logon no painel Windows Server Essentials, configurar espaços de armazenamento de maneira semelhante como no servidor antigo e, em seguida, recuperar os volumes de dados.  
  
##### <a name="to-restore-the-server-system-drive-from-a-backup-using-installation-media"></a>Para restaurar a unidade do sistema de servidor de um backup usando a mídia de instalação  
  
1.  Insira o DVD de instalação do Windows Server Essentials na unidade de DVD de servidor, reiniciar o servidor e, em seguida, pressione qualquer tecla para iniciar a partir do DVD.  
  
    > [!NOTE]
    >  Se o processo de restauração não começar automaticamente, verifique as configurações do BIOS do seu servidor para garantir que a unidade de DVD parece primeira no menu de inicialização.  
  
     - Ou -  
  
     Se o fabricante pré-carregados a mídia de instalação no servidor, pressione F8 no momento da inicialização para iniciar a partir da mídia de instalação.  
  
2.  Após o Windows Server arquivos carregar, escolha seu idioma e outras preferências e, em seguida, clique em **próxima**.  
  
3.  Na próxima página do assistente, clique em **reparar o computador**.  
  
    > [!CAUTION]
    >  Não escolha o **instalar agora** opção. Essa opção o guiará por meio de uma instalação completa do sistema que exclui todas as configurações e todos os dados na unidade do sistema.  
  
4.  Sobre o **escolha uma opção** página, clique em **solucionar**.  
  
5.  Sobre o **recuperação da imagem do sistema** página, selecione o sistema atual? qualquer **Windows Server Essentials** ou **Windows Server Essentials**.  
  
     O Assistente para recriar uma imagem o computador é aberta.  
  
6.  Sobre o **selecione um backup de imagem de sistema** página, você pode optar por usar o backup mais recente ou você pode selecionar um backup anterior. O sistema será restaurado para o estado que se encontrava no momento do backup que você escolher para restaurar ou reparar seu servidor. Dados que foi adicionados ou as alterações nas configurações que foram feitas após o backup foi salvo precisam ser recriadas.  
  
     Selecione uma das opções a seguir e clique em **próxima**:  
  
    -   **Usar a imagem de sistema mais recentes disponíveis (recomendada)**  
  
    -   **Selecione uma imagem do sistema**  
  
    > [!NOTE]
    >  Se você tiver um backup bem-sucedido muito recente do servidor, e você sabe que o backup contém todos os seus dados críticos, sua escolha é bastante simples. Você só precisará recriar os dados que foi criados após suas última boa backup e reconfigure configurações as alterações feitas após o backup.  
    >   
    >  Se você estiver restaurando seu servidor devido a um vírus, selecione um backup que você saiba que ocorreu antes de receber o vírus. Talvez seja necessário voltar vários dias para selecionar um backup que é limpo.  
    >   
    >  Se você estiver restaurando seu servidor devido às configurações de configuração incorreta, selecione um backup que você saiba que ocorreu antes da alteração da configuração de configuração que está causando o problema no servidor.  
  
7.  Siga as instruções no Assistente para concluir a restauração do sistema.  
  
8.  Depois que o servidor com êxito é restaurado, remova o DVD de instalação, se você usou uma e, em seguida, reiniciar o servidor.  
  
> [!NOTE]
>  Para restaurar e fazê-lo no servidor, talvez seja necessário executar etapas adicionais. Para obter mais informações, consulte [restaurar arquivos e pastas no servidor](Restore-or-repair-your-server-running-Windows-Server-Essentials.md#BKMK_RestoreFilesAndFolders).  
  
###  <a name="BKMK_Restore_2"></a>Restaurar ou restaurar seu servidor de um computador cliente usando a DVD de recuperação  
 No Windows Server Essentials, você pode iniciar o servidor de uma unidade flash USB inicializável que você cria e, em seguida, recuperar o servidor de um computador cliente usando a DVD que você recebeu do fabricante do servidor de recuperação. O computador cliente deve estar na mesma rede que o servidor. Esse método não está disponível no Windows Server Essentials.  
  
 O procedimento a seguir fornece etapas gerais para executar uma restauração do servidor. As etapas são igualmente aplicáveis para restaurar a partir de um ou restaurando para as configurações padrão de fábrica. Para obter instruções mais específicas, consulte a documentação do fabricante do servidor.  
  
##### <a name="to-restore-or-reset-the-server-from-a-client-computer-using-the-recovery-dvd"></a>Para restaurar o servidor de um computador cliente usando a DVD de recuperação  
  
1.  Insira a mídia de recuperação de servidor Windows Server Essentials que você recebeu do fabricante do servidor em um computador cliente.  
  
     Abre o Assistente para recuperar o servidor.  
  
2.  Siga as instruções no Assistente para criar uma unidade flash inicializável USB que você usará para iniciar o servidor no modo de recuperação.  
  
3.  Depois que o Assistente para recuperar o servidor prepara a unidade flash USB inicializável, insira a unidade flash no servidor e, em seguida, inicie o servidor no modo de recuperação. Para saber como iniciar o servidor no modo de recuperação, consulte a documentação do fabricante do hardware do servidor.  
  
     Depois de iniciar o servidor no modo de recuperação, o Assistente para recuperar o servidor localizará o servidor e, em seguida, estabelece uma conexão.  
  
4.  Siga as instruções no Assistente para concluir a restauração do servidor.  
  
> [!NOTE]
>  Esse método de recuperação de servidor ignora os dispositivos de armazenamento externo que estão conectados ao servidor durante a recuperação. Se você quiser apagar os dados em um dispositivo de armazenamento externo, você deve fazer isso manualmente.  
  
> [!NOTE]
>  Se você criou adicionais pastas compartilhadas no servidor, depois que você restaure os dados a partir do backup, as pastas compartilhadas adicionais podem não ser reconhecidas pelo servidor. Você deve compartilhar essas pastas novamente. Para obter mais informações, consulte [restaurar arquivos e pastas no servidor](Restore-or-repair-your-server-running-Windows-Server-Essentials.md#BKMK_RestoreFilesAndFolders).  
  
##  <a name="BKMK_RestoreFilesAndFolders"></a>Restaurar arquivos e pastas no servidor  
 Dependendo do método que você usou para restaurar ou reparar o seu servidor e usa o tipo de armazenamento do servidor, você talvez precise recuperar os volumes de dados, depois de restaurar a unidade do sistema. Em alguns casos, você talvez precise compartilhar pastas existentes novamente para que o servidor reconhece-los.  
  
 A seguir é alguns exemplos de quando você talvez precise restaurar arquivos e pastas:  
  
-   [Restaurar arquivos e pastas de um backup do servidor](Restore-or-repair-your-server-running-Windows-Server-Essentials.md#BKMK_RestoreFilesFromBackup). Se você substituiu o disco do sistema, ou se as informações de partição no disco do sistema fica ilegíveis, você poderá restaurar o sistema, mas você não pode restaurar dados de outros volumes do disco. Para restaurar arquivos e pastas de outros volumes de dados, você deve usar o Assistente de pastas e restaurar arquivos.  
  
-   [Restaurar pastas compartilhadas no servidor](Restore-or-repair-your-server-running-Windows-Server-Essentials.md#BKMK_ConfigreSharedFolders). Se você criou adicionais pastas compartilhadas no servidor, depois de restaurar a unidade do sistema usando o backup, as pastas compartilhadas são ainda na partição de dados ou foram restauradas para a partição de dados, mas não podem ser reconhecidas pelo servidor. Você deve compartilhar essas pastas novamente.  
  
###  <a name="BKMK_RestoreFilesFromBackup"></a>Restaurar arquivos e pastas de um backup do servidor  
 Restaurar arquivos e pastas assistente ajuda a proteger seus dados se o disco rígido para de funcionar ou seus arquivos são apagados acidentalmente. Com o Backup do Windows Server Essentials, você pode criar uma cópia de todos os dados em seu disco rígido e armazenar os dados em um dispositivo de armazenamento externo. Se os dados originais no disco rígido é acidentalmente apagados, sobrescritos ou ficarão inacessíveis devido a uma falha, você poderá restaurar os dados usando o backup. A restaurar arquivos ou pastas assistente ajuda você a restaurar um único arquivo ou pasta, vários arquivos ou pastas ou todo o disco rígido de um backup existente.  
  
 Após uma restauração do sistema, você talvez precise usar o Assistente de pastas e restaurar arquivos para restaurar arquivos e pastas que não foram mantidas durante a restauração. Por exemplo, se você substituiu o disco do sistema, ou se as informações de partição no disco do sistema fica ilegíveis, você não pode restaurar dados de outros volumes no disco do sistema.  
  
> [!NOTE]
>  Você não pode usar o Assistente de pastas e arquivos de restauração para restaurar a unidade do sistema completo. Para obter informações sobre como restaurar o sistema completo, consulte [restaurar ou reparar seu servidor usando a mídia de instalação](Restore-or-repair-your-server-running-Windows-Server-Essentials.md#BKMK_Restore_1) ou [restaurar ou redefinir seu servidor de um computador cliente usando a DVD de recuperação](Restore-or-repair-your-server-running-Windows-Server-Essentials.md#BKMK_Restore_2).  
  
##### <a name="to-restore-files-and-folders-from-a-server-backup"></a>Para restaurar arquivos e pastas de um backup do servidor  
  
1.  Abrir o painel do Windows Server Essentials e clique no **dispositivos** guia.  
  
2.  Clique no nome do servidor e, em seguida, clique em **restaurar arquivos ou pastas para o servidor** no **tarefas** painel.  
  
     O abre restaurar arquivos e pastas assistente.  
  
3.  Siga as instruções no Assistente para restaurar os arquivos ou pastas.  
  
> [!WARNING]
>  Para obter mais informações sobre como fazer backup e restaurando arquivos e pastas, consulte [Gerenciar Backup e restauração](Manage-Backup-and-Restore-in-Windows-Server-Essentials.md).  
  
###  <a name="BKMK_ConfigreSharedFolders"></a>Restaurar pastas compartilhadas no servidor  
 Depois de restaurar a unidade do sistema de servidor s, se pastas compartilhadas são ainda na partição de dados ou foram restauradas para a partição de dados, talvez seja necessário configurar as pastas compartilhadas novamente para o servidor reconhecer as pastas. O procedimento a seguir descreve como adicionar pastas compartilhadas que foram compartilhado antes.  
  
##### <a name="to-add-an-existing-folder-to-the-server-shared-folders"></a>Para adicionar uma pasta existente para o servidor de pastas compartilhadas  
  
1.  No Explorador de arquivos, localize a pasta compartilhada no disco rígido.  
  
2.  Clique com botão direito na pasta compartilhada, clique em **propriedades**, clique no **compartilhamento** guia e, em seguida, escreva as permissões de pasta.  
  
3.  Fazer logon no Windows Server Essentials painel, clique no **armazenamento** guia e, em seguida, clique em **adicionar uma pasta** no **tarefas de pastas de servidor** painel.  
  
     Adicionar um Assistente de pasta é aberta.  
  
4.  Digite um nome para o compartilhamento no **nome** caixa.  
  
5.  Clique em **procurar**, navegue até *< drive\ > \ \ < ServerName\ >*\ServerFolders (por exemplo *d:\Contoso\ServerFolders*), selecione a pasta que você deseja compartilhar e, em seguida, clique em **Okey**.  
  
6.  Clique em **próxima**.  
  
7.  Especifique as permissões que você anotou na etapa 2 e, em seguida, clique em **adicionar pasta**.  
  
    > [!IMPORTANT]
    >  Essas permissões substituirá qualquer permissão existente que não foram adicionado à pasta usando o painel do Windows Server Essentials.  
  
> [!IMPORTANT]
>  Depois que terminar de adicionar pastas à lista de pastas compartilhadas, certifique-se de que as pastas estão incluídas no backup do servidor. Para obter informações sobre como adicionar pastas para o backup do servidor, consulte [definido para cima e para personalizar o backup do servidor](Set-up-or-customize-server-backup.md).  
  
## <a name="see-also"></a>Consulte também  
  
-   [Gerenciar o Backup e restauração](Manage-Backup-and-Restore-in-Windows-Server-Essentials.md)  
  
-   [Gerenciar o Windows Server Essentials](Manage-Windows-Server-Essentials.md)  
  
-   [Usar o Windows Server Essentials](../use/Use-Windows-Server-Essentials.md)
