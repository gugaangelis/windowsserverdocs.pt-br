---
ms.assetid: e33673ff-ea1c-4476-a549-3bf5899a47dd
title: Instalar o serviço de função Serviço de Federação
description: ''
author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adfs
ms.author: billmath
ms.openlocfilehash: 73c564e1c1117b229f759ca114b18a2d4c8002fa
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71408387"
---
# <a name="install-the-federation-service-role-service"></a>Instalar o serviço de função Serviço de Federação

Agora que você configurou corretamente um computador com os aplicativos e certificados de pré-requisito, você está pronto para instalar o serviço de função Serviço de Federação do Serviços de Federação do Active Directory (AD FS) \(AD FS\). Quando você instala o Serviço de Federação em um computador, esse computador torna-se um servidor de Federação.  
  
> [!NOTE]  
> Para o\-de logon\-único da Web federado em \(design de\) SSO, você deve ter pelo menos um servidor de Federação na organização do parceiro de conta e pelo menos um servidor de Federação na organização do parceiro de recurso. Para obter mais informações, consulte [Onde colocar um servidor de federação](https://technet.microsoft.com/library/dd807127.aspx).  
  
Você pode usar o procedimento a seguir para instalar o serviço de função Serviço de Federação do AD FS em um computador que se tornará o primeiro servidor de Federação ou em um computador que se tornará um servidor de Federação para um farm de servidores de Federação existente.  
  
## <a name="prerequisites"></a>Pré-requisitos  
Verifique se um certificado SSL com a chave privada já foi instalado ou importado para o repositório de certificados local \(repositório pessoal\) antes de iniciar este procedimento. Se você estiver usando um token\-certificado de assinatura emitido por uma autoridade de certificação \(\)de AC, verifique se um token\-certificado de autenticação com a chave privada já foi instalado ou importado para o repositório de certificados local \(repositório pessoal\) antes de iniciar este procedimento. Como alternativa, você pode criar um certificado de assinatura de token\-\-assinado por conta própria usando o assistente para adicionar funções, conforme descrito neste procedimento. Para obter mais informações sobre certificados de assinatura de\-de token, consulte [requisitos de certificado para servidores de Federação](https://technet.microsoft.com/library/dd807040.aspx).  
  
A associação a **Administradores**, ou equivalente, no computador local é o requisito mínimo para concluir esse procedimento.  Examine os detalhes sobre como usar as contas apropriadas e as associações de grupo em [grupos padrão e de domínio](https://go.microsoft.com/fwlink/?LinkId=83477) \(http:\/\/go.Microsoft.com\/fwlink\/? LinkId\=\)83477.   
  
#### <a name="to-install-the-federation-service-role-service"></a>Para instalar o serviço de função Serviço de Federação  
  
1.  Na tela **Iniciar** , digite**Gerenciador do servidor**e pressione Enter.  
  
2.  Clique em **gerenciar**e em **adicionar funções e recursos** para iniciar o assistente para adicionar funções e recursos.  
  
3.  Na página **Before you begin**, clique em **Next**.  
  
4.  Na página **Selecionar tipo de instalação** , clique em **função\-baseada em\-instalação baseada em recursos**e clique em **Avançar**.  
  
5.  Na página **selecionar servidor de destino** , clique em **selecionar um servidor no pool de servidores**, verifique se o computador de destino está realçado e clique em **Avançar**.  
  
6.  Na página **selecionar funções de servidor** , clique em **serviços de Federação do Active Directory (AD FS)** e em Avançar.  
  
    > [!NOTE]  
    > Se for solicitado que você instale .NET Framework adicionais ou recursos do serviço de ativação de processos do Windows, clique em **Adicionar recursos** para instalá-los.  
  
7.  Na página **selecionar recursos** , verifique se os recursos estão definidos e clique em **Avançar**.  
  
8.  Na página **Active Directory Serviço de Federação \(AD FS\)** , clique em **Avançar**.  
  
9. Na página **selecionar serviços de função** , marque a caixa de seleção **serviço de Federação** e clique em **Avançar**.  
  
10. Na página **função do servidor Web \(\)do IIS** , clique em **Avançar**.  
  
11. Na página **Selecionar serviços de função**, clique em **Avançar**.  
  
12. Depois de verificar as informações na página **Confirmar seleções de instalação** , marque a caixa de seleção **Reiniciar cada servidor de destino automaticamente, se necessário** e clique em **Instalar**.  
  
13. Na página **Progresso da instalação**, verifique se tudo foi instalado corretamente e clique em **Fechar**.  
  

