---
title: Recuperação de floresta do AD-fazendo backup de um servidor completo
ms.author: joflore
author: MicrosoftGuyJFlo
manager: mtillman
ms.date: 08/09/2018
ms.topic: article
ms.prod: windows-server
ms.assetid: 9238cb27-0020-42f7-90d6-fcebf7e3c0bc
ms.technology: identity-adds
ms.openlocfilehash: 321f927a3efc4f2391daff92ac4c8b7acb47c055
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80824272"
---
# <a name="ad-forest-recovery---backing-up-the-system-state-data"></a>Recuperação de floresta do AD-fazendo backup dos dados de estado do sistema  

>Aplica-se a: Windows Server 2016, Windows Server 2012 e 2012 R2, Windows Server 2008 e 2008 R2

Use o procedimento a seguir para executar um backup de estado do sistema em um controlador de domínio usando Backup do Windows Server ou Wbadmin. exe.  

## <a name="to-perform-a-system-state-backup-using-windows-server-backup"></a>Para executar um backup de estado do sistema usando Backup do Windows Server

1. Abra **Gerenciador do servidor**, clique em **ferramentas**e, em seguida, clique em **backup do Windows Server**.
   - No Windows Server 2008 R2 e no Windows Server 2008, clique em **Iniciar**, aponte para **Ferramentas administrativas**e clique em **backup do Windows Server**. 

   ![Instalar backup](media/AD-Forest-Recovery-Backing-up-a-Full-Server/fullbackup1.png)

2. Se solicitado, na caixa de diálogo **controle de conta de usuário** , forneça credenciais de operador de backup e clique em **OK**.
3. Clique em **backup local**.
4. No menu **Ação**, clique em **Backup único**.
5. No assistente de backup único, na página **Opções de backup** , clique em **opções diferentes**e, em seguida, clique em **Avançar**.

   ![Instalar backup](media/AD-Forest-Recovery-Backing-up-a-Full-Server/fullbackup3.png)

6. Na página **selecionar configuração de backup** , clique em **personalizado)** e, em seguida, clique em **Avançar**.
7. Na tela **selecionar itens para backup** , clique em **Adicionar itens** e selecione **estado do sistema** e clique em **OK**.
   - No Windows Server 2008 R2 e no Windows Server 2008, selecione os volumes a serem incluídos no backup. Se você marcar a caixa de seleção **habilitar recuperação do sistema** , todos os volumes críticos serão selecionados. 

   ![Instalar backup](media/AD-Forest-Recovery-Backing-up-System-State/systemstatebackup.png)  

8. Na página **especificar tipo de destino** , clique **em unidades locais** ou **pasta compartilhada remota**e, em seguida, clique em **Avançar**.  Se você estiver fazendo backup em uma pasta compartilhada remota, faça o seguinte:  
   - Digite o caminho para a pasta compartilhada.
   - Em **controle de acesso**, selecione **não herdar** ou **herdar** para determinar o acesso ao backup e clique em **Avançar**.  
   - Na caixa de diálogo **fornecer credenciais de usuário para backup** , forneça o nome de usuário e a senha para um usuário que tenha acesso de gravação à pasta compartilhada e clique em **OK**.

9. Para o Windows Server 2008 R2 e o Windows Server 2008, na página **especificar opção avançada** , selecione **backup de cópia do VSS** e clique em **Avançar**.
10. Na página **Selecionar destino do backup** , escolha o local do backup.  Se você selecionou unidade local, escolha uma unidade local ou, se você selecionou compartilhamento remoto, escolha um compartilhamento de rede.
11. Na tela de confirmação, clique em **backup**.
12. Depois que isso for concluído, clique em **fechar**.
13. Feche Backup do Windows Server.

## <a name="to-perform-a-system-state-backup-using-wbadminexe"></a>Para executar um backup de estado do sistema usando o Wbadmin. exe

Abra um prompt de comando com privilégios elevados, digite o seguinte comando e pressione ENTER:  
  
   ```
   wbadmin start systemstatebackup -backuptarget:<targetDrive>:
   ```

   ![Instalar backup](media/AD-Forest-Recovery-Backing-up-System-State/systemstatebackup2.png)  

## <a name="next-steps"></a>{1&gt;{2&gt;Próximas etapas&lt;2}&lt;1}

- [Guia de recuperação de floresta do AD](AD-Forest-Recovery-Guide.md)
- [Recuperação de floresta do AD – Procedimentos](AD-Forest-Recovery-Procedures.md)
