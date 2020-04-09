---
title: ETAPA 3 configurar DC1
description: Este tópico faz parte do guia de laboratório de teste – demonstre o DirectAccess com autenticação OTP e RSA SecurID para Windows Server 2016
manager: brianlic
ms.prod: windows-server
ms.technology: networking-da
ms.topic: article
ms.assetid: 836a2a08-3d22-48d2-873e-80d7e57ebbd6
ms.author: lizross
author: eross-msft
ms.openlocfilehash: 07167f2bc68ab8c465a96ce00552339d04dbb198
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80856069"
---
# <a name="step-3-configure-dc1"></a>ETAPA 3 configurar DC1

>Aplicável ao: Windows Server (canal semestral), Windows Server 2016

DC1 atua como um controlador de domínio, servidor DNS e servidor DHCP para o domínio corp.contoso.com. Configure o DC1 da seguinte maneira:  
  
## <a name="verify-user1-has-a-user-principal-name-defined-on-dc1"></a>Verifique se user1 tem um nome UPN definido em DC1  
  
1.  No DC1, abra Gerenciador do Servidor e clique em **AD DS** no painel esquerdo. Clique com o botão direito do mouse em **DC1** e selecione **Active Directory usuários e computadores**. No painel esquerdo, expanda **Corp. contoso. com\Users**e clique duas vezes em Usuário1.  
  
2.  Na guia **conta** , verifique se **nome de logon do usuário** está definido como Usuário1. Caso contrário, digite **user1** no campo **nome de logon do usuário** .  
  
3.  Clique em **OK**. Feche o console **Usuários e Computadores do Active Directory**.  
  


