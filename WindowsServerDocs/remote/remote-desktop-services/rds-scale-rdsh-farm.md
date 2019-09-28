---
title: Expandir sua implantação do RDS adicionando um farm de Host da Sessão RD
description: Adicione um segundo Host da Sessão RD ao seu ambiente de RDS.
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: remote-desktop-services
ms.author: elizapo
ms.date: 04/10/2017
ms.tgt_pltfrm: na
ms.topic: article
author: lizap
manager: dongill
ms.openlocfilehash: da0dbd4332cd05d580c2b1f4dc5eb0734b36b13e
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71403888"
---
# <a name="scale-out-your-remote-desktop-services-deployment-by-adding-an-rd-session-host-farm"></a>Expandir sua implantação de Serviços de Área de Trabalho Remota adicionando um farm de Host da Sessão RD

>Aplica-se a: Windows Server (Canal Semestral), Windows Server 2019, Windows Server 2016

Você pode melhorar a disponibilidade e a escala de sua implantação do RDS adicionando um farm do RDSH (Host da Sessão da Área de Trabalho Remota).   
  
 
Use as etapas a seguir para adicionar outro Host da Sessão RD à sua implantação:  
  
1. Crie um servidor para hospedar o segundo Host da Sessão RD. Se estiver usando máquinas virtuais do Azure, certifique-se de incluir a nova VM no mesmo conjunto de disponibilidade que contém o primeiro Host da Sessão RD.
2. Habilite o gerenciamento remoto no novo servidor ou máquina virtual:
   1. No Gerenciador do Servidor, clique em **Servidor Local > Configuração atual do gerenciamento remoto (desabilitado)** . 
   2. Selecione **Habilitar o gerenciamento remoto para este servidor** e, em seguida, clique em **OK**. 
   3. Opcional: É possível configurar temporariamente o Windows Update para não baixar e instalar as atualizações automaticamente. Isso ajuda a impedir alterações e reinicializações do sistema enquanto você implanta o servidor RDSH. No Gerenciador do Servidor, clique em **Servidor Local > Configuração atual do Windows Update**. Clique em **Opções avançadas > Adiar atualizações**. 
3. Adicione o servidor ou VM ao domínio:
   1. No Gerenciador do Servidor, clique em **Servidor Local > Configuração atual do grupo de trabalho**. 
   2. Clique em **Alterar > Domínio** e, em seguida, insira o nome de domínio (por exemplo, Contoso.com). 
   3. Insira as credenciais do administrador de domínio. 
   4. Reinicie o servidor ou VM.
4. Adicione novo Host da Sessão RD ao farm:
   >[!NOTE] 
   > A Etapa 1, criar um endereço IP público para a máquina virtual RDMS, só é necessária quando você está usando uma VM para o RDMS e ela ainda não tem um endereço IP atribuído.
   
   1. Crie um endereço IP público para a máquina virtual executando o RDMS (Serviços de Gerenciamento da Área de Trabalho Remota). A máquina virtual do RDMS normalmente será a máquina virtual executando a primeira instância da função do Agente de Conexão de Área de Trabalho Remota.  
       1. No portal do Azure, clique em **Procurar > Grupos de recursos**, clique no grupo de recursos para a implantação e, em seguida, clique na máquina virtual do RDMS (por exemplo, Contoso-Cb1).  
       2. Clique em **Configurações > Interfaces de rede** e, em seguida, clique no adaptador de rede correspondente.   
       3. Clique em **Configurações > Endereço IP**.
       4. Em **Endereço IP público**, selecione **Habilitado** e clique em **Endereço IP**.   
       5. Se tiver um endereço IP público que deseja usar, selecione-o na lista. Caso contrário, clique em **Criar novo**, insira um nome e, em seguida, clique em **OK** e em **Salvar**.   
   2. Entre no RDMS.
   3. Adicione o novo servidor do RDSH ao Gerenciador do Servidor:   
       1. Inicie o Gerenciador do Servidor, clique em **Gerenciar > Adicionar Servidores**.   
       2. Na caixa de diálogo Adicionar Servidores, clique em **Localizar Agora**.   
       3. Selecione o servidor que você deseja usar para o Host da Sessão RD ou a máquina virtual recém-criada (por exemplo, Contoso-Sh2) e clique em **OK**.
   4. Adicione o servidor de RDSH à implantação
       1. Inicie o Gerenciador do Servidor.  
       2. Clique em **Serviços de Área de Trabalho Remota > Visão Geral > Servidores de Implantação > Tarefas > Adicionar servidores Host da Sessão RD**.   
       3. Selecione o novo servidor (por exemplo, Contoso-Sh2) e, em seguida, clique em **Avançar**.  
       4. Na página de confirmação, selecione **Reiniciar computadores remotos, conforme o necessário** e, em seguida, clique em **Adicionar**.   
   5. Adicione o servidor de RDSH ao farm da coleção:
       1. Inicie o Gerenciador do Servidor.   
       2. Clique em **Serviços de Área de Trabalho Remota** e, em seguida, clique na coleção à qual você deseja adicionar o servidor RDSH (por exemplo, ContosoDesktop) recém-criado.   
       3. Em **Servidores de Host**, clique em **Tarefas > Adicionar servidores Host da Sessão RD**.   
       4. Selecione o servidor recém-criado (por exemplo, Contoso-Sh2) e, em seguida, clique em **Avançar**.   
       5. Na página de confirmação, clique em **Adicionar**.   

