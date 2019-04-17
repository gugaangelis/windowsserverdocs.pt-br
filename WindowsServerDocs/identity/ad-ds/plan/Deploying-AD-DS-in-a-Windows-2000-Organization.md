---
ms.assetid: 7530cafe-98d7-46c9-95d9-e49d39caa021
title: "Implantando o AD DS em uma organização do Windows 2000"
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adds
ms.openlocfilehash: 1c46ff6aee03eee4169dbfe0cdff99febad312d2
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 12/12/2017
---
# <a name="deploying-ad-ds-in-a-windows-2000-organization"></a>Implantando o AD DS em uma organização do Windows 2000

>Aplica-se a: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Se sua organização está executando o Windows 2000 Active Directory, você pode implantar o Windows Server 2008 Active Directory serviços de domínio (AD DS) fazendo uma atualização in-loco de alguns ou todos os sistemas operacionais dos controladores de domínio para o Windows Server 2008 ou introduzindo controladores de domínio que executam o Windows Server 2008 em seu ambiente.  
  
Antes de adicionar um controlador de domínio que executam o Windows Server 2008 a um domínio do Active Directory do Windows 2000 existente, você deve executar **adprep**, uma ferramenta de linha de comando. Adprep estende o esquema do AD DS, atualiza os descritores de segurança padrão dos objetos selecionados e adiciona novos objetos de diretório conforme exigido pela alguns aplicativos. Adprep está disponível no disco de instalação do Windows Server 2008 (\sources\adprep\adprep.exe). Para obter mais informações, consulte Adprep ([https://go.microsoft.com/fwlink/?LinkId=99215](https://go.microsoft.com/fwlink/?LinkId=99215)).  
  
> [!NOTE]  
> Se você quiser realizar uma atualização in-loco de um controlador de domínio do Windows 2000 AD DS existente para o Windows Server 2008, você deve primeiro atualizar o servidor para o Windows Server 2003 e, em seguida, atualizar para o Windows Server 2008.  
  
A ilustração a seguir mostra as etapas para implantar o Windows Server 2008 AD DS em um ambiente de rede que executa o Windows 2000 Active Directory.  
  
![Implantar em uma organograma de 2000 do windows](media/Deploying-AD-DS-in-a-Windows-2000-Organization/ee51218a-a858-49d9-8b99-9986679191c1.gif)  
  
> [!NOTE]  
> Se você deseja definir o nível funcional do domínio ou floresta para o Windows Server 2008, todos os controladores de domínio em seu ambiente devem executar o sistema operacional Windows Server 2008.  
  
Consolidando domínios de recurso e a conta que são atualizados no lugar de um ambiente do Windows 2000 como parte do seu Windows Server 2008 AD DS implantação pode exigir interflorestal ou reestruturação do domínio entre florestas. Reestruturação domínios do AD DS entre florestas ajuda a reduzir a complexidade da sua organização e os custos associados administrativos. Reestruturação domínios em uma floresta do AD DS ajuda a reduzir a sobrecarga administrativa para sua organização, reduzindo o tráfego de replicação, reduzindo a quantidade de usuário e a administração de grupo que é necessária e simplificar a administração da política de grupo. Para obter mais informações, consulte o guia de migração do ADMT v 3.1 ([https://go.microsoft.com/fwlink/?LinkId=93678](https://go.microsoft.com/fwlink/?LinkId=93678)).  
  
Para obter uma lista de tarefas detalhadas que você pode usar para planejar e implantar o AD DS em uma organização que executa o Windows 2000 Active Directory, consulte [lista de verificação: Implantando o AD DS em uma organização do Windows 2000](https://technet.microsoft.com/library/cc732737.aspx).  
  


