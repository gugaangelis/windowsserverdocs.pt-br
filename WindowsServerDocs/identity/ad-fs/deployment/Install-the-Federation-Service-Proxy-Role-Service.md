---
ms.assetid: c50ecc6a-9504-4b4a-816f-e762dcf3a95e
title: "Instalar o serviço de função de Proxy de serviço de Federação"
description: 
author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.author: billmath
ms.openlocfilehash: aea1ef335604aa18f0b1a1c22ef13f6fee1601b3
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 12/12/2017
---
# <a name="install-the-federation-service-proxy-role-service"></a>Instalar o serviço de função de Proxy de serviço de Federação

>Aplica-se a: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Depois de configurar um computador com os aplicativos pré-requisito e certificados, você estará pronto para instalar o serviço de função Proxy de serviço de Federação dos serviços de Federação do Active Directory \(AD FS\). Você pode usar o procedimento a seguir para instalar o serviço de função Proxy de serviço de Federação. Quando você instala o serviço de função Proxy de serviço de Federação em um computador, o computador se torna um proxy de servidor de Federação.  
  
A associação ao grupo **administradores**, ou equivalente, no computador local é o requisito mínimo para concluir este procedimento.  Examinar detalhes sobre como usar as contas apropriadas e agrupar associações em [Local e os grupos de domínio padrão ](https://go.microsoft.com/fwlink/?LinkId=83477).   
  
### <a name="to-install-the-federation-service-proxy-role-service"></a>Para instalar o serviço de função Proxy de serviço de Federação  
  
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
  
9. No **selecione Serviços de função** página, selecione o **Proxy de serviço de Federação** caixa de seleção e, em seguida, clique em **próxima**.  
  
10. Sobre o **\(IIS\) função servidor Web** página, clique em **próxima**.  
  
11. Sobre o **selecione Serviços de função** página, clique em **próxima**.  
  
12. Depois de verificar as informações sobre o **confirmar seleções de instalação** página, selecione o **reiniciar o servidor de destino automaticamente, se necessário** caixa de seleção e, em seguida, clique em **instalar**.  
  
13. Sobre o **progresso da instalação** página, verificar se tudo está instalado corretamente e, em seguida, clique em **fechar**.  
  
## <a name="additional-references"></a>Referências adicionais  
[Lista de verificação: Configurar um servidor de Federação](Checklist--Setting-Up-a-Federation-Server.md)  
  
[Lista de verificação: Configurando um Proxy de servidor de Federação](Checklist--Setting-Up-a-Federation-Server-Proxy.md)  
  

