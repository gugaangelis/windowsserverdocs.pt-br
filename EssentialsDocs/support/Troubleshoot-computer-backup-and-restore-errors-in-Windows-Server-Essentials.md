---
title: Solucionar problemas de backup do computador e restaurar erros no Windows Server Essentials
description: Descreve como usar o Windows Server Essentials
ms.custom: na
ms.date: 06/25/2013
ms.prod: windows-server-2016-essentials
ms.reviewer: na
ms.suite: na
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 5cc73aff-d2c0-4cf9-a23d-ef928ae5ddc9
author: nnamuhcs
ms.author: coreyp
manager: dongill
ms.openlocfilehash: 37e79661442ba9f66a564b6c6c8fb57db1978454
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 12/12/2017
---
# <a name="troubleshoot-computer-backup-and-restore-errors-in-windows-server-essentials"></a>Solucionar problemas de backup do computador e restaurar erros no Windows Server Essentials

>Aplica-se a: Windows Server 2016 Essentials, Windows Server 2012 R2 Essentials, Windows Server 2012 Essentials

Use estes procedimentos para solucionar problemas de backups de computador no Windows Server Essentials, incluindo problemas de configuração de backup, incompletos ou malsucedidos backups, alertas de integridade de backup e problemas com o arquivo, pasta ou todo o sistema restaura.  
  
> [!NOTE]
>  Para obter as informações de solução de problemas mais recentes da comunidade do Windows Server Essentials, visite o [Fórum do Windows Server Essentials](https://social.technet.microsoft.com/Forums//winserveressentials/threads).  
  
##  <a name="BKMK_TroubleshootBackupConfigurationIssues"></a>Solucionar problemas de configuração de backup para um computador conectado  
 Use estes procedimentos para solucionar problemas com as configurações de backup para computadores que são armazenados em backup no seu servidor Windows Server Essentials.  
  
### <a name="errors"></a>Erros  
  
-   Configuração de backup não foi concluída com êxito  
  
-   A coleta de informações de erro para o computador  
  
-   Computador de remoção de erro de backup  
  
### <a name="resolutions"></a>Resoluções  
  
##### <a name="to-troubleshoot-errors-that-occur-while-you-configure-backups-for-a-connected-computer"></a>Para solucionar os erros que ocorrem enquanto você configura os backups para um computador conectado  
  
1.  Verifique se que o computador estiver conectado à rede por meio de um dispositivo de rede.  
  
2.  Verifique se o dispositivo de rede que o computador estiver conectado a também está conectado à rede, ligada e funcionando corretamente.  
  
3.  Certifique-se de que o serviço de Backup do cliente do Windows Server e o serviço de provedor de Backup do Windows Server cliente computador estiver executando no servidor.  
  
    ###### <a name="to-start-computer-backup-services-on-the-server"></a>Para iniciar os serviços de backup do computador no servidor  
  
    1.  No servidor, clique em **iniciar**, clique em **ferramentas administrativas**e clique em **serviços**.  
  
    2.  Role para baixo e clique em **provedor de serviço do Windows Server cliente computador Backup**. Se o status do serviço não for **iniciado**, clique com botão direito do serviço e, em seguida, clique em **iniciar**.  
  
    3.  Clique em **serviço Backup do computador do cliente Windows Server**. Se o status do serviço não for **iniciado**, clique com botão direito do serviço e, em seguida, clique em **iniciar**.  
  
    4.  Fechar **serviços**.  
  
4.  Certifique-se de que o serviço de provedor de Backup do Windows Server cliente computador está em execução no computador cliente.  
  
    ###### <a name="to-start-the-computer-backup-service-on-the-client-computer"></a>Para iniciar o serviço de backup do computador no computador cliente  
  
    1.  No computador cliente, clique em **iniciar**, tipo **serviços** no **pesquisar programas e arquivos** caixa de texto e pressione Enter.  
  
    2.  Role para baixo e clique em **provedor de serviço do Windows Server cliente computador Backup**. Se o status do serviço não for **iniciado**, clique com botão direito do serviço e, em seguida, clique em **iniciar**.  
  
    3.  Fechar **serviços**.  
  
5.  Verifique os alertas para determinar se há erros no banco de dados de backup. Se houver erros, siga as instruções no alerta para reparar o banco de dados de backup.  
  
6.  Desinstalar o software do Windows Server Essentials conector do computador e reinstalá-lo. Para obter mais informações, consulte os tópicos [desinstalar o software do conector](../use/Get-Connected-in-Windows-Server-Essentials.md#BKMK_13) e [instalar o software do conector](../use/Get-Connected-in-Windows-Server-Essentials.md#BKMK_11).  
  
##  <a name="BKMK_TroubleshootaBackupThatDidNotCompleteProperly"></a>Solucionar problemas de um backup que não foi concluída corretamente  
 Quando um backup tem status malsucedida, nenhuma parte do backup foi bem-sucedida e os dados não estão disponíveis para a restauração. No entanto, quando um backup tem status incompleto, nem todos os itens especificado no backup configuração incluídos no backup, mas alguns dados podem estar disponíveis para a restauração.  
  
### <a name="errors"></a>Erros  
  
-   Backup está incompleto  
  
-   Backup malsucedida  
  
### <a name="resolutions"></a>Resoluções  
  
##### <a name="to-identify-volumes-that-were-not-backed-up-successfully"></a>Para identificar os volumes que não foram feitos com êxito  
  
1.  Abrir o painel do Windows Server Essentials e clique em **computadores e Backup**.  
  
2.  Clique no nome do computador onde backup não for concluída com êxito e, em seguida, clique em **exibir as propriedades de computador** no **tarefas** painel.  
  
3.  Clique no backup que não for concluída com êxito e, em seguida, clique em **exibir detalhes**.  
  
4.  No **Backup detalhes** caixa de diálogo, uma marca de seleção verde é exibida no status de cada volume que foi feito com êxito.  
  
##### <a name="to-troubleshoot-an-unsuccessful-backup-of-a-volume"></a>Para solucionar um backup malsucedido de um volume  
  
1.  Certifique-se de que o disco rígido está conectado ao computador, ligado e funcionando corretamente.  
  
2.  Executar **chkdsk /f /r** corrigir os erros no disco rígido (**/f**) e recupere informações legíveis de todos os setores defeituosos (**/r**). Para obter mais informações sobre como executar **chkdsk**, consulte [CHKDSK](https://go.microsoft.com/fwlink/?LinkId=206562).  
  
3.  Certifique-se de que o computador não foi desligado ou desconectado da rede enquanto estava executando o backup.  
  
4.  Verifique se há espaço livre suficiente em cada volume para backup para ser executado. Backup requer espaço em disco adicional no computador cliente para criar um instantâneo VSS. Em qualquer volume que não é reservado do sistema, pelo menos 10% do espaço em disco disponível devem ser gratuito. Em um volume de sistema reservado, se o tamanho do volume tem menos de 500 MB, VSS requer 32 MB de espaço livre criar um instantâneo; Se o volume for 500 MB ou superior, VSS requer 320 MB de espaço livre.  
  
     Se houver espaço livre suficiente em um volume, tente um dessas resoluções:  
  
    -   Estenda o volume. Você pode ampliar qualquer volume básico ou dinâmico, exceto o volume do sistema reservado.  
  
        ###### <a name="to-extend-a-volume"></a>Para estender um volume  
  
        1.  No painel de controle, clique em **sistema e segurança**.  
  
        2.  Em **ferramentas administrativas**, clique em **criar e formatar partições do disco rígido**.  
  
        3.  Clique com botão direito no volume que você deseja estender. Se **estender Volume** é habilitado, selecione essa opção. Se a opção não estiver habilitada, você não pode estender o volume.  
  
        4.  Siga as etapas no Assistente de Volume estender para estender o volume.  
  
    -   Exclua o conteúdo do volume para liberar mais espaço.  
  
        > [!NOTE]
        >  Se você precisar liberar espaço no volume do sistema reservado, você pode mover a imagem de recuperação do sistema para um volume diferente. Para obter instruções, consulte [implantar uma imagem de recuperação do sistema](https://technet.microsoft.com/library/dd744280\(v=ws.10\).aspx).  
  
    -   Exclua o volume usando o backup do cliente. Faça isso somente se ele não é importante para você manter uma cópia de backup dos dados no volume.  
  
        > [!WARNING]
        >  Se você excluir o volume do sistema reservado de um backup do cliente, o sistema de cliente não será feito backup, e não será capaz de executar uma restauração completa do sistema no computador.  
  
5.  Procure outras alertas no servidor que pode indicar que não há espaço em disco suficiente no servidor de backup seja concluído com êxito. Siga as instruções no alerta para corrigir o problema.  
  
6.  Executar **vssadmin** em um prompt de comando para solucionar problemas de Volume Shadow Copy Service (VSS) emite. Para obter informações sobre **vssadmin**, consulte [VSSADMIN](https://go.microsoft.com/fwlink/?LinkID=94332).  
  
##  <a name="BKMK_TroubleshootBackupHealthAlertIssues"></a>Solucionar problemas de alerta de integridade de backup  
  
### <a name="errors"></a>Erros  
  
-   O serviço de provedor do Windows Server Solutions computador Backup parou de funcionar  
  
-   O serviço de provedor do Windows Server Solutions computador cliente Backup parou de funcionar  
  
### <a name="resolutions"></a>Resoluções  
  
##### <a name="to-troubleshoot-a-backup-health-alert"></a>Para solucionar um alerta de integridade de backup  
  
1.  Se um alerta informa que o banco de dados de backup tem problemas, siga as instruções no alerta para corrigir o problema.  
  
2.  Se um alerta informa que um serviço de backup não está funcionando, tente iniciar o serviço no servidor ou o computador cliente que recebeu a mensagem de erro.  
  
    ###### <a name="to-start-backup-services-on-the-server"></a>Para iniciar os serviços de backup no servidor  
  
    1.  No servidor, clique em **iniciar**e clique em **ferramentas administrativas**e clique em **serviços**.  
  
        > [!NOTE]
        >  Se você estiver administrando o servidor remotamente, você deve usar a Conexão de área de trabalho remota para acessar a área de trabalho do servidor. Para obter informações sobre como usar a Conexão de área de trabalho remota, consulte [conectar a outro computador usando a Conexão de área de trabalho remota](https://windows.microsoft.com/windows-vista/Connect-to-another-computer-using-Remote-Desktop-Connection).  
  
    2.  Role para baixo e clique em **provedor de serviço do Windows Server cliente computador Backup**. Se o status do serviço não for **iniciado**, clique com botão direito do serviço e, em seguida, clique em **iniciar**.  
  
    3.  Clique em **serviço Backup do computador do cliente Windows Server**. Se o status do serviço não for **iniciado**, clique com botão direito do serviço e, em seguida, clique em **iniciar**.  
  
    4.  Fechar **serviços**.  
  
    ###### <a name="to-start-backup-services-on-a-client-computer"></a>Para iniciar os serviços de backup em um computador cliente  
  
    1.  No computador cliente, clique em **iniciar**, tipo **serviços** no **pesquisar programas e arquivos** caixa de texto e clique em ENTER.  
  
    2.  Clique com botão direito **provedor de serviço do Windows Server cliente computador Backup**e clique em **iniciar**.  
  
    3.  Fechar o **serviços**.  
  
3.  Verifique os logs de eventos no computador cliente ou servidor para problemas referentes a serviços de backup ou drivers.  
  
4.  Reinicialize o servidor ou computador cliente que recebeu a mensagem de erro.  
  
5.  Verifique os alertas de integridade para outros problemas que podem ter um efeito no backup do cliente.  
  
##  <a name="BKMK_FileAndFolder"></a>Solucionar problemas de uma restauração de arquivo ou pasta  
  
### <a name="errors"></a>Erros  
  
-   Restauração de arquivo ou pasta não foi concluída com êxito.  
  
### <a name="resolutions"></a>Resoluções  
  
##### <a name="to-troubleshoot-an-unsuccessful-file-or-folder-restore"></a>Para solucionar uma restauração de arquivo ou pasta malsucedida  
  
1.  Verifique se que o computador estiver conectado à rede por meio de um dispositivo de rede.  
  
2.  Verifique se o dispositivo de rede que o computador estiver conectado a também está conectado à rede, ligada e funcionando corretamente.  
  
3.  Verifique os alertas para determinar se há erros no banco de dados de backup. Se houver erros, siga as instruções no alerta para reparar o banco de dados de backup.  
  
4.  Tente restaurar os arquivos ou pastas a partir de outro backup.  
  
5.  Certifique-se de que o **Driver restaurar o Windows Server solução computador** está instalado e funcionando corretamente.  
  
    ###### <a name="to-check-the-status-of-the-windows-server-solution-computer-restore-driver"></a>Para verificar o status do Windows Server solução computador restaurar Driver  
  
    1.  Clique em **iniciar**, tipo **Gerenciador de dispositivos** no **pesquisar programas e arquivos** caixa de texto e clique em ENTER.  
  
    2.  No Gerenciador de dispositivos, clique em **dispositivos do sistema**, role até **Windows Server Solutions computador restaurar Driver**.  
  
    3.  Se o driver não for exibido:  
  
        1.  Abra um prompt de comando com privilégios de administrador e execute o seguinte comando:  
  
             **%ProgramFiles%\Windows Server\Bin\BackupDriverInstaller.exe?  Eu**  
  
        2.  Atualize o Gerenciador de dispositivos. O driver deve aparecer.  
  
    4.  Se o ícone é exibido um monitor de computador, o driver é instalado e executado corretamente. Feche o Gerenciador de dispositivos.  
  
    5.  Se o ícone seja exibido não for um monitor de computador  
  
        1.  Clique com botão direito **Windows Server Solutions computador restaurar Driver**e clique em **propriedades**.  
  
        2.  Clique no **Driver** guia e, em seguida, clique em **Atualizar Driver**.  
  
        3.  Clique em **pesquisar automaticamente software de driver atualizado de**e siga as instruções para atualizar o driver.  
  
    6.  Feche o Gerenciador de dispositivos.  
  
6.  Desinstalar o software do Windows Server Essentials conector do computador e reinstalá-lo. Para obter mais informações, consulte os tópicos [desinstalar o software do conector](../use/Get-Connected-in-Windows-Server-Essentials.md#BKMK_13) e [instalar o software do conector](../use/Get-Connected-in-Windows-Server-Essentials.md#BKMK_11).  
  
##  <a name="BKMK_Troubleshootfullsystemrestoreissues"></a>Solucionar problemas de uma restauração completa do sistema  
  
### <a name="errors"></a>Erros  
  
-   Não é possível fazer logon no computador cliente após uma restauração completa do sistema.  
  
### <a name="resolutions"></a>Resoluções  
 Se você alterar o nome de um computador, e você precisa restaurar um backup que foi salvo antes que o nome do computador alterado, após a restauração, quando você tenta fazer logon em sua conta de domínio, você receberá esse erro: "o banco de dados de segurança no servidor não tem uma conta de computador para essa relação de confiança de estação de trabalho." Para obter acesso à rede para o computador novamente, remova o software do conector, remover o computador do domínio do Windows e, em seguida, conectar ao servidor novamente.  
  
##### <a name="to-regain-network-access-to-a-restored-computer-after-a-computer-name-change"></a>Para recuperar o acesso à rede para um computador restaurado após uma mudança de nome de computador  
  
1.  Faça logon no computador como um administrador local.  
  
2.  Desinstale o software do conector. Para obter mais informações, consulte [desinstalar o software do conector](../use/Get-Connected-in-Windows-Server-Essentials.md#BKMK_13).  
  
3.  Remova o computador do domínio. Para obter mais informações, consulte [remover um computador de um domínio do Windows](../use/Get-Connected-in-Windows-Server-Essentials.md#BKMK_8).  
  
4.  Conecte o computador para o servidor novamente. Para obter mais informações, consulte [conectar computadores para o servidor](../use/Get-Connected-in-Windows-Server-Essentials.md#BKMK_9).  
  
## <a name="see-also"></a>Consulte também  
  
-   [Suporte ao Windows Server Essentials](Support-Windows-Server-Essentials.md)