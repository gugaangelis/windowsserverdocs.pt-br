---
ms.assetid: e33673ff-ea1c-4476-a549-3bf5899a47dd
title: "Instalar o serviço de função do serviço de Federação"
description: 
author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.author: billmath
ms.openlocfilehash: c520cbe22739f2bde263e133c7feb681d824d251
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 12/12/2017
---
# <a name="install-the-federation-service-role-service"></a>Instalar o serviço de função do serviço de Federação

>Aplica-se a: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Agora que você configurou um computador com os aplicativos pré-requisito e certificados corretamente, você estará pronto para instalar o serviço de função do serviço de Federação dos serviços de Federação do Active Directory \(AD FS\). Quando você instala o serviço de Federação em um computador, o computador se torna um servidor de Federação.  
  
> [!NOTE]  
> Para o design \(SSO\) federados Single\-Sign\-On da Web, você deve ter pelo menos um servidor de federação na organização do parceiro de conta e pelo menos um servidor de federação na organização do parceiro de recurso. Para obter mais informações, consulte [onde colocar um servidor de Federação](https://technet.microsoft.com/library/dd807127.aspx).  
  
Você pode usar o procedimento a seguir para instalar o serviço de função do serviço de Federação do AD FS em um computador que tornarão o primeiro servidor de Federação ou em um computador que se tornará um servidor de federação para um farm de servidores de Federação existente.  
  
## <a name="prerequisites"></a>Pré-requisitos  
Verifique se que um certificado SSL com a chave privada já foi instalado ou importado para o certificado local store \(Personal store\) antes de iniciar este procedimento. Se você for usar um certificado de assinatura token\ é emitido por uma autoridade de certificação \(CA\), verifique se que um certificado de assinatura de token\ com a chave privada já foi instalado ou importado para o certificado local store \(Personal store\) antes de iniciar este procedimento. Como alternativa, você pode criar um certificado assinado self\, assinatura de token\ usando o Assistente para adicionar funções, conforme descrito neste procedimento. Para obter mais informações sobre certificados de assinatura de token\, consulte [requisitos de certificado para servidores de Federação](https://technet.microsoft.com/library/dd807040.aspx).  
  
A associação ao grupo **administradores**, ou equivalente, no computador local é o requisito mínimo para concluir este procedimento.  Examinar detalhes sobre como usar as contas apropriadas e agrupar associações em [Local e os grupos de domínio padrão](https://go.microsoft.com/fwlink/?LinkId=83477) \ (http:///\/ go.microsoft.com\/fwlink\ /? LinkId\ = 83477\).   
  
#### <a name="to-install-the-federation-service-role-service"></a>Para instalar o serviço de função do serviço de Federação  
  
1.  Sobre o **iniciar** de tela, digite**Gerenciador do servidor**, e pressione ENTER.  
  
2.  Clique em **gerenciar**e clique em **adicionar funções e recursos** para começar a adição de funções e recursos do assistente.  
  
3.  Sobre o **antes de começar** página, clique em **próxima**.  
  
4.  Sobre o **selecionar o tipo de instalação** página, clique em **instalação baseado em Feature\ ou Role\**e clique em **próxima**.  
  
5.  No **servidor de destino Select** página, clique em **selecionar um servidor do pool servidor**, verifique se o computador de destino está realçado e, em seguida, clique em **próxima**.  
  
6.  Sobre o **selecionar funções de servidor** página, clique em **serviços de Federação do Active Directory**e, em seguida, clique em Avançar.  
  
    > [!NOTE]  
    > Se você for solicitado a instalar recursos adicionais do .NET Framework ou o serviço de ativação de processos do Windows, clique em **adicionar recursos** para instalá-los.  
  
7.  Sobre o **Selecione recursos** página, verifique se os recursos são definidos e, em seguida, clique em **próxima**.  
  
8.  Sobre o **\(AD FS\) serviço de Federação do Active Directory** página, clique em **próxima**.  
  
9. No **selecione Serviços de função** página, selecione o **serviço de Federação** caixa de seleção e, em seguida, clique em **próxima**.  
  
10. Sobre o **\(IIS\) função servidor Web** página, clique em **próxima**.  
  
11. Sobre o **selecione Serviços de função** página, clique em **próxima**.  
  
12. Depois de verificar as informações sobre o **confirmar seleções de instalação** página, selecione o **reiniciar o servidor de destino automaticamente, se necessário** caixa de seleção e, em seguida, clique em **instalar**.  
  
13. Sobre o **progresso da instalação** página, verificar se tudo está instalado corretamente e, em seguida, clique em **fechar**.  
  

