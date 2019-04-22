---
ms.assetid: e6b72a80-e8b7-4305-be0c-0a290f468d36
title: Implantar o AD DS em uma organização com o Windows Server 2003
description: ''
author: MicrosoftGuyJFlo
ms.author: joflore
manager: mtillman
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adds
ms.openlocfilehash: 033973ad7a726054f6c47c7154fa54a3767beab4
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59816357"
---
# <a name="deploying-ad-ds-in-a-windows-server-2003-organization"></a>Implantar o AD DS em uma organização com o Windows Server 2003

>Aplica-se a: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Se sua organização estiver executando o Windows Server 2003 Active Directory, você pode implantar o Windows Server 2008 Active Directory serviços de domínio (AD DS) executando uma atualização in-loco de alguns ou todos os sistemas operacionais de seus controladores de domínio  Windows Server 2008 ou com a introdução de controladores de domínio executando o Windows Server 2008 em seu ambiente.  
  
Antes de adicionar um controlador de domínio executando o Windows Server 2008 para um domínio do Active Directory do Windows Server 2003 existente, você deve executar **adprep**, uma ferramenta de linha de comando. Adprep estende o esquema do AD DS, atualiza os descritores de segurança padrão dos objetos selecionados e adiciona novos objetos de diretório necessários para alguns aplicativos. Adprep está disponível no disco de instalação do Windows Server 2008 (\sources\adprep\adprep.exe). Para obter mais informações, consulte o Adprep ([https://go.microsoft.com/fwlink/?LinkId=99215](https://go.microsoft.com/fwlink/?LinkId=99215)).  
  
A ilustração a seguir mostra as etapas para implantar o AD DS do Windows Server 2008 em um ambiente de rede que está executando o Windows Server 2003 Active Directory.  
  
![implantar em uma organização do windows 2003](media/Deploying-AD-DS-in-a-Windows-Server-2003-Organization/900c4eee-1119-4a9a-9310-755597428b71.gif)  
  
> [!NOTE]  
> Se você deseja definir o nível funcional do domínio ou floresta para o Windows Server 2008, todos os controladores de domínio em seu ambiente devem executar o sistema operacional Windows Server 2008.  
  
Consolidação de domínios de conta que serão atualizados no local de um ambiente do Windows Server 2003 como parte da implantação do AD DS do Windows Server 2008 pode exigir reestruturações ou reestruturação de domínio entre florestas e domínios de recursos. Reestruturando domínios entre florestas do AD DS ajuda a reduzir a complexidade da representação da sua organização no AD DS, e ele ajuda a reduzir os custos administrativos associados. Reestruturando domínios dentro de uma floresta do AD DS ajuda a diminuir a sobrecarga administrativa para a sua organização, reduzindo o tráfego de replicação, reduzindo a quantidade de usuário e a administração de grupo que é necessária e simplificando a administração de grupo Política. Para obter mais informações, consulte o guia de migração do ADMT v3.1 ([https://go.microsoft.com/fwlink/?LinkId=93678](https://go.microsoft.com/fwlink/?LinkId=93678)).  
  
Para obter uma lista de tarefas detalhadas que você pode usar para planejar e implantar o AD DS em uma organização que está executando o Windows Server 2003 Active Directory, consulte [lista de verificação: Implantando o AD DS em uma organização do Windows Server 2003](https://technet.microsoft.com/library/cc771407.aspx).  
  


