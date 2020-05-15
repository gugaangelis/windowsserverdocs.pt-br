---
title: 'Etapa 6: Rebaixar e remover o servidor de origem da nova rede do Windows Server Essentials'
description: Descreve como usar o Windows Server Essentials
ms.date: 10/03/2016
ms.prod: windows-server
ms.topic: article
ms.assetid: 86244c66-2c5e-488d-adb8-112e1ca3e2e1
author: nnamuhcs
ms.author: coreyp
manager: dongill
ms.openlocfilehash: 406010234528a094bfb36a606072f98c043c397a
ms.sourcegitcommit: 2f072c0c02e3e0deae331ca64b375d63b89d0522
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 05/14/2020
ms.locfileid: "83404519"
---
# <a name="step-6-demote-and-remove-the-source-server-from-the-new-windows-server-essentials-network"></a>Etapa 6: Rebaixar e remover o servidor de origem da nova rede do Windows Server Essentials

>Aplica-se a: Windows Server 2016 Essentials, Windows Server 2012 R2 Essentials

Depois de concluir a instalação do Windows Server Essentials e concluir a migração, você deve executar as seguintes tarefas:  
  
1.  [Remover os serviços de certificados do Active Directory](Step-6--Demote-and-remove-the-Source-Server-from-the-new-Windows-Server-Essentials-network.md#BKMK_ADCS)  
  
2.  [Desconectar impressoras diretamente conectadas ao servidor de origem](Step-6--Demote-and-remove-the-Source-Server-from-the-new-Windows-Server-Essentials-network.md#BKMK_PhysicallyDisconnect)  
  
3.  [Rebaixar o servidor de origem](Step-6--Demote-and-remove-the-Source-Server-from-the-new-Windows-Server-Essentials-network.md#BKMK_DemoteTheSourceServer)  
  
4.  [Remover e realocar o servidor de origem](Step-6--Demote-and-remove-the-Source-Server-from-the-new-Windows-Server-Essentials-network.md#BKMK_RemoveTheSourceServer)  
  
##  <a name="remove-active-directory-certificate-services"></a><a name="BKMK_ADCS"></a>Remover Active Directory serviços de certificados  
 O procedimento é ligeiramente diferente se você tiver vários serviços de função de serviços de certificados do Active Directory (AD CS) instalados em um único servidor. Você pode usar o procedimento a seguir para desinstalar um serviço de função do AD CS e reter outros serviços de função do AD CS.  
  
 Para concluir este procedimento, faça logon com as mesmas permissões do usuário que instalou a autoridade de certificação (CA). Se você estiver desinstalando uma AC corporativa, a associação em Admins Corporativos ou equivalente será o requisito mínimo para concluir este procedimento.  
  
#### <a name="to-remove-ad-cs"></a>Para remover o AD CS  
  
1.  Faça logon no servidor de origem como um administrador de domínio.  
  
2.  Clique em **Iniciar**, clique em **Ferramentas Administrativas** e clique em **Gerenciador do Servidores**.  
  
3.  Clique em **Continuar** na caixa de diálogo **Controle de Conta de Usuário**.  
  
4.  Na seção **Resumo de Funções**, clique em **Remover Funções**.  
  
5.  No Assistente para Remover Funções, clique em **Avançar**.  
  
6.  Desmarque a caixa de seleção **Serviços de Certificados do Active Directory** e clique em **Avançar**.  
  
7.  Na página **Confirmar Opções de Remoção**, examine as informações e clique em **Remover**.  
  
    > [!NOTE]
    >  Se estiver executando Serviços de Informações da Internet (IIS), precisará interromper o serviço antes de continuar. Clique em **OK**.  
  
    > [!NOTE]
    >  Primeiro, talvez seja necessário remover **Registro via Web de autoridade de certificação**, se ele está instalado.  
  
8.  Quando o Assistente para Remover Funções terminar, reinicie o servidor para concluir o processo de desinstalação.  
  
    > [!IMPORTANT]
    >  Reinicie o servidor mesmo que isso não seja solicitado.  
  
##  <a name="disconnect-printers-that-are-directly-connected-to-the-source-server"></a><a name="BKMK_PhysicallyDisconnect"></a>Desconecte as impressoras que estão conectadas diretamente ao servidor de origem  
 Antes de rebaixar o servidor de origem, desconecte fisicamente todas as impressoras diretamente conectadas ao servidor de origem e compartilhadas por meio do servidor de origem. Garanta que nenhum objeto do Active Directory permaneça para as impressoras estiverem diretamente conectadas ao servidor de origem. As impressoras podem ser conectadas diretamente ao servidor de destino e compartilhadas do Windows Server Essentials.  
  
##  <a name="demote-the-source-server"></a><a name="BKMK_DemoteTheSourceServer"></a>Rebaixar o servidor de origem  
 Antes de rebaixar o servidor de origem da função de controlador de domínio AD DS para a função de um servidor membro de domínio, certifique-se de que as configurações de política de grupo tenham sido aplicadas a todos os computadores cliente, conforme descrito no procedimento a seguir.  
  
> [!IMPORTANT]
>  O servidor de origem e o servidor de destino devem estar conectados à rede enquanto as alterações de política de grupo são atualizadas nos computadores cliente.  
  
#### <a name="to-force-a-group-policy-update-on-a-client-computer"></a>Para forçar uma atualização de Política de Grupo em um computador cliente  
  
1. Autentique-se no computador cliente como administrador.  
  
2. Abra uma janela de Prompt de comando como administrador.  
  
3. No prompt de comando, digite **gpupdate /force** e pressione ENTER.  
  
4. O processo pode exigir fazer logoff e logon novamente para ser concluído. Clique em **Sim** para confirmar.  
  
   Se você estiver migrando do Windows Server Essentials ou de suas versões anteriores, para rebaixar o servidor, consulte [remover Active Directory Domain Services](https://technet.microsoft.com/library/hh472163.aspx). Depois de adicionar o servidor de origem como um membro de um grupo de trabalho e desconectá-lo da rede, remova-o do AD DS no servidor de destino.  
  
   Se você estiver migrando do Windows Server Essentials, use Gerenciador do Servidor para remover a função Active Directory Domain Services, rebaixando o controlador de domínio no servidor de origem usando o seguinte procedimento:  
  
#### <a name="to-remove-the-source-server-from-active-directory"></a>Para remover o servidor de origem do Active Directory  
  
1.  No servidor de destino, abra **Usuários e Computadores do Active Directory**.  
  
2.  No painel de navegação **Usuários e Computadores do Active Directory**, expanda o nome do domínio e expanda **Computadores**.  
  
3.  Se o servidor de origem ainda existe na lista de servidores, clique com botão direito no nome do servidor de origem, clique em **Excluir** e depois clique em **Sim**.  
  
4.  Verifique se o servidor de origem não está listado e, em seguida, feche **Usuários e Computadores do Active Directory**.  
  
##  <a name="remove-and-repurpose-the-source-server"></a><a name="BKMK_RemoveTheSourceServer"></a>Remover e realocar o servidor de origem  
 Desative o servidor de origem e desconecte-o da rede. É recomendável não reformatar o servidor de origem por pelo menos uma semana para garantir que todos os dados necessários sejam migrados para o servidor de destino. Depois de ter verificado que todos os dados foram migrados, você pode reinstalar este servidor na rede como um servidor secundário para outras tarefas, se necessário.  
  
> [!NOTE]
>  Depois de rebaixar e remover o servidor de origem, reinicie o servidor de destino.  
  
 Depois de rebaixar o servidor de origem, ele não está em estado íntegro. Se você desejar realocar o servidor de origem, a maneira mais simples é reformatá-lo, instalar um sistema operacional de servidor e, em seguida, configurá-lo para uso como um servidor adicional.  
  
## <a name="next-steps"></a>Próximas etapas  
 Você rebaixou e removeu o servidor de origem da nova rede do Windows Server Essentials. Agora vá para a [etapa 7: executar tarefas de pós-implantação para a migração do Windows Server Essentials](Step-7--Perform-post-migration-tasks-for-the-Windows-Server-Essentials-migration.md).  
  

Para exibir todas as etapas, consulte [migrar para o Windows Server Essentials](Migrate-from-Previous-Versions-to-Windows-Server-Essentials-or-Windows-Server-Essentials-Experience.md).

