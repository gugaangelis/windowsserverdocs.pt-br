---
title: Gerenciar usuários na coleção de RDS
description: Saiba como gerenciar usuários nos Serviços de Área de Trabalho Remota.
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: remote-desktop-services
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 2727e1ab-69b8-46f3-9f6d-2540324fe596
author: christianmontoya
ms.author: chrimo
ms.date: 03/27/2018
manager: scottman
ms.openlocfilehash: 870a6360f685c2de31485135202b0f1415c90d85
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71403856"
---
# <a name="manage-users-in-your-rds-collection"></a>Gerenciar usuários na coleção de RDS

>Aplica-se a: Windows Server (Canal Semestral), Windows Server 2019, Windows Server 2016

Como administrador, você pode gerenciar diretamente quais usuários têm acesso a coleções específicas. Dessa forma, você pode criar uma coleção com aplicativos padrão para os operadores de informações, mas, em seguida, crie uma coleção separada com aplicativos de modelagem de gráficos avançados para engenheiros. Há duas etapas principais para gerenciar o acesso de usuário em uma implantação de RDS (Serviços de Área de Trabalho Remota):

1.  [Criar usuários e grupos no Active Directory](#create-your-users-and-groups-in-active-directory)
2.  [Atribuir usuários e grupos a coleções](#assign-users-and-groups-to-collections)


## <a name="create-your-users-and-groups-in-active-directory"></a>Criar seus usuários e grupos no Active Directory

Em uma implantação de RDS, o AD DS (Active Directory Domain Services) é a origem de todos os usuários, grupos e outros objetos no domínio. Você pode gerenciar o Active Directory diretamente com o PowerShell ou pode usar ferramentas de interface do usuário internas que adicionam flexibilidade e facilidade. As etapas a seguir vão guiá-lo para instalar essas ferramentas (se você ainda não tiver instalado). Em seguida, use essas ferramentas para gerenciar usuários e grupos.

### <a name="install-ad-ds-tools"></a>Instalar ferramentas do AD DS

As etapas a seguir detalham como instalar as ferramentas do AD DS em um servidor que já executa o AD DS. Depois de instalar, você pode criar usuários ou grupos.

1. Conecte-se com o servidor que executa o Active Directory Domain Services. Para implantações do Azure:
   1. No portal do Azure, clique em **Procurar > Grupos de recursos** e clique no grupo de recursos para a implantação
   2. Selecione a máquina virtual AD.
   3. Clique em **Conectar > Abrir** para abrir o cliente de Área de Trabalho Remota. Se a opção **Conectar** estiver esmaecida, a máquina virtual poderá não ter um endereço IP público. Para dar a ela um endereço, execute as etapas a seguir e, em seguida, tente novamente.
      1. Clique em **Configurações > Interfaces de rede** e, em seguida, clique no adaptador de rede correspondente.
      2. Clique em **Configurações > Endereço IP**.
      3. Em **Endereço IP público**, selecione **Habilitado** e clique em **Endereço IP**.
      4. Se tiver um endereço IP público que deseja usar, selecione-o na lista. Caso contrário, clique em **Criar novo**, insira um nome e, em seguida, clique em **OK** e em **Salvar**.
   4. No cliente, clique em **Conectar**e, em seguida, clique em **Usar outra conta**. Insira o nome de usuário e a senha da conta de administrador de domínio.
   5. Clique em **Sim** quando for perguntado sobre o certificado.
2. Instale as ferramentas do AD DS:
   1. No Gerenciador do Servidor, clique em **Gerenciar > Adicionar Funções e Recursos**.
   2. Clique em **Instalação com base em função ou recurso** e clique em no servidor atual do AD. Siga as etapas até chegar à guia **Recursos**.
   3. Expanda **Ferramentas de Administração de Servidor Remoto > Ferramentas de Administração de Funções > Ferramentas do AD DS e AD LDS**. Em seguida, selecione **Ferramentas do AD DS**.
   4. Selecione **Reiniciar cada servidor de destino automaticamente, se necessário** e clique em **Instalar**.

### <a name="create-a-group"></a>Criar um grupo

Você pode usar grupos do AD DS para permitir acesso a um conjunto de usuários que precisam usar os mesmos recursos remotos.

1. No Gerenciador do Servidor no servidor que executa o AD DS, clique em **Ferramentas > Usuários e Computadores do Active Directory**.
2. Expanda o domínio no painel esquerdo para exibir suas subpastas.
3. Clique com o botão direito do mouse na pasta na qual você deseja criar o grupo e clique em **Novo > Grupo**.
4. Insira um nome de grupo apropriado e selecione **Global** e **Segurança**.

### <a name="create-a-user-and-add-to-a-group"></a>Criar um usuário e adicionar a um grupo
1. No Gerenciador do Servidor no servidor que executa o AD DS, clique em **Ferramentas > Usuários e Computadores do Active Directory**.
2. Expanda o domínio no painel esquerdo para exibir suas subpastas.
3. Clique com o botão direito do mouse em **Usuários** e clique em **Novo > Usuário**.
4. Insira, no mínimo, um nome e um nome de logon do usuário.
5. Insira e confirme a senha para o usuário. Configure as opções de usuário apropriadas, como **O usuário deverá alterar a senha no próximo logon**.
6. Adicione o novo usuário a um grupo:
   1. Na pasta **Usuários**, clique com o botão direito do mouse no novo usuário.
   2. Clique em **Adicionar a um grupo**.
   3. Insira o nome do grupo ao qual deseja atribuir adicionar o usuário.

## <a name="assign-users-and-groups-to-collections"></a>Atribuir usuários e grupos a coleções
Agora que você criou os usuários e grupos no Active Directory, poderá adicionar alguma granularidade em relação a quem tem acesso às coleções de Área de Trabalho Remota em sua implantação.

1. Conecte-se ao servidor que executa a função de Agente de Conexão de Área de Trabalho Remota seguindo as etapas descritas anteriormente.
2. Adicione os outros servidores de Área de Trabalho Remota ao pool do Agente de Conexão de Área de Trabalho Remota de servidores gerenciados:
   1. No Gerenciador do Servidor, clique em **Gerenciar > Adicionar Servidores**.
   2. Clique em **Localizar Agora**.
   3. Clique em cada servidor na implantação que está executando uma função de Serviços de Área de Trabalho Remota e, em seguida, clique em **OK**.
3. Edite uma coleção para atribuir acesso a usuários ou grupos específicos:
   1. No Gerenciador do Servidor, clique em **Serviços de Área de Trabalho Remota > Visão geral** e, em seguida, clique em uma coleção específica.
   2. Em **Propriedades**, clique em **Tarefas > Editar propriedades**.
   3. Clique em **Grupos de usuários**.
   4. Clique em **Adicionar** e insira o usuário ou grupo que você deseja que tenha acesso à coleção. Você também pode remover usuários e grupos desta janela selecionando o usuário ou grupo que deseja remover. Em seguida, clique em **Remover**. 
   
   >[!NOTE] 
   > A janela Grupos de usuários nunca pode estar vazia. Para restringir o escopo de usuários que têm acesso à coleção, primeiro adicione usuários ou grupos específicos antes de remover grupos mais amplos.