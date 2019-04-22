---
title: Planeje de etapa 4 de OTP no servidor de acesso remoto
description: Este tópico faz parte do guia de implantação de acesso remoto com autenticação OTP no Windows Server 2016.
manager: brianlic
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology:
- networking-ras
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 4b97b2fd-767a-45c1-a64e-5b3edd0c8a47
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 470eebc82177a21985afb8d0bf143427a33d65fb
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59825437"
---
# <a name="step-4-plan-for-otp-on-the-remote-access-server"></a>Planeje de etapa 4 de OTP no servidor de acesso remoto

>Aplica-se a: Windows Server (canal semestral), Windows Server 2016

Depois de planejar as configurações de certificado e a senha de uso único servidor RADIUS (OTP), a etapa final do planejamento da implantação OTP de acesso remoto é planejar as configurações de OTP de cliente no servidor de acesso remoto.  
  
|Tarefa|Descrição|  
|----|--------|  
|[4.1 planejar isenções de cliente OTP](#bkmk_4_1_Exemptions)|Planejar isenções para os usuários que não exigem a autenticação de OTP.|  
|[4.2 planejar para clientes do Windows 7](#bkmk_4_2_Win7)|Planeje implantar o DirectAccess conectividade DCA (Assistente) 2.0 para computadores cliente do Windows 7.|  
|[4.3 plano para cartões inteligentes](#BKMK_smartcard)|Planejar o uso de cartões inteligentes para autorização adicional.|  
  
## <a name="bkmk_4_1_Exemptions"></a>4.1 planejar isenções de cliente OTP  
Quando a autenticação de OTP está habilitada, por padrão, todos os usuários são necessárias para se autenticar usando uma combinação de nome de usuário e senha e as credenciais OTP. No entanto, você pode permitir que os usuários selecionados autentiquem usando um nome de usuário e senha, sem a OTP. Para fazer isso, crie um grupo de segurança e adicione todos os usuários que você deseja a isentar da autenticação de OTP.  
  
> [!NOTE]  
> Somente os computadores cliente de uma única floresta podem ser isentados devido ao fato de que apenas um grupo de segurança pode ser selecionado para isolamentos do cliente.  
  
## <a name="bkmk_4_2_Win7"></a>4.2 planejar para clientes do Windows 7  
Por padrão, os computadores cliente do Windows 7 não podem autenticar usando OTP.  Computadores cliente do Windows 7 exigem DCA 2.0 autenticar usando a OTP em uma implantação de acesso remoto do Windows Server 2012. Para obter mais informações sobre o DCA 2.0, consulte [2.0 de Assistente de conectividade do DirectAccess](https://go.microsoft.com/fwlink/?LinkId=253699) no Microsoft Download Center.  
  
## <a name="BKMK_smartcard"></a>4.3 plano para cartões inteligentes  
Quando a autenticação de OTP está habilitada, a opção para habilitar o uso de cartões inteligentes para autorização adicional está disponível. Crie um grupo de segurança para permitir o acesso temporário que cartão inteligente de um usuário não está funcionando.  
  
## <a name="BKMK_Links"></a>Consulte também  
  
-   [Configurar o DirectAccess com autenticação OTP](https://technet.microsoft.com/windows-server-docs/networking/remote-access/ras/otp/deploy-ra-otp)  
  


