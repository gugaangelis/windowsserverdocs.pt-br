---
title: Preparar suas máquinas virtuais para Área de Trabalho Remota
description: Prepare suas VMs para componentes de área de trabalho remota
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: remote-desktop-services
ms.author: elizapo
ms.date: 07/21/2017
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 2fc39dff-61ca-4eba-81ab-52289081bead
author: lizap
manager: dongill
ms.openlocfilehash: cb06963c3ae51fb9337827c7b29b93b8c2736c16
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59871877"
---
# <a name="create-virtual-machines-for-remote-desktop"></a>Criar máquinas virtuais para a Área de Trabalho Remota

>Aplica-se a: Windows Server (canal semestral), Windows Server 2016

Você pode instalar componentes de serviços de área de trabalho remota em servidores físicos ou em máquinas virtuais. 

A primeira etapa é [criar máquinas virtuais do Windows Server no Azure](/azure/virtual-machines/windows/quick-create-portal). Você deseja criar três VMs: um para o Host de sessão de área de trabalho remota, um para o agente de Conexão e um para a Web da área de trabalho remota e Gateway de área de trabalho remota. Para garantir a disponibilidade da sua implantação do RDS, crie uma disponibilidade definida (sob **alta disponibilidade** no processo de criação de VM) e agrupar várias VMs em que o conjunto de disponibilidade.
 
Depois de criar suas VMs, use as seguintes etapas para prepará-los para RDS.

1.  Conecte-se à máquina virtual usando o cliente de Conexão de área de trabalho remota (RDC):  
    1.  No portal do Azure, abra a exibição de grupos de recursos e, em seguida, clique no grupo de recursos a ser usado para a implantação.  
    2.  Selecione a nova máquina de virtual do RDSH (por exemplo, Contoso-Sh1).  
    3.  Clique em **Connect > Abrir** para abrir o cliente de área de trabalho remota.  
    4.  No cliente, clique em **Connect**e, em seguida, clique em **usar outra conta de usuário**. Insira o nome de usuário e a senha da conta de administrador local.  
    5.  Clique em **Sim** quando avisado sobre o certificado.  
2.  Habilite o gerenciamento remoto:  
    1.  No Gerenciador do servidor, clique em **servidor Local > configuração atual do gerenciamento remoto (desabilitada)**.  
    2.  Selecione **habilitar o gerenciamento remoto deste servidor**.  
    3.  Clique em **OK**.  
3.  Opcional: Você pode definir temporariamente o Windows Update para baixar e instalar as atualizações não automaticamente. Isso ajuda a impedir alterações e reinicializações do sistema, enquanto você implanta o servidor de RDSH.  
    1.  No Gerenciador do servidor, clique em **servidor Local > configuração atual do Windows Update**.  
    2.  Selecione **Advanced options > Adiar atualizações**.   
4.  Adicione o servidor ao domínio:  
    1.  No Gerenciador do servidor, clique em **servidor Local > configuração atual do grupo de trabalho**.  
    2.  Clique em **alteração > domínio**e, em seguida, insira o nome de domínio (por exemplo, Contoso.com).  
    3.  Insira as credenciais de administrador de domínio.  
    4.  Reinicie a máquina virtual.  
5.  Repita as etapas 1 a 4 para a máquina virtual Web da área de trabalho remota e GW.  
6.  Repita as etapas 1 a 4 para a máquina de virtual do agente de Conexão de área de trabalho.  
7.  Inicializar e formatar o disco anexado na máquina de virtual do agente de Conexão de área de trabalho:  
    1.  Conecte-se para a máquina virtual do agente de Conexão de área de trabalho (etapa 1 acima).  
    2.  No Gerenciador do servidor, clique em **Ferramentas > Gerenciamento do computador**.  
    3.  Clique em **gerenciamento de disco**.  
    4.  Selecione o disco associado, em seguida **MBR (registro mestre de inicialização)** e, em seguida, clique em **Okey**.  
    5.  Clique com botão direito no novo disco (marcado como **não alocado**) e clique em **Novo Volume simples**.  
    6.  No **Novo Volume simples** assistente, aceite os valores padrão, mas forneça um nome aplicável para o **rótulo do Volume** (como compartilhamentos).  
8.  Na máquina virtual de agente de Conexão de área de trabalho crie compartilhamentos de arquivos para os discos de perfil de usuário e certificados:   
    1.  Abra o Explorador de arquivos, clique em **este PC**e abra o disco que você adicionou para compartilhamentos de arquivos.  
    2.  Clique em **página inicial** e **nova pasta**.  
    3.  Insira um nome para a pasta de discos do usuário, por exemplo, **UserDisks**.  
    4.  Clique com botão direito na nova pasta e clique em **Propriedades > compartilhamento > compartilhamento avançado**.  
    5.  Selecione **compartilhar esta pasta** e clique em **permissões**.  
    6.  Selecione **Everyone**e, em seguida, clique em **remover**. Agora, clique em **Add**, insira **Admins. do domínio**e clique em **Okey**.  
    7.  Selecione **Allow Full Control**e, em seguida, clique em **Okey > Okey > fechar**.  
    8.  Repita as etapas de c. para g. para criar uma pasta compartilhada para certificados.   


