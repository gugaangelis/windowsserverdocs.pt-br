---
ms.assetid: c50ecc6a-9504-4b4a-816f-e762dcf3a95e
title: Instalar o serviço de função de proxy do Serviço de Federação
author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adfs
ms.author: billmath
ms.openlocfilehash: 8d2ff177c821baf31bb5453b7c50e3eadca2aab7
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80855379"
---
# <a name="install-the-federation-service-proxy-role-service"></a>Instalar o serviço de função de proxy do Serviço de Federação

Depois de configurar um computador com os aplicativos e certificados de pré-requisito, você estará pronto para instalar o serviço de função Proxy do Serviço de Federação do Serviços de Federação do Active Directory (AD FS) \(AD FS\). Você pode usar o procedimento a seguir para instalar o serviço de função Proxy do Serviço de Federação. Quando você instala o serviço de função Proxy do Serviço de Federação em um computador, esse computador torna-se um proxy de servidor de Federação.  
  
A associação em **Administradores**, ou equivalente, no computador local é o mínimo necessário para concluir este procedimento.  Examine os detalhes sobre como usar as contas apropriadas e as associações de grupo em [grupos padrão e de domínio](https://go.microsoft.com/fwlink/?LinkId=83477).   
  
### <a name="to-install-the-federation-service-proxy-role-service-using-the-server-manager"></a>Para instalar o serviço de função Proxy do Serviço de Federação usando o Gerenciador do Servidor
  
1.  Na tela **Iniciar** , digite**Gerenciador do servidor**e pressione Enter.  
  
2.  Clique em **Gerenciar** e clique em **Adicionar Funções e Recursos** para iniciar o Assistente para Adicionar Funções e Recursos.  
  
3.  Na página **Antes de começar**, clique em **Avançar**.  
  
4.  Na página **Selecionar tipo de instalação** , clique em **função\-baseada em\-instalação baseada em recursos**e clique em **Avançar**.  
  
5.  Na página **Selecionar servidor de destino**, clique em **Selecionar um servidor no pool de servidores**, verifique se o computador alvo está selecionado e, depois, clique em **Avançar**.  
  
6.  Na página **selecionar funções de servidor** , clique em **acesso remoto**e, em seguida, clique em Avançar.  
  
    > [!NOTE]  
    > Se for solicitado instalar recursos adicionais do Framework .NET adicional ou do Serviço de Ativação do Processo do Windows, clique em **Adicionar Recursos** para instalar eles.  
  
7. Na página **Selecionar serviços de função**, marque a caixa de seleção **Proxy de Serviço de Federação** e clique em **Avançar**.  

8. Após ter verificado as informações na página **Confirmar as seleções de instalação**, marque a caixa de seleção **Reiniciar o servidor de destino automaticamente se for necessário**, e em seguida clique em **Instalar**.  
  
13. Na página **Progresso da instalação**, verifique se tudo foi instalado corretamente e clique em **Fechar**.  

### <a name="to-install-the-federation-service-proxy-role-service-using-powershell"></a>Para instalar o serviço de função de Proxy do Serviço de Federação usando o PowerShell

1. Abrir o Windows PowerShell (executar como administrador)

2. Digite o seguinte comando e pressione **Enter**:

        Install-WindowsFeature Web-Application-Proxy -IncludeManagementTools



  
## <a name="additional-references"></a>Referências adicionais  
[Lista de verificação: Configurando um servidor de Federação](Checklist--Setting-Up-a-Federation-Server.md)  
  
[Lista de verificação: Configurando um proxy de servidor de Federação](Checklist--Setting-Up-a-Federation-Server-Proxy.md)  
  

