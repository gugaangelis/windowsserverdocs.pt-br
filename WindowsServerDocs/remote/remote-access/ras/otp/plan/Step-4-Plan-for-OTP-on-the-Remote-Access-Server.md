---
title: Etapa 4 planejar a OTP no servidor de acesso remoto
description: Este tópico faz parte do guia implantar o acesso remoto com autenticação OTP no Windows Server 2016.
manager: brianlic
ms.topic: article
ms.assetid: 4b97b2fd-767a-45c1-a64e-5b3edd0c8a47
ms.author: lizross
author: eross-msft
ms.openlocfilehash: def6fa1b3e252a2fd2ad8b3e00df1c222b9844f0
ms.sourcegitcommit: dfa48f77b751dbc34409aced628eb2f17c912f08
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/07/2020
ms.locfileid: "87958324"
---
# <a name="step-4-plan-for-otp-on-the-remote-access-server"></a>Etapa 4 planejar a OTP no servidor de acesso remoto

>Aplica-se a: Windows Server (Canal Semestral), Windows Server 2016

Depois de planejar o servidor RADIUS de OTP (senha única) e as configurações de certificado, a etapa final no planejamento de uma implantação de OTP de acesso remoto é planejar as configurações de OTP do cliente no servidor de acesso remoto.

|Tarefa|Descrição|
|----|--------|
|[4,1 plano para isenções de cliente de OTP](#bkmk_4_1_Exemptions)|Planeje as isenções para os usuários que você não precisa para autenticação usando a OTP.|
|[plano 4,2 para clientes do Windows 7](#bkmk_4_2_Win7)|Planeje implantar o DCA (Assistente de conectividade do DirectAccess) 2,0 em computadores cliente com Windows 7.|
|[plano 4,3 para cartões inteligentes](#BKMK_smartcard)|Planeje o uso de cartões inteligentes para autorização adicional.|

## <a name="41-plan-for-otp-client-exemptions"></a><a name="bkmk_4_1_Exemptions"></a>4,1 plano para isenções de cliente de OTP
Quando a autenticação OTP está habilitada, por padrão, todos os usuários precisam se autenticar usando uma combinação de nome de usuário e senha, e credenciais de OTP. No entanto, você pode permitir que os usuários selecionados se autentiquem usando apenas um nome de usuário e senha, sem OTP. Para fazer isso, crie um grupo de segurança e adicione todos os usuários que você deseja isentar da autenticação OTP.

> [!NOTE]
> Somente computadores cliente de uma única floresta podem ser isentos devido ao fato de que apenas um grupo de segurança pode ser selecionado para isenções de cliente.

## <a name="42-plan-for-windows-7-clients"></a><a name="bkmk_4_2_Win7"></a>plano 4,2 para clientes do Windows 7
Por padrão, os computadores cliente do Windows 7 não podem se autenticar usando a OTP.  Os computadores cliente do Windows 7 exigem o DCA 2,0 para autenticar usando a OTP em uma implantação de acesso remoto do Windows Server 2012. Para obter mais informações sobre o DCA 2,0, consulte o [Assistente de conectividade do directaccess 2,0](https://go.microsoft.com/fwlink/?LinkId=253699) no centro de download da Microsoft.

## <a name="43-plan-for-smart-cards"></a><a name="BKMK_smartcard"></a>plano 4,3 para cartões inteligentes
Quando a autenticação OTP estiver habilitada, a opção para habilitar o uso de cartões inteligentes para autorização adicional estará disponível. Crie um grupo de segurança para permitir o acesso temporário no caso de o cartão inteligente do usuário não estar funcionando.

## <a name="see-also"></a><a name="BKMK_Links"></a>Consulte também

-   [Configurar o DirectAccess com autenticação OTP](../deploy-ra-otp.md)

