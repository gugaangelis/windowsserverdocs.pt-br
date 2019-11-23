---
ms.assetid: c50ecc6a-9504-4b4a-816f-e762dcf3a95e
title: Instalar o serviço de função de proxy do Serviço de Federação
description: ''
author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adfs
ms.author: billmath
ms.openlocfilehash: 1f66e863c28aea7c9214c8363328a103b0a92f06
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71359572"
---
# <a name="install-the-federation-service-proxy-role-service"></a>Instalar o serviço de função de proxy do Serviço de Federação

Depois de configurar um computador com os aplicativos e certificados de pré-requisito, você estará pronto para instalar o serviço de função Proxy do Serviço de Federação do Serviços de Federação do Active Directory (AD FS) \(AD FS\). Você pode usar o procedimento a seguir para instalar o serviço de função Proxy do Serviço de Federação. Quando você instala o serviço de função Proxy do Serviço de Federação em um computador, esse computador torna-se um proxy de servidor de Federação.  
  
A associação a **Administradores**, ou equivalente, no computador local é o requisito mínimo para concluir esse procedimento.  Examine os detalhes sobre como usar as contas apropriadas e as associações de grupo em [grupos padrão e de domínio](https://go.microsoft.com/fwlink/?LinkId=83477).   
  
### <a name="to-install-the-federation-service-proxy-role-service-using-the-server-manager"></a>Para instalar o serviço de função Proxy do Serviço de Federação usando o Gerenciador do Servidor
  
1.  Na tela **Iniciar** , digite**Gerenciador do servidor**e pressione Enter.  
  
2.  Clique em **gerenciar**e em **adicionar funções e recursos** para iniciar o assistente para adicionar funções e recursos.  
  
3.  Na página **Before you begin**, clique em **Next**.  
  
4.  Na página **Selecionar tipo de instalação** , clique em **função\-baseada em\-instalação baseada em recursos**e clique em **Avançar**.  
  
5.  Na página **selecionar servidor de destino** , clique em **selecionar um servidor no pool de servidores**, verifique se o computador de destino está realçado e clique em **Avançar**.  
  
6.  Na página **selecionar funções de servidor** , clique em **acesso remoto**e, em seguida, clique em Avançar.  
  
    > [!NOTE]  
    > Se for solicitado que você instale .NET Framework adicionais ou recursos do serviço de ativação de processos do Windows, clique em **Adicionar recursos** para instalá-los.  
  
7. Na página **selecionar serviços de função** , marque a caixa de seleção **proxy do serviço de Federação** e clique em **Avançar**.  

8. Depois de verificar as informações na página **Confirmar seleções de instalação** , marque a caixa de seleção **Reiniciar cada servidor de destino automaticamente, se necessário** e clique em **Instalar**.  
  
13. Na página **Progresso da instalação**, verifique se tudo foi instalado corretamente e clique em **Fechar**.  

### <a name="to-install-the-federation-service-proxy-role-service-using-powershell"></a>Para instalar o serviço de função de Proxy do Serviço de Federação usando o PowerShell

1. Abrir o Windows PowerShell (executar como administrador)

2. Digite o seguinte comando e pressione **Enter**:

        Install-WindowsFeature Web-Application-Proxy -IncludeManagementTools



  
## <a name="additional-references"></a>Referências adicionais  
[Lista de verificação: Configurando um servidor de Federação](Checklist--Setting-Up-a-Federation-Server.md)  
  
[Lista de verificação: Configurando um proxy de servidor de Federação](Checklist--Setting-Up-a-Federation-Server-Proxy.md)  
  

