---
title: Preparar suas máquinas virtuais para Área de Trabalho Remota
description: Preparar as VMs para os componentes da Área de Trabalho Remota
ms.prod: windows-server
ms.technology: remote-desktop-services
ms.author: elizapo
ms.date: 07/21/2017
ms.topic: article
ms.assetid: 2fc39dff-61ca-4eba-81ab-52289081bead
author: lizap
manager: dongill
ms.openlocfilehash: 5aec90275db1e09906e051419929086a40a34800
ms.sourcegitcommit: 3a3d62f938322849f81ee9ec01186b3e7ab90fe0
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/23/2020
ms.locfileid: "80858129"
---
# <a name="create-virtual-machines-for-remote-desktop"></a>Criar máquinas virtuais para a Área de Trabalho Remota

>Aplica-se a: Windows Server (Canal Semestral), Windows Server 2019, Windows Server 2016

É possível instalar componentes de Serviços de Área de Trabalho Remota em servidores físicos ou em máquinas virtuais. 

A primeira etapa é [Criar máquinas virtuais do Windows Server no Azure](/azure/virtual-machines/windows/quick-create-portal). Você deverá criar três VMs: um para o Host da Sessão RD, uma para o Agente de Conexão e uma para a Web da Área de Trabalho Remota e o Gateway de Área de Trabalho Remota. Para garantir a disponibilidade da implantação de RDS, crie um conjunto de disponibilidade (em **Alta disponibilidade** no processo de criação da VM) e agrupe várias VMs nesse conjunto de disponibilidade.
 
Depois de criar as VMs, use as seguintes etapas para prepará-las para RDS.

1.  Conecte à máquina virtual usando o cliente RDC (Conexão de Área de Trabalho Remota):  
    1.  No portal do Azure, abra Exibir grupos de recursos e, em seguida, clique no grupo de recursos a ser usado para a implantação.  
    2.  Selecione a nova máquina virtual RDSH (por exemplo, Contoso-Sh1).  
    3.  Clique em **Conectar > Abrir** para abrir o cliente da Área de Trabalho Remota.  
    4.  No cliente, clique em **Conectar** e, em seguida, clique em **Usar outra conta de usuário**. Insira o nome de usuário e a senha da conta de administrador local.  
    5.  Clique em **Sim** quando for avisado sobre o certificado.  
2.  Habilite o gerenciamento remoto:  
    1.  No Gerenciador de Servidores, clique em **Servidor local > Configuração atual do gerenciamento remoto (desabilitado)** .  
    2.  Selecione **Habilitar o gerenciamento remoto para este servidor**.  
    3.  Clique em **OK**.  
3.  Opcional: É possível configurar temporariamente o Windows Update para não baixar e instalar as atualizações automaticamente. Isso ajuda a impedir alterações e reinicializações do sistema enquanto você implanta o servidor RDSH.  
    1.  No Gerenciador de Servidores, clique em **Servidor local > Configuração atual do Windows Update**.  
    2.  Selecione **Opções avançadas > Adiar atualizações**.   
4.  Adicione o servidor ao domínio:  
    1.  No Gerenciador de Servidores, clique em **Servidor local > Configuração atual do grupo de trabalho**.  
    2.  Clique em **Alterar > Domínio** e, em seguida, insira o nome de domínio (por exemplo, Contoso.com).  
    3.  Insira as credenciais do administrador de domínio.  
    4.  Reinicie a máquina virtual.  
5.  Repita as etapas 1 a 4 para a máquina virtual da Web e do GW de Área de Trabalho Remota.  
6.  Repita as etapas 1 a 4 para a máquina virtual do Agente de Conexão de Área de Trabalho Remota.  
7.  Inicialize e formate o disco conectado na máquina virtual do Agente de Conexão de Área de Trabalho Remota:  
    1.  Conecte à máquina virtual do Agente de Conexão de Área de Trabalho Remota (etapa 1 acima).  
    2.  No Gerenciador de Servidores, clique em **Ferramentas > Gerenciamento do computador**.  
    3.  Clique em **Gerenciamento de disco**.  
    4.  Selecione o disco conectado e, em seguida **MBR (Registro Mestre de Inicialização)** e, em seguida, clique em **OK**.  
    5.  Clique com botão direito do mouse no novo disco (marcado como **Não alocado**) e clique em **Novo volume simples**.  
    6.  No assistente **Novo volume simples**, aceite os valores padrão, mas forneça um nome aplicável para o **Rótulo do volume** (como Compartilhamentos).  
8.  Na máquina virtual do Agente de Conexão de Área de Trabalho Remota, crie compartilhamentos de arquivo para os discos de perfil de usuário e certificados:   
    1.  Abra o Explorador de arquivos, clique em **Este PC** e abra o disco que você adicionou para compartilhamentos de arquivo.  
    2.  Clique em **Página inicial** e **Nova pasta**.  
    3.  Insira um nome para a pasta de discos do usuário, por exemplo, **UserDisks**.  
    4.  Clique com botão direito do mouse na nova pasta e clique em **Propriedades > Compartilhamento > Compartilhamento avançado**.  
    5.  Selecione **Compartilhar esta pasta** e clique em **Permissões**.  
    6.  Clique em **Todos** e, em seguida, **Remover**. Agora, clique em **Adicionar**, insira **Admins. do domínio** e clique em **OK**.  
    7.  Selecione **Permitir controle total** e, em seguida, clique em **OK > OK > Fechar**.  
    8.  Repita as etapas c. a g. para criar uma pasta compartilhada para certificados.   


