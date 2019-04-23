---
title: Gerenciar usuários na coleção de RDS
description: Saiba como gerenciar usuários nos serviços de área de trabalho remota.
ms.custom: na
ms.prod: windows-server-threshold
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
ms.openlocfilehash: 4b45061697926a3003712a88610cb17ef3c00c45
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59859637"
---
# <a name="manage-users-in-your-rds-collection"></a>Gerenciar usuários na coleção de RDS

>Aplica-se a: Windows Server (canal semestral), Windows Server 2016

Como administrador, você pode gerenciar diretamente quais usuários têm acesso a coleções específicas. Dessa forma, você pode criar uma coleção de aplicativos padrão para os operadores de informações, mas, em seguida, crie uma coleção separada com aplicativos de modelagem intensivo de gráficos para engenheiros. Há duas etapas principais para gerenciar o acesso de usuário em uma implantação de serviços de área de trabalho remota (RDS):

1.  [Criar usuários e grupos no Active Directory](#create-your-users-and-groups-in-active-directory)
2.  [Atribuir usuários e grupos para coleções](#assign-users-and-groups-to-collections)


## <a name="create-your-users-and-groups-in-active-directory"></a>Criar os usuários e grupos no Active Directory

Em uma implantação do RDS, os serviços de domínio do Active Directory (AD DS) é a origem de todos os usuários, grupos e outros objetos no domínio. Você pode gerenciar o Active Directory diretamente com o PowerShell, ou você pode usar ferramentas de interface do usuário que adicionar flexibilidade e facilidade internas. As etapas a seguir irá guiá-lo para instalar essas ferramentas — se você não tiver já instalado — e, em seguida, usar essas ferramentas para gerenciar usuários e grupos.

### <a name="install-ad-ds-tools"></a>Instalar ferramentas do AD DS

As etapas a seguir detalham como instalar as ferramentas de AD DS em um servidor já em execução do AD DS. Uma vez instalado, você pode criar usuários ou criar grupos.

1. Conecte-se ao servidor executando o Active Directory Domain Services. Para implantações do Azure:
   1. No portal do Azure, clique em **procurar > grupos de recursos**e, em seguida, clique no grupo de recursos para a implantação
   2. Selecione a máquina virtual AD.
   3. Clique em **Connect > Abrir** para abrir o cliente de área de trabalho remota. Se **Connect** estiver acinzentada, a máquina virtual não pode ter um endereço IP público. Para dar a ele um executar as etapas a seguir, em seguida, tente novamente.
      1. Clique em **Configurações > interfaces de rede**e, em seguida, clique em interface de rede correspondente.
      2. Clique em **Configurações > endereço IP**.
      3. Para **endereço IP público**, selecione **Enabled**e, em seguida, clique em **endereço IP**.
      4. Se você tiver um endereço IP público existente que você deseja usar, selecione-o na lista. Caso contrário, clique em **criar novo**, insira um nome e, em seguida, clique em **Okey** e **salvar**.
   4. No cliente, clique em **Connect**e, em seguida, clique em **usar outra conta**. Insira o nome de usuário e senha para uma conta de administrador de domínio.
   5. Clique em **Sim** quando for perguntado sobre o certificado.
2. Instale as ferramentas do AD DS:
   1. No Gerenciador do servidor, clique em **Gerenciar > Adicionar funções e recursos**.
   2. Clique em **instalação baseada em função ou recurso**e, em seguida, clique em servidor do AD atual. Siga as etapas até chegar à **recursos** guia.
   3. Expandir **ferramentas de administração de servidor remoto > Ferramentas de administração de funções > AD DS e AD LDS**e, em seguida, selecione **ferramentas do AD DS**.
   4. Selecione **reiniciar o servidor de destino automaticamente se necessário**e, em seguida, clique em **instalar**.

### <a name="create-a-group"></a>Criar um grupo

Você pode usar grupos do AD DS para conceder acesso a um conjunto de usuários que precisam usar os mesmos recursos remotos.

1. No Gerenciador do servidor no servidor que executa o AD DS, clique em **Ferramentas > Active Directory Users and Computers**.
2. Expanda o domínio no painel esquerdo para exibir suas subpastas.
3. Clique na pasta onde você deseja criar o grupo e, em seguida, clique em **Novo > grupo**.
4. Insira um nome de grupo apropriado e selecione **Global** e **segurança**.

### <a name="create-a-user-and-add-to-a-group"></a>Criar um usuário e adicionar a um grupo
1. No Gerenciador do servidor no servidor que executa o AD DS, clique em **Ferramentas > Active Directory Users and Computers**.
2. Expanda o domínio no painel esquerdo para exibir suas subpastas.
3. Clique com botão direito **os usuários**e, em seguida, clique em **New > usuário**.
4. Insira, no mínimo, um nome e um nome de logon do usuário.
5. Insira e confirme uma senha para o usuário. Configurar opções de usuário apropriado, como **usuário deve alterar a senha no próximo logon**.
6. Adicione o novo usuário a um grupo:
   1. No **usuários** pasta com o botão direito do novo usuário.
   2. Clique em **adicionar a um grupo**.
   3. Insira o nome do grupo ao qual você deseja adicionar o usuário.

## <a name="assign-users-and-groups-to-collections"></a>Atribuir usuários e grupos para coleções
Agora que você criou os usuários e grupos no Active Directory, você pode adicionar alguns granularidade sobre quem tem acesso às coleções de área de trabalho remota em sua implantação.

1. Conecte-se ao servidor que executa a função de agente de Conexão de área de trabalho remota (agente de Conexão de área de trabalho remota), seguindo as etapas descritas anteriormente.
2. Adicione os outros servidores de área de trabalho remota para o pool do agente de Conexão de área de trabalho remota de servidores gerenciados:
   1. No Gerenciador do servidor, clique em **Gerenciar > Adicionar servidores**.
   2. Clique em **Localizar Agora**.
   3. Clique em cada servidor na implantação que está executando uma função de serviços de área de trabalho remota e, em seguida, clique em **Okey**.
3. Edite uma coleção para atribuir acesso a usuários ou grupos específicos:
   1. No Gerenciador do servidor, clique em **Remote Desktop Services > Visão geral**e, em seguida, clique em uma coleção específica.
   2. Sob **propriedades**, clique em **tarefas > Editar propriedades**.
   3. Clique em **grupos de usuários**.
   4. Clique em **adicionar** e insira o usuário ou grupo que você deseja ter acesso à coleção. Você também pode remover usuários e grupos nesta janela selecionando o usuário ou grupo que deseja remover e, em seguida, clicando em **remover**. 
   
   >[!NOTE] 
   > A janela de grupos do usuário nunca pode ser vazia. Para restringir o escopo de usuários que têm acesso à coleção, você deve primeiro adicionar usuários ou grupos específicos antes de remover grupos mais amplos.