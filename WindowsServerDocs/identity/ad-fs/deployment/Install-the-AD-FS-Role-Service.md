---
ms.assetid: c28a1b8b-5bec-4eed-8c95-a1a29cfc957c
title: Instalar o serviço de função do AD FS
description: ''
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adfs
ms.openlocfilehash: 46231015ffb91f7c6a1f15615e0eede3fc948be2
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71408354"
---
# <a name="install-the-ad-fs-role-service"></a>Instalar o serviço de função do AD FS

Você pode usar o procedimento a seguir para instalar o serviço de função AD FS em um computador que esteja executando o Windows Server 2012 R2 para se tornar o primeiro servidor de Federação em um farm de servidores de Federação ou um servidor de Federação em um farm de servidores de Federação existente.  
  
A associação em **Administradores**, ou equivalente, no computador local é o requisito mínimo para concluir este procedimento.  Examine os detalhes sobre como usar as contas apropriadas e as associações de grupo em [grupos padrão e de domínio](https://go.microsoft.com/fwlink/?LinkId=83477).   
  
### <a name="to-install-the-ad-fs-server-role-via-the-add-roles-and-features-wizard"></a>Para instalar a função de servidor AD FS por meio do assistente para adicionar funções e recursos  
  
1.  Abra o Gerenciador do Servidor. Para abrir Gerenciador do Servidor, clique em **Gerenciador do servidor** na tela **Iniciar** ou **Gerenciador do servidor** na barra de tarefas na área de trabalho. Na guia **Início Rápido** do bloco de **Boas-vindas** da página **Painel**, clique em **Adicionar funções e recursos**. Como alternativa, é possível clicar em **Adicionar Funções e Recursos** no menu **Gerenciar**.  
  
2.  Na página **Before you begin**, clique em **Next**.  
  
3.  Na página **Selecionar tipo de instalação** , clique em **função\-baseada em\-instalação baseada em recursos**e clique em **Avançar**.  
  
4.  Na página **Selecionar servidor de destino**, clique em **Selecionar um servidor no pool de servidor**, verifique se o computador de destino está selecionado e clique em **Avançar**.  
  
5.  Na página **Selecionar funções de servidor** , clique em **Serviços de Federação do Active Directory**e, depois, em **Avançar**.  
  
6.  Na página **Selecionar recursos**, clique em **Avançar**. Os pré-requisitos necessários são preselecionados para você. Você não precisa selecionar nenhum outro recurso.  
  
7.  Na página **Active Directory Serviço de Federação \(AD FS\)** , clique em **Avançar**.  
  
8.  Depois de verificar as informações na página **confirmar seleções de instalação** , clique em **instalar**.  
  
9. Na página **Progresso da instalação**, verifique se tudo foi instalado corretamente e clique em **Fechar**.  
  
### <a name="to-install-the-ad-fs-server-role-via-windows-powershell"></a>Para instalar a função de servidor AD FS por meio do Windows PowerShell  
  
1.  No computador que você deseja configurar como um servidor de Federação, abra a janela de comando do Windows PowerShell e execute o seguinte comando: `Install-windowsfeature adfs-federation –IncludeManagementTools`.  
  
## <a name="see-also"></a>Consulte também 

[Implantação do AD FS](../../ad-fs/AD-FS-Deployment.md)  

[Guia de implantação do Windows Server 2012 R2 AD FS](../../ad-fs/deployment/Windows-Server-2012-R2-AD-FS-Deployment-Guide.md)  
 
[Como implantar um farm de servidores de federação](../../ad-fs/deployment/Deploying-a-Federation-Server-Farm.md)  
  

