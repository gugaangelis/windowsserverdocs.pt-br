---
title: Etapa 3 configurar DC1
description: Este tópico faz parte do guia de laboratório de teste - demonstrar o DirectAccess com autenticação OTP e SecurID de RSA para o Windows Server 2016
manager: brianlic
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: networking-da
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 836a2a08-3d22-48d2-873e-80d7e57ebbd6
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 338e214ac10796d3f9864aef74190f2d27f173b7
ms.sourcegitcommit: afb0602767de64a76aaf9ce6a60d2f0e78efb78b
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/20/2019
ms.locfileid: "67281353"
---
# <a name="step-3-configure-dc1"></a>Etapa 3 configurar DC1

>Aplica-se a: Windows Server (canal semestral), Windows Server 2016

DC1 atua como um controlador de domínio, servidor DNS e servidor DHCP para o domínio corp.contoso.com. Configure o DC1 da seguinte maneira:  
  
## <a name="verify-user1-has-a-user-principal-name-defined-on-dc1"></a>Verifique se o que Usuário1 tem um nome de entidade de segurança do usuário definido no DC1  
  
1.  No DC1, abra o Gerenciador do servidor e clique em **AD DS** no painel esquerdo. Clique com botão direito **DC1** e selecione **Active Directory Users and Computers**. No painel esquerdo, expanda **corp.contoso.com\Users**e clique duas vezes em User1.  
  
2.  No **conta** guia Verifique **nome de logon do usuário** é definido como User1. Se não, em seguida, insira **User1** na **nome de logon do usuário** campo.  
  
3.  Clique em **OK**. Feche o console **Usuários e Computadores do Active Directory** .  
  


