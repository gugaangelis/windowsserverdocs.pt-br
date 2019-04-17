---
title: "Recuperação de floresta do AD - fazer backup de um servidor completo"
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 07/07/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.assetid: 9238cb27-0020-42f7-90d6-fcebf7e3c0bc
ms.technology: identity-adfs
ms.openlocfilehash: a86d61536f8b426e1a5258c661d4e53da63d4162
ms.sourcegitcommit: 84a2bdcb92ba6af45781fab9727617e50fa5e911
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/07/2017
---
# <a name="ad-forest-recovery---backing-up-the-system-state-data"></a>Recuperação de floresta do AD - backup dos dados de estado do sistema  

>Aplica-se a: Windows Server 2016, Windows Server 2012 e 2012 R2, Windows Server 2008 e 2008 R2
 
Use o procedimento a seguir para executar um backup do estado do sistema em um controlador de domínio usando o Backup do Windows Server ou wbadmin.exe.  
  
## <a name="to-perform-a-system-state-backup-using-windows-server-backup"></a>Para executar um backup do estado do sistema usando o Backup do Windows Server  
1. Abrir **Gerenciador do servidor**, clique em **ferramentas**e clique em **Backup do Windows Server**.
    - No Windows Server 2008 R2 e Windows Server 2008, clique em **iniciar**, aponte para **ferramentas administrativas**e clique em **Backup do Windows Server**. 
![Instalar o Backup](media/AD-Forest-Recovery-Backing-up-a-Full-Server/fullbackup1.png) 
2. Se você for solicitado, além do **User Account Control** caixa de diálogo, forneça credenciais de operador de Backup e, em seguida, clique em **Okey**.
3. Clique em **Backup Local**.
4. Sobre o **ação** menu, clique em **Backup único**.
5. No Assistente de Backup uma vez, no **opções de Backup** página, clique em **opções diferentes**e clique em **próxima**.
![Instalar o Backup](media/AD-Forest-Recovery-Backing-up-a-Full-Server/fullbackup3.png)
6. Pelo **selecione configuração backup** página, clique em **personalizada)**e clique em **próxima**.
7. Sobre o **selecionar itens para Backup** de tela, clique em **adicionar itens** e selecione **estado do sistema** e clique em **Okey**.
    - No Windows Server 2008 R2 e Windows Server 2008, selecione os volumes para incluir no backup. Se você selecionar o **habilitar recuperação do sistema** caixa de seleção, todos os volumes críticos estão selecionados. 
![Instalar o Backup](media/AD-Forest-Recovery-Backing-up-System-State/systemstatebackup.png)  
8. Sobre o **especificar o tipo de destino** página, clique em **unidades locais** ou **remoto pasta compartilhada**e, em seguida, clique em **próxima**.  Se você estiver fazendo backup em uma pasta compartilhada remota, faça o seguinte:  
  
 1.  Digite o caminho para a pasta compartilhada.  
 2.  Em **o controle de acesso**, selecione **não herdam** ou **Inherit** para determinar o acesso para o backup e, em seguida, clique em **próxima**.  
 3.  No **fornecer as credenciais do usuário para o Backup** caixa de diálogo, forneça o nome de usuário e senha para um usuário que tem acesso de gravação para a pasta compartilhada e clique em **Okey**.
9. Para o Windows Server 2008 R2 e Windows Server 2008, no **especificar a opção avançada** página, selecione **backup de cópia VSS** e, em seguida, clique em **próxima**.
10. Sobre o **Selecionar destino do Backup** página, escolha o local de backup.  Se você selecionou local unidade escolher uma unidade local ou se você selecionou remoto compartilhamento escolher um compartilhamento de rede.
11. Na tela de confirmação, clique em **Backup**.
12. Quando tiver terminado, clique em **fechar**.
13. Feche o Backup do Windows Server.

  
## <a name="to-perform-a-system-state-backup-using-wbadminexe"></a>Para executar um backup de estado do sistema usando Wbadmin.exe  
  
1.  Abra um prompt de comando com privilégios elevados, digite o seguinte comando e pressione ENTER:  
  
    ```  
    wbadmin start systemstatebackup -backuptarget:<targetDrive>:
    ```  
![Instalar o Backup](media/AD-Forest-Recovery-Backing-up-System-State/systemstatebackup2.png)  

## <a name="next-steps"></a>Próximas etapas

- [Guia de recuperação de floresta do AD](AD-Forest-Recovery-Guide.md)
- [Recuperação de floresta do AD - procedimentos](AD-Forest-Recovery-Procedures.md)
