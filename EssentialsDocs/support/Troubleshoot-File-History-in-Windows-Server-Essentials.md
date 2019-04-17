---
title: "Solucionar problemas de histórico de arquivos no Windows Server Essentials"
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
ms.sourcegitcommit: 70c1b6cedad55b9c7d2068c9aa4891c6c533ee4c
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/03/2017
---
# <a name="troubleshoot-file-history-in-windows-server-essentials"></a>Solucionar problemas de histórico de arquivos no Windows Server Essentials

>Aplica-se a: Windows Server 2016 Essentials, Windows Server 2012 R2 Essentials, Windows Server 2012 Essentials 
  
## <a name="troubleshoot-issues-with-user-file-history-backups"></a>Solucionar problemas com os backups de histórico de arquivos do usuário  
 Os seguintes problemas podem ocorrer durante a gerenciar backups de histórico de arquivos para um usuário ou um computador que foi adicionado a um servidor que executa o Windows Server Essentials.  
  
### <a name="file-history-data-is-not-automatically-deleted"></a>Dados de histórico de arquivos não são excluídos automaticamente  
 Os dados do histórico de arquivos não podem ser excluídos automaticamente se:  
  
-   Ao excluir uma conta de usuário, você optar por não excluir a conta de usuário s dados do histórico de arquivos e optar por excluir os dados manualmente.  
  
-   Quando você tentar excluir os dados do histórico de arquivos, os dados do histórico de arquivos estão em uso por outro processo.  
  
 Para resolver esse problema, você deve excluir manualmente o histórico de arquivos usando o seguinte procedimento:  
  
####  <a name="BKMK_manuallyDelete"></a>Para excluir backups de histórico de arquivos para um usuário ou um computador manualmente  
  
1.  Faça logon no servidor como administrador.  
  
2.  Execute o Explorador de arquivos como administrador.  
  
3.  Navegue até a pasta de Backups de histórico de arquivos. O local padrão é C:\ServerFolders\File histórico de Backups.  
  
4.  Exclua a pasta compartilhada que armazena o backup do histórico de arquivos:  
  
    -   Para excluir o histórico de arquivos para um usuário, exclua a pasta de backup filho do histórico de arquivos que tem o nome do usuário s.  
  
    -   Para excluir o histórico de arquivos para um computador, exclua a pasta de backup filho do histórico de arquivos com o nome do computador. Por exemplo, se um usuário desativou < MyComputer01\ > depois que ela começou a trabalhar em seu novo notebook, < MyComputer02\ > Excluir histórico de C:\ServerFolders\File Backups\\ < MyAccount\ > \ \ < MyComputer01\ > depois de verificar com o usuário que ela transferiu todos os arquivos e pastas para seu novo notebook e tem não é necessário para o histórico de arquivos no futuro.  
  
### <a name="cannot-apply-file-history-setting-to-a-new-user"></a>Não é possível aplicar a configuração de histórico de arquivos para um novo usuário  
 Se você adicionar um novo usuário cujo nome de usuário é idêntico ao nome do usuário de um usuário que tenha sido excluído do Windows Server Essentials, a configuração de histórico de arquivos para o novo usuário poderá falhar devido a um conflito de nomes ao Windows Server Essentials tenta criar uma pasta para armazenar o histórico de arquivos do novo usuário. Para resolver esse problema, você pode renomear a pasta de histórico de arquivos para o usuário excluído.  
  
##### <a name="to-locate-user-file-history-on-the-server"></a>Para localizar o histórico de arquivos do usuário no servidor  
  
1.  Faça logon no servidor como administrador.  
  
2.  No painel Windows Server Essentials, clique em **armazenamento**.  
  
3.  Sobre o **pastas de servidor** guia, anote o local da pasta Backups de histórico de arquivos. O local padrão é %SystemDrive%\ServerFolders\File Backups\\ histórico.  
  
##### <a name="to-resolve-file-history-issues-for-a-new-user-with-a-name-conflict"></a>Para resolver problemas de histórico do arquivo para um novo usuário com um conflito de nomes  
  
1.  Faça logon no servidor como administrador.  
  
2.  Execute o Explorador de arquivos como administrador.  
  
3.  Navegue até a pasta de Backups de histórico de arquivos. O local padrão é C:\ServerFolders\File histórico de Backups.  
  
     A pasta de Backups de histórico de arquivos tem uma subpasta para cada conta de usuário que foi adicionada ao Windows Server Essentials. Por exemplo, o histórico de arquivos para o usuário John Smith seria armazenado na subpasta Backups\JohnSmith de histórico de arquivos.  
  
4.  Renomeie a subpasta que você excluiu, por exemplo, os usuários * * < *UserName*> Deleted * *. Se você não precisa mais histórico de arquivos do usuário, você pode excluir a pasta.  
  

5.  Agora você pode adicionar o novo usuário. Para obter instruções, consulte Adicionar uma conta de usuário? em [gerenciar contas de usuário](../manage/Manage-User-Accounts-in-Windows-Server-Essentials.md).  
  
### <a name="a-user-account-was-removed-but-the-users-file-history-remains"></a>Uma conta de usuário foi removida, mas permanece histórico de arquivos do usuário  
 Em alguns casos, o administrador da rede pode escolher para remover um usuário ou computador do servidor, mas para manter o histórico de arquivo de backup para uso futuro. Quando você não precisa mais o histórico de arquivos, remova a pasta de Backups de histórico de arquivos para o usuário ou o computador de pastas compartilhadas no servidor. Para fazer isso, consulte [para excluir backups de histórico de arquivos para um usuário ou um computador manualmente](Troubleshoot-File-History-in-Windows-Server-Essentials.md#BKMK_manuallyDelete).  

5.  Agora você pode adicionar o novo usuário. Para obter instruções, consulte Adicionar uma conta de usuário? em [gerenciar contas de usuário](../manage/Manage-User-Accounts-in-Windows-Server-Essentials.md).  
  
### <a name="a-user-account-was-removed-but-the-users-file-history-remains"></a>Uma conta de usuário foi removida, mas permanece histórico de arquivos do usuário  
 Em alguns casos, o administrador da rede pode escolher para remover um usuário ou computador do servidor, mas para manter o histórico de arquivo de backup para uso futuro. Quando você não precisa mais o histórico de arquivos, remova a pasta de Backups de histórico de arquivos para o usuário ou o computador de pastas compartilhadas no servidor. Para fazer isso, consulte [para excluir backups de histórico de arquivos para um usuário ou um computador manualmente](../support/Troubleshoot-File-History-in-Windows-Server-Essentials.md#BKMK_manuallyDelete).  

  
## <a name="see-also"></a>Consulte também  
  
-   [Gerenciar o Backup do cliente](../manage/Manage-Client-Computer-Backup-in-Windows-Server-Essentials.md)  
  

-   [Suporte ao Windows Server Essentials](Support-Windows-Server-Essentials.md)

-   [Suporte ao Windows Server Essentials](../support/Support-Windows-Server-Essentials.md)

