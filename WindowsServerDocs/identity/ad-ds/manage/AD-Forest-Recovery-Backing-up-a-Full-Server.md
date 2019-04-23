---
title: Recuperação de floresta do AD - fazer backup de um servidor completo
description: ''
ms.author: joflore
author: MicrosoftGuyJFlo
manager: mtillman
ms.date: 08/09/2018
ms.topic: article
ms.prod: windows-server-threshold
ms.assetid: 398918dc-c8ab-41a6-a377-95681ec0b543
ms.technology: identity-adds
ms.openlocfilehash: fec8de8ea1dadb392f6a3bd1c881e8df2266f404
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59846517"
---
# <a name="ad-forest-recovery---backing-up-a-full-server"></a>Recuperação de floresta do AD - fazer backup de um servidor completo  

>Aplica-se a: Windows Server 2016, Windows Server 2012 e 2012 R2, Windows Server 2008 e 2008 R2

Um backup completo do servidor é recomendado para se preparar para uma recuperação de floresta, porque ele pode ser restaurado para um hardware diferente ou uma instância diferente do sistema operacional.  Usando o Backup do Windows Server, você pode executar um backup completo do seu servidor. 

## <a name="windows-server-backup"></a>Backup do Windows Server

Backup do Windows Server não está instalado por padrão. No Windows Server 2016 e no Windows Server 2012 R2, instale-o seguindo as etapas abaixo.

>[!NOTE]
>Esteja ciente de que as etapas podem variar ligeiramente entre o Windows Server 2016 e Windows Server 2012 R2.

Para obter as etapas para instalá-lo no Windows Server 2008 e Windows Server 2008 R2, consulte [instalando o Backup do Windows Server](https://technet.microsoft.com/library/cc771232.aspx).  

### <a name="to-install-windows-server-backup"></a>Para instalar o Backup do Windows Server

1. Abra **Gerenciador de servidores** e clique em **adicionar funções e recursos**.
2. Sobre o **assistente Adicionar funções e recursos** clique em **próxima**.
3. Sobre o **tipo de instalação** tela, deixe o padrão **instalação baseada em função ou recurso** e clique em **próxima**.
4. Sobre o **seleção de servidor** tela, clique em **próxima**.
5. Sobre o **funções de servidor** tela clique **próxima**.
6. Sobre o **recursos** tela, selecione **Backup do Windows Server** e clique em **próxima**
   ![instalar o Backup](media/AD-Forest-Recovery-Backing-up-a-Full-Server/fullbackup2.png)
7. Clique em **Instalar**.
8. Depois que a instalação for concluída, clique em **fechar**.

### <a name="to-perform-a-backup-with-windows-server-backup"></a>Para executar um backup com o Backup do Windows Server

1. Abra **Gerenciador de servidores**, clique em **ferramentas**e, em seguida, clique em **Backup do Windows Server**.
   - No Windows Server 2008 R2 e Windows Server 2008, clique em **inicie**, aponte para **ferramentas administrativas**e, em seguida, clique em **Backup do Windows Server**.

   ![Instalar o Backup](media/AD-Forest-Recovery-Backing-up-a-Full-Server/fullbackup1.png) 

2. Se você for solicitado, além de **User Account Control** caixa de diálogo, forneça as credenciais de operador de Backup e, em seguida, clique em **Okey**.
3. Clique em **Backup Local**.
4. No menu **Ação**, clique em **Backup único**.
5. No Assistente de Backup uma vez, sobre o **as opções de Backup** , clique em **diferentes opções**e, em seguida, clique em **próxima**.

   ![Instalar o Backup](media/AD-Forest-Recovery-Backing-up-a-Full-Server/fullbackup3.png)

6. Sobre o **Selecionar configuração de backup** , clique em **servidor completo (recomendado)** e, em seguida, clique em **próxima**.
7. Sobre o **especificar tipo de destino** , clique em **unidades locais** ou **pasta compartilhada remota**e, em seguida, clique em **Avançar**.
8. Sobre o **Selecionar destino do Backup** , escolha o local de backup.  Se você tiver selecionado locais unidade escolha uma unidade local ou se você selecionou remoto compartilhamento Escolha um compartilhamento de rede.
9. Na tela de confirmação, clique em **Backup**.

   ![Instalar o Backup](media/AD-Forest-Recovery-Backing-up-a-Full-Server/fullbackup4.png)

10. Quando tiver terminado, clique em **fechar**.
11. Feche o Backup do Windows Server.

>[!NOTE]
>Se você receber um erro indicando que nenhum local de armazenamento de backup estiver disponível, você precisará excluir um dos volumes que foi selecionado ou adicionar um novo volume ou compartilhamento remoto.
>Se você receber um aviso informando que o volume selecionado também está incluído na lista de itens para backup, determine se deseja ou não remover e clique em **Okey**.

## <a name="using-wbadminexe-to-backup-a-windows-server"></a>Usando Wbadmin.exe para fazer backup de um servidor do windows

Wbadmin.exe é um utilitário de linha de comando que lhe permite fazer backup e restaurar seu sistema operacional, volumes, arquivos, pastas e aplicativos em um prompt de comando.

### <a name="to-perform-a-full-server-backup-using-wbadminexe"></a>Para executar um backup completo do servidor usando Wbadmin.exe
  
- Abra um prompt de comando com privilégios elevados, digite o seguinte comando e pressione ENTER:  

   ```
   wbadmin start backup -backuptarget:<Drive_letter_to store_backup>: -include:<Drive_letter_to_include>:
   ```

   ![Instalar o Backup](media/AD-Forest-Recovery-Backing-up-a-Full-Server/fullbackup5.png)

## <a name="next-steps"></a>Próximas etapas

- [Guia de recuperação da floresta do AD](AD-Forest-Recovery-Guide.md)
- [Recuperação de floresta do AD - procedimentos](AD-Forest-Recovery-Procedures.md)
