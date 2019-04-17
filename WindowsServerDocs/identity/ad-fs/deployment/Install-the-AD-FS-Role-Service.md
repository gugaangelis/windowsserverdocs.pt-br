---
ms.assetid: c28a1b8b-5bec-4eed-8c95-a1a29cfc957c
title: "Instalar o serviço de função FS AD"
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: 9851134d1ad73092ee44c34c99bc2d873d20ca07
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 12/12/2017
---
# <a name="install-the-ad-fs-role-service"></a>Instalar o serviço de função FS AD

>Aplica-se a: Windows Server 2016, Windows Server 2012 R2

Você pode usar o procedimento a seguir para instalar o serviço de função do AD FS em um computador que executa o Windows Server 2012 R2 se torne o primeiro servidor de Federação em um farm de servidores de Federação ou um servidor de Federação em um farm de servidores de Federação existente.  
  
A associação ao grupo **administradores**, ou equivalente, no computador local é o requisito mínimo para concluir este procedimento.  Examinar detalhes sobre como usar as contas apropriadas e agrupar associações em [Local e os grupos de domínio padrão ](https://go.microsoft.com/fwlink/?LinkId=83477).   
  
### <a name="to-install-the-ad-fs-server-role-via-the-add-roles-and-features-wizard"></a>Para instalar a função de servidor AD FS através do assistente Adicionar funções e recursos  
  
1.  Abra o Gerenciador do servidor. Para abrir o Gerenciador do servidor, clique em **Gerenciador do servidor** sobre o **iniciar** tela, ou **Gerenciador do servidor** na barra de tarefas da área de trabalho. No **início rápido** guia do **boas-vindas** bloco no **painel** página, clique em **adicionar funções e recursos**. Como alternativa, você pode clicar em **adicionar funções e recursos** sobre o **gerenciar** menu.  
  
2.  Sobre o **antes de começar** página, clique em **próxima**.  
  
3.  No **selecionar o tipo de instalação** página, clique em **instalação baseado em Feature\ ou Role\**e clique em **próxima**.  
  
4.  No **servidor de destino Select** página, clique em **selecionar um servidor do pool servidor**, verifique se o computador de destino e clique em **próxima**.  
  
5.  Sobre o **selecionar funções de servidor** página, clique em **serviços de Federação do Active Directory**e, em seguida, clique em **próxima**.  
  
6.  Sobre o **Selecione recursos** página, clique em **próxima**. Os pré-requisitos necessários são pré-selecionado para você. Você não precisa selecionar quaisquer outros recursos.  
  
7.  Sobre o **\(AD FS\) serviço de Federação do Active Directory** página, clique em **próxima**.  
  
8.  Depois de verificar as informações sobre o **confirmar seleções de instalação** página, clique em **instalar**.  
  
9. Sobre o **progresso da instalação** página, verificar se tudo está instalado corretamente e, em seguida, clique em **fechar**.  
  
### <a name="to-install-the-ad-fs-server-role-via-windows-powershell"></a>Para instalar a função de servidor AD FS por meio do Windows PowerShell  
  
1.  No computador em que você deseja configurar como um servidor de federação, abra a janela de comando do Windows PowerShell e, em seguida, execute o seguinte comando: `Install-windowsfeature adfs-federation –IncludeManagementTools`.  
  
## <a name="see-also"></a>Consulte também 

[AD FS implantação](../../ad-fs/AD-FS-Deployment.md)  

[Guia de implantação do Windows Server 2012 R2 AD FS](../../ad-fs/deployment/Windows-Server-2012-R2-AD-FS-Deployment-Guide.md)  
 
[Implantando um Farm de servidores de Federação](../../ad-fs/deployment/Deploying-a-Federation-Server-Farm.md)  
  

