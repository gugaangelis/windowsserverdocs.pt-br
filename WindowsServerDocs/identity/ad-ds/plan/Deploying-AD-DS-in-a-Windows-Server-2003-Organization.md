---
ms.assetid: e6b72a80-e8b7-4305-be0c-0a290f468d36
title: "Implantando o AD DS em uma organização do Windows Server 2003"
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adds
ms.openlocfilehash: df1267ded5ece95dd5a3ab17e4ec6ad5d87a2f12
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 12/12/2017
---
# <a name="deploying-ad-ds-in-a-windows-server-2003-organization"></a>Implantando o AD DS em uma organização do Windows Server 2003

>Aplica-se a: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Se sua organização está executando o Windows Server 2003 Active Directory, você pode implantar o Windows Server 2008 Active Directory serviços de domínio (AD DS) fazendo uma atualização in-loco de alguns ou todos os sistemas operacionais dos controladores de domínio para o Windows Server 2008 ou introduzindo controladores de domínio que executam o Windows Server 2008 em seu ambiente.  
  
Antes de adicionar um controlador de domínio que executam o Windows Server 2008 a um domínio do Active Directory do Windows Server 2003 existente, você deve executar **adprep**, uma ferramenta de linha de comando. Adprep estende o esquema do AD DS, atualiza os descritores de segurança padrão dos objetos selecionados e adiciona novos objetos de diretório conforme exigido pela alguns aplicativos. Adprep está disponível no disco de instalação do Windows Server 2008 (\sources\adprep\adprep.exe). Para obter mais informações, consulte Adprep ([https://go.microsoft.com/fwlink/?LinkId=99215](https://go.microsoft.com/fwlink/?LinkId=99215)).  
  
A ilustração a seguir mostra as etapas para implantar o Windows Server 2008 AD DS em um ambiente de rede que executa o Active Directory do Windows Server 2003.  
  
![Implantar em uma organograma de 2003 do windows](media/Deploying-AD-DS-in-a-Windows-Server-2003-Organization/900c4eee-1119-4a9a-9310-755597428b71.gif)  
  
> [!NOTE]  
> Se você deseja definir o nível funcional do domínio ou floresta para o Windows Server 2008, todos os controladores de domínio em seu ambiente devem executar o sistema operacional Windows Server 2008.  
  
Consolidando domínios do recurso e conta que são atualizados no lugar de um ambiente do Windows Server 2003, como parte da implantação do Windows Server 2008 AD DS pode exigir interflorestal ou reestruturação do domínio entre florestas. Reestruturação domínios do AD DS entre florestas ajuda a reduzir a complexidade da representação da sua organização no AD DS e ajuda a reduzir os custos associados administrativos. Reestruturação domínios em uma floresta do AD DS ajuda a reduzir a sobrecarga administrativa para sua organização, reduzindo o tráfego de replicação, reduzindo a quantidade de usuário e a administração de grupo que é necessária e simplificar a administração da política de grupo. Para obter mais informações, consulte o guia de migração do ADMT v 3.1 ([https://go.microsoft.com/fwlink/?LinkId=93678](https://go.microsoft.com/fwlink/?LinkId=93678)).  
  
Para obter uma lista de tarefas detalhadas que você pode usar para planejar e implantar o AD DS em uma organização que está executando o Windows Server 2003 Active Directory, consulte [lista de verificação: Implantando o AD DS em uma organização do Windows Server 2003](https://technet.microsoft.com/library/cc771407.aspx).  
  


