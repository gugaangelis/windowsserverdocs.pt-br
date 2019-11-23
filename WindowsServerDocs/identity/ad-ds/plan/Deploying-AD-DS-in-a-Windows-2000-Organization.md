---
ms.assetid: 7530cafe-98d7-46c9-95d9-e49d39caa021
title: Implantar o AD DS em uma organização com o Windows 2000
description: ''
author: MicrosoftGuyJFlo
ms.author: joflore
manager: mtillman
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adds
ms.openlocfilehash: cad5deb32a31f15277c3e0e985d5b7d9b07856aa
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71408917"
---
# <a name="deploying-ad-ds-in-a-windows-2000-organization"></a>Implantar o AD DS em uma organização com o Windows 2000

>Aplica-se a: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Se sua organização estiver atualmente executando o Windows 2000 Active Directory, você poderá implantar o Windows Server 2008 Active Directory Domain Services (AD DS) executando uma atualização in-loco de alguns ou de todos os sistemas operacionais dos controladores de domínio para o Windows Server 2008 ou introduzindo controladores de domínio que executam o Windows Server 2008 em seu ambiente.  
  
Antes de adicionar um controlador de domínio executando o Windows Server 2008 a um domínio existente do Windows 2000 Active Directory, você deve executar **adprep**, uma ferramenta de linha de comando. A adprep estende o esquema de AD DS, atualiza os descritores de segurança padrão dos objetos selecionados e adiciona novos objetos de diretório conforme exigido por alguns aplicativos. A Adprep está disponível no disco de instalação do Windows Server 2008 (\sources\adprep\adprep.exe). Para obter mais informações, consulte adprep ([https://go.microsoft.com/fwlink/?LinkId=99215](https://go.microsoft.com/fwlink/?LinkId=99215)).  
  
> [!NOTE]  
> Se você quiser executar uma atualização in-loco de um controlador de domínio existente do Windows 2000 AD DS para o Windows Server 2008, você deve primeiro atualizar o servidor para o Windows Server 2003 e, em seguida, atualizá-lo para o Windows Server 2008.  
  
A ilustração a seguir mostra as etapas para implantar o Windows Server 2008 AD DS em um ambiente de rede que está atualmente executando o Windows 2000 Active Directory.  
  
![Implantando em uma organização do Windows 2000](media/Deploying-AD-DS-in-a-Windows-2000-Organization/ee51218a-a858-49d9-8b99-9986679191c1.gif)  
  
> [!NOTE]  
> Se você quiser definir o nível funcional de domínio ou floresta para o Windows Server 2008, todos os controladores de domínio em seu ambiente devem executar o sistema operacional Windows Server 2008.  
  
Consolidar domínios de recursos e contas que são atualizados no lugar de um ambiente do Windows 2000 como parte do seu Windows Server 2008 AD DS implantação pode exigir a reestruturação de domínio entre florestas ou intraflorestal. A reestruturação de AD DS domínios entre florestas ajuda a reduzir a complexidade de sua organização e os custos administrativos associados. A reestruturação de AD DS domínios em uma floresta ajuda a diminuir a sobrecarga administrativa para sua organização, reduzindo o tráfego de replicação, reduzindo a quantidade de administração de usuários e grupos necessária e simplificando a administração do Política de Grupo. Para obter mais informações, consulte Guia de migração do ADMT v 3.1 ([https://go.microsoft.com/fwlink/?LinkId=93678](https://go.microsoft.com/fwlink/?LinkId=93678)).  
  
Para obter uma lista de tarefas detalhadas que você pode usar para planejar e implantar AD DS em uma organização que está executando o Windows 2000 Active Directory, consulte [lista de verificação: Implantando AD DS em uma organização do windows 2000](https://technet.microsoft.com/library/cc732737.aspx).  
  


