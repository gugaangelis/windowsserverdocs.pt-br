---
title: Recuperação de floresta do AD-fazendo backup de um servidor completo
ms.author: joflore
author: MicrosoftGuyJFlo
manager: mtillman
ms.date: 08/09/2018
ms.topic: article
ms.prod: windows-server
ms.assetid: 398918dc-c8ab-41a6-a377-95681ec0b543
ms.technology: identity-adds
ms.openlocfilehash: 1579f8e88ea852ddf3f973b51b1b6ceed7c50a00
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80824269"
---
# <a name="ad-forest-recovery---backing-up-a-full-server"></a>Recuperação de floresta do AD-fazendo backup de um servidor completo  

>Aplica-se a: Windows Server 2016, Windows Server 2012 e 2012 R2, Windows Server 2008 e 2008 R2

Um backup de servidor completo é recomendado para se preparar para uma recuperação de floresta, pois ela pode ser restaurada para um hardware diferente ou para uma instância diferente do sistema operacional.  Usando Backup do Windows Server você pode executar um backup completo do servidor. 

## <a name="windows-server-backup"></a>Backup do Windows Server

O Backup do Windows Server não é instalado por padrão. No Windows Server 2016 e no Windows Server 2012 R2, instale-o seguindo as etapas abaixo.

>[!NOTE]
>Esteja ciente de que as etapas podem variar um pouco entre o Windows Server 2016 e o Windows Server 2012 R2.

Para obter as etapas para instalá-lo no Windows Server 2008 e no Windows Server 2008 R2, consulte [instalando backup do Windows Server](https://technet.microsoft.com/library/cc771232.aspx).  

### <a name="to-install-windows-server-backup"></a>Para instalar o Backup do Windows Server

1. Abra **Gerenciador do servidor** e clique em **adicionar funções e recursos**.
2. No **Assistente Adicionar funções e recursos,** clique em **Avançar**.
3. Na tela **tipo de instalação** , deixe a instalação padrão baseada em **função ou recurso** e clique em **Avançar**.
4. Na tela de **seleção de servidor** , clique em **Avançar**.
5. Na tela **funções de servidor** , clique em **Avançar**.
6. Na tela **recursos** , selecione **backup do Windows Server** e clique em **Avançar**
   ![instalar o backup](media/AD-Forest-Recovery-Backing-up-a-Full-Server/fullbackup2.png)
7. Clique em **Instalar**.
8. Quando a instalação for concluída, clique em **fechar**.

### <a name="to-perform-a-backup-with-windows-server-backup"></a>Para executar um backup com Backup do Windows Server

1. Abra **Gerenciador do servidor**, clique em **ferramentas**e, em seguida, clique em **backup do Windows Server**.
   - No Windows Server 2008 R2 e no Windows Server 2008, clique em **Iniciar**, aponte para **Ferramentas administrativas**e clique em **backup do Windows Server**.

   ![Instalar backup](media/AD-Forest-Recovery-Backing-up-a-Full-Server/fullbackup1.png) 

2. Se solicitado, na caixa de diálogo **controle de conta de usuário** , forneça credenciais de operador de backup e clique em **OK**.
3. Clique em **backup local**.
4. No menu **Ação**, clique em **Backup único**.
5. No assistente de backup único, na página **Opções de backup** , clique em **opções diferentes**e, em seguida, clique em **Avançar**.

   ![Instalar backup](media/AD-Forest-Recovery-Backing-up-a-Full-Server/fullbackup3.png)

6. Na página **selecionar configuração de backup** , clique em **servidor completo (recomendado)** e, em seguida, clique em **Avançar**.
7. Na página **especificar tipo de destino** , clique **em unidades locais** ou **pasta compartilhada remota**e, em seguida, clique em **Avançar**.
8. Na página **Selecionar destino do backup** , escolha o local do backup.  Se você selecionou unidade local, escolha uma unidade local ou, se você selecionou compartilhamento remoto, escolha um compartilhamento de rede.
9. Na tela de confirmação, clique em **backup**.

   ![Instalar backup](media/AD-Forest-Recovery-Backing-up-a-Full-Server/fullbackup4.png)

10. Depois que isso for concluído, clique em **fechar**.
11. Feche Backup do Windows Server.

>[!NOTE]
>Se você receber um erro informando que não há local de armazenamento de backup disponível, será necessário excluir um dos volumes que foram selecionados ou adicionar um novo volume ou compartilhamento remoto.
>Se você receber um aviso informando que o volume selecionado também está incluído na lista de itens para backup, determine se deseja ou não remover e clique em **OK**.

## <a name="using-wbadminexe-to-backup-a-windows-server"></a>Usando o Wbadmin. exe para fazer backup de um Windows Server

O Wbadmin. exe é um utilitário de linha de comando que permite fazer backup e restaurar seu sistema operacional, volumes, arquivos, pastas e aplicativos a partir de um prompt de comando.

### <a name="to-perform-a-full-server-backup-using-wbadminexe"></a>Para executar um backup completo do servidor usando o Wbadmin. exe
  
- Abra um prompt de comando com privilégios elevados, digite o seguinte comando e pressione ENTER:  

   ```
   wbadmin start backup -backuptarget:<Drive_letter_to store_backup>: -include:<Drive_letter_to_include>:
   ```

   ![Instalar backup](media/AD-Forest-Recovery-Backing-up-a-Full-Server/fullbackup5.png)

## <a name="next-steps"></a>{1&gt;{2&gt;Próximas etapas&lt;2}&lt;1}

- [Guia de recuperação de floresta do AD](AD-Forest-Recovery-Guide.md)
- [Recuperação de floresta do AD – Procedimentos](AD-Forest-Recovery-Procedures.md)
