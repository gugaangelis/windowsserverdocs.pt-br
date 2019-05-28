---
ms.assetid: c50ecc6a-9504-4b4a-816f-e762dcf3a95e
title: Instalar o serviço de função de proxy do Serviço de Federação
description: ''
author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.author: billmath
ms.openlocfilehash: 6e78c52f1928a3401c0532ab7c25616b012a1d8b
ms.sourcegitcommit: 0b5fd4dc4148b92480db04e4dc22e139dcff8582
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 05/24/2019
ms.locfileid: "66192104"
---
# <a name="install-the-federation-service-proxy-role-service"></a>Instalar o serviço de função de proxy do Serviço de Federação

Depois de configurar um computador com os aplicativos de pré-requisitos e os certificados, você está pronto para instalar o serviço de função Proxy do serviço de Federação dos serviços de Federação do Active Directory \(do AD FS\). Você pode usar o procedimento a seguir para instalar o serviço de função Proxy do serviço de Federação. Quando você instala o serviço de função Proxy do serviço de Federação em um computador, esse computador torna-se um proxy do servidor de Federação.  
  
A associação a **Administradores**, ou equivalente, no computador local é o requisito mínimo para concluir esse procedimento.  Examine os detalhes sobre como usar as contas apropriadas e associações de grupos em [domínio grupos padrão Local e](https://go.microsoft.com/fwlink/?LinkId=83477).   
  
### <a name="to-install-the-federation-service-proxy-role-service-using-the-server-manager"></a>Para instalar o serviço de função Proxy do serviço de Federação usando o Gerenciador do servidor
  
1.  Sobre o **inicie** tela, digite**Gerenciador do servidor**, e, em seguida, pressione ENTER.  
  
2.  Clique em **Manage**e, em seguida, clique em **para adicionar funções e recursos** para iniciar o assistente Adicionar funções e recursos.  
  
3.  Na página **Antes de começar** , clique em **Avançar**.  
  
4.  Sobre o **Selecionar tipo de instalação** , clique em **função\-ou em recurso\-instalação baseada em**e clique em **Avançar**.  
  
5.  Sobre o **Selecionar servidor de destino** , clique em **selecione um servidor no pool de servidores**, verifique se o computador de destino é realçado e, em seguida, clique em **próxima**.  
  
6.  Sobre o **selecionar funções de servidor** , clique em **acesso remoto**e, em seguida, clique em Avançar.  
  
    > [!NOTE]  
    > Se você for solicitado a instalar recursos adicionais do .NET Framework ou o serviço de ativação de processos do Windows, clique em **adicionar recursos** instalá-los.  
  
7. Sobre o **selecionar serviços de função** , selecione o **Proxy do serviço de Federação** caixa de seleção e, em seguida, clique em **próxima**.  

8. Depois de verificar as informações na página **Confirmar seleções de instalação** , marque a caixa de seleção **Reiniciar cada servidor de destino automaticamente, se necessário** e clique em **Instalar**.  
  
13. Na página **Progresso da instalação**, verifique se tudo foi instalado corretamente e clique em **Fechar**.  

### <a name="to-install-the-federation-service-proxy-role-service-using-powershell"></a>Para instalar o serviço de função Proxy do serviço de Federação usando o PowerShell

1. Abra o Windows PowerShell (Executar como administrador)

2. Digite o seguinte comando e pressione **Enter**:

        Install-WindowsFeature Web-Application-Proxy -IncludeManagementTools



  
## <a name="additional-references"></a>Referências adicionais  
[Lista de verificação: Como configurar um servidor de federação](Checklist--Setting-Up-a-Federation-Server.md)  
  
[Lista de verificação: Como configurar um proxy do servidor de federação](Checklist--Setting-Up-a-Federation-Server-Proxy.md)  
  

