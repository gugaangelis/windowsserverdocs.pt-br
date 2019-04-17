---
title: "Recuperação de floresta do AD - fazer backup de um servidor completo"
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 07/07/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.assetid: 398918dc-c8ab-41a6-a377-95681ec0b543
ms.technology: identity-adfs
ms.openlocfilehash: b1af97c2eb23d65c2d106906bc0f5bb1f10b23ec
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 12/12/2017
---
# <a name="ad-forest-recovery---backing-up-a-full-server"></a>Recuperação de floresta do AD - fazer backup de um servidor completo  

>Aplica-se a: Windows Server 2016, Windows Server 2012 e 2012 R2, Windows Server 2008 e 2008 R2

Um backup do servidor completo é recomendado para preparar para uma recuperação de floresta porque ele pode ser restaurado para um hardware diferente ou uma instância de sistema operacional diferente.  Usando o Backup do Windows Server, você pode executar um backup completo do seu servidor. 

## <a name="windows-server-backup"></a>Backup do Windows Server
Backup do Windows Server não é instalado por padrão. No Windows Server 2012 R2 e no Windows Server 2016, instalá-lo seguindo as etapas abaixo.

>[!NOTE]
>Esteja ciente de que as etapas podem variar ligeiramente entre o Windows Server 2016 e Windows Server 2012 R2.

Para obter etapas para instalá-lo no Windows Server 2008 e Windows Server 2008 R2, consulte [instalando o Backup do Windows Server](https://technet.microsoft.com/library/cc771232.aspx).  

### <a name="to-install-windows-server-backup"></a>Para instalar o Backup do Windows Server
1. Abrir **Gerenciador do servidor** e clique em **adicionar funções e recursos**.
2. Sobre o **assistente Adicionar funções e recursos** clique **próxima**.
3. Sobre o **tipo de instalação** tela, deixe o padrão **instalação baseada em função ou recurso baseado** e clique em **próxima**.
4. Sobre o **seleção de servidor** de tela, clique em **próxima**.
5. Sobre o **funções de servidor** tela click **próxima**.
6. Sobre o **recursos** e selecione **Backup do Windows Server** e clique em **próxima**<ph x="4">
! [</ph> Instalar o Backup](media/AD-Forest-Recovery-Backing-up-a-Full-Server/fullbackup2.png)
7. Clique em **instalar**.
8. Depois que a instalação for concluída, clique em **fechar**.


### <a name="to-perform-a-backup-with-windows-server-backup"></a>Para executar um backup com o Backup do Windows Server

1. Abrir **Gerenciador do servidor**, clique em **ferramentas**e clique em **Backup do Windows Server**.
    - No Windows Server 2008 R2 e Windows Server 2008, clique em **iniciar**, aponte para **ferramentas administrativas**e clique em **Backup do Windows Server**. 
![Instalar o Backup](media/AD-Forest-Recovery-Backing-up-a-Full-Server/fullbackup1.png) 
2. Se você for solicitado, além do **User Account Control** caixa de diálogo, forneça credenciais de operador de Backup e, em seguida, clique em **Okey**.
3. Clique em **Backup Local**.
4. Sobre o **ação** menu, clique em **Backup único**.
5. No Assistente de Backup uma vez, no **opções de Backup** página, clique em **opções diferentes**e clique em **próxima**.
![Instalar o Backup](media/AD-Forest-Recovery-Backing-up-a-Full-Server/fullbackup3.png)
6. No **selecione configuração backup** página, clique em **servidor completo (recomendado)**e clique em **próxima**.
7. Sobre o **especificar o tipo de destino** página, clique em **unidades locais** ou **remoto pasta compartilhada**e, em seguida, clique em **próxima**.
8. Sobre o **Selecionar destino do Backup** página, escolha o local de backup.  Se você selecionou local unidade escolher uma unidade local ou se você selecionou remoto compartilhamento escolher um compartilhamento de rede.
9. Na tela de confirmação, clique em **Backup**.
![Instalar o Backup](media/AD-Forest-Recovery-Backing-up-a-Full-Server/fullbackup4.png)
10. Quando tiver terminado, clique em **fechar**.
11. Feche o Backup do Windows Server.

>[!NOTE]
>Se você receber um erro informando que nenhum local de armazenamento de backup está disponível, você precisará excluir um dos volumes que foi selecionado ou adicione um novo volume ou compartilhamento remoto.
>Se você receber um aviso informando que o volume selecionado também está incluído na lista de itens para fazer backup, determinar se deseja ou não remover e clique em **Okey**.

## <a name="using-wbadminexe-to-backup-a-windows-server"></a>Usando Wbadmin.exe para fazer backup de um servidor do windows
Wbadmin.exe é um utilitário de linha de comando que permite que você faça backup e restauração do sistema operacional, volumes, arquivos, pastas e aplicativos em um prompt de comando.

#### <a name="to-perform-a-full-server-backup-using-wbadminexe"></a>Para executar um backup do servidor completo usando Wbadmin.exe  
  
1.  Abra um prompt de comando com privilégios elevados, digite o seguinte comando e pressione ENTER:  

        wbadmin start backup -backuptarget:<Drive_letter_to store_backup>: -include:<Drive_letter_to_include>:

![Instalar o Backup](media/AD-Forest-Recovery-Backing-up-a-Full-Server/fullbackup5.png)
## <a name="next-steps"></a>Próximas etapas

- [Guia de recuperação de floresta do AD](AD-Forest-Recovery-Guide.md)
- [Recuperação de floresta do AD - procedimentos](AD-Forest-Recovery-Procedures.md)
