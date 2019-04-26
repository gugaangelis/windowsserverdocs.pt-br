---
title: Histórico de arquivos de solução de problemas no Windows Server Essentials
description: Descreve como usar o Windows Server Essentials
ms.custom: na
ms.date: 10/03/2016
ms.prod: windows-server-2016-essentials
ms.reviewer: na
ms.suite: na
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: ed062945-27e9-4572-b1bb-6c8cf1b9c2f4
author: nnamuhcs
ms.author: coreyp
manager: dongill
ms.openlocfilehash: 34442565b54b089064c1fa19317a24f591e44fda
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59868597"
---
# <a name="troubleshoot-file-history-in-windows-server-essentials"></a>Histórico de arquivos de solução de problemas no Windows Server Essentials

>Aplica-se a: Windows Server 2016 Essentials, Windows Server 2012 R2 Essentials, Windows Server 2012 Essentials 
  
## <a name="troubleshoot-issues-with-user-file-history-backups"></a>Solucionar problemas com backups de Histórico de Arquivos de usuário  
 Os problemas a seguir podem ocorrer ao gerenciar backups de Histórico de Arquivos para um usuário em um computador que foi adicionado a um servidor executando o Windows Server Essentials.  
  
### <a name="file-history-data-is-not-automatically-deleted"></a>Os dados de Histórico de Arquivos não são excluídos automaticamente  
 Os dados de Histórico de arquivos podem não ser excluídos automaticamente se:  
  
-   Ao excluir uma conta de usuário, você opta por não excluir a conta de usuário s dados de histórico de arquivos e optar por excluir manualmente os dados.  
  
-   Quando você tenta excluir os dados de Histórico de Arquivos, esses dados estão em uso por outro processo.  
  
 Para resolver esse problema, você deve excluir o Histórico de Arquivos manualmente usando o procedimento a seguir:  
  
####  <a name="BKMK_manuallyDelete"></a> Para excluir manualmente os backups de histórico de arquivos para um usuário ou um computador  
  
1.  Faça logon no servidor como administrador.  
  
2.  Execute o Explorador de Arquivos como administrador.  
  
3.  Navegue até a pasta Backups de Histórico de Arquivos. O local padrão é C:\ServerFolders\File History Backups.  
  
4.  Exclua a pasta compartilhada que armazena o backup de Histórico de Arquivos:  
  
    -   Para excluir o histórico de arquivos para um usuário, exclua a pasta filho de backup de histórico de arquivos que tem o nome do usuário s.  
  
    -   Para excluir o histórico de arquivos para um computador, exclua a pasta filho de backup de Histórico de Arquivos com o nome desse computador. Por exemplo, se um usuário desativado < MyComputer01\> depois de começar a trabalhar em seu laptop novo, < MyComputer02\>, você excluiria C:\ServerFolders\File History Backups\\< MyAccount\> \\ < MyComputer01\> depois de verificar com o usuário que ela transferiu todos os arquivos e pastas para seu laptop novo e tem o sem a necessidade do histórico de arquivos no futuro.  
  
### <a name="cannot-apply-file-history-setting-to-a-new-user"></a>Não é possível aplicar a configuração de Histórico de Arquivos a um novo usuário  
 Se você adiciona um novo usuário cujo nome de usuário é idêntico àquele de um usuário que foi excluído do Windows Server Essentials, a configuração do Histórico de Arquivos para o novo usuário pode falhar por um conflito de nomeação, que ocorre quando o Windows Server Essentials tenta criar uma pasta para armazenar o histórico de arquivos do novo usuário. Para resolver esse problema, você pode renomear a pasta de Histórico de Arquivos para o usuário excluído.  
  
##### <a name="to-locate-user-file-history-on-the-server"></a>Para localizar o histórico de arquivos do usuário no servidor  
  
1.  Faça logon no servidor como administrador.  
  
2.  No Painel do Windows Server Essentials, clique em **Armazenamento**.  
  
3.  Na guia **Pastas de Servidor**, anote o local da pasta Backups de Histórico de Arquivos. O local padrão é %SystemDrive%\ServerFolders\File History Backups\\.  
  
##### <a name="to-resolve-file-history-issues-for-a-new-user-with-a-name-conflict"></a>Para resolver problemas no histórico de arquivos para um novo usuário com um conflito de nome de usuário  
  
1.  Faça logon no servidor como administrador.  
  
2.  Execute o Explorador de Arquivos como administrador.  
  
3.  Navegue até a pasta Backups de Histórico de Arquivos. O local padrão é C:\ServerFolders\File History Backups.  
  
     A pasta Backups de Histórico de Arquivos tem uma subpasta para cada conta de usuário que foi adicionada ao Windows Server Essentials. Por exemplo, o histórico de arquivos para o usuário John Smith seria armazenado na subpasta File History Backups/JohnSmith.  
  
4.  Renomeie a subpasta para o usuário que você excluiu, por exemplo,  **< *nome de usuário*> _excluído**. Se você não precisa mais do histórico de arquivos do usuário, você pode excluir a pasta correspondente.  
  

5.  Agora, você pode adicionar o novo usuário. Para obter instruções, consulte Adicionar uma conta de usuário? na [gerenciar contas de usuário](../manage/Manage-User-Accounts-in-Windows-Server-Essentials.md).  
  
### <a name="a-user-account-was-removed-but-the-users-file-history-remains"></a>Uma conta de usuário foi removida, mas o histórico de arquivos do usuário permanece  
 Em alguns casos, o administrador de rede pode escolher remover um usuário ou computador do servidor, mas manter o backup de Histórico de Arquivos para uso futuro. Quando você não precisar mais do histórico de arquivos, remova a pasta File History Backups para esse usuário ou computador das pastas compartilhadas no servidor. Para fazer isso, consulte [To manually delete File History backups for a user or a computer](Troubleshoot-File-History-in-Windows-Server-Essentials.md#BKMK_manuallyDelete).  

5.  Agora, você pode adicionar o novo usuário. Para obter instruções, consulte Adicionar uma conta de usuário? na [gerenciar contas de usuário](../manage/Manage-User-Accounts-in-Windows-Server-Essentials.md).  
  
### <a name="a-user-account-was-removed-but-the-users-file-history-remains"></a>Uma conta de usuário foi removida, mas o histórico de arquivos do usuário permanece  
 Em alguns casos, o administrador de rede pode escolher remover um usuário ou computador do servidor, mas manter o backup de Histórico de Arquivos para uso futuro. Quando você não precisar mais do histórico de arquivos, remova a pasta File History Backups para esse usuário ou computador das pastas compartilhadas no servidor. Para fazer isso, consulte [To manually delete File History backups for a user or a computer](../support/Troubleshoot-File-History-in-Windows-Server-Essentials.md#BKMK_manuallyDelete).  

  
## <a name="see-also"></a>Consulte também  
  
-   [Gerenciar o Backup do cliente](../manage/Manage-Client-Computer-Backup-in-Windows-Server-Essentials.md)  
  

-   [Suporte ao Windows Server Essentials](Support-Windows-Server-Essentials.md)

-   [Suporte ao Windows Server Essentials](../support/Support-Windows-Server-Essentials.md)

