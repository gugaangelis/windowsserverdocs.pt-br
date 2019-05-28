---
ms.assetid: e33673ff-ea1c-4476-a549-3bf5899a47dd
title: Instalar o serviço de função Serviço de Federação
description: ''
author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.author: billmath
ms.openlocfilehash: 80a6cb2bc8e6f0fdb1a777a42f5d245f98ac3dee
ms.sourcegitcommit: 0b5fd4dc4148b92480db04e4dc22e139dcff8582
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 05/24/2019
ms.locfileid: "66192093"
---
# <a name="install-the-federation-service-role-service"></a>Instalar o serviço de função Serviço de Federação

Agora que você configurou corretamente um computador com os aplicativos de pré-requisitos e os certificados, você está pronto para instalar o serviço de função serviço de Federação dos serviços de Federação do Active Directory \(do AD FS\). Quando você instala o serviço de Federação em um computador, esse computador torna-se um servidor de Federação.  
  
> [!NOTE]  
> Para um único de Web federado\-sinal\-na \(SSO\) design, você deve ter pelo menos um servidor de federação na organização do parceiro de conta e pelo menos um servidor de federação na organização do parceiro de recurso . Para obter mais informações, consulte [Onde colocar um servidor de federação](https://technet.microsoft.com/library/dd807127.aspx).  
  
Você pode usar o procedimento a seguir para instalar o serviço de função serviço de Federação do AD FS em um computador que se tornará o primeiro servidor de Federação ou em um computador que se tornará um servidor de federação para um farm de servidor de Federação existente.  
  
## <a name="prerequisites"></a>Pré-requisitos  
Verifique se que um certificado SSL com a chave privada já foi instalado ou importado para o repositório de certificados local \(repositório pessoal\) antes de iniciar este procedimento. Se você estiver usando um token\-assinatura de certificado é emitido por uma autoridade de certificação \(autoridade de certificação\), verificar se um token\-certificado de assinatura com a chave privada já foi instalado ou importado para o o repositório de certificados local \(repositório pessoal\) antes de iniciar este procedimento. Como alternativa, você pode criar um self\-assinado, token\-assinatura de certificado usando o Assistente para adicionar funções, conforme descrito neste procedimento. Para obter mais informações sobre token\-certificados de assinatura, consulte [requisitos de certificado para servidores de Federação](https://technet.microsoft.com/library/dd807040.aspx).  
  
A associação a **Administradores**, ou equivalente, no computador local é o requisito mínimo para concluir esse procedimento.  Examine os detalhes sobre como usar as contas apropriadas e associações de grupos em [domínio grupos padrão Local e](https://go.microsoft.com/fwlink/?LinkId=83477) \(http:\/\/go.microsoft.com\/fwlink\/? LinkId\=83477\).   
  
#### <a name="to-install-the-federation-service-role-service"></a>Para instalar o serviço de função Serviço de Federação  
  
1.  Sobre o **inicie** tela, digite**Gerenciador do servidor**, e, em seguida, pressione ENTER.  
  
2.  Clique em **Manage**e, em seguida, clique em **para adicionar funções e recursos** para iniciar o assistente Adicionar funções e recursos.  
  
3.  Na página **Antes de começar** , clique em **Avançar**.  
  
4.  Sobre o **Selecionar tipo de instalação** , clique em **função\-ou em recurso\-instalação baseada em**e clique em **Avançar**.  
  
5.  Sobre o **Selecionar servidor de destino** , clique em **selecione um servidor no pool de servidores**, verifique se o computador de destino é realçado e, em seguida, clique em **próxima**.  
  
6.  Sobre o **selecionar funções de servidor** , clique em **serviços de Federação do Active Directory**e, em seguida, clique em Avançar.  
  
    > [!NOTE]  
    > Se você for solicitado a instalar recursos adicionais do .NET Framework ou o serviço de ativação de processos do Windows, clique em **adicionar recursos** instalá-los.  
  
7.  Sobre o **selecionar recursos** página, verifique se os recursos são definidos e, em seguida, clique em **próxima**.  
  
8.  Sobre o **serviço de Federação do Active Directory \(do AD FS\)**  , clique em **próxima**.  
  
9. Sobre o **selecionar serviços de função** , selecione o **serviço de Federação** caixa de seleção e, em seguida, clique em **próxima**.  
  
10. Sobre o **função de servidor Web \(IIS\)**  , clique em **próxima**.  
  
11. Na página **Selecionar serviços de função**, clique em **Avançar**.  
  
12. Depois de verificar as informações na página **Confirmar seleções de instalação** , marque a caixa de seleção **Reiniciar cada servidor de destino automaticamente, se necessário** e clique em **Instalar**.  
  
13. Na página **Progresso da instalação**, verifique se tudo foi instalado corretamente e clique em **Fechar**.  
  

