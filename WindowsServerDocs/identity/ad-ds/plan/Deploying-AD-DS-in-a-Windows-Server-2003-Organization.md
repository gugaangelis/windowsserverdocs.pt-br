---
ms.assetid: e6b72a80-e8b7-4305-be0c-0a290f468d36
title: Implantar o AD DS em uma organização com o Windows Server 2003
author: MicrosoftGuyJFlo
ms.author: joflore
manager: mtillman
ms.date: 05/31/2017
ms.topic: article
ms.openlocfilehash: c9d634467af9f883a88a27c6b5f1cad15326fff6
ms.sourcegitcommit: dfa48f77b751dbc34409aced628eb2f17c912f08
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/07/2020
ms.locfileid: "87943205"
---
# <a name="deploying-ad-ds-in-a-windows-server-2003-organization"></a>Implantar o AD DS em uma organização com o Windows Server 2003

> Aplica-se a: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Se sua organização estiver atualmente executando o Windows Server 2003 Active Directory, você poderá implantar o Windows Server 2008 Active Directory Domain Services (AD DS) executando uma atualização in-loco de alguns ou de todos os sistemas operacionais dos controladores de domínio para o Windows Server 2008 ou introduzindo controladores de domínio que executam o Windows Server 2008 em seu ambiente.

Antes de adicionar um controlador de domínio executando o Windows Server 2008 a um domínio existente do Windows Server 2003 Active Directory, você deve executar **adprep**, uma ferramenta de linha de comando. A adprep estende o esquema de AD DS, atualiza os descritores de segurança padrão dos objetos selecionados e adiciona novos objetos de diretório conforme exigido por alguns aplicativos. A Adprep está disponível no disco de instalação do Windows Server 2008 (\sources\adprep\adprep.exe). Para obter mais informações, consulte [adprep](/previous-versions/windows/it-pro/windows-server-2012-r2-and-2012/cc731728(v=ws.11)).

A ilustração a seguir mostra as etapas para implantar o Windows Server 2008 AD DS em um ambiente de rede que está atualmente executando o Windows Server 2003 Active Directory.

![implantar em uma organização do Windows 2003](media/Deploying-AD-DS-in-a-Windows-Server-2003-Organization/900c4eee-1119-4a9a-9310-755597428b71.gif)

> [!NOTE]
> Se você quiser definir o nível funcional de domínio ou floresta para o Windows Server 2008, todos os controladores de domínio em seu ambiente devem executar o sistema operacional Windows Server 2008.

A consolidação de domínios de recursos e domínios de conta que são atualizados no lugar de um ambiente do Windows Server 2003 como parte do seu Windows Server 2008 AD DS implantação pode exigir a reestruturação de domínio entre florestas ou intraflorestal. A reestruturação de AD DS domínios entre florestas ajuda a reduzir a complexidade da representação da sua organização no AD DS e ajuda a reduzir os custos administrativos associados. A reestruturação de AD DS domínios em uma floresta ajuda a diminuir a sobrecarga administrativa para sua organização, reduzindo o tráfego de replicação, reduzindo a quantidade de administração de usuários e grupos necessária e simplificando a administração do Política de Grupo. Para obter mais informações, consulte [Guia de ADMT: migrando e Reestruturando Active Directory domínios](/previous-versions/windows/it-pro/windows-server-2008-r2-and-2008/cc974332(v=ws.10)).

Para obter uma lista de tarefas detalhadas que você pode usar para planejar e implantar AD DS em uma organização que esteja executando o Windows Server 2003 Active Directory, consulte [lista de verificação: Implantando AD DS em uma organização do Windows server 2003](/previous-versions/windows/it-pro/windows-server-2008-r2-and-2008/cc771407(v=ws.10)).
