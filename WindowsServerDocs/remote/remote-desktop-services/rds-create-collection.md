---
title: Criar e implantar uma coleção de Serviços de Área de Trabalho Remota
description: Saiba como adicionar programas de RDSH e RemoteApp em sua implantação do RDS.
ms.prod: windows-server
ms.technology: remote-desktop-services
ms.author: elizapo
ms.date: 10/22/2019
ms.topic: article
ms.assetid: ae9767e3-864a-4eb2-96c0-626759ce6d60
author: lizap
manager: dongill
ms.openlocfilehash: 6a842c7984dc63fe40c05300f6cfbb6718846525
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80852949"
---
# <a name="create-a-remote-desktop-services-collection-for-desktops-and-apps-to-run"></a>Criar uma coleção de serviços de área de trabalho remota para aplicativos e áreas de trabalho a executar

>Aplica-se a: Windows Server (Canal Semestral), Windows Server 2019, Windows Server 2016

Use as etapas a seguir para criar uma coleção de sessão dos Serviços de Área de Trabalho Remota. Uma coleção de sessão reúne os aplicativos e áreas de trabalho que você deseja disponibilizar para os usuários. Crie e depois publique a coleção para que os usuários possam acessá-la.

Antes de criar uma coleção, decida qual tipo de coleção é necessária: sessões de área de trabalho pessoal ou em pool. 

- **Use sessões da área de trabalho em pool para a virtualização baseada em sessão**: aproveite o poder de computação do Windows Server para fornecer um ambiente de várias sessões econômico para conduzir as cargas de trabalho cotidianas dos usuários
- **Use sessões da área de trabalho pessoal para criar uma VDI (Virtual Desktop Infrastructure)** : aproveite o cliente do Windows para fornecer o alto desempenho, a compatibilidade de aplicativos e a familiaridade que seus usuários já passaram a esperar de sua experiência de área de trabalho do Windows.
 
Com uma sessão em pool, vários usuários acessam um pool compartilhado de recursos, enquanto na sessão de área de trabalho pessoal, os usuários recebem sua própria área de trabalho dentro do pool. A sessão em pool fornece o menor custo geral, enquanto as sessões pessoais permitem que os usuários personalizem sua experiência de área de trabalho.

Se for preciso compartilhar aplicativos hospedados com uso intensivo de gráficos, combine as áreas de trabalho de sessão pessoal com a nova funcionalidade DDA (Atribuição de Dispositivo Discreto) para também oferecer suporte aos aplicativos hospedados que exijam a aceleração de gráficos. Confira [Qual tecnologia de virtualização de gráficos é ideal para você](rds-graphics-virtualization.md) para acessar mais informações.


Independentemente do tipo de coleção que você escolher, preencha essas coleções com RemoteApps, os programas e recursos que os usuários podem acessar em qualquer dispositivo com suporte e que trabalham com o mesmo que o programa estava em execução no local.

## <a name="create-a-pooled-desktop-session-collection"></a>Criar uma coleção de sessão de área de trabalho em pool

1.  No Gerenciador do Servidor, clique em **Serviços de Área de Trabalho Remota > Coleções > Tarefas > Criar Coleções de Sessões**.  
2.  Digite um nome para a coleção, por exemplo, **ContosoAps**.  
3.  Selecione o servidor de host para a sessão de área de trabalho remota que você criou (por exemplo, Contoso-Shr1).  
4.  Aceite os **Grupos de usuários** padrão.  
5.  Insira o local do compartilhamento de arquivo que você criou para os discos de perfil do usuário dessa coleção (por exemplo, **\Contoso-Cb1\UserDisksr**).   
6.  Clique em **Criar**. Quando a coleção for criada, clique em **Fechar**.  


## <a name="create-a-personal-desktop-session-collection"></a>Criar uma coleção de sessão de área de trabalho pessoal

Use o cmdlet New-RDSessionCollection para criar uma coleção de sessão da área de trabalho pessoal. Os três parâmetros a seguir fornecem as informações de configuração necessárias para áreas de trabalho com sessão pessoal:

- **-PersonalUnmanaged** – especifica o tipo de coleção de sessão que permite atribuir usuários a um servidor de host de sessão pessoal. Se esse parâmetro não for especificado, a coleção é criada como uma coleção do host de sessão de área de trabalho remota tradicional, na qual os usuários são atribuídos para o próximo host de sessão disponível ao entrarem.
- **-GrantAdministrativePrivilege** – se você usar **-PersonalUnmanaged**, especifica que o usuário atribuído ao host de sessão receberá privilégios administrativos. Se você não usar esse parâmetro, os usuários recebem apenas os privilégios de usuário padrão.
- **-AutoAssignUser** – se você usar **-PersonalUnmanaged**, especifica que os novos usuários que se conectam por meio do agente de conexão de área de trabalho remota serão automaticamente atribuídos a um host de sessão não atribuído. Se não houver nenhum host de sessão não atribuído na coleção, o usuário verá uma mensagem de erro. Se você não usar esse parâmetro, precisará [atribuir manualmente os usuários a um host de sessão](rds-manage-personal-collection.md#manually-assign-a-user-to-a-personal-session-host) antes que ele entre.

Você pode usar os cmdlets do PowerShell para gerenciar suas coleções de sessões de área de trabalho pessoal. Veja [Gerenciar coleções de sessão de área de trabalho pessoal](rds-manage-personal-collection.md) para obter mais informações.

## <a name="publish-remoteapp-programs"></a>Publicar programas RemoteApp
Use as etapas a seguir para publicar os aplicativos e recursos em sua coleção:

1.  No Gerenciador do Servidor, selecione a nova coleção (**ContosoApps**).  
2.  Em Programas RemoteApp, clique em **Publicar Programas RemoteApp**.  
3. Selecione os programas a publicar e clique em **Publicar**.  
