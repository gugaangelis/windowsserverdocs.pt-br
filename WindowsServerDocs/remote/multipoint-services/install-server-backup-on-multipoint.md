---
title: Instalar o backup do servidor no MultiPoint Server
description: Orienta você pelas etapas para instalar as ferramentas de backup e recuperação
ms.date: 07/22/2016
ms.prod: windows-server
ms.technology: multipoint-services
ms.topic: article
ms.assetid: e4331370-ba07-4529-92ab-db14a41bfc3b
author: evaseydl
manager: scottman
ms.author: evas
ms.openlocfilehash: 7aa2c422151247d13dcb1dafd474fb70873f54e1
ms.sourcegitcommit: d5e27c1f2f168a71ae272bebf8f50e1b3ccbcca3
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/23/2020
ms.locfileid: "86966908"
---
# <a name="install-server-backup-on-your-multipoint-server"></a>Instalar o backup do servidor no MultiPoint Server
É recomendável que você considere um plano de backup e recuperação para seus servidores MultiPoint.
  
Um bom plano de backup e recuperação é importante para qualquer ambiente de tamanho. Backup do Windows Server é um recurso do Windows Server 2016 que fornece um conjunto de assistentes e outras ferramentas para executar tarefas básicas de backup e recuperação para o servidor no qual ele está instalado. Você pode usar Backup do Windows Server para fazer backup de um servidor completo (todos os volumes), dos volumes selecionados, do estado do sistema ou de arquivos ou pastas específicos e criar um backup que pode ser usado para recompilar o sistema.  
  
Você pode recuperar volumes, pastas, arquivos, certos aplicativos e o estado do sistema. E, para desastres como falhas de disco rígido, você pode recriar um sistema a partir do zero ou usando hardware alternativo. Para fazer isso, você deve ter um backup do servidor completo ou apenas dos volumes que contêm arquivos do sistema operacional e do ambiente de recuperação do Windows. Isso restaura o sistema completo no sistema antigo ou em um novo disco rígido.  
  
Um recurso importante do Backup do Windows Server é a capacidade de agendar backups para serem executados automaticamente.  
  
Use os procedimentos a seguir para configurar o tipo de backup que você precisa.  
  
## <a name="install-backup-and-recovery-tools"></a>Instalar ferramentas de backup e recuperação  
  
1.  Na tela **Iniciar** , abra **Gerenciador do servidor**.  
  
2.  Clique em **adicionar funções e recursos** para iniciar o assistente para adicionar funções. Em seguida, clique em **Avançar** depois de examinar as anotações **antes de começar** .  
  
3.  Selecione a opção de instalação baseada em **função ou recurso** e, em seguida, clique em **Avançar**.  
  
4.  Selecione o computador local que você está gerenciando e clique em **Avançar**.  
  
    O Assistente para Adicionar Recursos é aberto.  
  
5.  Na página **selecionar recursos** , expanda backup do Windows Server recursos, marque as caixas de seleção para **backup do Windows Server** e **ferramentas de linha de comando**e clique em **Avançar**.  
  
    > [!NOTE]  
    > Ou, se você apenas deseja instalar o snap-in e a ferramenta de linha de comando Wbadmin, expanda **backup do Windows Server recursos**e marque a caixa de seleção **backup do Windows Server** apenas – verifique se a caixa de seleção **ferramentas de linha de comando** está desmarcada.  
  
6.  Na página **confirmar seleções de instalação** , examine suas escolhas e clique em **instalar**.  
  
    Se ocorrerem erros durante a instalação, a página **resultados da instalação** notará os erros.  
  
7.  Depois que a instalação for concluída com êxito, você poderá acessar essas ferramentas de backup e recuperação:  
  
    -   Para abrir o snap-in Backup do Windows Server, na tela **Iniciar** , digite **backup**e, em seguida, clique em **backup do Windows Server** nos resultados.  
  
    -   Para iniciar a ferramenta Wbadmin e exibir a sintaxe de seus comandos: na tela **Iniciar** , digite **comando**. Nos resultados, clique com o botão direito do mouse em **prompt de comando**, clique em **Executar como administrador** na parte inferior da página e, em seguida, clique em **Sim** no prompt de confirmação. No prompt de comando, digite **Wbadmin/?** e pressione ENTER. Você deve ver a sintaxe do comando e as descrições da ferramenta.  
  
## <a name="configure-backups-using-windows-server-backup"></a>Configurar backups usando o Backup do Windows Server  
  
-   Siga as orientações em [fazendo backup do servidor](/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/cc753528(v=ws.11)). 
