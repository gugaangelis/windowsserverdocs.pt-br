---
title: Criar uma coleção de serviços de área de trabalho remota
description: Saiba como adicionar e programas de RDSH e RemoteApp para sua implantação do RDS.
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: remote-desktop-services
ms.author: elizapo
ms.date: 11/08/2016
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: ae9767e3-864a-4eb2-96c0-626759ce6d60
author: lizap
manager: dongill
ms.openlocfilehash: 52806e28c4ef87453995728623efe2954a76dfd9
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59839237"
---
# <a name="create-a-remote-desktop-services-collection-for-desktops-and-apps-to-run"></a>Criar uma coleção de serviços de área de trabalho remota para áreas de trabalho e aplicativos para serem executados

>Aplica-se a: Windows Server (canal semestral), Windows Server 2016

Use as etapas a seguir para criar uma coleção de sessão dos serviços de área de trabalho remota. Uma coleção de sessão mantém os aplicativos e áreas de trabalho que você deseja disponibilizar para os usuários. Depois de criar a coleção, publicá-lo para que os usuários podem acessá-lo.

Antes de criar uma coleção, você precisa decidir que tipo de coleção que você precisa: agrupada sessões da área de trabalho ou pessoal da área de trabalho. 

- **Usar sessões da área de trabalho em pool para virtualização baseada em sessão**: Aproveite o poder de computação do Windows Server para fornecer um ambiente de várias sessões e econômico para cargas de trabalho cotidiano dos usuários da unidade
- **Usar sessões da área de trabalho pessoas para criar uma virtual desktop infrastructure (VDI)**: Aproveite o cliente do Windows para fornecer o alto desempenho, a compatibilidade de aplicativos e a familiaridade que seus usuários já conhecem de sua experiência de área de trabalho do Windows.
 
Com uma sessão em pool, vários usuários acessar um pool compartilhado de recursos, enquanto com uma sessão de área de trabalho pessoal, os usuários recebem seu próprios da área de trabalho de dentro do pool. A sessão em pool fornece o menor custo geral, enquanto as sessões pessoais permitem aos usuários personalizar sua experiência de área de trabalho.

Se você precisa para aplicativos de compartilhamento hospedado que fazem uso intensivo de gráficos, você pode combinar a áreas de trabalho de sessão pessoal com vGPU do RemoteFX configurado para acelerações de gráficos. Como alternativa, você pode combinar as áreas de trabalho de sessão pessoal com o novo recurso de atribuição de dispositivo discretos (DDA) também dar suporte aos aplicativos hospedados que exigem acelerada de elementos gráficos. Fazer check-out [qual tecnologia de virtualização de gráficos é ideal para você](rds-graphics-virtualization.md) para obter mais informações.


Independentemente do tipo de coleção que você escolher, você vai preencher essas coleções com RemoteApps - programas e recursos que os usuários podem acessar de qualquer dispositivo com suporte e trabalhar com o mesmo que o programa estava em execução localmente.

## <a name="create-a-pooled-desktop-session-collection"></a>Criar uma coleção de sessão de área de trabalho em pool

1.  No Gerenciador do servidor, clique em **Remote Desktop Services > coleções > tarefas > criar coleções de sessão**.  
2.  Insira um nome para a coleção, por exemplo **ContosoAps**.  
3.  Selecione o servidor de Host de sessão de área de trabalho remota que você criou (por exemplo, Contoso-Shr1).  
4.  Aceite o padrão **grupos de usuários**.  
5.  Insira o local do compartilhamento de arquivo que você criou para os discos de perfil do usuário para essa coleção (por exemplo, **\Contoso-Cb1\UserDisksr**).   
6.  Clique em **Criar**. Quando a coleção é criada, clique em **fechar**.  


## <a name="create-a-personal-desktop-session-collection"></a>Criar uma coleção de sessão de área de trabalho pessoal

Use o cmdlet New-RDSessionCollection para criar uma coleção de sessão pessoal da área de trabalho. Os três parâmetros a seguir fornecem as informações de configuração necessárias para áreas de trabalho de sessão pessoal:

- **-PersonalUnmanaged** -Especifica o tipo de coleção de sessão que permite que você atribuir usuários a um servidor de host de sessão pessoal. Se você não especificar esse parâmetro, a coleção é criada como uma coleção de Host de sessão de área de trabalho remota tradicional, onde os usuários são atribuídos para o próximo host de sessão disponíveis quando eles entrarem.
- **-GrantAdministrativePrivilege** – se você usar **- PersonalUnmanaged**, especifica que o usuário atribuído ao host de sessão receber privilégios administrativos. Se você não usar esse parâmetro, os usuários recebem apenas os privilégios de usuário padrão.
- **-AutoAssignUser** – se você usar **- PersonalUnmanaged**, especifica que os novos usuários se conectam por meio do agente de Conexão de área de trabalho são automaticamente atribuídos a um host de sessão não atribuídos. Se não houver nenhum host de sessão não atribuídos na coleção, o usuário verá uma mensagem de erro. Se você não usar esse parâmetro, você precisará [atribuir manualmente os usuários a um host de sessão](rds-manage-personal-collection.md#manually-assign-a-user-to-a-personal-session-host) antes que ele entrar.

Você pode usar os cmdlets do PowerShell para gerenciar suas coleções de sessão de área de trabalho pessoal. Ver [gerenciar as coleções de sessão de área de trabalho pessoal](rds-manage-personal-collection.md) para obter mais informações.

## <a name="publish-remoteapp-programs"></a>Publicar programas do RemoteApp
Use as etapas a seguir para publicar os aplicativos e recursos em sua coleção:

1.  No Gerenciador do servidor, selecione a nova coleção (**ContosoApps**).  
2.  Em programas RemoteApp, clique em **programas RemoteApp publicar**.  
3. Selecione os programas que você deseja publicar e, em seguida, clique em **publicar**.  
