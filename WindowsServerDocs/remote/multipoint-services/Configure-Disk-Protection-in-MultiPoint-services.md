---
title: Configurar Proteção de Disco no MultiPoint Services
description: Saiba como configurar a proteção de disco para os serviços do MultiPoint
ms.custom: na
ms.prod: windows-server
ms.technology: multipoint-services
ms.reviewer: na
ms.suite: na
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: bd9bf5b9-e481-499b-9c15-7ee5a4f470c4
author: evaseydl
manager: scottman
ms.author: evas
ms.date: 08/04/2016
ms.openlocfilehash: ae930162de32335ac32e3bda0ac381a26c5ea6dd
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71389823"
---
# <a name="configure-disk-protection"></a>Configurar a proteção de disco
Você pode usar a proteção de disco nos serviços do MultiPoint para proteger o volume do sistema contra atualizações não intencionais, para agendar atualizações do Windows a serem retidas enquanto a proteção de disco estiver ativa, para desabilitar temporariamente a proteção de disco e para desinstalar a proteção de disco.  
  
Ao habilitar a proteção de disco no MultiPoint Services, você pode proteger o volume do sistema (a unidade em que o Windows está instalado, geralmente C:) contra alterações indesejadas. Quando a proteção de disco está habilitada, as alterações feitas no volume do sistema são armazenadas em um local temporário, de modo que simplesmente reiniciar o computador os descarta e retorna automaticamente o sistema para o estado mais conhecido anterior.  
  
O administrador pode facilmente instalar software ou fazer alterações de configuração, desabilitando temporariamente a proteção de disco. Para manter o sistema atualizado com as atualizações do Windows e as definições de anti-malware, a proteção de disco agenda uma janela de manutenção para baixar e instalar atualizações. O administrador também pode fornecer um script personalizado para ser executado durante a janela de manutenção para acomodar as necessidades de manutenção além Windows Update.  
  
## <a name="enable-disk-protection"></a>Habilitar proteção de disco  
Antes de habilitar a proteção de disco, verifique se todos os aplicativos e drivers estão instalados e atualizados e mova seus perfis de usuário para um volume que não será protegido. Se precisar fazer atualizações manuais depois de habilitar a proteção de disco, você poderá desabilitar temporariamente a proteção de disco. No entanto, é mais fácil colocar o sistema em um estado ideal antes que a proteção de disco seja ativada.  
  
 
1.  Faça logon no servidor que executa os serviços do MultiPoint como administrador.  
  
2.  Antes de habilitar a proteção de disco:  
  
    -   Verifique se o sistema de serviços do MultiPoint está em exatamente o estado no qual você deseja que ele permaneça. Por exemplo, verifique se o software instalado, as configurações do sistema e as atualizações estão corretas.  
  
    -   Mova perfis de usuário para um volume que não esteja protegido ou configure um local de arquivo compartilhado fora do volume do sistema, conforme descrito em [habilitar o compartilhamento de arquivos nos serviços do MultiPoint](Enable-file-sharing-in-MultiPoint-services.md).  
  
3.  Na tela **Iniciar** , abra o **Gerenciador do MultiPoint**.  
  
4.  Clique na guia **início** , clique em **habilitar proteção de disco**e, em seguida, clique em **OK**.  
  
Quando a proteção de disco é habilitada pela primeira vez, o sistema é preparado pela instalação de um driver e pela criação de um arquivo de cache no volume do sistema. O arquivo de cache armazenará temporariamente todas as alterações feitas no volume do sistema enquanto a proteção de disco estiver ativa. Como as atualizações do sistema são armazenadas no arquivo de cache, elas não alteram o conteúdo protegido do volume fora do arquivo de cache. Cada vez que o sistema é iniciado, o arquivo de cache é redefinido, o que descarta as alterações armazenadas ali desde o início do sistema anterior. Portanto, o sistema sempre é iniciado no mesmo estado em que a proteção de disco foi habilitada.  
  
O Windows precisa atualizar alguns arquivos do sistema – incluindo o arquivo de paginação do sistema, o local do despejo de memória e os logs de eventos. Esses arquivos não são descartados quando a proteção de disco está habilitada. Para fazer isso, um novo volume chamado DpReserved é criado quando a proteção de disco é habilitada pela primeira vez e esses arquivos são movidos para esse volume. A partição DpReserved não está protegida, portanto, as gravações nesses arquivos persistem por meio de reinicializações, mesmo quando a proteção de disco está habilitada.  
  
## <a name="schedule-software-updates"></a>Agendar atualizações de software  
Se o Windows estiver configurado para instalar atualizações do Windows automaticamente, a proteção de disco permitirá essas atualizações no momento configurado e não descartará as atualizações. Por exemplo, se as atualizações do Windows estiverem agendadas para 3:00 a.m., a proteção de disco verificará se há atualizações por dia às 3:00. Se alguma atualização for encontrada, os serviços do MultiPoint desabilitarão temporariamente a proteção de disco, aplicará as atualizações e habilitará novamente a proteção de disco.  
   
1.  No Gerenciador do MultiPoint, exiba a guia **página inicial** e clique em **agendar atualizações de software**.  
  
2.  Na caixa de diálogo agendar atualizações de software, clique em **Atualizar em**e selecione uma hora para atualizações-por exemplo, **3:00 am**.  
  
3.  Marque a caixa de seleção **executar Windows Update** .  
  
4.  Se sua organização executar seu próprio script de atualização, marque a caixa de seleção **executar o seguinte programa** e especifique o local do script de atualização da sua organização.  
  
5.  Selecione um tempo máximo para permitir que as atualizações sejam executadas.  
  
6.  Em **quando concluído**, escolha se deseja que o sistema retorne para seu estado de energia anterior ou desligue depois de aplicar as atualizações.  
  
7.  Clique em **OK**.  
  
## <a name="temporarily-disable-disk-protection"></a>Desabilitar temporariamente a proteção de disco  
Se um administrador precisar instalar o software, alterar as configurações do sistema ou executar outras tarefas de manutenção que envolvam atualizações do sistema, eles poderão desabilitar temporariamente a proteção de disco. Depois que as alterações forem feitas, habilite novamente a proteção de disco. Durante a reinicialização do sistema, o sistema reterá seu estado quando a proteção de disco tiver sido habilitada.  
    
1.  No Gerenciador do MultiPoint, clique na guia **página inicial** .  
  
2.  Na guia início, clique em **desabilitar proteção de disco**e, em seguida, clique em **OK**.  
  
> [!NOTE]  
> Lembre-se de reabilitar a proteção de disco após a conclusão da manutenção. O sistema não será protegido novamente até que o administrador reabilite a proteção de disco explicitamente.  
  
## <a name="uninstall-disk-protection"></a>Desinstalar proteção de disco  
A desinstalação da proteção de disco remove o driver e o arquivo de cache, portanto, você deve fazer isso somente se quiser parar de usar a proteção de disco a longo prazo. Se você simplesmente deseja executar a manutenção ou interromper a proteção temporariamente, use a tarefa desabilitar proteção de disco em vez disso.  
  
Você pode desinstalar a proteção de disco se ela estiver habilitada ou desabilitada.  
   
1.  No Gerenciador do MultiPoint, clique na guia **página inicial** .  
  
2.  Na guia início, clique em **desinstalar proteção de disco**e, em seguida, clique em **OK**.  
  
    Depois de clicar em **OK**, o computador será reiniciado. O processo de desinstalação requer várias reinicializações, durante as quais o arquivo de driver e de cache são removidos. A partição DpReserved permanece e o arquivo de paginação, o local de despejo de memória e os arquivos de log de eventos permanecem configurados para usar a partição DpReserved.