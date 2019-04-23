---
ms.assetid: 7530cafe-98d7-46c9-95d9-e49d39caa021
title: Implantar o AD DS em uma organização com o Windows 2000
description: ''
author: MicrosoftGuyJFlo
ms.author: joflore
manager: mtillman
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adds
ms.openlocfilehash: 5015c0ca2c01fca966927af4207a7e3501d9cd39
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59854277"
---
# <a name="deploying-ad-ds-in-a-windows-2000-organization"></a>Implantar o AD DS em uma organização com o Windows 2000

>Aplica-se a: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Se sua organização estiver executando o Active Directory do Windows 2000, você pode implantar o Windows Server 2008 Active Directory serviços de domínio (AD DS) executando uma atualização in-loco de alguns ou todos os sistemas operacionais de seus controladores de domínio Windows Server 2008 ou com a introdução de controladores de domínio executando o Windows Server 2008 em seu ambiente.  
  
Antes de adicionar um controlador de domínio executando o Windows Server 2008 em um domínio existente do Active Directory do Windows 2000, você deve executar **adprep**, uma ferramenta de linha de comando. Adprep estende o esquema do AD DS, atualiza os descritores de segurança padrão dos objetos selecionados e adiciona novos objetos de diretório necessários para alguns aplicativos. Adprep está disponível no disco de instalação do Windows Server 2008 (\sources\adprep\adprep.exe). Para obter mais informações, consulte o Adprep ([https://go.microsoft.com/fwlink/?LinkId=99215](https://go.microsoft.com/fwlink/?LinkId=99215)).  
  
> [!NOTE]  
> Se você quiser executar uma atualização in-loco de um controlador de domínio do AD DS do Windows 2000 existente para o Windows Server 2008, você deve atualizar primeiro o servidor para o Windows Server 2003 e, em seguida, atualizá-lo para o Windows Server 2008.  
  
A ilustração a seguir mostra as etapas para implantar o AD DS do Windows Server 2008 em um ambiente de rede que está executando o Windows 2000 Active Directory.  
  
![Implantando em uma organização do windows 2000](media/Deploying-AD-DS-in-a-Windows-2000-Organization/ee51218a-a858-49d9-8b99-9986679191c1.gif)  
  
> [!NOTE]  
> Se você deseja definir o nível funcional do domínio ou floresta para o Windows Server 2008, todos os controladores de domínio em seu ambiente devem executar o sistema operacional Windows Server 2008.  
  
A consolidação de domínios de recursos e contas que serão atualizados no local de um ambiente do Windows 2000 como parte do seu Windows Server 2008 AD DS implantação exigir reestruturações ou reestruturação de domínio entre florestas. Reestruturando domínios entre florestas do AD DS ajuda a reduzir a complexidade da sua organização e os custos administrativos associados. Reestruturando domínios dentro de uma floresta do AD DS ajuda a diminuir a sobrecarga administrativa para a sua organização, reduzindo o tráfego de replicação, reduzindo a quantidade de usuário e a administração de grupo que é necessária e simplificando a administração do Diretiva de grupo. Para obter mais informações, consulte o guia de migração do ADMT v3.1 ([https://go.microsoft.com/fwlink/?LinkId=93678](https://go.microsoft.com/fwlink/?LinkId=93678)).  
  
Para obter uma lista de tarefas detalhadas que você pode usar para planejar e implantar o AD DS em uma organização que está executando o Windows 2000 Active Directory, consulte [lista de verificação: Implantando o AD DS em uma organização do Windows 2000](https://technet.microsoft.com/library/cc732737.aspx).  
  


