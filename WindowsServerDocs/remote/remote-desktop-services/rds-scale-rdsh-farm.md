---
title: Escalar horizontalmente sua implantação do RDS com a adição de um farm de Host de sessão de área de trabalho remota
description: Adicione um segundo Host de sessão de área de trabalho remota para seu ambiente de RDS.
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: remote-desktop-services
ms.author: elizapo
ms.date: 04/10/2017
ms.tgt_pltfrm: na
ms.topic: article
author: lizap
manager: dongill
ms.openlocfilehash: 0e3852b4ea5f1080a3798c0806e5c87ca808c3be
ms.sourcegitcommit: eaf071249b6eb6b1a758b38579a2d87710abfb54
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 05/31/2019
ms.locfileid: "66446525"
---
# <a name="scale-out-your-remote-desktop-services-deployment-by-adding-an-rd-session-host-farm"></a>Escalar horizontalmente sua implantação de serviços de área de trabalho remota com a adição de um farm de Host de sessão de área de trabalho remota

>Aplica-se a: Windows Server (canal semestral), Windows Server 2019, Windows Server 2016

Você pode melhorar a disponibilidade e escala de sua implantação do RDS com a adição de um farm de Host de sessão de área de trabalho remota (RDSH).   
  
 
Use as etapas a seguir para adicionar outro Host de sessão de área de trabalho remota para sua implantação:  
  
1. Crie um servidor para hospedar o segundo Host de sessão de área de trabalho remota. Se você estiver usando máquinas virtuais do Azure, certifique-se de incluir a nova VM no mesmo conjunto de disponibilidade que contém seu Host de sessão de área de trabalho remota primeiro.
2. Habilite o gerenciamento remoto no novo servidor ou máquina virtual:
   1. No Gerenciador do servidor, clique em **servidor Local > configuração atual do gerenciamento remoto (desabilitada)** . 
   2. Selecione **habilitar o gerenciamento remoto para este servidor**e, em seguida, clique em **Okey**. 
   3. Opcional: Você pode definir temporariamente o Windows Update para baixar e instalar as atualizações não automaticamente. Isso ajuda a impedir alterações e reinicializações do sistema, enquanto você implanta o servidor de RDSH. No Gerenciador do servidor, clique em **servidor Local > configuração atual do Windows Update**. Clique em **Advanced options > Adiar atualizações**. 
3. Adicione o servidor ou vm no domínio:
   1. No Gerenciador do servidor, clique em **servidor Local > configuração atual do grupo de trabalho**. 
   2. Clique em **alteração > domínio**e, em seguida, insira o nome de domínio (por exemplo, Contoso.com). 
   3. Insira as credenciais de administrador de domínio. 
   4. Reinicie o servidor ou vm.
4. Adicione novo Host de sessão RD no farm:
   >[!NOTE] 
   > Etapa 1, criar um endereço IP público para a máquina virtual RDMS, só é necessário se você estiver usando uma vm para os RDMS e se ele ainda não tiver um endereço IP atribuído.
   
   1. Crie um endereço IP público para a máquina virtual executando serviços de gerenciamento de área de trabalho remota (RDMS). A máquina virtual RDMS normalmente será a máquina virtual executando a primeira instância da função de agente de Conexão de área de trabalho.  
       1. No portal do Azure, clique em **procurar > grupos de recursos**, clique no grupo de recursos para a implantação e, em seguida, clique na máquina de virtual RDMS (por exemplo, Contoso-Cb1).  
       2. Clique em **Configurações > interfaces de rede**e, em seguida, clique em interface de rede correspondente.   
       3. Clique em **Configurações > endereço IP**.
       4. Para **endereço IP público**, selecione **Enabled**e, em seguida, clique em **endereço IP**.   
       5. Se você tiver um endereço IP público existente que você deseja usar, selecione-o na lista. Caso contrário, clique em **criar novo**, insira um nome e, em seguida, clique em **Okey** e, em seguida, **salvar**.   
   2. Logon nos RDMS.
   3. Adicione o novo servidor RDSH ao Gerenciador do servidor:   
       1. Inicie o Gerenciador do servidor, clique em **Gerenciar > Adicionar servidores**.   
       2. Na caixa de diálogo Adicionar servidores, clique em **Localizar agora**.   
       3. Selecione o servidor que você deseja usar para o Host de sessão de área de trabalho remota ou a máquina virtual recém-criada (por exemplo, Contoso-Sh2) e clique em **Okey**.
   4. Adicionar o servidor de RDSH à implantação
       1. Inicie o Gerenciador do servidor.  
       2. Clique em **dos serviços de área de trabalho remota > Visão geral > servidores de implantação > tarefas > Adicionar servidores de Host de sessão de área de trabalho remota**.   
       3. Selecione o novo servidor (por exemplo, Contoso-Sh2) e, em seguida, clique em **próxima**.  
       4. Na página de confirmação, selecione **reiniciar computadores remotos, conforme a necessidade**e, em seguida, clique em **Add**.   
   5. Adicione servidor de RDSH ao farm da coleção:
       1. Inicie o Gerenciador do Servidor.   
       2. Clique em **Remote Desktop Services** e, em seguida, clique na coleção à qual você deseja adicionar o servidor RDSH (por exemplo, ContosoDesktop) recém-criado.   
       3. Sob **servidores de Host**, clique em **tarefas > Adicionar servidores de Host de sessão de área de trabalho remota**.   
       4. Selecione o servidor criado recentemente (por exemplo, Contoso-Sh2) e, em seguida, clique em **próxima**.   
       5. Na página de confirmação, clique em **adicionar**.   

