---
ms.assetid: 10d6723e-c857-43da-9d2d-acb5641d3da8
title: "Ingressar um computador em um domínio"
description: 
author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.author: billmath
ms.openlocfilehash: 641a1541143206d06973a6a0f11c689390abea21
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 12/12/2017
---
# <a name="join-a-computer-to-a-domain"></a>Ingressar um computador em um domínio

>Aplica-se a: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Para os serviços de Federação do Active Directory \(AD FS\) à função, cada computador que funciona como um servidor de federação deve ter ingressado em um domínio. Proxies de servidor de federação podem ser associados a um domínio, mas isso não é um requisito.  
  
Você não precisa ingressar um servidor Web em um domínio, se o servidor Web está hospedando somente aplicativos com reconhecimento de claims\.  
  
A associação ao grupo **administradores**, ou equivalente, no computador local é o requisito mínimo para concluir este procedimento.  Examinar detalhes sobre como usar as contas apropriadas e agrupar associações em [Local e os grupos de domínio padrão ](https://go.microsoft.com/fwlink/?LinkId=83477).   
  
### <a name="to-join-a-computer-to-a-domain"></a>Para ingressar um computador em um domínio  
  
1.  Sobre o **iniciar** de tela, digite**painel de controle**, e pressione ENTER.  
  
2.  Navegue até **sistema e segurança**e clique em **sistema**.  
  
3.  Em **configurações de grupo de trabalho, domínio e nome de computador**, clique em **alterar configurações**.  
  
4.  Sobre o **nome do computador**, clique em **alteração**.  
  
5.  Em **membro do**, clique em **domínio**, digite o nome do domínio que este computador ingressar e, em seguida, clique em **Okey**.  
  
6.  Clique em **Okey**e reinicie o computador.  
  
## <a name="additional-references"></a>Referências adicionais  
[Lista de verificação: Configurar um servidor de Federação](Checklist--Setting-Up-a-Federation-Server.md)  
  
[Lista de verificação: Configurando um Proxy de servidor de Federação](Checklist--Setting-Up-a-Federation-Server-Proxy.md)  
  

