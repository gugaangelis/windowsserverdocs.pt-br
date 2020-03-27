---
title: ETAPA 3 configurar DC1
description: Este tópico faz parte do guia de laboratório de teste – demonstre o DirectAccess com autenticação OTP e RSA SecurID para Windows Server 2016
manager: brianlic
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: networking-da
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 836a2a08-3d22-48d2-873e-80d7e57ebbd6
ms.author: lizross
author: eross-msft
ms.openlocfilehash: 8104868103fff29044041136b48c59d966cc7bd0
ms.sourcegitcommit: da7b9bce1eba369bcd156639276f6899714e279f
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/26/2020
ms.locfileid: "80314402"
---
# <a name="step-3-configure-dc1"></a>ETAPA 3 configurar DC1

>Aplicável ao: Windows Server (canal semestral), Windows Server 2016

DC1 atua como um controlador de domínio, servidor DNS e servidor DHCP para o domínio corp.contoso.com. Configure o DC1 da seguinte maneira:  
  
## <a name="verify-user1-has-a-user-principal-name-defined-on-dc1"></a>Verifique se user1 tem um nome UPN definido em DC1  
  
1.  No DC1, abra Gerenciador do Servidor e clique em **AD DS** no painel esquerdo. Clique com o botão direito do mouse em **DC1** e selecione **Active Directory usuários e computadores**. No painel esquerdo, expanda **Corp. contoso. com\Users**e clique duas vezes em Usuário1.  
  
2.  Na guia **conta** , verifique se **nome de logon do usuário** está definido como Usuário1. Caso contrário, digite **user1** no campo **nome de logon do usuário** .  
  
3.  Clique em **OK**. Feche o console **Usuários e Computadores do Active Directory**.  
  


