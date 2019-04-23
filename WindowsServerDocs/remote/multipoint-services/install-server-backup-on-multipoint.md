---
title: Instale o Backup do servidor no servidor do MultiPoint
description: Orienta você pelas etapas para instalar as ferramentas de backup e recuperação
ms.custom: na
ms.date: 07/22/2016
ms.prod: windows-server-threshold
ms.technology: multipoint-services
ms.reviewer: na
ms.suite: na
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: e4331370-ba07-4529-92ab-db14a41bfc3b
author: evaseydl
manager: scottman
ms.author: evas
ms.openlocfilehash: 51932c5f0796cfd757d3322e10c17de2a3081f4c
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59832487"
---
# <a name="install-server-backup-on-your-multipoint-server"></a>Instale o Backup do servidor no servidor do MultiPoint
É recomendável que você considere um plano de backup e recuperação para seus servidores do MultiPoint.
  
Um bom plano de backup e recuperação é importante para qualquer ambiente de tamanho. Backup do Windows Server é um recurso no Windows Server 2016 que fornece um conjunto de assistentes e outras ferramentas para executar tarefas básicas de backup e recuperação para o servidor no qual ele está instalado. Você pode usar o Backup do Windows Server para fazer backup de um servidor completo (todos os volumes), volumes selecionados, o estado do sistema ou arquivos ou pastas específicas e para criar um backup que você pode usar para recriar o seu sistema.  
  
Você pode recuperar volumes, pastas, arquivos, certos aplicativos e o estado do sistema. E, para desastres como falhas no disco rígido, você pode reconstruir um sistema do zero ou usando hardware alternativo. Para fazer isso, você deve ter um backup de servidor completo ou apenas dos volumes que contêm arquivos do sistema operacional e o ambiente de recuperação do Windows. Isso restaura todo o sistema para o sistema antigo ou para um novo disco rígido.  
  
Um recurso fundamental do Backup do Windows Server é a capacidade de agendar backups para serem executados automaticamente.  
  
Use os procedimentos a seguir para configurar o tipo de backup que você precisa.  
  
## <a name="install-backup-and-recovery-tools"></a>Instalar as ferramentas de backup e recuperação  
  
1.  Dos **inicie** tela, abra **Gerenciador do servidor**.  
  
2.  Clique em **adicionar funções e recursos** para iniciar o Assistente para adicionar funções. Em seguida, clique em **próxima** depois de examinar a **antes de começar** notas.  
  
3.  Selecione o **instalação baseada em baseado em função ou recurso** opção e, em seguida, clique em **próxima**.  
  
4.  Selecione o computador local que você está gerenciando e clique em **próxima**.  
  
    O Assistente para Adicionar Recursos é aberto.  
  
5.  Sobre o **selecionar recursos** , expanda recursos de Backup do Windows Server, selecione as caixas de seleção **Backup do Windows Server** e **ferramentas de linha de comando**e, em seguida, clique em  **Próxima**.  
  
    > [!NOTE]  
    > Ou, se você quiser instalar o snap-in e a ferramenta de linha de comando Wbadmin, expanda **recursos de Backup do Windows Server**e, em seguida, selecione o **Backup do Windows Server** apenas a caixa de seleção — Verifique se o **Ferramentas de linha de comando** caixa de seleção estiver desmarcada.  
  
6.  Sobre o **confirmar seleções de instalação** página, examine suas escolhas e, em seguida, clique em **instalar**.  
  
    Se ocorrerem erros durante a instalação, o **resultados da instalação** página observarão os erros.  
  
7.  Depois que a instalação for concluída com êxito, você deve ser capaz de acessar essas ferramentas de backup e recuperação:  
  
    -   Para abrir o Backup do Windows Server snap-in, nos **inicie** tela, digite **backup**e, em seguida, clique em **Backup do Windows Server** nos resultados.  
  
    -   Para iniciar a ferramenta de Wbadmin e exibir a sintaxe para seus comandos: Sobre o **inicie** tela, digite **comando**. Nos resultados, clique com botão direito **Prompt de comando**, clique em **executar como administrador** na parte inferior da página e, em seguida, clique **Sim** no prompt de confirmação. No prompt de comando, digite **wbadmin /?** e pressione ENTER. Você deve ver a sintaxe e descrições para a ferramenta.  
  
## <a name="configure-backups-using-windows-server-backup"></a>Configurar backups usando o Backup do Windows Server  
  
-   Siga as orientações [fazendo backup do servidor](https://technet.microsoft.com/library/cc753528.aspx). 