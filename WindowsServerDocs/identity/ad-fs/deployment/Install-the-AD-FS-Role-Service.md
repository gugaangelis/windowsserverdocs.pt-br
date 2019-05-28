---
ms.assetid: c28a1b8b-5bec-4eed-8c95-a1a29cfc957c
title: Instalar o serviço de função do AD FS
description: ''
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: ffa9b20d4d7b5c84b0e29ac446b8aa6f3a932850
ms.sourcegitcommit: 0b5fd4dc4148b92480db04e4dc22e139dcff8582
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 05/24/2019
ms.locfileid: "66192116"
---
# <a name="install-the-ad-fs-role-service"></a>Instalar o serviço de função do AD FS

Você pode usar o procedimento a seguir para instalar o serviço de função do AD FS em um computador que esteja executando o Windows Server 2012 R2 para se tornar o primeiro servidor de Federação em um farm de servidores de Federação ou um servidor de Federação em um farm de servidor de Federação existente.  
  
Associação na **administradores**, ou equivalente, no computador local é o requisito mínimo para concluir este procedimento.  Examine os detalhes sobre como usar as contas apropriadas e associações de grupos em [domínio grupos padrão Local e](https://go.microsoft.com/fwlink/?LinkId=83477).   
  
### <a name="to-install-the-ad-fs-server-role-via-the-add-roles-and-features-wizard"></a>Para instalar a função de servidor AD FS por meio do assistente Adicionar funções e recursos  
  
1.  Abra o Gerenciador do Servidor. Para abrir o Gerenciador do servidor, clique em **Gerenciador do servidor** sobre o **inicie** tela, ou **Gerenciador do servidor** na barra de tarefas na área de trabalho. Na guia **Início Rápido** do bloco de **Boas-vindas** da página **Painel**, clique em **Adicionar funções e recursos**. Como alternativa, é possível clicar em **Adicionar Funções e Recursos** no menu **Gerenciar**.  
  
2.  Na página **Antes de começar** , clique em **Avançar**.  
  
3.  Sobre o **Selecionar tipo de instalação** , clique em **função\-ou em recurso\-instalação baseada em**e, em seguida, clique em **Avançar**.  
  
4.  Na página **Selecionar servidor de destino**, clique em **Selecionar um servidor no pool de servidor**, verifique se o computador de destino está selecionado e clique em **Avançar**.  
  
5.  Na página **Selecionar funções de servidor** , clique em **Serviços de Federação do Active Directory**e, depois, em **Avançar**.  
  
6.  Na página **Selecionar recursos**, clique em **Avançar**. Os pré-requisitos necessários são pré-selecionados para você. Você não precisa selecionar nenhum outro recurso.  
  
7.  Sobre o **serviço de Federação do Active Directory \(do AD FS\)**  , clique em **próxima**.  
  
8.  Depois de verificar as informações sobre o **confirmar seleções de instalação** , clique em **instalar**.  
  
9. Na página **Progresso da instalação**, verifique se tudo foi instalado corretamente e clique em **Fechar**.  
  
### <a name="to-install-the-ad-fs-server-role-via-windows-powershell"></a>Para instalar a função de servidor AD FS por meio do Windows PowerShell  
  
1.  No computador que você deseja configurar como um servidor de federação, abra a janela de comando do Windows PowerShell e, em seguida, execute o seguinte comando: `Install-windowsfeature adfs-federation –IncludeManagementTools`.  
  
## <a name="see-also"></a>Consulte também 

[Implantação do AD FS](../../ad-fs/AD-FS-Deployment.md)  

[Guia de implantação do Windows Server 2012 R2 AD FS](../../ad-fs/deployment/Windows-Server-2012-R2-AD-FS-Deployment-Guide.md)  
 
[Como implantar um farm de servidores de federação](../../ad-fs/deployment/Deploying-a-Federation-Server-Farm.md)  
  

