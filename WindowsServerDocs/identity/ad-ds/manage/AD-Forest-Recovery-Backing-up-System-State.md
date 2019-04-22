---
title: Recuperação de floresta do AD - fazer backup de um servidor completo
description: ''
ms.author: joflore
author: MicrosoftGuyJFlo
manager: mtillman
ms.date: 08/09/2018
ms.topic: article
ms.prod: windows-server-threshold
ms.assetid: 9238cb27-0020-42f7-90d6-fcebf7e3c0bc
ms.technology: identity-adds
ms.openlocfilehash: a5306960bb2dca3849bdb4fc7304781af3f25335
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59815867"
---
# <a name="ad-forest-recovery---backing-up-the-system-state-data"></a>Recuperação de floresta do AD - backup de dados de estado do sistema  

>Aplica-se a: Windows Server 2016, Windows Server 2012 e 2012 R2, Windows Server 2008 e 2008 R2

Use o procedimento a seguir para executar um backup de estado do sistema em um controlador de domínio usando o Backup do Windows Server ou wbadmin.exe.  

## <a name="to-perform-a-system-state-backup-using-windows-server-backup"></a>Para executar um backup de estado do sistema usando o Backup do Windows Server

1. Abra **Gerenciador de servidores**, clique em **ferramentas**e, em seguida, clique em **Backup do Windows Server**.
   - No Windows Server 2008 R2 e Windows Server 2008, clique em **inicie**, aponte para **ferramentas administrativas**e, em seguida, clique em **Backup do Windows Server**. 

   ![Instalar o Backup](media/AD-Forest-Recovery-Backing-up-a-Full-Server/fullbackup1.png)

2. Se você for solicitado, além de **User Account Control** caixa de diálogo, forneça as credenciais de operador de Backup e, em seguida, clique em **Okey**.
3. Clique em **Backup Local**.
4. No menu **Ação**, clique em **Backup único**.
5. No Assistente de Backup uma vez, sobre o **as opções de Backup** , clique em **diferentes opções**e, em seguida, clique em **próxima**.

   ![Instalar o Backup](media/AD-Forest-Recovery-Backing-up-a-Full-Server/fullbackup3.png)

6. Diante a **Selecionar configuração de backup** , clique em **personalizada)** e, em seguida, clique em **próxima**.
7. Sobre o **selecionar itens para Backup** tela, clique em **adicionar itens** e selecione **estado do sistema** e clique em **Okey**.
   - No Windows Server 2008 R2 e Windows Server 2008, selecione os volumes para incluir no backup. Se você selecionar o **habilitar a recuperação de sistema** caixa de seleção, todos os volumes críticos estão selecionados. 

   ![Instalar o Backup](media/AD-Forest-Recovery-Backing-up-System-State/systemstatebackup.png)  

8. Sobre o **especificar tipo de destino** , clique em **unidades locais** ou **pasta compartilhada remota**e, em seguida, clique em **Avançar**.  Se você estiver fazendo backup em uma pasta compartilhada remota, faça o seguinte:  
   - Digite o caminho para a pasta compartilhada.
   - Sob **controle de acesso**, selecione **não herdam** ou **herdar** determinam o acesso para o backup e, em seguida, clique em **Avançar**.  
   - No **fornecer as credenciais do usuário para o Backup** caixa de diálogo, forneça o nome de usuário e senha para um usuário que tem acesso de gravação para a pasta compartilhada e, em seguida, clique em **Okey**.

9. Para o Windows Server 2008 R2 e Windows Server 2008, sobre o **especifique opções avançadas** página, selecione **backup de cópia VSS** e, em seguida, clique em **próxima**.
10. Sobre o **Selecionar destino do Backup** , escolha o local de backup.  Se você tiver selecionado locais unidade escolha uma unidade local ou se você selecionou remoto compartilhamento Escolha um compartilhamento de rede.
11. Na tela de confirmação, clique em **Backup**.
12. Quando tiver terminado, clique em **fechar**.
13. Feche o Backup do Windows Server.

## <a name="to-perform-a-system-state-backup-using-wbadminexe"></a>Para executar um backup de estado do sistema usando Wbadmin.exe

Abra um prompt de comando com privilégios elevados, digite o seguinte comando e pressione ENTER:  
  
   ```
   wbadmin start systemstatebackup -backuptarget:<targetDrive>:
   ```

   ![Instalar o Backup](media/AD-Forest-Recovery-Backing-up-System-State/systemstatebackup2.png)  

## <a name="next-steps"></a>Próximas etapas

- [Guia de recuperação da floresta do AD](AD-Forest-Recovery-Guide.md)
- [Recuperação de floresta do AD - procedimentos](AD-Forest-Recovery-Procedures.md)
