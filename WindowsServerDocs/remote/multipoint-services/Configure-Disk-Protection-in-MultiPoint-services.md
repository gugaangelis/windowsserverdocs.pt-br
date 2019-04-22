---
title: Configurar Proteção de Disco no MultiPoint Services
description: Saiba como configurar a proteção de disco para os serviços do MultiPoint
ms.custom: na
ms.prod: windows-server-threshold
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
ms.openlocfilehash: dc0a46b57753f08cc7d79fd05de7a9e81469cc86
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59820167"
---
# <a name="configure-disk-protection"></a>Configurar a proteção de disco
Você pode usar a proteção de disco no Multipoint Services para proteger o volume do sistema de atualizações não intencionais, para agendar atualizações do Windows a ser retido enquanto a proteção de disco está ativa, para desabilitar temporariamente a proteção de disco e desinstalar a proteção de disco.  
  
Ao habilitar a proteção de disco no MultiPoint Services, você pode proteger o volume do sistema (a unidade em que o Windows está instalado, geralmente c) contra alterações indesejadas. Quando a proteção de disco é habilitada, as alterações feitas no volume do sistema são armazenadas em um local temporário para que simplesmente reiniciar o computador descarta-os e retorna automaticamente o sistema ao estado anterior de boas.  
  
O administrador pode facilmente instalar software ou fazer alterações de configuração, desative temporariamente a proteção de disco. Para manter o sistema atualizado com as atualizações do Windows e definições anti-malware, proteção de disco agenda uma janela de manutenção para baixar e instalar atualizações. O administrador também pode fornecer um script personalizado para execução durante a janela de manutenção para acomodar qualquer necessidade de manutenção, além do Windows Update.  
  
## <a name="enable-disk-protection"></a>Habilitar a proteção de disco  
Antes de habilitar a proteção de disco, verifique se que todos os drivers e aplicativos estão instalados e atualizados e mover seus perfis de usuário para um volume que não será protegido. Se você precisar fazer atualizações manuais depois de habilitar a proteção de disco, você pode desabilitar temporariamente a proteção de disco. No entanto, é mais fácil de obter o sistema em um estado ideal antes da proteção de disco está ativada.  
  
 
1.  Faça logon no servidor que executa o MultiPoint Services como um administrador.  
  
2.  Antes de habilitar a proteção de disco:  
  
    -   Verifique se o sistema está em exatamente o estado no qual você deseja que ele permaneça do MultiPoint Services. Por exemplo, certifique-se de que o software instalado, as configurações do sistema e as atualizações estão corretas.  
  
    -   Mover perfis de usuário para um volume que não é protegido, ou configurar um local de arquivo compartilhado desativado o volume do sistema, conforme descrito em [habilitar compartilhamento de arquivos no MultiPoint Services](Enable-file-sharing-in-MultiPoint-services.md).  
  
3.  Dos **inicie** tela, abra **MultiPoint Manager**.  
  
4.  Clique o **página inicial** , clique em **habilitar a proteção de disco**e, em seguida, clique em **Okey**.  
  
Quando a proteção de disco é habilitada pela primeira vez, o sistema estiver preparado, instalar um driver e criando um arquivo de cache no volume do sistema. O arquivo de cache temporariamente armazena todas as alterações feitas no volume do sistema, enquanto a proteção de disco está ativa. Como as atualizações do sistema são armazenadas no arquivo de cache, elas não alteram o conteúdo protegido do volume fora do arquivo de cache. Cada vez que o sistema é iniciado, o arquivo de cache é redefinido, que descarta as alterações armazenadas lá desde o início do sistema anterior. Portanto, o sistema sempre começa no mesmo estado quando a proteção de disco foi habilitada.  
  
Precisa atualizar alguns arquivos do sistema – incluindo o arquivo de paginação do sistema, local de despejo de pane e logs de eventos do Windows. Esses arquivos não são descartados quando a proteção de disco está habilitada. Para fazer isso, um novo volume chamado DpReserved é criado quando a proteção de disco é habilitada pela primeira vez, e esses arquivos são movidos para esse volume. A partição DpReserved não estiver protegida, para manter a gravações para esses arquivos por meio de reinicializações, mesmo quando a proteção de disco está habilitada.  
  
## <a name="schedule-software-updates"></a>Agendar atualizações de software  
Se o Windows estiver configurado para instalar automaticamente as atualizações do Windows, proteção de disco permite que essas atualizações ao tempo configurado e não descarta as atualizações. Por exemplo, se as atualizações do Windows estiver agendadas para 3:00, a proteção de disco verifica se há atualizações por dia às 3 horas. Se todas as atualizações forem encontradas, MultiPoint Services desabilita temporariamente a proteção de disco, aplica as atualizações e, em seguida, habilita a proteção de disco novamente.  
   
1.  No Gerenciador do MultiPoint, exibir o **página inicial** guia e, em seguida, clique em **agendar atualizações de software**.  
  
2.  Na caixa de diálogo Agendar atualizações de Software, clique em **atualizar cada**e selecione uma hora para atualizações - por exemplo, **3:00**.  
  
3.  Selecione o **executar o Windows Update** caixa de seleção.  
  
4.  Se sua organização executa seu próprio script de atualização, selecione o **execute o seguinte programa** caixa de seleção e especifique o local do script de atualização da sua organização.  
  
5.  Selecione um tempo máximo para permitir atualizações ser executado.  
  
6.  Sob **quando terminar**, escolha se deseja que o sistema retornar ao estado de energia anterior ou desligado após a aplicação de atualizações.  
  
7.  Clique em **OK**.  
  
## <a name="temporarily-disable-disk-protection"></a>Desabilitar temporariamente a proteção de disco  
Se um administrador precisa instalar o software, alterar configurações do sistema ou executar outras tarefas de manutenção que envolvem atualizações do sistema, eles podem desabilitar temporariamente a proteção de disco. Depois que as alterações forem feitas, reabilite a proteção de disco. Durante a reinicialização do sistema, o sistema manterá seu estado quando a proteção de disco foi habilitada.  
    
1.  No Gerenciador do MultiPoint, clique o **Home** guia.  
  
2.  Na guia página inicial, clique em **desabilitar a proteção de disco**e, em seguida, clique em **Okey**.  
  
> [!NOTE]  
> Lembre-se de habilitar a proteção de disco novamente após a conclusão da manutenção. O sistema não será protegido novamente até que o administrador explicitamente reabilita a proteção de disco.  
  
## <a name="uninstall-disk-protection"></a>Desinstalar a proteção de disco  
Desinstalando a proteção de disco remove o driver e o arquivo de cache, portanto, você deve fazer isso somente se você quiser parar de usar a proteção de disco de longo prazo. Se você simplesmente deseja realizar a manutenção ou interromper temporariamente a proteção, use a tarefa de proteção de disco Disable.  
  
Você pode desinstalar a proteção de disco se ele está habilitado ou desabilitado.  
   
1.  No Gerenciador do MultiPoint, clique o **Home** guia.  
  
2.  Na guia página inicial e clique **desinstalar a proteção de disco**e, em seguida, clique em **Okey**.  
  
    Depois de clicar em **Okey**, o computador for reiniciado. O processo de desinstalação requer várias reinicializações, durante o qual o driver e o arquivo de cache são removidos. O DpReserved particionar permanece e o arquivo de paginação, local de despejo de pane e log de eventos arquivos permanecem configurado para usar a partição DpReserved.