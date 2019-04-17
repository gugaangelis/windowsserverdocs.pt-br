---
title: 'Etapa 6: Rebaixar e remova o servidor de origem a nova rede do Windows Server Essentials'
description: Descreve como usar o Windows Server Essentials
ms.custom: na
ms.date: 10/03/2016
ms.prod: windows-server-2016-essentials
ms.reviewer: na
ms.suite: na
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 86244c66-2c5e-488d-adb8-112e1ca3e2e1
author: nnamuhcs
ms.author: coreyp
manager: dongill
ms.openlocfilehash: 24a1f2da2333c7e6854e9efd9d996391d0fcb3b9
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 12/12/2017
---
# <a name="step-6-demote-and-remove-the-source-server-from-the-new-windows-server-essentials-network"></a>Etapa 6: Rebaixar e remova o servidor de origem a nova rede do Windows Server Essentials

>Aplica-se a: Windows Server 2016 Essentials, Windows Server 2012 R2 Essentials, Windows Server 2012 Essentials

Depois de concluir a instalação do Windows Server Essentials e concluir a migração, você deve executar as seguintes tarefas:  
  
1.  [Remover os serviços de certificados do Active Directory](Step-6--Demote-and-remove-the-Source-Server-from-the-new-Windows-Server-Essentials-network.md#BKMK_ADCS)  
  
2.  [Desconecte impressoras que estão conectadas diretamente para o servidor de origem](Step-6--Demote-and-remove-the-Source-Server-from-the-new-Windows-Server-Essentials-network.md#BKMK_PhysicallyDisconnect)  
  
3.  [Rebaixar o servidor de origem](Step-6--Demote-and-remove-the-Source-Server-from-the-new-Windows-Server-Essentials-network.md#BKMK_DemoteTheSourceServer)  
  
4.  [Remover e reutilizar o servidor de origem](Step-6--Demote-and-remove-the-Source-Server-from-the-new-Windows-Server-Essentials-network.md#BKMK_RemoveTheSourceServer)  
  
##  <a name="BKMK_ADCS"></a>Remover os serviços de certificados do Active Directory  
 O procedimento é um pouco diferente se você tiver vários serviços de função de serviços de certificados do Active Directory (AD CS) instalados em um único servidor. Você pode usar o procedimento a seguir para desinstalar um serviço de função do AD CS e reter outros serviços de função do AD CS.  
  
 Para concluir este procedimento, você deve fazer logon com as mesmas permissões que o usuário que instalou a autoridade de certificação (CA). Se você estiver desinstalando uma autoridade de certificação corporativa, a associação ao grupo Administradores corporativos ou seu equivalente é o requisito mínimo para concluir este procedimento.  
  
#### <a name="to-remove-ad-cs"></a>Para remover o AD CS  
  
1.  Faça logon no servidor de origem como um administrador de domínio.  
  
2.  Clique em **iniciar**, clique em **ferramentas administrativas**e clique em **Gerenciador do servidor**.  
  
3.  Clique em **continuar** no **User Account Control** caixa de diálogo.  
  
4.  No **resumo de funções** seção, clique em **remover funções**.  
  
5.  No Assistente para remover funções, clique em **próxima**.  
  
6.  Limpar o **serviços de certificados do Active Directory** caixa de seleção e, em seguida, clique em **próxima**.  
  
7.  Sobre o **confirmar as opções de remoção** página, examine as informações e, em seguida, clique em **remover**.  
  
    > [!NOTE]
    >  Se estiver executando o Internet Information Services (IIS), você for solicitado a interromper o serviço antes de prosseguir. Clique em **Okey**.  
  
    > [!NOTE]
    >  Primeiro, talvez seja necessário remover **registro de Web de autoridade de certificação**, se ele está instalado.  
  
8.  Quando concluir o Assistente para remover funções, reinicie o servidor para concluir o processo de desinstalação.  
  
    > [!IMPORTANT]
    >  Reinicie o servidor, mesmo se você não precisará fazer isso.  
  
##  <a name="BKMK_PhysicallyDisconnect"></a>Desconecte impressoras que estão conectadas diretamente para o servidor de origem  
 Antes de você rebaixar o servidor de origem, Desconecte fisicamente todas as impressoras que estão conectadas diretamente para o servidor de origem e são compartilhadas por meio do servidor de origem. Certifique-se de que nenhum objetos do Active Directory permaneçam para as impressoras que foram conectadas diretamente ao servidor de origem. As impressoras podem ser diretamente conectadas ao servidor de destino e compartilhadas do Windows Server Essentials.  
  
##  <a name="BKMK_DemoteTheSourceServer"></a>Rebaixar o servidor de origem  
 Antes de você rebaixar o servidor de origem da função do controlador de domínio do AD DS para a função de um servidor membro do domínio, certifique-se de que as configurações de política de grupo são aplicadas a todos os computadores cliente, conforme descrito no procedimento a seguir.  
  
> [!IMPORTANT]
>  O servidor de origem e o servidor de destino devem estar conectado à rede enquanto as alterações de política de grupo são atualizadas nos computadores cliente.  
  
#### <a name="to-force-a-group-policy-update-on-a-client-computer"></a>Para forçar uma atualização de política de grupo em um computador cliente  
  
1.  Entrar no computador cliente como administrador.  
  
2.  Abra uma janela de Prompt de comando como administrador.  
  
3.  No prompt de comando, digite **gpupdate /force**, e pressione ENTER.  
  
4.  O processo de exigir que você faça logoff e logon novamente para concluir. Clique em **Sim** para confirmar.  
  
 Se você estiver migrando do Windows Server Essentials ou suas versões anteriores, para rebaixar o servidor, consulte [remover Active Directory Domain Services](https://technet.microsoft.com/library/hh472163.aspx). Depois de adicionar o servidor de origem como um membro de um grupo de trabalho e desconecte-o da rede, você deve removê-lo do AD DS no servidor de destino.  
  
 Se você estiver migrando do Windows Server Essentials, use o Gerenciador do servidor para remover a função Serviços de domínio do Active Directory, rebaixando, assim, o controlador de domínio no servidor de origem usando o seguinte procedimento:  
  
#### <a name="to-remove-the-source-server-from-active-directory"></a>Para remover o servidor de origem do Active Directory  
  
1.  No servidor de destino, abra **usuários e Active Directory computadores**.  
  
2.  No **usuários e Active Directory computadores** painel de navegação, expanda o nome do domínio e, em seguida, **computadores**.  
  
3.  Se o servidor de origem ainda existe na lista de servidores, clique com botão direito no nome do servidor de origem, clique em **excluir**e clique em **Sim**.  
  
4.  Verifique se o servidor de origem não está listado e, em seguida, fechar **usuários e Active Directory computadores**.  
  
##  <a name="BKMK_RemoveTheSourceServer"></a>Remover e reutilizar o servidor de origem  
 Desativar o servidor de origem e desconecte-o da rede. Recomendamos que você não reformatar o servidor de origem pelo menos uma semana para garantir que todos os dados necessários migrados para o servidor de destino. Depois de você ter confirmado que todos os dados tem migrados, você poderá reinstalar esse servidor na rede como um servidor secundário para outras tarefas, se necessário.  
  
> [!NOTE]
>  Depois que você rebaixar e remove o servidor de origem, reinicie o servidor de destino.  
  
 Depois que você rebaixar o servidor de origem, ele não está em um estado íntegro. Se você quiser reutilizar o servidor de origem, a maneira mais simples é reformatá-la, instalar um sistema operacional de servidor e, em seguida, configurou para uso como um servidor adicional.  
  
## <a name="next-steps"></a>Próximas etapas  
 Você rebaixado e removeu o servidor de origem da nova rede do Windows Server Essentials. Acesse agora [etapa 7: executar tarefas posteriores à migração para a migração do Windows Server Essentials](Step-7--Perform-post-migration-tasks-for-the-Windows-Server-Essentials-migration.md).  
  

Para ver todas as etapas, consulte [migrar para o Windows Server Essentials](Migrate-from-Previous-Versions-to-Windows-Server-Essentials-or-Windows-Server-Essentials-Experience.md).

