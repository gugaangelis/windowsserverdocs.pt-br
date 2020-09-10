---
title: Solucionar problemas referentes a erros de backup restauração do computador no Windows Server Essentials
description: Descreve como usar o Windows Server Essentials
ms.date: 06/25/2013
ms.topic: article
ms.assetid: 5cc73aff-d2c0-4cf9-a23d-ef928ae5ddc9
author: nnamuhcs
ms.author: geschuma
manager: mtillman
ms.openlocfilehash: ce2d6d51f230d4a06f2e1b9acfd378d61bfaf863
ms.sourcegitcommit: db2d46842c68813d043738d6523f13d8454fc972
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/10/2020
ms.locfileid: "89625105"
---
# <a name="troubleshoot-computer-backup-and-restore-errors-in-windows-server-essentials"></a>Solucionar problemas referentes a erros de backup restauração do computador no Windows Server Essentials

> Aplica-se a: Windows Server 2016 Essentials, Windows Server 2012 R2 Essentials, Windows Server 2012 Essentials

Use estes procedimentos para solução de problemas de backups de computador no Windows Server Essentials, incluindo problemas de configuração de backup, os backups incompletos ou malsucedidos, alertas de integridade de backup e problemas com arquivos, pastas ou restaurações de todo o sistema.

> [!NOTE]
> Para obter as informações de solução de problemas mais recentes da Comunidade do Windows Server Essentials, visite o [Fórum do Windows Server Essentials](/answers/topics/windows-server-essentials.html).

## <a name="troubleshoot-backup-configuration-issues-for-a-connected-computer"></a>Solucionar problemas de configuração de backup para um computador conectado

Use estes procedimentos para solucionar problemas com as configurações de backup para computadores que estão incluídos no backup em seu servidor do Windows Server Essentials.

### <a name="errors"></a>Errors

- Configuração de backup não foi concluído com êxito

- Erro na coleta de informações para o computador

- Erro ao remover o computador do backup

### <a name="resolutions"></a>Resoluções

#### <a name="to-troubleshoot-errors-that-occur-while-you-configure-backups-for-a-connected-computer"></a>Para solucionar problemas de erros que ocorrem enquanto você configurar backups para um computador conectado

1. Verifique se que o computador está conectado à rede por meio de um dispositivo de rede.

2. Verifique se o dispositivo de rede que o computador está conectado a também está conectado à rede, ligado e funcionando corretamente.

3. Certifique-se de que o serviço de backup do computador cliente do Windows Server e o Serviço de provedor de backup do computador cliente do Windows Server estão sendo executados no servidor.

#### <a name="to-start-computer-backup-services-on-the-server"></a>Para iniciar os serviços de backup do computador no servidor

1. No servidor, clique em **Iniciar**, clique em **Ferramentas Administrativas** e clique em **Serviços**.

2. Role para baixo e clique em **Serviço de provedor de backup do computador cliente do Windows Server**. Se o status do serviço não for **Iniciado**, clique com o botão direito no serviço e depois clique em **Iniciar**.

3. Clique em **Serviço de backup do computador cliente do Windows Server**. Se o status do serviço não for **Iniciado**, clique com o botão direito no serviço e depois clique em **Iniciar**.

4. Feche **Serviços**.

5. Certifique-se de que o Serviço de provedor de backup do computador cliente do Windows Server está sendo executado no computador cliente.

#### <a name="to-start-the-computer-backup-service-on-the-client-computer"></a>Para iniciar o serviço de backup do computador no computador cliente

1. No computador cliente, clique em **Iniciar**, digite **Serviços** na caixa de texto **Pesquisar programas e arquivos** e pressione Enter.

2. Role para baixo e clique em **Serviço de provedor de backup do computador cliente do Windows Server**. Se o status do serviço não for **Iniciado**, clique com o botão direito no serviço e depois clique em **Iniciar**.

3. Feche **Serviços**.

4. Verifique os alertas para determinar se há erros no banco de dados de backup. Se houver erros, siga as instruções no alerta para reparar o banco de dados de backup.

5. Desinstale o software do Windows Server Essentials Connector do computador e então reinstale-o. Para obter mais informações, consulte os tópicos [Desinstalar o software Connector](../use/Get-Connected-in-Windows-Server-Essentials.md#BKMK_13) e [Instalar o software Connector](../use/Get-Connected-in-Windows-Server-Essentials.md#BKMK_11).

## <a name="troubleshoot-a-backup-that-did-not-complete-properly"></a>Solucionar problemas de um backup que não foi concluído corretamente

Quando um backup tem status Malsucedido, nenhuma parte do backup foi bem-sucedida e nenhum dado está disponível para você realizar a restauração. No entanto, quando um backup tem status Incompleto, nem todos os itens especificados na configuração do backup tiveram seu backup feito, mas alguns dados podem estar disponíveis para você realizar a restauração.

### <a name="errors"></a>Errors

- O backup é incompleto

- Backup malsucedido

### <a name="resolutions"></a>Resoluções

#### <a name="to-identify-volumes-that-were-not-backed-up-successfully"></a>Para identificar os volumes cujo backup não foram feito com êxito

1. Abra o Painel do Windows Server Essentials e, em seguida, clique em **Computadores e Backup**.

2. Clique no nome do computador em que backup não concluído com êxito e, em seguida, clique em **Exibir as propriedades do computador** no painel **Tarefas**.

3. Clique no backup que não foi concluído com êxito e, em seguida, clique em **Exibir detalhes**.

4. Na caixa de diálogo **Detalhes de Backup**, uma marca de seleção verde é exibida no status de todos os volumes cujo backup foi realizado com êxito.

#### <a name="to-troubleshoot-an-unsuccessful-backup-of-a-volume"></a>Para solucionar problemas de um backup malsucedido de um volume

1. Certifique-se de que o disco rígido está conectado ao computador, ligado e funcionando corretamente.

2. Execute **chkdsk /f /r** para corrigir quaisquer erros no disco rígido (**/f**) e recupere informações que podem ser lidas a partir de eventuais setores ruins (**/r**). Para obter mais informações sobre a execução do **chkdsk**, consulte [CHKDSK](https://go.microsoft.com/fwlink/?LinkId=206562).

3. Certifique-se de que o computador não foi desligado ou desconectado da rede durante a execução do backup.

4. Verifique se há espaço livre suficiente em cada volume para que o backup seja executado. O backup exige espaço em disco adicional no computador cliente para criar uma imagem instantânea VSS. Em qualquer volume que não é reservado do sistema, pelo menos 10% do espaço em disco disponível deve estar livre. Em um volume de sistema reservado, se o tamanho do volume é menor do que 500 MB, o VSS requer 32 MB de espaço livre criar uma imagem instantânea; se o volume for 500 MB ou maior, o VSS requer 320 MB de espaço livre.

    Se não houver espaço livre suficiente disponível em um volume, tente uma dessas soluções:

    - Estenda o volume. Você pode estender qualquer volume básico ou dinâmico, exceto o volume reservado pelo sistema.

        **Para estender um volume**

        1. No painel de controle, clique em **Sistema e segurança**.

        2. Em **Ferramentas Administrativas**, clique em **Criar e formatar partições de disco rígido**.

        3. Clique com o botão direito no volume que você deseja estender. Se a opção **Estender volume** está habilitada, selecione essa opção. Se a opção não estiver habilitada, você não pode estender o volume.

        4. Para estender o volume, siga as etapas do Assistente para estender volume.

    - Exclua o conteúdo do volume para disponibilizar mais espaço.

        > [!NOTE]
        > Se você precisar liberar espaço no volume do sistema reservado, você pode mover a imagem de recuperação do sistema para um volume diferente. Para instruções, consulte [Implantar uma imagem de recuperação de sistema](/previous-versions/windows/it-pro/windows-7/dd744280(v=ws.10)).

    - Exclua o volume do backup do cliente. Faça isso apenas se não é importante para você manter uma cópia de backup dos dados no volume.

        > [!WARNING]
        > Se você excluir o volume reservado pelo sistema de um backup do cliente, o backup do sistema do cliente não será feito e você não poderá realizar uma restauração completa do sistema no computador.

5. Verifique se há outros alertas no servidor que podem indicar que não há espaço em disco suficiente no servidor para que o backup seja concluído com êxito. Siga as instruções no alerta para corrigir o problema.

6. Execute **vssadmin** em um prompt de comando para solucionar problemas do Volume Shadow Copy Service (VSS). Para obter informações sobre o **vssadmin**, consulte [VSSADMIN](/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/cc754968(v=ws.10)).

## <a name="troubleshoot-backup-health-alert-issues"></a>Solucionar problemas de alerta de integridade de backup

### <a name="errors"></a>Errors

- O serviço de provedor de backup do computador do Windows Server Solutions parou de funcionar

- O serviço de provedor de backup do computador cliente do Windows Server Solutions parou de funcionar

### <a name="resolutions"></a>Resoluções

#### <a name="to-troubleshoot-a-backup-health-alert"></a>Para solucionar problemas de um alerta de integridade de backup

1. Se um alerta informa que o banco de dados de backup tem problemas, siga as instruções no alerta para corrigir o problema.

2. Se um alerta informa que um serviço de backup não está em execução, tente iniciar o serviço no servidor ou no computador cliente em que você recebeu a mensagem de erro.

#### <a name="to-start-backup-services-on-the-server"></a>Para iniciar os serviços de backup no servidor * *

1. No servidor, clique em **Iniciar**, clique em **Ferramentas Administrativas** e clique em **Serviços**.

    > [!NOTE]
    >  Se você estiver administrando o servidor remotamente, você deve usar a conexão de área de trabalho remota para acessar a área de trabalho do servidor. Para obter informações sobre como usar a Conexão de Área de Trabalho Remota, consulte [Conectar-se a outro computador usando a Conexão de Área de Trabalho Remota](https://support.microsoft.com/help/4028379/windows-10-how-to-use-remote-desktop).

2. Role para baixo e clique em **Serviço de provedor de backup do computador cliente do Windows Server**. Se o status do serviço não for **Iniciado**, clique com o botão direito no serviço e depois clique em **Iniciar**.

3. Clique em **Serviço de backup do computador cliente do Windows Server**. Se o status do serviço não for **Iniciado**, clique com o botão direito no serviço e depois clique em **Iniciar**.

4. Feche **Serviços**.

##### <a name="to-start-backup-services-on-a-client-computer"></a>Para iniciar serviços de backup em um computador cliente

1. No computador cliente, clique em **Iniciar**, digite **Serviços** na caixa de texto **Pesquisar programas e arquivos** e pressione ENTER.

2. Clique com botão direito em **Serviço de provedor de backup do computador cliente do Windows Server** e clique em **Iniciar**.

3. Feche os **Serviços**.

4. Verifique os logs de eventos no computador cliente ou servidor para problemas relacionados a serviços de backup ou drivers.

5. Reinicie o computador servidor ou cliente em que você recebeu a mensagem de erro.

6. Verifique alertas de integridade para outros problemas que podem ter um efeito sobre backup do cliente.

## <a name="troubleshoot-a-file-or-folder-restore"></a>Solucionar problemas de uma restauração de arquivo ou pasta

### <a name="errors"></a>Errors

- A restauração de arquivo ou pasta não foi concluída com êxito.

### <a name="resolutions"></a>Resoluções

#### <a name="to-troubleshoot-an-unsuccessful-file-or-folder-restore"></a>Para solucionar problemas de uma restauração de arquivo ou pasta malsucedida

1. Verifique se que o computador está conectado à rede por meio de um dispositivo de rede.

2. Verifique se o dispositivo de rede que o computador está conectado a também está conectado à rede, ligado e funcionando corretamente.

3. Verifique os alertas para determinar se há erros no banco de dados de backup. Se houver erros, siga as instruções no alerta para reparar o banco de dados de backup.

4. Tente restaurar os arquivos ou pastas de outro backup.

5. Certifique-se de que o **Driver para Restauração de Computador do Windows Server Solution** está instalado e funcionando corretamente.

    **Para verificar o status do Driver para Restauração de Computador do Windows Server Solution**

    1. Clique em **Iniciar**, digite **Gerenciador de dispositivos** na caixa de texto **Pesquisar programas e arquivos** e clique em ENTER.

    2. No Gerenciador de dispositivos, clique em **Dispositivos de sistema**, role até **Driver para Restauração de Computador do Windows Server Solution**.

    3. Se o driver não for exibido:

        1. Abra um prompt de comando com privilégios de administrador e execute o seguinte comando:

             **%ProgramFiles%\Windows Server\Bin\BackupDriverInstaller.exe? -i**

        2. Atualize o Gerenciador de dispositivos. O driver deve aparecer.

    4. Se o ícone é exibido um monitor de computador, o driver está instalado e funcionando corretamente. Feche o Gerenciador de dispositivos.

    5. Se o ícone exibido não for um monitor de computador

        1. Clique com o botão direito em **Driver para Restauração de Computador do Windows Server Solution** e clique em **Propriedades**.

        2. Clique na guia **Driver** e, em seguida, clique em **Atualizar Driver**.

        3. Clique em **Pesquisar automaticamente software de driver atualizado** e, em seguida, siga as instruções para atualizar o driver.

    6. Feche o Gerenciador de dispositivos.

6. Desinstale o software do Windows Server Essentials Connector do computador e então reinstale-o. Para obter mais informações, consulte os tópicos [Desinstalar o software Connector](../use/Get-Connected-in-Windows-Server-Essentials.md#BKMK_13) e [Instalar o software Connector](../use/Get-Connected-in-Windows-Server-Essentials.md#BKMK_11).

## <a name="troubleshoot-a-full-system-restore"></a>Solucionar problemas de uma restauração completa do sistema

### <a name="errors"></a>Errors

- Não é possível fazer logon no computador cliente após uma restauração completa do sistema.

### <a name="resolutions"></a>Resoluções

Se você alterar o nome de um computador e, posteriormente, precisar restaurar um backup que foi salvo antes do nome do computador ser alterado, após a restauração, quando você tentar fazer logon em sua conta de domínio, você receberá esse erro: "O banco de dados de segurança no servidor não tem uma conta de computador para essa relação de confiança da estação de trabalho." Para obter acesso à rede para o computador novamente, remova o software Connector, remova o computador do domínio do Windows e então conecte-se novamente ao servidor.

#### <a name="to-regain-network-access-to-a-restored-computer-after-a-computer-name-change"></a>Para recuperar o acesso à rede para um computador restaurado após uma alteração de nome do computador

1. Faça logon no computador como um administrador local.

2. Desinstale o software Connector. Para obter mais informações, consulte [Desinstalar o software Connector](../use/Get-Connected-in-Windows-Server-Essentials.md#BKMK_13).

3. Remova o computador do domínio. Para obter mais informações, consulte [Remover um computador de um domínio do Windows](../use/Get-Connected-in-Windows-Server-Essentials.md#BKMK_8).

4. Conecte o computador ao servidor novamente. Para obter mais informações, consulte [Computadores conectados ao servidor](../use/Get-Connected-in-Windows-Server-Essentials.md#BKMK_9).

## <a name="additional-references"></a>Referências adicionais

- [Suporte ao Windows Server Essentials](Support-Windows-Server-Essentials.md)